# Báo cáo Day 08 — 2A202603254 - Pham Viet Hoang

> Tổng hợp từ `submission/qc/threshold_sweep.md`, `review_stats.md`, `qc_report_auto.md`, `work/runs.csv`, `qc_sample.csv` và kiểm tra trực quan các mask. Những thông tin không có trong artifact được ghi rõ là chưa được ghi nhận; không suy ra thời gian hoặc thao tác chưa lưu.

## 1. Model và recipe (Part 2–3)

| Lớp | Model + cỡ | Ngưỡng | Căn cứ |
|---|---|---:|---|
| person | YOLO26-M | 0.50 | Sweep đạt recall 15/15 = 100%, precision 15/22 = 68.2%; run pilot r01–r02 |
| bicycle | YOLO26-M | 0.30 mặc định | Chỉ có 1 vật gold; không đủ mẫu để chọn riêng |
| motorbike | YOLO26-M | 0.35 | Sweep đạt recall 10/11 = 90.9%, precision 10/12 = 83.3%; r01–r02 |
| car | YOLO26-M | 0.30 mặc định | Không ngưỡng ≤0.50 đạt recall 90% (tối đa 7/8 = 87.5%) |
| bus | YOLO26-M | 0.30 mặc định | Không có vật bus trong gold |
| truck | YOLO26-M | 0.30 mặc định | Chỉ có 1 vật truck trong gold |
| three_wheeler | YOLOE-S | 0.15 | Sweep đạt recall 6/6 = 100%, precision 6/9 = 66.7%; run r03 |

- Chuỗi ngưỡng được sweep khuyến nghị: `person=0.5,motorbike=0.35,three_wheeler=0.15`; các lớp khác dùng `--conf 0.3`.
- Lệnh merge chính xác đã chạy không được lưu trong `work/runs.csv`; không thể xác nhận toàn bộ đối số thực tế. Chuỗi trên là cấu hình khuyến nghị theo sweep, không khẳng định là command history.
- Lỗi không sửa được chỉ bằng ngưỡng: một vật ba bánh bị tách thành hai box ở `img_035.jpg` (QC IDs 401, 428). Cần kiểm tra box theo toàn bộ vật và loại trùng/chia box; đổi confidence không hợp nhất đúng hai phần này.
- Sweep không ghi ID box điểm cao sai hay điểm thấp đúng; không đủ bằng chứng để nêu một ví dụ confidence cụ thể.
- Sweep dùng bốn ảnh gold (`img_001`, `img_012`, `img_020`, `img_036`), đồng thời chọn và đo ngưỡng trên cùng gold; precision/recall là ước lượng in-sample lạc quan.
- Các lượt có trong `work/runs.csv`: r01–r02 YOLO26-M pilot (215/139 box; 1.10/0.34 giây/ảnh), r03 YOLOE-S pilot (8 box; 0.17 giây/ảnh), r04 YOLO26-M + SAM 2.1 b (16 mask trên 2 ảnh; 1.93 giây/ảnh), r05 YOLO26-M all (1,669 box; 0.11 giây/ảnh), r06 YOLOE-S all (125 box; 0.06 giây/ảnh). Các thời gian này là thời gian chạy model ghi trong run log, không phải thời gian làm bài.

## 2. AI error profile (từ `submission/qc/review_stats.md`)

| Trạng thái | Số | Tỉ lệ | Mẫu số |
|---|---:|---:|---|
| accepted | 380 | 380/426 = 89.2% | 426 box máy |
| edited | 16 | 16/426 = 3.8% | 426 box máy |
| deleted | 25 | 25/426 = 5.9% | 426 box máy |
| added | 14 | 14/410 = 3.4% | 410 đối tượng sau duyệt |

