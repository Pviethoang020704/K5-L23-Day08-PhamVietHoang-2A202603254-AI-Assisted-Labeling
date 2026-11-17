# Báo cáo QC tự động (nháp)
## Mẫu
- 5 ảnh (0 ca khó + 5 ngẫu nhiên), 89 đối tượng.
- taxonomy v1.0

## Số đo
| Chỉ số | Giá trị | Mẫu số |
|---|---|---|
| Lỗi **theo đối tượng** | 3/89 = 3.4% | 89 đối tượng trong mẫu |
| **Ảnh có lỗi** (dùng cho ngưỡng) | 2/5 = 40.0% | 5 ảnh trong mẫu |
| Máy: accepted / edited / deleted | 380/426 = 89.2% / 16/426 = 3.8% / 25/426 = 5.9% | 426 box máy |
| Máy: added | 14/410 = 3.4% | 410 đối tượng sau duyệt |
| Bắt lỗi bẫy | 11/12 = 91.7% | 12 bẫy |

## Top 3 loại lỗi
| Loại lỗi | Số lỗi | Ảnh ví dụ |
|---|---|---|
| sai_vi_tri_kich_thuoc | 3 | img_031.jpg, img_035.jpg |

**Lỗi hệ thống**: 'sai_vi_tri_kich_thuoc' lặp lại -> sửa quy tắc / recipe, không chỉ sửa từng ảnh.

## Giới hạn thống kê
- 2 ảnh lỗi trong 5 ảnh mẫu: cận trên 95% (Wilson) của tỉ lệ ảnh lỗi thật là **77%**. Ngưỡng 2% không thể được **chứng minh** bằng mẫu nhỏ như vậy: hãy nói rõ giới hạn này trong báo cáo.

## Gợi ý quyết định
**REWORK**

- ảnh có lỗi 40.0% > 2%
- lỗi hệ thống: 'sai_vi_tri_kich_thuoc' chiếm 3/3 lỗi

Cây quyết định: Hold (guideline chưa rõ / dữ liệu hỏng) -> Rework (> ngưỡng, lỗi lặp, bắt bẫy / nhất quán thấp) -> Release.
