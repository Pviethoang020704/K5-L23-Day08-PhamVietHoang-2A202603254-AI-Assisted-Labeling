# Label contract — Phố châu Á — BỔ SUNG v1.1 — 2A202603254 - Pham Viet Hoang

## 1. Câu hỏi mở đã chốt hoặc còn để ngỏ

| # | Câu hỏi | Quyết định | Ảnh ví dụ | Case |
|---|---|---|---|---|
| 1 | Xe đạp điện / xe máy điện | Chưa chốt: artifact được kiểm tra không có ca xe điện đủ rõ để xác định quy tắc bằng chứng. | Chưa có ví dụ xác minh | — |
| 2 | Cụm xe máy đỗ dày đặc gần camera | Gán riêng khi silhouette từng xe phân biệt được và box cao ít nhất 20 px. Chỉ gộp thành một `ignore_region` khi có ít nhất 5 vật và không thể tách từng cá thể rõ. | `img_014` | CAL-03 |
| 3 | Hàng hóa nhô ra trên xe đạp / ba gác | Box gồm khung xe và giá, khay hoặc thùng chở hàng gắn cố định. Hàng rời không làm thay đổi box xe nếu nhô quá khoảng 20% chiều dài xe; không gán hàng rời thành object riêng. | `img_031` | CAL-02 |
| 4 | Người khuất trong xe ba bánh có mui | Chưa chốt thêm ngoài quy tắc hiện hành: chỉ gán người khi phần nhìn thấy đạt ngưỡng guideline v1.0; artifact chưa có ca mui kín đủ rõ để hiệu chỉnh ngưỡng. | Chưa có ví dụ xác minh | — |
| 5 | Kính chắn gió / cửa sổ trong mask | Chưa chốt: chưa có ca mask kính phù hợp để phân biệt kính và phần rỗng xuyên nhìn. | Chưa có ví dụ xác minh | — |
| 6 | Box chồng nhau trong ảnh đêm | Chưa chốt: tập QC và log hiện có không chỉ ra một ca ảnh đêm cụ thể để làm ví dụ calibration. | Chưa có ví dụ xác minh | — |
| 7 | Ngưỡng confidence tối thiểu chung | Không đặt một ngưỡng accept chung chỉ dựa vào confidence. Dùng ngưỡng theo lớp từ sweep khi tạo pre-label; người vẫn phải kiểm tra trực quan box. Kết quả sweep chỉ là ước lượng in-sample từ 4 ảnh gold. | `img_001`, `img_012`, `img_020`, `img_036` (gold dùng sweep) | — |

## 2. Ca khó và quy tắc bổ sung

| Ảnh | Tình huống / bằng chứng | Quyết định và quy tắc bổ sung | Case |
|---|---|---|---|
| `img_035` | Pre-label có hai box `three_wheeler` (IDs 456, 457) tách mái và thân của cùng xe; phiếu QC final ghi lỗi kích thước ở IDs 401, 428. | Một xe ba bánh là một object. Box phải bao toàn bộ mái, khung và thân gắn liền nhìn thấy; không tách mái và thân thành hai box. | CAL-01 |
| `img_031` | QC ID 358: box `bicycle` chưa bao hết phần giá/khay chở hàng gắn liền nhìn thấy. | Bao cấu trúc chở hàng lắp cố định vào xe. Không nới box theo hàng rời nhô quá khoảng 20% chiều dài xe. | CAL-02 |
| `img_014` | Cụm xe máy gần camera; các xe phân biệt được dù đứng sát nhau. | Ưu tiên box riêng nếu silhouette rõ và cao ít nhất 20 px. Dùng một `ignore_region` cho cụm từ 5 vật trở lên chỉ khi không thể tách từng cá thể. | CAL-03 |
| `img_005` | `review_stats.md` ghi lọt bẫy `loose` tại ảnh này (1/3 bẫy loose). | Sau khi rà class và vật bị bỏ sót, kiểm độc lập bốn cạnh box với ảnh gốc; ghi lại box còn lỏng. Không kết luận checklist trước đó đã bỏ qua bước nào vì log không ghi nguyên nhân. | CAL-04 |

## 3. Lịch sử thay đổi

| Phiên bản | Thay đổi | Lý do / case |
|---|---|---|
| v1.0 | Bản guideline gốc | — |
| v1.1 | Chốt tiêu chí cụm xe gần camera, phần hàng gắn cố định, ngưỡng confidence không thay thế kiểm tra ảnh; thêm rà biên và quy tắc box duy nhất cho một xe ba bánh. Các câu 1, 4, 5, 6 còn mở. | CAL-01–CAL-04; `img_014`, `img_031`, `img_035`, `img_005` |
