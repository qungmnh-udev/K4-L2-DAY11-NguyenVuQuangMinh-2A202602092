# Kế hoạch review từ lỗi quan sát được

Từ `findings.csv` và `zone_table.md`, chọn **hai lát cắt của bài ADASIND một camera** cần review trước. Bảng này giải thích dữ liệu thật vừa thực hiện; nó không thay cho kế hoạch bốn camera giả lập ở `45_sampling_plan.csv`.

| Lát cắt / frame | Số ca và loại lỗi | Vì sao review trước | Bằng chứng cần giữ |
|---|---|---|---|
| `adasind_258420.jpg` (Vùng `mid` - Phương tiện che khuất & Lỗi Reference) | 1 ca lỗi teaching reference (`E0_reference_defect` tại `L10+M6`, xe Car `[547, 462, 654, 535]`) và các ca tranh chấp ranh giới hộp xe bị che khuất một phần ở vùng xa | Đây là rủi ro nghiêm trọng nhất trong kiểm soát chất lượng dữ liệu: lỗi bắt nguồn từ chính bộ nhãn chuẩn (`teaching reference`), làm sai lệch điểm số đánh giá của cả annotator lẫn mô hình AI. Cần làm rõ quy tắc R12 trước khi mở rộng review hàng loạt | Ảnh chụp `submission/screenshots/escalation_adasind_258420.png`, ticket `30_escalation_ticket.md`, mục DEC-04 trong `40_decision_log.csv`, và dữ liệu đối chiếu trong `r3_diag/local_quality_conflicts.csv` |
| `adasind_310008.jpg` (Vùng `edge` - Méo quang học cực đại & Người đi bộ sát mép) | 2 ca người đi bộ sát mép kính (`Pedestrian edge` tại `[56, 948]` và `[38, 947]`), biến dạng hình học thân người và tiếp giáp `lens_border` | Vùng mép thấu kính (`edge zone`) chịu hệ số biến dạng phi tuyến lớn nhất, khiến tỷ lệ khung hình người đi bộ bị kéo giãn dị thường; rất dễ dẫn đến gán nhãn thiếu chân chạm đất hoặc nhầm lẫn giữa `truncated` và `occluded` | File XML `submission/r1_craft/annotations.xml`, bảng tỉ lệ lấp đầy K12 trong `selfqc.md`, báo cáo `local_quality.md` và bảng `r3_diag/zone_table.md` |

Giới hạn của kết luận từ ba frame ADASIND:
Ba frame (`adasind_236370`, `adasind_258420`, `adasind_310008`) chỉ được trích xuất từ **một góc camera vật lý duy nhất** (camera sườn/vòm bánh xe) trong cùng một điều kiện thời tiết ban ngày tại khu vực bãi đỗ/đường nội bộ. Do đó:
1. Không thể khái quát hóa cho các điều kiện môi trường bất lợi khác (như mưa rơi trên mặt kính, đèn pha rọi ban đêm, đường hầm thiếu sáng).
2. Không phản ánh được đặc thù quang học của 3 vị trí camera còn lại (camera trước nhìn xa, camera lùi chúi gầm, camera đối diện).
3. Hoàn toàn không ghi nhận được các hiện tượng đa cảm biến cốt lõi trong hệ thống SVM 360 như: đường ranh giới ghép mí (seam lines), hiện tượng trôi lệch thời gian giữa các luồng video (timestamp desync), và sự phân mảnh đối tượng xuyên camera.

## Chuyển sang kế hoạch bốn camera giả lập

Cách soát độ phủ của 200 frame ở `45_sampling_plan.csv` (kể cả tránh đếm nhiều frame liền nhau trong cùng cảnh như nhiều ca độc lập), và vì sao kế hoạch đó chỉ giúp tìm ca cần soi, chưa đo được tỷ lệ lỗi:
1. **Kiểm soát độ phủ và tính độc lập của mẫu:**
   - Áp dụng phương pháp lấy mẫu phân tầng (Stratified Sampling) theo 4 góc camera (`front`, `rear`, `left`, `right`) và 2 nhóm độ khó (`normal`, `hard`).
   - Ngăn chặn triệt để hiện tượng đếm trùng cảnh liền kề (temporal pseudo-replication): Quy định nghiêm ngặt khoảng cách giữa hai frame liên tiếp được chọn trong cùng một chuỗi quay video phải cách nhau tối thiểu $\Delta t \ge 3$ giây hoặc quãng đường di chuyển $\Delta s \ge 15\text{ m}$. Cấm tuyệt đối việc chọn các frame liên tiếp cạnh nhau (consecutive frames) vì chúng mang thông tin dư thừa, góc phối cảnh và ánh sáng giống hệt nhau, làm méo mó tính độc lập thống kê.
   - Trải đều mẫu qua ma trận kịch bản hoạt động ODD (Operational Design Domain): Đô thị đông đúc, ngõ hẹp, bãi đỗ xe ngầm, cao tốc, ánh sáng ngược nắng, ban đêm có đèn đường.
2. **Bản chất của kế hoạch 200 frame:**
   - Kế hoạch 200 frame là quy trình lấy mẫu tập trung vào rủi ro (Risk-driven Purposive Sampling) nhằm săn tìm các trường hợp biên nguy hiểm (edge cases, corner cases), vùng thấu kính méo và vùng ghép mí để phát hiện sớm các khiếm khuyết trong guideline gán nhãn và lỗ hổng mô hình.
   - Vì mẫu phân bổ có tỷ trọng ca khó rất cao (85 frame `hard` chiếm tới $42.5\%$ tổng mẫu, cao hơn rất nhiều so với tỷ lệ ca khó tự nhiên trong 50.000 frame), nên tỷ lệ lỗi đo được trên 200 frame này mang tính thiên lệch có chủ đích (intentional sampling bias). Nó phản ánh mức độ nghiêm trọng ở những điểm xung yếu nhất, chứ **không phải** là ước lượng tỷ lệ lỗi không chệch (unbiased error rate) của toàn bộ 50.000 frame dữ liệu vận hành thực tế.
