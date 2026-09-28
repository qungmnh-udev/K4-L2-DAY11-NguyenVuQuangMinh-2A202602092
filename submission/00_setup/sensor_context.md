# Sensor context

- **Rig**: Camera mắt cá được gắn hướng về phía trước trên xe hai bánh. Vị trí đặt camera ở khoảng ngang ngực của hành khách, góc nhìn siêu rộng bao quát toàn cảnh lòng đường, người đi bộ, phương tiện ngược chiều và ven đường.
- **`ego_body`**: Xuất hiện rất rõ ràng ở góc dưới - bên trái và mép đáy khung hình trong cả 3 frame:
  - Nhìn thấy cánh tay áo sơ mi kẻ ca rô (plaid/checkered shirt), bàn tay, chân, đầu gối và giày dép (dép kẹp, dép Crocs) của người ngồi trên xe ego.
  - Nhìn thấy một phần tay lái/gương chiếu hậu và thân vỏ xe máy của xe gắn camera.
  - Cả 3 frame của slice B4-edge đều có thân xe/người ego nhìn thấy được nên đều cần vẽ polygon `ego_body` (`reason: ego_body`) để loại trừ.
- **Vòng kính (lens circle)**: Vòng tròn quang học mắt cá nằm chính giữa ảnh theo chiều ngang (cx ≈ 436–630 px, cy ≈ 938–1038 px), bán kính quang học r ≈ 813–818 px trên khung hình đứng (1080×1920 px). Vòng kính chiếm khoảng 70%–75% diện tích khung hình; phần rìa 4 góc ngoài vòng kính là vành đen quang học hoàn toàn (`lens_border`).
