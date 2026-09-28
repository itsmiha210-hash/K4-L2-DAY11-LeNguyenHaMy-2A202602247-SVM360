# Guideline patch

- **Rule mới đề xuất:** Quy định xử lý cụm phương tiện ở hậu cảnh xa và bị che khuất nặng (Heavy Occlusion & Distant Cluster): Khi các phương tiện giao thông ở xa bị che khuất > 50% diện tích hoặc kích thước chiều cao dao động sát ngưỡng 40 px mà không thể xác định rõ ràng ranh giới tiếp xúc mặt đất/bánh xe, nghiêm cấm vẽ phỏng đoán từng box rời rạc. Toàn bộ cụm xe này phải được bao bọc bằng một polygon `ignore_region` với thuộc tính `reason="crowd_or_group"` (nếu là cụm xe đông đúc) hoặc `reason="unreadable"` (nếu đối tượng bị mờ nhòe/khuất gần hết).
- **Áp dụng cho:** Tất cả các class phương tiện giao thông đường bộ (`Car`, `ThreeWheeler`, `Truck`, `Bus`), trọng tâm tại zone `center` và `mid` trong các khung cảnh giao thông mật độ cao (dense traffic).
- **Vì sao luật hiện tại (`docs/02-rules-vi.md`) không đủ:** Luật hiện hành v1.0.0 chỉ có quy tắc ngưỡng kích thước đơn lẻ R01 (H ≥ 40 px) và danh mục lý do ignore R06, nhưng chưa làm rõ ranh giới xử lý tình huống các xe nối đuôi nhau ở xa bị che khuất phân nửa. Điều này dẫn đến sự không thống nhất giữa các annotator: người thì cố gắng vẽ box phỏng đoán gây ra lỗi SPURIOUS / BOX_GEOMETRY, người thì bỏ qua gây lỗi MISSING.
- **`rules_version` mới:** v1.1.0
- **Hiệu lực từ:** Áp dụng ngay từ vòng `rework` và toàn bộ các đợt kiểm thử QA tiếp theo.
