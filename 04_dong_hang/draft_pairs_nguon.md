# Nguồn và độ tin cậy của `draft_pairs.tsv`

File này KHÔNG phải sản phẩm nộp — chỉ là tài liệu làm việc nội bộ để biết mỗi dòng trong `draft_pairs.tsv` lấy từ trang nào và độ tin cậy ra sao. `draft_pairs.tsv` hiện là bản đọc thủ công của Claude (đóng vai một nguồn OCR bổ sung), **chưa chạy qua PaddleOCRV5 / HunyuanOCR / Qwen theo đúng quy trình 02_ocr_tho của Guideline**, và **chưa được bạn đối chiếu bằng mắt với ảnh gốc**. Coi đây là bản nháp để bạn kiểm lại, không phải kết quả cuối.

Nguồn: `01_nguon/MauCauDoiHoanhPhi.pdf`, trang in (số chân trang) 12–35, mục "I. Câu đối ở đền miếu".

| Dòng TSV | Trang in | Độ tin cậy | Ghi chú |
|---|---|---|---|
| 1–2 | 12 | Trung bình | Có cột âm Hán–Việt bên cạnh hỗ trợ, không có dòng "Hán:" in sẵn |
| 3–4 | 13 | Trung bình | Như trên |
| 5–8 | 14 | **Thấp — xem mục 1 trong `ghi_chu_khong_chac_chan.md`** | Nhãn A)/B) in sẵn không khớp với khối thư pháp ngay bên dưới; đã ghép lại theo nội dung chữ, không theo nhãn A)/B) gốc |
| 9–10 | 15 | Trung bình | Cột âm hỗ trợ |
| 11–12 | 16 | Trung bình | Cột âm hỗ trợ |
| 13–14 | 17 | Trung bình | Cột âm hỗ trợ |
| 15–16 | 18 | Trung bình | Cột âm hỗ trợ |
| 17–18 | 20 | Trung bình–Khá | Có dòng "Hán:" in sẵn (phiên âm, không phải chữ Hán) + "Dịch:" giúp xác định đúng câu cổ văn quen thuộc |
| 19–20 | 21 | Trung bình–Khá | Như trên |
| 21–22 | 23 | Trung bình | Cột âm hỗ trợ |
| 23–24 | 24 | Trung bình | Cột âm hỗ trợ |
| 25–26 (mới chèn, 2026-11-01) | 25 | **Rất cao** | Lấy từ kimhannom, khớp tuyệt đối với dòng "Hán:" in sẵn trong sách (xem `02_ocr_tho/kimhannom/results.json`, `ghi_chu_khong_chac_chan.md` mục 10b). Trang 25 trước đó bị loại, nay bổ sung lại. |
| ⚠️ Từ đây trở đi, số dòng trong cột "Dòng TSV" bị lệch +2 so với các mục ghi bên dưới (do 2 dòng trang 25 mới chèn vào giữa) — **dùng nội dung câu để đối chiếu, không dùng số dòng tuyệt đối** cho các mục phía sau, trừ khi đã cập nhật lại. | | | |
| 27–28 (số cũ) | 26 | Trung bình–Khá | Có dòng "Hán:" in sẵn, câu thuộc cổ văn quen thuộc ("hữu khai tất tiên...") |
| 27–28 | 27 | Trung bình | Cột âm hỗ trợ |
| 29–30 | 29 | Khá | Có cả "Hán:" và "Dịch:" in sẵn, có chú thích phụ "(Con cháu ở xa về cúng tiến)" đã loại bỏ khỏi vế Việt theo đúng quy tắc làm sạch |
| 31–32 | 32 | Trung bình | Cột âm hỗ trợ |
| 33–36 | 33 | Khá | Có "Hán:" và "Dịch:" in sẵn cho cả 2 câu A) và B); không phát hiện lệch nhãn như trang 14 |
| 37–38 | 34 | Trung bình | Cột âm hỗ trợ |
| 39–40 | 35 | Trung bình | Cột âm hỗ trợ |

| 41–42 | 36 | Khá | Có "Hán:" in sẵn |
| 43–44 | 38 | Trung bình | Cột âm hỗ trợ |
| 45–46 | 39 | Trung bình | Cột âm hỗ trợ |
| 47–48 | 40 | Trung bình | Cột âm hỗ trợ |
| 49–50 | 41 | Trung bình | Cột âm hỗ trợ |
| 51–52 | 42 | Khá | Có "Hán:" in sẵn |
| 53–54 | 43 | Trung bình | Cột âm hỗ trợ |
| 55–56 | 47 | Khá | Có "Hán:" in sẵn, đầu mục "III. Câu đối ở bàn thờ gia tiên hoặc tiểu chi" |
| 57–58 | 48 | Trung bình | Cột âm hỗ trợ |
| 59–60 | 49 | Trung bình | Cột âm hỗ trợ |
| 61–62 | 51 | Trung bình | Cột âm hỗ trợ; trang có **2 bản dịch** (chính + "Hoặc:..." thay thế) — chỉ lấy bản chính, xem `ghi_chu_khong_chac_chan.md` mục 11 |
| 63–64 | 52 | Trung bình | Cột âm hỗ trợ |
| 65–66 | 53 | Trung bình | Cột âm hỗ trợ |
| 67–68 | 54 | Trung bình | Cột âm hỗ trợ |