- Theo lớp, `three_wheeler` có 17 deleted, nhiều nhất trong các lớp (tổng deleted 25); tiếp theo là `motorbike` 4, `truck` 2, `bicycle` 1 và `car` 1. Đây là số liệu theo lớp, không xác định riêng ảnh cho từng deletion. Tổng cộng có 8 trường hợp đổi lớp.
- Riêng QC `img_035.jpg` cho thấy hai box `three_wheeler` 401/428 bao phần thân và mui của cùng xe thay vì một box trọn xe. QC `img_031.jpg` ghi nhận box bicycle 358 chưa ôm hết phần xe/giá hàng gắn liền.
- So với gold: precision máy 40/(40+20) = 66.7%; recall 40/42 = 95.2% (42 đối tượng gold). Khoảng tin cậy 95% được ghi trong `review_stats.md`; kết quả chỉ dựa trên 4 ảnh.

## 3. Thiên kiến tự động hóa và ảnh bẫy

- Bắt được 11/12 = 91.7%; bẫy lọt là `loose` ở `img_005.jpg` (1/3 bẫy loose).
- Artifact không ghi bước checklist cụ thể nào đã bị bỏ qua, vì vậy không quy kết nguyên nhân thao tác. Một lỗi loose bị lọt cho thấy bước rà cạnh box vẫn cần được kiểm tra độc lập.
- Accepted 380/426 = 89.2% cao nhưng không chứng minh box đúng: vẫn có 16 edited, 25 deleted và một bẫy loose lọt. Bắt bẫy 11/12 = 91.7% đạt ngưỡng 80%, nhưng mẫu bẫy nhỏ và không thay thế QC ảnh độc lập.

## 4. Máy có đáng dùng không?

- `submission/time_log.csv` hiện chỉ có header, không có thời gian `gold_manual` hoặc `batch_review`; không thể tính phút/ảnh hay kết luận máy tiết kiệm thời gian. Thời gian inference trong `work/runs.csv` không thay thế thời gian thao tác người.
- Kết luận tạm thời: chỉ dùng làm pre-label có người duyệt. Căn cứ là 380/426 box được chấp nhận nguyên trạng (89.2%), nhưng có 25/426 deleted, 16/426 edited và QC thấy lỗi ở 2/5 ảnh. Chưa có dữ liệu thời gian để khẳng định hiệu quả năng suất.
- Ba quy tắc giảm thiên kiến, có thể kiểm tra:
  1. Trước khi hiện overlay, quét toàn ảnh một lần và ghi vật có thể bị bỏ sót; checklist phải được đánh dấu cho từng ảnh.
  2. Với từng box giữ nguyên, rà bốn cạnh với ảnh gốc; ghi riêng số box đã edge-check và không chấp nhận box chỉ vì confidence cao.
  3. Sau review, đối chiếu toàn bộ bẫy theo loại; mục tiêu ít nhất 10/12 và rà lại mọi bẫy loose chưa bắt được.

## 5. Mask SAM cho 3 vật (Part 5)

Run r04 trong `work/runs.csv` ghi `sam2.1_b.pt`; có ba ảnh mask trong `submission/masks/`. File không ghi số click theo từng ảnh hoặc ánh xạ chắc chắn từng ảnh mask với run/phiên bản, nên không suy ra số click.

| Ảnh | SAM (bằng chứng hiện có) | Số click | Quan sát mask | File |
|---|---|---|---|---|
| img_013 | Run log r04 dùng SAM 2.1 b; liên kết per-file chưa lưu | Không được ghi | Mask tập trung vào xe ba bánh; người lái phần lớn tách khỏi mask xe, còn thấy rỗ/viền nhiễu quanh mái và thân | `tuktuk_img013.jpg` |
| img_015 | Phiên bản/click per-file chưa được lưu | Không được ghi | Mask tập trung vào xe đạp/giỏ; người đi xe không được gộp thành một vật xe | `mask_img015.jpg` |
| img_031 | Phiên bản/click per-file chưa được lưu | Không được ghi | Mask người bao người bán hàng; hàng hóa/giỏ không thuộc mask người | `person_img031.jpg` |

- Quy tắc hữu ích ở mục 4.2 là mask theo phần nhìn thấy, không gộp người với xe và không gộp bóng đổ. Các ảnh kết quả còn viền/rỗ hoặc chỗ biên chưa sạch, nên cần người kiểm tra thay vì coi mask là nhãn hoàn tất.
- Không có bằng chứng về lần thử Interactor SAM trong CVAT; không kết luận đã thử hoặc so sánh.

