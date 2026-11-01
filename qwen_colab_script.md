# Script chạy Qwen2.5-VL-7B-Instruct trên Colab tự tạo (không cần xin quyền ai)

Copy từng khối code bên dưới vào từng cell riêng trong Colab, chạy theo thứ tự.

**Nếu vừa gặp lỗi `CUDA out of memory`:** trước khi chạy lại, bắt buộc `Runtime > Restart session` (hoặc "Khởi động lại phiên") để giải phóng sạch bộ nhớ GPU — process bị lỗi thường để lại bộ nhớ chưa giải phóng hết, chạy lại ngay mà không restart dễ bị lỗi tiếp dù đã sửa code. Sau khi restart, chạy lại từ Cell 1.

## Cell 1 — cài thư viện
```python
!pip install -q transformers accelerate qwen-vl-utils bitsandbytes
```

## Cell 2 — tải model (bản 4-bit để vừa GPU T4 miễn phí 16GB)
```python
import torch
from transformers import Qwen2_5_VLForConditionalGeneration, AutoProcessor, BitsAndBytesConfig

bnb_config = BitsAndBytesConfig(load_in_4bit=True, bnb_4bit_compute_dtype=torch.float16)

model = Qwen2_5_VLForConditionalGeneration.from_pretrained(
    "Qwen/Qwen2.5-VL-7B-Instruct",
    quantization_config=bnb_config,
    device_map="auto",
)

# Giới hạn số pixel ảnh đầu vào — BẮT BUỘC, nếu không sẽ lỗi CUDA OutOfMemory
# vì ảnh scan 300dpi quá lớn khiến model tạo quá nhiều "visual token".
min_pixels = 256 * 28 * 28
max_pixels = 1024 * 28 * 28
processor = AutoProcessor.from_pretrained(
    "Qwen/Qwen2.5-VL-7B-Instruct",
    min_pixels=min_pixels,
    max_pixels=max_pixels,
)
```
Nếu vẫn lỗi thiếu VRAM: thử thêm `max_memory={0: "14GiB"}` vào `from_pretrained` của model, hoặc giảm `max_pixels` xuống `768*28*28`.

## Cell 3 — upload ảnh (zip toàn bộ `_pages_raw/` rồi upload 1 lần)
```python
from google.colab import files
uploaded = files.upload()  # chọn file zip chứa các ảnh page-0XX.png

import zipfile, os
with zipfile.ZipFile(list(uploaded.keys())[0], 'r') as z:
    z.extractall('pages')
print(sorted(os.listdir('pages'))[:5])
```

## Cell 4 — hàm OCR 1 ảnh (đổi prompt để lấy Hán hoặc Việt)
```python
from PIL import Image

def ocr_image(image_path, mode="han"):
    if mode == "han":
        prompt = ("Đọc chính xác toàn bộ chữ Hán trong ảnh này theo thứ tự "
                   "từ phải sang trái, từ trên xuống dưới (chữ viết dọc kiểu câu đối). "
                   "Chỉ trả về nguyên văn chữ Hán, không dịch, không giải thích, "
                   "không thêm ký tự nào khác ngoài chữ Hán đọc được.")
    else:
        prompt = ("Đọc chính xác toàn bộ phần chữ Việt (dòng 'Dịch:' hoặc chú thích tiếng Việt) "
                   "trong ảnh này. Chỉ trả về nguyên văn tiếng Việt đọc được, không thêm giải thích.")

    image = Image.open(image_path).convert("RGB")
    # resize thêm 1 lớp an toàn phòng khi processor không tự áp dụng max_pixels
    # (ảnh scan 300dpi gốc có thể ~2500x3300px, quá lớn gây OOM)
    max_side = 1280
    if max(image.size) > max_side:
        ratio = max_side / max(image.size)
        new_size = (int(image.width * ratio), int(image.height * ratio))
        image = image.resize(new_size, Image.LANCZOS)

    messages = [{"role": "user", "content": [
        {"type": "image", "image": image},
        {"type": "text", "text": prompt},
    ]}]
    text = processor.apply_chat_template(messages, tokenize=False, add_generation_prompt=True)
    inputs = processor(text=[text], images=[image], return_tensors="pt").to(model.device)
    output_ids = model.generate(**inputs, max_new_tokens=512)
    output_text = processor.batch_decode(
        output_ids[:, inputs.input_ids.shape[1]:], skip_special_tokens=True
    )[0]
    return output_text.strip()
```

