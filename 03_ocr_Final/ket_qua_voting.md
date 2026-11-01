# Kết quả OCR cuối cùng (sau voting) — CV13_Couplets

## Nguồn dữ liệu đưa vào voting

| Nguồn | Phạm vi | Vai trò |
|---|---|---|
| Claude (đọc tay, vision) | 148/148 trang | **Nguồn chính** — đọc trực tiếp ảnh, ưu tiên dùng cột âm Hán-Việt in sẵn trong sách làm căn cứ khi có |
| Qwen2.5-VL-7B-Instruct | 148/148 ảnh | Đối chiếu chéo toàn bộ, phát hiện sai sót của nguồn chính |
| kimhannom.fit.hcmus.edu.vn | 15 trang khó nhất | Đối chiếu bổ sung cho các trang nguồn chính không tự tin đọc được |

## Quy tắc voting đã áp dụng

1. **Mặc định giữ bản đọc tay của Claude** làm gốc, vì phần lớn các trang có cột âm Hán-Việt in sẵn trong sách — đây là căn cứ khách quan mà cả Qwen và kimhannom đều không có quyền truy cập (chúng chỉ nhìn ảnh thuần).
2. **Chỉ sửa theo engine OCR khi**:
   - Engine đọc ra kết quả **khớp với dòng "Hán:"/cột âm in sẵn trong sách** (bằng chứng khách quan, không phải suy luận) — áp dụng cho **trang 25** (kimhannom khớp tuyệt đối).
   - Hoặc **người thực hiện tự xem ảnh gốc và xác nhận trực tiếp** — áp dụng cho **trang 117** (xác nhận 姜, không phải 慶).
3. **Giữ nguyên bản Claude, không sửa theo engine khi** engine đọc khác nhưng bản Claude có cột âm hỗ trợ xác nhận — áp dụng cho 3 trang phát hiện qua đối chiếu Qwen (**92, 105, 124** — xem `02_ocr_tho/doi_chieu_qwen_vs_draft.md`).
4. **Loại khỏi bộ dữ liệu** khi cả 2–3 nguồn đều không đạt độ tin cậy cần thiết (không có căn cứ đối chiếu, hoặc engine lỗi/hallucinate) — áp dụng cho **16 trang**: 10, 11, 28, 44, 45, 46, 50, 55, 60, 69, 78, 80, 107–108, 128, 129 (chi tiết lý do từng trang: `ghi_chu_khong_chac_chan.md`, mục "KẾT LUẬN CUỐI" và mục 22).

## Kết quả

- **263 cặp Hán–Việt/hoành phi** đã qua voting, tách thành 2 file song song:
  - `han_final.txt` — 263 dòng, văn bản Hán đã làm sạch
  - `viet_final.txt` — 263 dòng, văn bản Việt đã làm sạch
  - Dòng thứ N ở 2 file tương ứng với nhau (giống cấu trúc `04_dong_hang/draft_pairs.tsv`, chỉ tách cột thay vì TSV).
- **16 trang bị loại** khỏi bộ dữ liệu chính (không đưa vào voting vì không có bản để so) — ghi nhận đầy đủ lý do trong `ghi_chu_khong_chac_chan.md` để trích vào báo cáo tự kiểm tra chất lượng (mục 7 Guideline).
- **14 trang chữ Nôm** tách riêng từ đầu, không tham gia voting này — xem `_nom_trung_gian/nom_raw_notes.md`.

## Nguồn gốc kỹ thuật (bằng chứng)

- Đối chiếu Qwen chi tiết từng trang: `02_ocr_tho/doi_chieu_qwen_vs_draft.md`
- Kết quả thô kimhannom (15 trang khó): `02_ocr_tho/kimhannom/results.json`
- Toàn bộ ghi chú từng trường hợp mơ hồ + quyết định cuối: `ghi_chu_khong_chac_chan.md`
- Bản dóng hàng gốc (trước khi tách 2 file): `04_dong_hang/draft_pairs.tsv`

## Việc tiếp theo

File `han_final.txt` + `viet_final.txt` ở đây là **nguồn để xuất file chính thức** `CV13_Couplets_parallel.tsv`/`.xlsx` (theo đúng yêu cầu Guideline mục 2: "văn bản Hán và Việt đã làm sạch" là sản phẩm nộp riêng, còn file dóng hàng `.tsv`/`.xlsx` là sản phẩm gộp 2 cột). Bước tiếp theo: xuất 2 file deliverable chính thức từ dữ liệu đã voting này.
