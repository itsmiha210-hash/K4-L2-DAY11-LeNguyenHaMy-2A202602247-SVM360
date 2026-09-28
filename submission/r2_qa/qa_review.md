# QA review · B2-dense

Mã khóa: A80D-A94F

| frame | object_ref | rule_id | nhận xét |
|---|---|---|---|
| adasind_069450.jpg | L6 | R01 | Chiều cao xe Car ở xa chỉ đạt ~26 px, dưới ngưỡng quy định H ≥ 40 px của R01, cần loại bỏ khỏi phạm vi gán nhãn |
| adasind_062370.jpg | L1 | R05 | ThreeWheeler sát rìa trái chạm vành kính, cần kiểm tra lại thuộc tính truncated theo ranh giới quang học R05 |
| adasind_117120.jpg | L5 | R02 | Người đi bộ Pedestrian đứng sát ThreeWheeler L3, phần thân dưới bị che khuất một phần cần kiểm tra lại độ ôm sát box và thuộc tính occluded theo R02 |

Ghi finding r2_qa: cell=L_only, rule_id có giá trị, why để trống.
