# Zone table (slice của bạn)

Lệnh `python3 lab11.py model` tự ghi bảng số (cùng cách đếm với `r1_craft/compare.md` và `model_compare.md`); chạy lại lệnh sẽ cập nhật bảng và giữ nguyên phần nhận xét. Bạn chỉ viết mục Nhận xét.

| Zone | n_ref | L missing | L spurious | M missing (`LR_noM` + `R_only`) | M thừa (`LM_noR` + `M_only`) | Lỗi L chính (`what`) |
|---|---:|---:|---:|---:|---:|---|
| center | 13 | 3 | 4 | 6 | 7 | SPURIOUS (3) |
| mid | 5 | 0 | 0 | 2 | 3 | ATTRIBUTE (1) |
| edge | 2 | 0 | 0 | 1 | 2 | — |

## Nhận xét

- **Zone gãy nhiều nhất ở cả L và M**: Cả người học (L) và mô hình (M) đều gãy nhiều nhất tại vùng **`center`**. Cụ thể: L có 3 missing và 4 spurious (lỗi chính là SPURIOUS với 3 ca); M có 6 missing (`LR_noM` + `R_only`) và 7 thừa (`LM_noR` + `M_only`). Trong khi đó ở vùng `mid` và `edge`, L đạt độ chính xác phát hiện tuyệt đối (0 missing, 0 spurious), còn M vẫn bị sót 2 missing ở mid và 1 missing ở edge.
- **Giả thuyết nguyên nhân và giới hạn slice 3 frame**:
  + *Nguyên nhân*: Vùng `center` tập trung mật độ phương tiện đông đúc nhất (n_ref = 13), xuất hiện nhiều đối tượng ở khoảng cách xa với kích thước nhỏ sát ngưỡng lọc quy định H = 40 px và bị che khuất một phần (occlusion). Annotator dễ vẽ thừa các đối tượng ở xa chưa đạt chuẩn kích thước hoặc box chưa đủ tight; mô hình YOLO bị nhầm lẫn và bỏ sót do độ phân giải thấp ở hậu cảnh. Ở vùng `edge`, hình dạng vật thể bị méo quang học cong dẹt theo thấu kính fisheye khiến mô hình bị giảm độ tự tin nhận diện.
  + *Giới hạn của slice 3 frame*: Mẫu khảo sát chỉ gồm 3 frame tĩnh (tổng cộng 20 vật thể reference), cỡ mẫu quá nhỏ để có ý nghĩa thống kê cho toàn bộ hệ thống camera ngoài SVM; không thể hiện được tính ổn định tracking qua thời gian hay đặc tính khác biệt giữa 4 vị trí gắn camera (front/rear/left/right).
