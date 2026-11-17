# Số đo review (det)

- Ảnh tính: 24 (batch,gold); box máy: 426; đối tượng sau duyệt: 410; ngưỡng accepted: IoU >= 0.9
- Đã **loại 26 đối tượng nằm quanh các bẫy** khỏi bảng này: lỗi nhân tạo không phản ánh chất lượng máy.

## 1. Bốn trạng thái (AI error profile)
| Trạng thái | Số | Tỉ lệ | Mẫu số |
|---|---|---|---|
| accepted (giữ nguyên) | 380 | 380/426 = 89.2% | 426 box máy |
| edited (sửa) | 16 | 16/426 = 3.8% | 426 box máy |
| deleted (xoá) | 25 | 25/426 = 5.9% | 426 box máy |
| added (vẽ thêm) | 14 | 14/410 = 3.4% | 410 đối tượng sau duyệt |

Trong số deleted + added, **8 trường hợp là đổi lớp** (máy gán sai lớp, box đúng chỗ): đó không phải máy bỏ sót. Số vật máy **sót thật** ≈ added − đổi lớp = 6.

### Theo lớp
| Lớp | accepted | edited | deleted | added |
|---|---|---|---|---|
| person | 182 | 6 | 0 | 2 |
| bicycle | 5 | 2 | 1 | 4 |
| motorbike | 115 | 6 | 4 | 2 |
| car | 50 | 0 | 1 | 1 |
| bus | 3 | 0 | 0 | 1 |
| truck | 4 | 0 | 2 | 2 |
| three_wheeler | 21 | 2 | 17 | 2 |

## 2. So với gold (nhãn gán tay từ đầu)
Ảnh gold: 4; đối tượng gold: 42 (mẫu số của recall)
| | TP | FP | FN | Precision | Recall |
|---|---|---|---|---|---|
| Máy (pre-label) | 40 | 20 | 2 | 66.7% (KTC 95%: 54–77%) | 95.2% (KTC 95%: 84–99%) |

*Lạc quan: ngưỡng được chọn trên chính các ảnh gold này (in-sample), và chỉ có 4 ảnh. Đọc khoảng tin cậy, đừng đọc con số đơn.*


(Ảnh gold không nằm trong nhãn sau duyệt: không tính residual / new error. Đó là chủ ý: duyệt lại ảnh bạn vừa tự gán tay sẽ đo trí nhớ, không đo thiên kiến.)

## 3. Ảnh bẫy
- Bắt được 11/12 = 91.7% (KTC 95%: 65–99%) (ngưỡng đạt: 80%). Chỉ 12 bẫy nên khoảng tin cậy rất rộng: đọc cùng danh sách bẫy lọt, đừng kết luận chỉ từ %.
  - extra: 3/3
  - wrong_class: 3/3
  - missing: 3/3
  - loose: 2/3
  - lọt: img_005.jpg (loose)

## Gợi ý đọc số
- `deleted` cao → ngưỡng thấp hoặc ánh xạ nhãn sai; `added` cao → máy bỏ sót (xem lớp nào);
- `accepted` thấp (< 50%) → sửa mỗi box lâu hơn vẽ mới: đổi model/ngưỡng hoặc vẽ tay;
- bắt bẫy < 80% → duyệt lại lô, siết checklist (quét ảnh TRƯỚC khi nhìn nhãn máy).
