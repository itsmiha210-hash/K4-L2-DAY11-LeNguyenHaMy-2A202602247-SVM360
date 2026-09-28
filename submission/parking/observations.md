# Quan sát vạch ô đỗ

- Hai vạch `parking_line` đã vẽ (mô tả vị trí trong ảnh):
  1. Vạch thứ nhất ở tiền cảnh chính giữa ảnh (bắt đầu từ đầu vạch nhìn thấy tại x ≈ 403, y ≈ 652 kéo dài xuống mép dưới ảnh tại x ≈ 536, y ≈ 720), phân chia hai ô đỗ liền kề ở hàng trước.
  2. Vạch thứ hai ở tiền cảnh bên phải ảnh (bắt đầu từ x ≈ 692, y ≈ 624 kéo dài chéo xuống mép phải ảnh tại x ≈ 960, y ≈ 684), phân chia ô đỗ tiếp theo ở hàng trước.
- Một vạch/dấu sơn hoặc biên **không** vẽ, và vì sao:
  - Dấu sơn ngắn/mờ ở góc đáy bên trái (x ≈ 28, y ≈ 690–718) và các vạch ngang ranh giới ở phía sau (midground): Đoạn sơn bên trái quá mờ, bị khuyết không đủ cơ sở xác định một ô đỗ trọn vẹn; các vạch ngang/đường ranh ở midground đóng vai trò phân giới hạn lối xe chạy (aisle boundary) chứ không phải vạch phân chia từng ô đỗ riêng lẻ (stall divider).
- Polygon `free_space` dừng ở đâu; có phần bị che nào không:
  - Polygon `free_space` bao trọn khoảng mặt đường nhựa (asphalt) trống nhìn thấy được của lối xe chạy chính giữa hai dãy ô đỗ:
    + Cận cảnh: Dừng ngay mép đầu các vạch ô đỗ tiền cảnh (y ≈ 624–665), không lấn vào diện tích đỗ xe.
    + Phía xa: Dừng trước hàng vạch của dãy ô đỗ thứ hai (y ≈ 570), không kéo qua dãy đỗ xa và chiếc xe đỏ ở góc trái.
    + Hai bên: Bao phủ toàn bộ bề rộng lối xe chạy nhìn thấy được (x ≈ 40–900). Không có vật cản (xe, người, cây cối, curb) nào che khuất trong vùng này.
- Ca chưa chắc cần hỏi người soát (nếu không có, ghi “không có”):
  - Các vạch sơn ở dãy đỗ xe thứ hai (midground) do khoảng cách xa và góc chụp thấp nên bị nén phối cảnh và mờ dần, ranh giới giữa vạch chia ô và mép lối xe chạy không hoàn toàn rõ nét nếu không có quy định bổ sung về ngưỡng khoảng cách/độ rõ nét tối thiểu.
