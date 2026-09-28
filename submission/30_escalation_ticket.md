# Escalation ticket

## Ticket 1

- **Frame:** adasind_117120.jpg
- **Ảnh chụp:** submission/screenshots/adasind_117120_cluster.png
- **Expected impact:** Giảm thiểu tới 30% lỗi phân loại sai và box trùng lặp (SPURIOUS/WRONG_CLASS) trong các tình huống giao thông ùn ứ; thống nhất tiêu chuẩn đánh giá giữa mô hình AI và đội ngũ gán nhãn.
- **Owner:** guideline
- **Recommendation:** Bổ sung điều khoản guideline v1.1.0 cho phép nhóm các phương tiện bị che khuất > 50% diện tích hoặc không rõ ranh giới bánh xe thành polygon `ignore_region` (`reason="crowd_or_group"`), tránh việc annotator tự suy đoán gây tranh chấp nhãn và làm méo mó độ đo đánh giá mô hình.