## Cell 5 — chạy OCR (có thể chạy hết 1 lần, hoặc chia đợt)
```python
import json, os

RESULTS_FILE = "results.json"
START = 0    # giữ 0
END = 9999   # số lớn hơn tổng số ảnh -> chạy hết 148 ảnh trong 1 lần gọi cell này
             # (muốn chia đợt thì đổi END=30, rồi lần sau START=30,END=60... như trước)

# tải lại kết quả cũ nếu đã có (để gộp dần qua từng đợt, không bị mất)
if os.path.exists(RESULTS_FILE):
    with open(RESULTS_FILE, "r", encoding="utf-8") as f:
        results = json.load(f)
else:
    results = {}

page_dir = "pages"
all_files = sorted(f for f in os.listdir(page_dir) if f.endswith(".png"))
batch = all_files[START:END]
print(f"Đợt này xử lý {len(batch)} ảnh: {batch[0]} .. {batch[-1]}")

for fname in batch:
    if fname in results:
        continue  # đã xử lý ở đợt trước, bỏ qua
    path = os.path.join(page_dir, fname)
    han_text = ocr_image(path, mode="han")
    viet_text = ocr_image(path, mode="viet")
    results[fname] = {"han": han_text, "viet": viet_text}
    print(fname, "->", han_text[:30], "|", viet_text[:30])

    # lưu đè sau MỖI ảnh — lỡ Colab ngắt giữa chừng cũng không mất ảnh đã xử lý
    with open(RESULTS_FILE, "w", encoding="utf-8") as f:
        json.dump(results, f, ensure_ascii=False, indent=2)

print(f"Đã có {len(results)}/{len(all_files)} ảnh trong results.json")
files.download(RESULTS_FILE)
```

**Chạy 1 lần hết toàn bộ 148 ảnh:** để `START=0, END=9999` như trên rồi chạy Cell 5 — vòng lặp `for` bên trong tự xử lý tuần tự cả 148 ảnh, không cần bạn bấm lại hay đổi số gì cả. Việc lưu `results.json` vẫn diễn ra sau **mỗi ảnh** như cũ, nên nếu giữa chừng Colab bị ngắt, ảnh đã xong không mất.

Đánh đổi: cell sẽ chạy liên tục khá lâu (148 ảnh × 2 lần gọi model mỗi ảnh), tab Colab cần mở và trình duyệt không được ngủ/đóng tab giữa chừng. Nếu mạng yếu hoặc muốn kiểm tra kết quả dần, nên chia đợt (`END=30` rồi tăng dần mỗi lần chạy lại) thay vì chạy 1 lần.

**Nếu Colab bị ngắt kết nối giữa chừng (dù chạy 1 lần hay chia đợt):** kết nối lại, chạy lại Cell 1–4 (tải model lại), rồi chạy lại Cell 5 với `START=0, END=9999` (hoặc đúng đợt đang dở) — các ảnh đã xong (đã nằm trong `results.json` tải lại từ đĩa) tự động được bỏ qua (dòng `if fname in results: continue`), không bị làm lại.

## Sau khi xong hết các đợt
1. Tải `results.json` về, đặt vào `02_ocr_tho/Qwen_vl2.5_7B/results.json`.
2. Nhờ Claude (hoặc tự viết script nhỏ) chuyển `results.json` sang đúng định dạng `Label.txt` + `fileState.txt` nếu cần khớp chuẩn PPOCRLabel.
