# Báo cáo lab: chọn tracker cho 5 video

**Nhóm:** 2 AE siu nhân **Thành viên:** Thái Phúc Tiến (2A202602873), Trần Đình Duy (2A202602631)

Detector cố định: `yolo26n.pt`, ảnh 640 px, Re-ID `osnet_x0_25_msmt17`. Không đổi các mục này trong bài nộp chính.

## 1. Cấu hình đã chọn

| Video | Tracker | conf | iou | Quan sát / lý do chọn | Đã thử nhưng loại |
|---|---|---:|---:|---|---|
| video_1 (quảng trường, tĩnh, ban ngày) | botsort | 0.3 | 0.5 | HOTA, MOTA và IDF1 cao hơn ByteTrack trên video có nhãn. | bytetrack (0.3, 0.5): metric tổng hợp thấp hơn. |
| video_2 (phố đêm, tĩnh, rất đông) | strongsort | 0.2 | 0.5 | Thử Re-ID cho cảnh đông; chất lượng cần đánh giá bằng mắt vì video không có nhãn. | bytetrack (0.3, 0.5). |
| video_3 (camera di động, ảnh nhỏ) | botsort | 0.2 | 0.5 | Thử BoT-SORT vì có bù chuyển động camera; cần xem video để xác nhận ID. | bytetrack (0.3, 0.5); ngưỡng khác nên chưa so sánh riêng ảnh hưởng tracker. |
| video_4 (trong nhà, camera di chuyển) | strongsort | 0.15 | 0.5 | Thử Re-ID trong cảnh camera di chuyển; chưa có nhãn để đo lỗi ID. | bytetrack (0.15, 0.5). |
| video_5 (trên xe bus, giao lộ đông) | botsort | 0.25 | 0.5 | Thử BoT-SORT cho cảnh chuyển động và che khuất; cần xem video để xác nhận. | bytetrack (0.25, 0.5). |

## 2. Số liệu video_1

| Tracker | HOTA | MOTA | IDF1 |
|---|---:|---:|---:|
| BoT-SORT | 29.626 | 19.633 | 29.379 |
| ByteTrack | 26.916 | 17.303 | 25.702 |

BoT-SORT nhỉnh hơn ở ba chỉ số này, nhưng có nhiều lần đổi ID hơn (27 so với 12) và đứt track nhiều hơn (107 so với 44). `IDs` là số ID tracker tạo ra, không phải số người nhận diện đúng.

`video_2` đến `video_5` không có nhãn, nên không có số HOTA / MOTA / IDF1 để báo cáo.

## 3. Phân tích

- **Video_1:** BoT-SORT đạt HOTA 29.626, so với 26.916 của ByteTrack. IDF1 cũng cao hơn, nhưng số lần đổi ID và đứt track tăng. Vì vậy, mình chọn BoT-SORT theo metric tổng hợp, dù kết quả chưa tốt hơn ở mọi mặt.
- **Video_3:** Mình thử BoT-SORT vì cảnh có camera di động; tracker có cơ chế bù chuyển động camera. File thử 150 frame có 23 ID với BoT-SORT và 18 với ByteTrack, nhưng số ID này không cho biết có bao nhiêu người được theo dõi đúng. Video không có nhãn nên cần đối chiếu preview trước khi kết luận tracker nào giữ ID tốt hơn.

## 4. Nếu có thêm thời gian

Xem lại preview của video_2–5, ghi các đoạn ID bị đổi hoặc người bị mất dấu. Với video_1, thử các ngưỡng `conf` trên cùng tracker để xem có giảm bỏ sót mà không tăng quá nhiều hộp giả không.
