# QA review · B2-edge

Mã khóa: B28A-2C10

| frame | object_ref | rule_id | nhận xét |
|---|---|---|---|
| adasind_069450.jpg | L4 | R05 | ThreeWheeler vàng-xanh ở lề trái chạm mép vòng kính (x=4.0) bị cắt một phần thân nhưng để truncated=false. Cần sửa thành truncated=true theo R05. |
| adasind_082170.jpg | L5 | R05 | Bike ở lề trái (x=14.5) chạm biên quang học cần đặt truncated=true. Ngoài ra L5 và L4 đỗ chồng lấn sát nhau nhưng đều để occluded=false. |
| adasind_102750.jpg | L_missing | R01 | Bỏ sót xe tải cabin đỏ (Truck) đỗ ở lề đường bên trái (x: 0–90, y: 854–959). Chiều cao vật ~105 px (vượt ngưỡng H=40 px theo R01), cần bổ sung box Truck. |
| adasind_102750.jpg | L3 | R02 | Box Truck lớn màu đỏ bên phải đường đã set truncated=true tốt, nhưng cạnh đáy (ybr=1158.8) chưa phủ sát chân bánh xe tiếp xúc mặt đường (ybr ≈ 1165). |

Ghi finding r2_qa: cell=L_only, rule_id có giá trị, why để trống.
