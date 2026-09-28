# Exit ticket

Đọc `docs/10-svm360-reading-vi.md` trước khi trả lời câu 1–2. Các câu về zone, `why`, rework, parking và sampling
đã nằm trong file tương ứng nên không hỏi lại ở đây.

1. **Một vật ở vùng seam giữa hai camera thật xuất hiện với hai box khác nhau: đó là lỗi `DUPLICATE` hay cần một quy tắc riêng? Vì sao?**  
   Đây **không phải lỗi `DUPLICATE`** của người gán nhãn, mà **cần một quy tắc / chính sách riêng (cross-camera policy)**.  
   *Vì sao:* Mỗi camera fisheye có góc đặt, độ cao và độ méo thấu kính riêng. Khi một vật thể nằm ở vùng chồng (seam) giữa hai camera (ví dụ góc trước - trái), vật thể xuất hiện tự nhiên trên cả hai khung hình (trên camera trước có thể ở vùng `edge`, còn trên camera sườn trái ở vùng `mid`). Ở tầng gán nhãn 2D trên ảnh raw fisheye, người gán nhãn bắt buộc phải vẽ box độc lập bám sát phần nhìn thấy trên từng camera để phục vụ huấn luyện mô hình phát hiện 2D. Việc gộp hoặc liên kết hai box này thành một thực thể duy nhất thuộc về tầng sensor fusion / BEV tracking phía sau (dựa trên ma trận calibration ngoại suy và timestamp đồng bộ), chứ không được tự tiện xóa hay coi là lỗi trùng lặp ở tầng 2D.

2. **Một vật đi qua nhiều frame trên cùng camera: khi nào giữ cùng track ID, khi nào thêm keyframe hoặc trạng thái Outside? Nêu bằng chứng sẽ cần trước khi nối track qua hai camera.**  
   - *Giữ cùng track ID:* Khi vật thể liên tục xuất hiện trong tầm nhìn và duy trì được đặc trưng nhận diện thực thể (identity) rõ ràng qua các frame liên tiếp.  
   - *Thêm keyframe:* Khi đối tượng có sự biến đổi hình học lớn (quay đầu, đổi góc nhìn đột ngột, phóng to/thu nhỏ nhanh) hoặc thay đổi thuộc tính (`occluded` khi bị vật khác che, `truncated` khi chạm vành kính).  
   - *Trạng thái Outside:* Khi đối tượng di chuyển hoàn toàn ra ngoài trường nhìn của ống kính hoặc bị che khuất 100% trong nhiều frame trước khi có khả năng xuất hiện trở lại.  
   - *Bằng chứng cần thiết trước khi nối track qua hai camera:* Cần 4 yếu tố then chốt: (1) Timestamp đồng bộ chính xác cấp độ mili-giây giữa hai camera; (2) Ma trận hiệu chuẩn ngoại suy (extrinsics) xác định vị trí tương đối giữa các camera; (3) Tính liên tục của quỹ đạo 3D / vector vận tốc khi di chuyển qua vùng seam; (4) Độ tương đồng đặc trưng nhận dạng trực quan (Re-ID visual embedding) để khẳng định cùng một thực thể.

3. **Nhìn lại cả buổi: một chỗ bạn tin nhãn mình đúng nhưng reference hoặc người soát nghĩ khác (dẫn frame/`object_ref`), bạn đã xử lý thế nào, và nếu làm lại slice này bạn sẽ đổi gì trong cách làm?**  
   - *Tình huống cụ thể:* Ở frame `adasind_069450.jpg`, đối tượng `L6` là một chiếc xe Car ở xa có chiều cao thực tế ~26 px. Tôi vẽ box vì thấy rõ đây là xe ô tô, nhưng rule R01 quy định nghiêm ngặt chỉ gán nhãn vật thể có chiều cao H ≥ 40 px trên ảnh gốc.  
   - *Cách xử lý:* Tôi đã xác định nguyên nhân là `E1_annotator_error` kết hợp `E2_guideline_gap`, ghi nhận vào `findings.csv` với hành động `action=rework` để loại bỏ box dưới 40 px theo chuẩn R01, đồng thời ghi vào `decision_log.csv` để thống nhất cách hiểu.  
   - *Nếu làm lại slice này:* Tôi sẽ dùng công cụ đo tọa độ pixel trên CVAT để kiểm tra kích thước đối tượng ở xa ngay từ đầu; nếu chiều cao < 40 px thì dứt khoát không vẽ box. Đối với các cụm xe bị che khuất phức tạp ở hậu cảnh, tôi sẽ gom vào polygon ignore `crowd_or_group` hoặc đưa vào escalation ticket thay vì tự ý vẽ phỏng đoán.
