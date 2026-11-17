# Thông tin học viên

- Họ và tên: Pham Viet Hoang 
- MSHV: 2A202603254
- Lớp (L2 / L3 / khác): K5-L23 
- Tài khoản GitHub: Pviethoang020704
- Repo bài nộp (URL): https://github.com/Pviethoang020704/K5-L23-Day08-PhamVietHoang-2A202603254-AI-Assisted-Labeling
- Commit chốt bài (hash): 2892f0f208f71625caadebd6f5c369794cd08147
- Máy: Windows
- CVAT (Docker): 2.81.0

## Kết quả tóm tắt (nguồn: `submission/qc/review_stats.json`, `qc_report_auto.md`, `time_log.csv`)

- Máy: accepted 380/426 = 89.2%; edited 16/426 = 3.8%; deleted 25/426 = 5.9% (mẫu số: 426 box máy); added 14/410 = 3.4% (mẫu số: 410 đối tượng sau duyệt).
- So với gold: precision máy 40/(40+20) = 66.7%; recall máy 40/42 = 95.2% (42 đối tượng gold, 4 ảnh).
- Bắt bẫy: 11/12 = 91.7%.
- Thời gian: gán tay và duyệt có pre-label chưa ghi trong `time_log.csv`; không tính phút/ảnh.
- Lỗi QC: 40.0% theo ảnh (2/5), 3.4% theo đối tượng (3/89).
- **Quyết định:** Rework — tỷ lệ ảnh có lỗi 2/5 = 40.0% vượt ngưỡng 2%; ba lỗi đều là `sai_vi_tri_kich_thuoc`. Chưa sửa box/nhãn và chưa review lại.
