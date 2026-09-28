# Kế hoạch review từ lỗi quan sát được

Từ `findings.csv` và `zone_table.md`, chọn **hai lát cắt của bài ADASIND một camera** cần review trước. Bảng này
giải thích dữ liệu thật bạn vừa làm; nó không thay cho kế hoạch bốn camera giả lập ở `45_sampling_plan.csv`.

| Lát cắt / frame | Số ca và loại lỗi | Vì sao review trước | Bằng chứng cần giữ |
|---|---|---|---|
| Cụm xe ùn tắc ở zone `center` (`adasind_117120.jpg`) | 5 ca (SPURIOUS, BOX_GEOMETRY, WRONG_CLASS) | Mật độ xe dày đặc che khuất nhau là nguyên nhân hàng đầu gây bất đồng nhãn giữa annotator và model | Ảnh crop cụm xe `submission/screenshots/adasind_117120_cluster.png` và các dòng findings r3_diag liên quan |
| Đối tượng ở xa sát ngưỡng H=40px (`adasind_069450.jpg`) | 3 ca (SPURIOUS đối tượng nhỏ & MISSING) | Dễ xảy ra sai sót gán nhãn theo cảm tính vi phạm rule kích thước tối thiểu R01 | Ảnh crop xe nhỏ `submission/screenshots/adasind_069450_small_car.png` kèm số đo pixel chiều cao |

**Giới hạn của kết luận từ ba frame ADASIND**: Dữ liệu chỉ phản ánh hành vi trên một camera góc nhìn phía trước trong một số khoảnh khắc tĩnh; cỡ mẫu quá nhỏ không mang tính đại diện thống kê và không thể khái quát cho toàn bộ hành vi của 3 camera còn lại (sau, trái, phải) trong hệ thống SVM 360°.

## Chuyển sang kế hoạch bốn camera giả lập

**Cách soát độ phủ của 200 frame ở `45_sampling_plan.csv` và giới hạn đo lường**:
- Để tránh hiện tượng đếm nhiều frame liền nhau trong cùng cảnh như các ca độc lập (spatial-temporal autocorrelation), quy trình chọn 200 frame phải áp dụng khoảng cách bước nhảy thời gian tối thiểu (stride ≥ 30 frame giữa các lần lấy mẫu trong cùng clip), đồng thời phân bổ đều trên các cung đường, thời điểm (ngày/đêm) và điều kiện thời tiết khác nhau.
- Kế hoạch 200 frame này tập trung vào các lát cắt có rủi ro cao (hard/edge cases) để phát hiện lỗ hổng và trường hợp cần can thiệp chính sách; nó **không phải là mẫu ngẫu nhiên độc lập (stratified random sample)** đại diện cho toàn bộ 50.000 frame, do đó không thể dùng tỷ lệ lỗi trên 200 frame này để ngoại suy thành tỷ lệ lỗi tổng thể của toàn hệ thống sản xuất.
