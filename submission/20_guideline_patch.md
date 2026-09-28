# Guideline patch

- **Rule mới đề xuất:** `R12 - Quy tắc xử lý ranh giới phương tiện bị che khuất xa trong vùng méo fisheye (Distantly Occluded Boundary Rule)`
- **Áp dụng cho:**
  - Classes: `Car`, `Truck`, `Bus`
  - Attributes: `occluded="true"`, `truncated="false"`
  - Zones: `mid` và `edge`
  - Kích thước: Bounding box có chiều cao $H \in [40, 60]\text{ px}$ bị che khuất từ $40\%$ đến $75\%$ bởi phương tiện khác hoặc kiến trúc hạ tầng đô thị.
- **Vì sao luật hiện tại (`docs/02-rules-vi.md`) không đủ:**
  - Trong `docs/02-rules-vi.md`, Rule R01 yêu cầu gán nhãn các xe có $H \ge 40\text{ px}$, và R04 yêu cầu bao phủ cả phần bị che khuất nếu hình dáng tổng thể ngoại suy được. Tuy nhiên, guideline chưa đưa ra ngưỡng định lượng về tỷ lệ che khuất (occlusion percentage threshold) đối với các xe ở vùng `mid` và `edge` nơi thấu kính fisheye làm cong méo phối cảnh.
  - Hậu quả thực tế thể hiện ở frame `adasind_258420.jpg`: Chiếc xe Car tại tọa độ `[547, 462, 654, 535]` ($H \approx 45\text{ px}$) bị che khuất $\approx 50\%$ bởi xe đi trước. Learner (`L10`) và Model YOLO26m (`M6`) đều nhận diện và đóng hộp bao gồm phần ngoại suy, nhưng Teaching Reference (`R5`) lại bỏ qua hoàn toàn (coi như không đủ điều kiện gán). Sự thiếu vắng quy tắc ngưỡng rõ ràng dẫn đến tranh chấp giữa Annotator, QA và Reference (xem `30_escalation_ticket.md`).
  - Quy tắc mới R12 chuẩn hóa:
    1. Khi phần nhìn thấy của xe còn lại $\ge 20\text{ px}$ theo chiều cao và nhận diện được ít nhất 2 chi tiết kết cấu (như cụm đèn, vòm bánh xe, hoặc cột kính), annotator bắt buộc phải vẽ bounding box bao quát toàn bộ kích thước xe (cả phần ẩn) và bật thuộc tính `occluded="true"`.
    2. Nếu bị che khuất trên $75\%$ và biên giới hạn của thân xe không thể ngoại suy chắc chắn trong không gian fisheye, cấm vẽ box đoán mò và phải gán polygon `ignore_region` với nhãn lý do `heavy_occlusion_uncertain`.
- **`rules_version` mới:** `v1.1.0` (nâng cấp từ bản chuẩn `v1.0.0`)
- **Hiệu lực từ:** Vòng `rework` (P5) và áp dụng bắt buộc cho quy trình thẩm định `gold_set_review`.
