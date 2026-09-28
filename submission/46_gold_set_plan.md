# Đề xuất gold set theo camera — tình huống giả lập

**Đầu bài:** 50.000 frame từ bốn camera SVM, ngân sách chọn 200 frame để review/gold. Đây là tình huống trên slide,
**không phải** 50.000 frame có trong repo. Phân bổ đúng 200 ở `45_sampling_plan.csv` cho bốn camera, mỗi camera có
normal và hard slice. “Gold set” ở đây là **kế hoạch tạo** reference sau kiểm chứng, không phải teaching reference
ADASIND hoặc nhãn bạn vừa vẽ. Nếu cần, dùng `notebooks/day11-svm360-colab.ipynb` để thử tổng phân bổ; notebook
không làm thay phần lý do.

| camera_id | Hard case cần chọn | Vì sao dễ sai | Annotation space / calibration cần giữ | Cách review trước khi gọi là gold |
|---|---|---|---|---|
| front | Xe ngược chiều/người băng qua đường ban đêm bị đèn pha chiếu chói và vật thể sát góc capo | Méo ngoại vi lớn làm box dẹt, chói sáng che mất ranh giới biên xe | Không gian fisheye gốc (raw fisheye space); giữ nguyên ma trận nội suy không biến dạng | Review chéo độc lập 2 annotator cấp cao + đối soát trên video chuỗi liên tiếp |
| rear | Chướng ngại vật thấp (cọc tiêu, thú cưng, curb) nằm sát cản sau khi lùi xe | Góc nhìn hướng xuống thấp, kích thước vật nhỏ và bị che khuất một phần | Raw fisheye space; giữ calibration khoảng cách mặt đất | Soát trực quan từng frame có zoom 200% và kiểm tra tiếp xúc mặt đường |
| left | Xe máy/xe đạp vượt sát gương chiếu hậu bên sườn trái trong vùng chuyển tiếp | Nằm vắt ngang vùng seam giữa camera trước và trái; méo tiếp tuyến mạnh | Hệ tọa độ fisheye sườn trái; đồng bộ timestamp với camera trước | Kiểm tra đồng thời cả 2 view (front + left) tại thời điểm timestamp trùng khớp |
| right | Người đi bộ bước từ vỉa hè xuống lòng đường sát thân xe bên phụ | Dễ nhầm giữa người đi bộ độc lập và người chuẩn bị trèo lên phương tiện; bóng râm vỉa hè | Raw fisheye space; cố định vị trí gắn camera trên gương phụ | Review blind kép và kiểm tra attribute occlusion/truncation |

- Khi nào cần refresh gold set (đổi camera, calibration hoặc rule): Cần làm mới (refresh) gold set khi có sự thay đổi về phần cứng camera (góc nhìn FOV, độ phân giải cảm biến), thay đổi vị trí/góc gắn camera (rig calibration ngoại suy), cập nhật thuật toán khử méo/stitch BEV, hoặc khi quy chuẩn gán nhãn (guideline/schema) thay đổi định nghĩa các class và ngưỡng thuộc tính.
- Một ca seam/cross-camera cần policy và evidence trước khi ghép hai box: Khi một đối tượng (ví dụ xe máy vượt) xuất hiện đồng thời trên cả camera trước và camera bên sườn tại vùng góc xe (seam), không được tùy tiện xóa một box hoặc tự ý gộp khi chưa có timestamp đồng bộ tuyệt đối và calibration ngoại suy giữa hai camera. Cần policy rõ ràng: giữ cả 2 box trên từng camera độc lập ở tầng gán nhãn ảnh gốc (raw fisheye) và chuyển dữ liệu đồng bộ cho tầng sensor fusion/tracking xử lý hợp nhất (fusion layer) trên không gian tọa độ xe (BEV/vehicle frame).
- Vì sao peer agreement hoặc quality report trên ảnh một camera chưa chứng minh gold set đúng cho cả bốn camera: Đánh giá chất lượng trên 1 camera (như camera trước đơn lẻ) chỉ phản ánh phân bố hình học và điều kiện chiếu sáng của hướng nhìn đó. Ba camera còn lại (sau, hai bên sườn) có đặc tính quang học, độ cao đặt cảm biến, góc chúi, tác động của luồng gió/bụi bẩn và vùng quan sát rất khác biệt. Sự đồng thuận cao trên camera trước không đảm bảo người gán nhãn hiểu đúng hành vi méo tiếp tuyến hay hiện tượng chồng lấp vùng seam trên các camera sườn và sau.