| 69–70 | 57 | Trung bình | Cột âm hỗ trợ; có chú thích phụ đã loại khỏi vế Việt |
| 71–74 | 58 | Khá | Có "Hán:" in sẵn cho cả A) và B), đầu mục "IV- CÂU ĐỐI ĐỀ LĂNG MỘ" |
| 75–78 | 62 | Khá | A) là thành ngữ cổ quen thuộc (Phúc như Đông Hải, Thọ tỉ Nam Sơn), B) có "Hán:" in sẵn; đầu mục "CÂU ĐỐI CHÚC TỤNG / I. Câu đối mừng thọ" |
| 79–80 | 63 | Trung bình | Cột âm hỗ trợ |
| 81–82 | 64 | Trung bình | Cột âm hỗ trợ |
| 83–84 | 65 | Trung bình | Cột âm hỗ trợ |
| 85–86 | 67 | Khá | Có "Hán:" in sẵn, có chú thích phụ đã loại khỏi vế Việt |
| 87–88 | 68 | Trung bình | Cột âm hỗ trợ |
| 89–90 | 70 | Trung bình | Cột âm hỗ trợ |
| 91–94 | 71 | Khá | Có "Hán:" in sẵn cho cả A) và B) |
| 95–96 | 72 | Trung bình | Cột âm hỗ trợ |
| 97–98 | 73 | Trung bình | Cột âm hỗ trợ |
| 99–100 | 74 | Trung bình | Cột âm hỗ trợ |
| 101–102 | 75 | Trung bình | Cột âm hỗ trợ |

| 103–138 | 76–95 | Trung bình (trừ 76 ghi "Khá" do có 2 bản dịch, lấy bản chính) | Tất cả đều có cột âm Hán–Việt hỗ trợ. Loại trang 78, 80 (không đủ căn cứ đối chiếu, xem `ghi_chu_khong_chac_chan.md`). Nhiều trang có chú thích phụ trong ngoặc đã loại khỏi vế Việt (trang 77, 85, 86, 87, 95). |

| 139–162 | 96–106 | Khá (96,97,106 có "Hán:" in sẵn) / Trung bình (98–105 cột âm) | Trang 106 là 4 vế (bộ 4 mùa), không phải cặp 2 vế thường |
| 163–180 | 109–115 | Khá (109, 114 có "Hán:" in sẵn) / Trung bình (110–113, 115 cột âm) | Loại trang 107–108 (câu đối chơi chữ phức tạp, xem `ghi_chu_khong_chac_chan.md` mục 22) |

| 181–202 | 116–126 | Trung bình/Khá | Trang 117 (dòng 183): đã xác nhận là 姜 (Khương), xem `ghi_chu_khong_chac_chan.md` mục 23 |
| 203–206 | 131–132 | Trung bình | Cột âm hỗ trợ; nội dung dùng chung cho cả cha/mẹ (có ghi chú "(hoặc...)" trong ngoặc) |
| 207–211 | 135 | Cao | Hoành phi — có đủ chữ Hán + phiên âm + nghĩa Việt in sẵn, không cần suy luận |

| 212–261 | 136–145 | Cao | Hoành phi — có đủ chữ Hán + phiên âm + nghĩa Việt in sẵn. Trang 142, 145 có chú thích phụ trong ngoặc ("chỉ dùng trường hợp mừng song thọ", "dành cho liệt sĩ") không đưa vào vế Việt. |

**Tổng cộng:** 261 cặp (xác nhận bằng `wc -l`, định dạng đã kiểm tra sạch: không dòng trống, không tab thừa).

**Quan trọng:** "Độ tin cậy Trung bình" nghĩa là Claude đọc trực tiếp nét chữ thư pháp (chữ Hán chạy/thảo), có đối chiếu với cột âm Hán–Việt in sẵn để chọn đúng âm, nhưng **không chắc chắn tuyệt đối về mặt chữ** — đặc biệt các chữ có thể viết giản thể/phồn thể/dị thể khác với bản khắc gỗ gốc. Trước khi dùng dòng nào trong file này, mở trang tương ứng trong `_pages_raw/` để so lại từng chữ.
