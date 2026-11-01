# kimhannom.fit.hcmus.edu.vn — kết quả thô

Công cụ web, chạy tay (upload từng ảnh), không chạy hàng loạt được như Qwen. Dùng cho 15 trang mà cả Claude (đọc tay) và Qwen đều không đọc được gì:

**10, 11, 25, 28, 44, 45, 46, 50, 55, 60, 69, 78, 80, 128, 129**

## Cách lưu / đặt tên (thống nhất với `Qwen_vl2.5_7B/results.json`)

1. Vào `kimhannom.fit.hcmus.edu.vn`, upload đúng file `_pages_raw/page-0XX.png` (giữ số trang PDF vật lý 3 chữ số, khớp tên ảnh gốc — **không dùng số trang in**).
2. Copy kết quả Hán mà engine trả về, dán vào đúng key `"page-0XX.png"` trong `results.json` ở cùng thư mục này (file mẫu đã tạo sẵn 15 key rỗng).
3. Nếu engine có luôn phần phiên âm/dịch nghĩa thì điền vào `"viet"`, không có thì để `""`.
4. Nếu 1 trang thử nhiều lần cho ra kết quả khác nhau, ghi lại CẢ các lần thử vào `"candidates"` (danh sách), chọn 1 bản vào `"han"` là bản bạn thấy hợp lý nhất kèm `"note"` giải thích vì sao chọn.
5. Không sửa `draft_pairs.tsv` trực tiếp từ đây — kết quả ở đây chỉ là **nguyên liệu thô**. Sau khi có đủ, gửi lại để đối chiếu rồi mới quyết định đưa vào `draft_pairs.tsv` hay không (nhiều trang trong 15 trang này có thể vẫn không đọc được, giữ nguyên trạng thái loại trừ).

## Định dạng `results.json`

```json
{
  "page-010.png": {"han": "", "viet": "", "candidates": [], "note": ""},
  "page-011.png": {"han": "", "viet": "", "candidates": [], "note": ""}
}
```

Giữ cùng format key `"page-0XX.png"` như `Qwen_vl2.5_7B/results.json` để sau này script đối chiếu dùng lại được luôn, không cần viết lại.
