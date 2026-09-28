# Đối chiếu chất lượng cục bộ — rectangle

Teaching reference, không phải gold set đã phê duyệt; không có điểm đạt tự động.
Nguồn: export r1_craft đã khóa SHA256 `a80da94f80c554f9fadd2d0b738862b70cad3230a689281b79b0e2f7ba33ca1c`; slice `B2-dense`.
Ghép hình học greedy một-một theo IoU ≥ 0.50, rồi so class; H ≥ 40 px.
Box trái nằm chủ yếu trong ignore_region reference không tính. Polygon, polyline, track không được chấm.
Đây là phép tính offline của lab, không phải báo cáo hay kết quả tương đương CVAT Premium.

Frame được tính: adasind_062370.jpg, adasind_069450.jpg, adasind_117120.jpg. Frame thiếu trong export: không.
TP=17; FP=4; FN=3; số lần đối chiếu=23; mean IoU của TP=0.845.

| Chỉ số | Micro | Macro | Nhãn thấp nhất |
|---|---:|---:|---:|
| accuracy | 0.739 | 0.949 | 0.826 |
| precision | 0.810 | 0.619 | 0.000 |
| recall | 0.850 | 0.619 | 0.000 |
| jaccard | 0.708 | 0.593 | 0.000 |
| dice | 0.829 | 0.619 | 0.000 |

| Nhãn | TP | FP | FN | Accuracy | Precision | Recall | Jaccard | Dice |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Bike | 4 | 0 | 0 | 1.000 | 1.000 | 1.000 | 1.000 | 1.000 |
| Bus | 0 | 1 | 0 | 0.957 | 0.000 | 0.000 | 0.000 | 0.000 |
| Car | 4 | 0 | 0 | 1.000 | 1.000 | 1.000 | 1.000 | 1.000 |
| Pedestrian | 4 | 0 | 0 | 1.000 | 1.000 | 1.000 | 1.000 | 1.000 |
| ThreeWheeler | 5 | 2 | 2 | 0.826 | 0.714 | 0.714 | 0.556 | 0.714 |
| Truck | 0 | 1 | 1 | 0.913 | 0.000 | 0.000 | 0.000 | 0.000 |

| Frame | TP | FP | FN | Accuracy | Precision | Recall |
|---|---:|---:|---:|---:|---:|---:|
| adasind_062370.jpg | 6 | 1 | 3 | 0.667 | 0.857 | 0.667 |
| adasind_069450.jpg | 5 | 0 | 0 | 1.000 | 1.000 | 1.000 |
| adasind_117120.jpg | 6 | 3 | 0 | 0.667 | 0.667 | 1.000 |

Confusion matrix: hàng = teaching reference; cột = export đã khóa.
`<missing>` là thiếu box; `<extra>` là box thừa. Xem `local_quality_confusion.csv`.

| Reference \ Export | Bike | Bus | Car | Pedestrian | ThreeWheeler | Truck | <missing> |
|---|---:|---:|---:|---:|---:|---:|---:|
| Bike | 4 | 0 | 0 | 0 | 0 | 0 | 0 |
| Bus | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| Car | 0 | 0 | 4 | 0 | 0 | 0 | 0 |
| Pedestrian | 0 | 0 | 0 | 4 | 0 | 0 | 0 |
| ThreeWheeler | 0 | 0 | 0 | 0 | 5 | 1 | 1 |
| Truck | 0 | 0 | 0 | 0 | 0 | 0 | 1 |
| <extra> | 0 | 1 | 0 | 0 | 2 | 0 | 0 |

Chi tiết xung đột trong `local_quality_conflicts.csv`; dữ liệu máy đọc trong `local_quality.json`.
Mismatching label đóng góp một FP cho class vẽ và một FN cho class reference; attribute khác được báo riêng.
Micro accuracy đếm mỗi cặp ghép sai class là một lần đối chiếu; Jaccard đếm cả FP và FN.
Macro/worst bỏ nhãn không xuất hiện ở cả hai phía; chỉ số không có mẫu là N/A.
