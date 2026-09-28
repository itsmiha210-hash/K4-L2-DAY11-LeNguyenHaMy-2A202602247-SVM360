# Error analysis card

## Zone × block

| zone | block | what | count |
|---|---|---|---:|
| center | B2 | BOX_GEOMETRY | 1 |
| center | B2 | MISSING | 8 |
| center | B2 | SPURIOUS | 13 |
| center | B2 | WRONG_CLASS | 1 |
| center | C0 | SPURIOUS | 2 |
| center | C0 | WRONG_CLASS | 1 |
| edge | B2 | MISSING | 1 |
| edge | B2 | SPURIOUS | 2 |
| mid | B2 | ATTRIBUTE | 2 |
| mid | B2 | MISSING | 2 |
| mid | B2 | SPURIOUS | 3 |
| mid | C0 | WRONG_CLASS | 1 |
| unknown | B2 | SPURIOUS | 1 |

## Top defects
- SPURIOUS: 21 (ví dụ frame adasind_019560.jpg)
- MISSING: 11 (ví dụ frame adasind_062370.jpg)
- WRONG_CLASS: 3 (ví dụ frame adasind_019560.jpg)

## Phân tích của bạn

Hai bảng trên do `python3 lab11.py card` tính từ `findings.csv`; chạy lại lệnh sẽ cập nhật bảng và giữ nguyên mục này. Viết cho lỗi nổi bật nhất, dẫn frame/`object_ref`.

- **Nguyên nhân khả dĩ (`why`) và vì sao bạn nghĩ vậy**:
  Lỗi nổi bật nhất trong thống kê là **SPURIOUS** (21 ca) và **MISSING** (11 ca), tập trung áp đảo ở vùng **`center`** (13 spurious, 8 missing tại block B2).
  + *Đối với người gán nhãn*: Nguyên nhân chính là `E1_annotator_error` kết hợp `E2_guideline_gap`. Trong cảnh giao thông phức tạp, đông đúc (slice B2-dense), nhiều phương tiện ở xa có kích thước dao động sát ngưỡng lọc quy định H = 40 px. Annotator dễ vẽ thừa theo trực giác (ví dụ object `L6` trên frame `adasind_069450.jpg` cao ~26 px nhưng vẫn được vẽ box), hoặc tách trùng lặp các xe bị che khuất ở hậu cảnh (ví dụ `L3, L4` trên frame `adasind_117120.jpg`).
  + *Đối với mô hình (YOLO26m)*: Nguyên nhân là `E4_model_domain`. Ở vùng center hậu cảnh, độ phân giải thấp và xe bị chồng lấn khiến mô hình dự đoán box ảo (spurious) hoặc không đủ độ tự tin để phát hiện (missing).
- **Cách sửa và ai nhận việc (`owner`)**:
  + `annotator`: Cần sử dụng công cụ đo pixel trong CVAT để kiểm tra kích thước đối tượng trước khi vẽ; dứt khoát không vẽ box cho các vật thể có chiều cao H < 40 px theo đúng rule R01; bám sát phần nhìn thấy và đánh dấu thuộc tính `occluded` theo R05.
  + `guideline`: Cần bổ sung hướng dẫn chi tiết về ranh giới lọc các phương tiện ở xa và cụm xe che khuất (>50% che khuất hoặc <40 px thì đưa vào polygon `ignore_region` với lý do `unreadable` hoặc `crowd_or_group` theo R06).
  + `ai_team`: Cần bổ sung dữ liệu huấn luyện đặc thù cho camera fisheye góc rộng và tinh chỉnh ngưỡng confidence/NMS cho vùng center.
- **Bằng chứng (ảnh trong `screenshots/`, dòng findings, rule)**:
  + Dòng finding `adasind_069450.jpg` object `L6` (SPURIOUS, rule R01): Chiều cao box trên ảnh gốc chỉ đạt 25.9 px, vi phạm ngưỡng lọc H ≥ 40 px.
  + Dòng finding `adasind_062370.jpg` object `L3` (WRONG_CLASS, rule R04): Xe ba bánh chở hàng bị gán nhầm thành Truck do góc nhìn sau.
  + Dòng finding `adasind_117120.jpg` object `L3, L4` (SPURIOUS, rule R01/R02): Các box ThreeWheeler vẽ chồng lấn trong đám đông xe bị che khuất ở hậu cảnh xa.
