# Đối chiếu `draft_pairs.tsv` (261 cặp, Claude đọc tay) với kết quả Qwen2.5-VL-7B (148/148 ảnh)

## Phương pháp
1. Chuẩn hoá cả 2 nguồn về chữ giản thể (dùng `opencc`, vì Qwen luôn trả về giản thể, còn bản Claude đọc theo đúng mặt chữ phồn thể trong ảnh).
2. Ánh xạ chính xác từng trang in → đúng các dòng trong `draft_pairs.tsv` (dựa theo đúng thứ tự Claude đã đọc, có trừ các trang bị loại và các trang hoành phi/4-vế).
3. Với mỗi trang, gộp toàn bộ chữ Hán Claude đọc được (tất cả các dòng thuộc trang đó) thành 1 chuỗi, so với chuỗi Hán Qwen đọc được cho đúng trang đó, tính độ khớp theo tỷ lệ ký tự trùng nhau (Jaccard).

## Kết quả tổng quan (106 trang có nội dung câu đối + hoành phi)

| Mức khớp | Số trang | Ý nghĩa |
|---|---|---|
| 0.9 – 1.0 | 62 | Khớp gần như tuyệt đối |
| 0.7 – 0.9 | 8 | Khớp tốt, lệch 1 vài chữ nhỏ |
| 0.5 – 0.7 | 33 | Qwen **chỉ đọc được 1 trong 2 vế** (bỏ sót vế còn lại) — phần đọc được thì khớp |
| 0.3 – 0.5 | 3 | Có khác biệt thật sự, xem chi tiết bên dưới |
| 0.0 – 0.3 | 0 | — |

**Kết luận chung: bản đọc tay của Claude (`draft_pairs.tsv`) được Qwen xác nhận ở mức rất cao.** Phần lớn điểm thấp là do **Qwen không đọc hết cả câu đối** (chỉ trả về 1 vế), không phải do nội dung sai.

## 3 trang có khác biệt thật sự — đã xem kỹ từng trường hợp

| Trang in | Claude đọc | Qwen đọc | Nhận định |
|---|---|---|---|
| 124 | 建造家風新骨格... | 家风新骨格 (thiếu "建造" + cả vế 2) | Qwen đọc thiếu, không phải sai |
| 105 | 關河是客情非客... | 吴河是客情非客 (關→吴) | Bản Claude có cột âm "Quan hà..." hỗ trợ xác nhận đúng là 關, không phải 吴 — nhiều khả năng Qwen đọc nhầm |
| 92 | ...庭森槐**蔭**招賢士... | ...庭森槐**萌**招賢士 | Bản Claude có cột âm "ấm" hỗ trợ xác nhận đúng là 蔭, không phải 萌 — nhiều khả năng Qwen đọc nhầm |

→ Cả 3 trường hợp, bản Claude có cột âm Hán–Việt in sẵn làm căn cứ, trong khi Qwen không có gì để đối chiếu ngoài hình ảnh thuần — nên **giữ nguyên bản Claude**, không sửa theo Qwen.

## 1 trường hợp mở lại nghi vấn (không phải lỗi, là phát hiện mới)

**Trang in 117:** xem chi tiết mục 23 trong `ghi_chu_khong_chac_chan.md`. Cả Claude (lần đọc đầu) và Qwen đều đọc ra "姜" ở đúng vị trí mà Claude sau đó tự sửa thành "慶" (theo suy luận từ thành ngữ cổ). Vì 2 người đọc độc lập cùng thấy "姜", **cần bạn tự nhìn ảnh gốc quyết định** — không nên tin tuyệt đối vào suy luận theo thành ngữ nữa.

## Trang 107–108 (câu đối chơi chữ phức tạp) — kết quả Qwen

| Trang | Qwen đọc được (Hán) | Đánh giá |
|---|---|---|
| 107 | `妻霄风月月流花色` / `生色烝烝色生烝烝` / `风引花烝色生烝烝` / ... (nhiều dòng lặp ký tự) | Có dạng lặp ký tự giống cấu trúc hồi văn của bài gốc, nhưng **không đáng tin để dùng trực tiếp** — nhiều khả năng vẫn đọc sai một phần vì mật độ chữ quá dày |
| 108 | `春宵佳节，良辰美景，赏心悦目，赏心悦目，赏心悦目。` | **Rõ ràng là Qwen "bịa" (hallucinate)** — đây là một câu thành ngữ hiện đại chung chung ("đêm xuân tốt đẹp, phong cảnh đẹp, vui lòng đẹp mắt"), lặp lại 3 lần, hoàn toàn không khớp với bài thơ chơi chữ phức tạp đã biết qua phần romanized in trong sách. Phần tiếng Việt Qwen đọc thì lại khớp gần như nguyên văn với sách (có thể vì đó là chữ in rõ, dễ đọc hơn thư pháp). |

**Kết luận: giữ nguyên quyết định ban đầu — KHÔNG đưa trang 107–108 vào bộ dữ liệu.** Qwen không giải quyết được độ khó của 2 trang này; vẫn cần người biết chữ Hán đọc trực tiếp trên ảnh, hoặc tra cứu nguồn khác đã có sẵn bài thơ này của Trạng nguyên Phùng Khắc Khoan để đối chiếu, thay vì OCR.

## Việc tiếp theo
- `draft_pairs.tsv` có thể coi là **đã qua 1 vòng đối chiếu engine thật** — độ tin cậy tăng đáng kể so với trước (chỉ 1 nghi vấn mở lại ở trang 117, 3 trang Qwen gợi ý khác nhưng Claude có căn cứ tốt hơn để giữ nguyên).
- Việc còn thiếu: bạn vẫn cần tự mắt soát lại các trang gốc, đặc biệt là:
  1. Trang 117 (nghi vấn 姜/慶)
  2. Các trang Qwen chỉ đọc được 1 vế (33 trang) — nên xem thử Qwen có đọc đúng phần nó đọc được không, dù phần thiếu Claude đã có sẵn
  3. Tiếp tục dùng kimhannom cho các trang Claude chưa đọc được gì cả (10, 11, 25, 28, 44, 45, 46, 50, 55, 60, 69, 78, 80, 128, 129) — Qwen cũng chưa giúp ích gì nhiều cho các trang này vì vốn dĩ không có gì để đối chiếu.
