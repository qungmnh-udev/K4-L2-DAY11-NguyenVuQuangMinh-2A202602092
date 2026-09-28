# Quan sát vạch ô đỗ

- **Hai vạch `parking_line` đã vẽ (mô tả vị trí trong ảnh)**:
  - Vạch 1: Vạch sơn trắng kẻ chéo ở tiền cảnh góc dưới - trung tâm (`points="416.71,718.59;546.99,655.85"`), phân định ranh giới giữa hai ô đỗ xe liền kề ở hàng trước.
  - Vạch 2: Vạch sơn trắng kẻ chéo ở tiền cảnh góc dưới bên phải (`points="719.47,698.81;999.00,643.00"`), tạo mép chia ô đỗ xe phía bên phải mặt đường.
- **Một vạch/dấu sơn hoặc biên không vẽ, và vì sao**:
  - Không vẽ vạch sơn mờ ở mép biên sát hàng rào cây xanh phía xa và các dải sơn phản quang lóa sáng ở hậu cảnh bên trái: Đây là ranh giới bao quanh bãi đỗ/lề an toàn, không có vai trò chia tách từng ô đỗ xe riêng biệt.
  - Dải mặt đường giữa hai dãy ô là lối xe chạy (driving lane / aisle), không được gán nhãn là `parking_line`.
- **Polygon `free_space` dừng ở đâu; có phần bị che nào không**:
  - Polygon `free_space` phủ kín mặt nhựa đường trống nhìn thấy được ở lối xe chạy trung tâm và các khoảng trống giữa các dãy ô đỗ.
  - Vùng dừng lại trước mép hàng rào gỗ/rặng cây ở hậu cảnh và bao quanh chiếc ô tô màu đỏ ở phía xa bên trái (polygon không đi xuyên qua chiếc xe màu đỏ hay chân hàng rào). Không có vật cản lớn che khuất tầm nhìn mặt đường ở tiền cảnh.
- **Ca chưa chắc cần hỏi người soát (nếu không có, ghi “không có”)**:
  - Đoạn mặt sân ở góc xa bên trái có ánh sáng mặt trời phản chiếu gây lóa trắng mạnh, một số đoạn vạch kẻ bị mờ ngắt quãng do phản quang và góc chụp xa, cần quy chuẩn xem có nên vẽ tiếp theo quán tính hay chỉ vẽ đoạn nhìn thấy rõ nét.
