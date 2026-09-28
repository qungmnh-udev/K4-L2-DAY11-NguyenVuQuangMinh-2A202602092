# Escalation ticket

## Ticket 1

- **Frame:** `adasind_258420.jpg`
- **Object Ref:** `L10+M6` (Class `Car`, zone `mid`, box `[547.4, 462.1, 654.4, 535.0]`)
- **Ảnh chụp:** `submission/screenshots/escalation_adasind_258420.png`
- **Expected impact:**
  - Lỗi teaching reference (`E0_reference_defect`): Teaching reference bỏ sót một phương tiện giao thông thực tế có kích thước đáng kể ($H \approx 45\text{ px}$, $W \approx 107\text{ px}$) đang tham gia giao thông trên luồng đường chính.
  - Tác động tiêu cực: Trong quy trình đánh giá tự động `local-quality`, annotator (`L10`) và model AI (`M6`) phát hiện đúng đối tượng này đều bị tính phạt là False Positive (FP), làm méo mó chỉ số IoU và độ chính xác mAP. Nguy hiểm hơn, nếu annotator chạy theo điểm số mà xóa bỏ đối tượng có thật này thì hệ thống nhận thức ADAS sẽ hình thành điểm mù (blind spot), đe dọa trực tiếp an toàn tự hành.
- **Owner:** `data_ops` (chủ trì cập nhật bộ nhãn chuẩn Reference/Gold) phối hợp với `guideline` (chuẩn hóa quy tắc thẩm định R12).
- **Recommendation:**
  1. Yêu cầu bộ phận `data_ops` phê duyệt và bổ sung box `Car` cho đối tượng `L10+M6` vào file reference chuẩn `_ref/B4-edge.xml` với thuộc tính `occluded="true"`, `truncated="false"`.
  2. Ban hành quy chuẩn `v1.1.0` theo bản vá `20_guideline_patch.md` để các annotator và QA có định lượng rõ ràng khi xử lý xe bị che khuất trong vùng méo fisheye.
  3. Đồng bộ quyết định vào `40_decision_log.csv` (mã `DEC-04`) với trạng thái `escalated`.