## 6. Calibration (Part 6)

- Có 4 quyết định trong `submission/qc/calibration_decisions.csv`. CAL-01 củng cố cách xử lý lỗi box/model; CAL-02 và CAL-03 chốt câu hỏi mở về hàng gắn xe và cụm xe đông; CAL-04 lấy từ bẫy loose lọt.
- Quy tắc bổ sung được ghi trong `submission/guidelines/guideline_v1.1_calibrated.md`: một box cho một xe ba bánh đầy đủ; tính giá/khung gắn cố định vào xe; chỉ dùng ignore-region khi cá thể không thể tách rõ theo điều kiện nêu trong guideline; thêm bước rà cạnh box.
- Nguyên nhân gồm lỗi model/box và hướng dẫn v1.0 còn mở. Những thay đổi này là quy tắc và tài liệu; chưa sửa nhãn COCO hay chạy lại review.
- Các câu hỏi về xe điện, người khuất trong xe có mui, kính trong mask và box chồng lấn ban đêm vẫn chưa chốt vì artifact hiện có không cung cấp ca đủ rõ.

## 7. QC và quyết định (Part 6)

- Mẫu gồm 5 ảnh: 0 ảnh `can_review` + 5 ảnh ngẫu nhiên theo `qc_sample.csv` (seed 42 theo thiết lập bài); người kiểm: PhamVietHoang. Giới hạn: người kiểm cũng là người duyệt lô.
- Lỗi theo đối tượng: 3/89 = 3.4%; theo ảnh: 2/5 = 40.0% (ngưỡng Release ≤ 2%).
- Loại lỗi duy nhất ghi nhận là `sai_vi_tri_kich_thuoc`: 3 lỗi ở 2 ảnh — `img_031.jpg` ID 358; `img_035.jpg` IDs 401, 428. Không ghi nhận các loại lỗi khác trong phiếu.
- Cận trên 95% Wilson của tỷ lệ ảnh lỗi là 77%; mẫu 5 ảnh quá nhỏ để chứng minh ngưỡng 2%.
- `qc_report.py` đếm lỗi trong phiếu và cảnh báo ảnh chưa kiểm/loại lỗi sai. Script không xác minh box có ôm sát vật thật, lớp đúng thực tế, vật bị cả người và máy cùng bỏ sót, hay việc checklist đã được làm thật.

**Quyết định: Rework**, ngày 2026-11-17. Lý do: 2/5 ảnh lỗi = 40.0%, lớn hơn 2%; `sai_vi_tri_kich_thuoc` lặp 3/3 lỗi. Bắt bẫy 11/12 = 91.7% đạt ngưỡng riêng nhưng không làm mất điều kiện Rework. Chưa sửa box/nhãn và chưa review lại; việc tiếp theo là sửa trong công cụ gán nhãn, xuất nhãn cập nhật, sau đó chạy lại build release, lấy mẫu QC mới và báo cáo vòng sau.

## 8. Ghi chú cá nhân

- Dùng Copilot Agent trong VS Code để đọc tài liệu, xem trực quan ảnh gốc/overlay/mask, đối chiếu object ID với COCO, điền báo cáo/calibration và kiểm CSV; kết quả QC được đối chiếu trực tiếp bằng ảnh.
- Tình trạng gold có xem pre-label trước khi gán xong không được ghi trong artifact; không thể xác nhận gold không bị nhiễm.
- Máy: Windows theo môi trường chạy; `work/runs.csv` ghi device CUDA. Phiên bản CVAT không có trong repo.
- Không thấy bằng chứng dùng `data/cache/`; `work/runs.csv` có các lượt r05/r06 tự chạy trên all.
- Không ghi nhận lỗi chạy script trong artifact hiện có. `check_submission.py` vẫn là kiểm tra cần chạy ở bước xác nhận cuối.
- Lần sau cần ghi ngay thời gian `gold_manual`/`batch_review`, nhận xét quan sát cho từng model run, click/phiên bản SAM theo từng ảnh và lệnh merge thực tế; hoàn tất sửa lỗi QC rồi mới đánh giá release.
