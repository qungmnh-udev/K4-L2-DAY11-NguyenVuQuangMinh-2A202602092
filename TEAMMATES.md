# Thành viên và phân vai — Day11 SVM 360 Fisheye

## 1. Thông tin nhóm

- Khóa/lớp: K4 - L2 - DAY11
- Tên nhóm: Nhóm K4-Day11 (Minh - Quân - Thạch)
- Repo Public: [https://github.com/qungmnh-udev/K4-L2-DAY11-NguyenVuQuangMinh-2A202602092](https://github.com/qungmnh-udev/K4-L2-DAY11-NguyenVuQuangMinh-2A202602092)
- Máy giữ hồ sơ chính / người quản lý: Nguyễn Vũ Quang Minh (`minh`)
- Slice chung lấy từ mode.json: `B4-edge`
- Tên định danh vai A dùng cho --self: `minh`
- Kênh trao đổi nội bộ: Kênh trao đổi lớp / Discord
- Đại diện nộp (vai C · Điều phối): Cầm Vũ Ngọc Thạch (`thach`)
- Commit chốt bài: HEAD (`qungmnh-udev/K4-L2-DAY11-NguyenVuQuangMinh-2A202602092`)

## 2. Ba vai chính

| Vai | Họ và tên | MSSV | Tên định danh trong mode | Trách nhiệm | Bằng chứng đóng góp |
|---|---|---|---|---|---|
| A · Gán nhãn | Nguyễn Vũ Quang Minh | 2A202602092 | `minh` | Gán nhãn bãi đỗ xe P0, hiệu chuẩn C0 P1, gán nhãn slice B4-edge P2, thực hiện self-QC 9 mục, khóa nhãn, sửa lỗi Rework P5 | [submission/r1_craft/annotations.xml](file:///d:/AI/repo/K4-L2-DAY11-NguyenVuQuangMinh-SVM/submission/r1_craft/annotations.xml) (mã khóa `C0E9-DE25`), [selfqc.md](file:///d:/AI/repo/K4-L2-DAY11-NguyenVuQuangMinh-SVM/submission/r1_craft/selfqc.md), [rework/annotations-v2.xml](file:///d:/AI/repo/K4-L2-DAY11-NguyenVuQuangMinh-SVM/submission/rework/annotations-v2.xml) (mã khóa `84DB-82C2`), [delta.md](file:///d:/AI/repo/K4-L2-DAY11-NguyenVuQuangMinh-SVM/submission/rework/delta.md) |
| B · QA/QC | Quân | [Điền MSSV] | `quan` | Đảm nhiệm QA/QC độc lập trước khi mở reference trên slice B2-edge, đối soát theo 11 rule guideline, lập danh sách finding QA/QC và kiểm duyệt lại chất lượng (QC) ca sửa rework | [submission/r2_qa/qa_review.md](file:///d:/AI/repo/K4-L2-DAY11-NguyenVuQuangMinh-SVM/submission/r2_qa/qa_review.md), [submission/r2_qa/qa_overlay.html](file:///d:/AI/repo/K4-L2-DAY11-NguyenVuQuangMinh-SVM/submission/r2_qa/qa_overlay.html) (mã QA `B28A-2C10`), 4 dòng finding vai `r2_qa` trong [findings.csv](file:///d:/AI/repo/K4-L2-DAY11-NguyenVuQuangMinh-SVM/submission/findings.csv) |
| C · Điều phối | Thạch | [Điền MSSV] | `thach` | Điều phối tiến độ nhóm, phân tích chẩn đoán lỗi mô hình YOLO26m, lập bảng zone_table, chủ trì phân xử ca tranh chấp, lập kế hoạch sampling 200 frame & Gold set, kiểm tra validator gate và chốt nộp | [submission/r3_diag/zone_table.md](file:///d:/AI/repo/K4-L2-DAY11-NguyenVuQuangMinh-SVM/submission/r3_diag/zone_table.md), [submission/10_error_card.md](file:///d:/AI/repo/K4-L2-DAY11-NguyenVuQuangMinh-SVM/submission/10_error_card.md), [submission/40_decision_log.csv](file:///d:/AI/repo/K4-L2-DAY11-NguyenVuQuangMinh-SVM/submission/40_decision_log.csv), [submission/45_sampling_plan.csv](file:///d:/AI/repo/K4-L2-DAY11-NguyenVuQuangMinh-SVM/submission/45_sampling_plan.csv), [submission/46_gold_set_plan.md](file:///d:/AI/repo/K4-L2-DAY11-NguyenVuQuangMinh-SVM/submission/46_gold_set_plan.md) |

Bảng này xác định vai của nhóm. Vòng QA tự sinh trong team.json thuộc quy trình nhiều hồ sơ của CLI; nhóm dùng một slice chung và quy trình A → B → C đã nêu trong hướng dẫn.

## 3. Bàn giao theo pha

| Mốc | Người giao → nhận | File / commit / mã khóa | Người nhận đã kiểm gì? | Trạng thái / vướng mắc |
|---|---|---|---|---|
| P0 · Chốt môi trường và vai | C (Điều phối) → A, B | `mode.json`, slice `B4-edge`, phân vai A=`minh`, B=`quan`, C=`thach` | Kiểm tra kết nối CVAT, cấu hình không gian ống kính fisheye, vị trí thân xe `ego_body` | Hoàn thành, môi trường chuẩn xác |
| P2 · Khóa bản đầu | A (Gán nhãn) → B, C | `r1_craft/annotations.xml`, `lock.txt` (mã khóa `C0E9-DE25`), 23 boxes, 14 polygons | B (QA/QC) kiểm tra 9 mục checklist `selfqc.md`, xác nhận tỷ lệ lấp đầy K12 (edge: 0.410, center: 0.682), không còn cảnh báo đỏ | Hoàn thành, đã khóa trước khi mở reference |
| P3 · Chốt QA/QC mù | B (QA/QC) → C, A | `r2_qa/qa_review.md`, 4 findings trong `findings.csv`, mã QA `B28A-2C10` | C (Điều phối) và A rà soát lỗi rule-based (R01 bỏ sót xe tải, R05 xe ba bánh bị cắt cụt, R02 điểm tiếp đất) | Hoàn thành, bảo đảm tính khách quan của QA/QC |
| P4 · Quyết định sửa | C (Điều phối) → A, B | `findings.csv` (27 dòng hợp lệ), `zone_table.md`, `40_decision_log.csv` (`DEC-01` đến `DEC-04`) | Điều phối phân tích đối soát WHAT/WHY, phát hiện ca xe Car `L10+M6` bị Reference R5 bỏ sót (`E0_reference_defect`), quyết định giữ nguyên box và leo thang ticket | Hoàn thành, đã mở ticket `30_escalation_ticket.md` |
| P5 · Kiểm bản sửa | A → B (QA/QC) → C | `rework/annotations-v2.xml`, `lock2.txt` (mã khóa `84DB-82C2`), `delta.md` | B (QA/QC) thực hiện QC kiểm chứng lại các box đã sửa, xóa bỏ box vi phạm ngưỡng $H<40\text{ px}$/nhiễu | Hoàn thành, đã khóa bản v2 |
| P6 · Chốt nộp | A, B → C (Điều phối) | `submission/manifest.json`, `lab11.py check` exit 0, commit chốt | C (Điều phối) kiểm tra toàn bộ 37 hồ sơ thành phần, không còn từ TODO, screenshots đầy đủ 2 ảnh | Hoàn thành sẵn sàng nộp |

## 4. Bất đồng và phối hợp

- **Một ca đã phân xử:** Frame `adasind_258420.jpg`, đối tượng `L10+M6` (Car tại tọa độ `[547.4, 462.1, 654.4, 535.0]`, zone `mid`, $H \approx 45\text{ px}$). Annotator A (Minh) và Model YOLO26m (`M6`) đều nhận diện và đóng hộp chính xác; QA/QC B (Quân) đồng thuận đối tượng có thật; tuy nhiên Teaching Reference `R5` lại bỏ sót hoàn toàn. Điều phối C (Thạch) chủ trì thảo luận và nhóm thống nhất không xóa box để chạy điểm theo reference sai mà bảo vệ tính toàn vẹn dữ liệu tự hành: phân loại `E0_reference_defect`, lập ticket [30_escalation_ticket.md](file:///d:/AI/repo/K4-L2-DAY11-NguyenVuQuangMinh-SVM/submission/30_escalation_ticket.md), ghi quyết định `DEC-04` trạng thái `escalated` trong [40_decision_log.csv](file:///d:/AI/repo/K4-L2-DAY11-NguyenVuQuangMinh-SVM/submission/40_decision_log.csv), và đề xuất Rule R12 trong [20_guideline_patch.md](file:///d:/AI/repo/K4-L2-DAY11-NguyenVuQuangMinh-SVM/submission/20_guideline_patch.md).
- **Ca còn mở:** Không còn ca tồn đọng chưa phân loại trên slice `B4-edge`; đang chờ phòng Data Ops tiếp nhận và cập nhật Teaching Reference cho các khóa đào tạo tiếp theo.
- **Đóng góp của A/B/C vào kế hoạch và exit ticket:**
  - Annotator A (Minh): Đóng góp kinh nghiệm thực tế về biến dạng hình học thấu kính fisheye vùng mép (`edge zone`), hiện tượng co giãn tỷ lệ người đi bộ sát mép kính và ranh giới `lens_border` trong [50_exit_ticket.md](file:///d:/AI/repo/K4-L2-DAY11-NguyenVuQuangMinh-SVM/submission/50_exit_ticket.md).
  - QA/QC B (Quân): Đóng góp tiêu chí thẩm định chất lượng độc lập, quy chuẩn nghiệm thu QC và quy định bằng chứng Seam Policy (Timestamp sync $\le 5\text{ ms}$, Extrinsic alignment $< 10\text{ cm}$) trước khi hợp nhất thực thể xuyên camera trong [46_gold_set_plan.md](file:///d:/AI/repo/K4-L2-DAY11-NguyenVuQuangMinh-SVM/submission/46_gold_set_plan.md).
  - Điều phối C (Thạch): Hoàn thiện ma trận phân bổ 200 frame cho 4 camera trong [45_sampling_plan.csv](file:///d:/AI/repo/K4-L2-DAY11-NguyenVuQuangMinh-SVM/submission/45_sampling_plan.csv) và phân tích bản chất định hướng rủi ro trong [45_review_plan.md](file:///d:/AI/repo/K4-L2-DAY11-NguyenVuQuangMinh-SVM/submission/45_review_plan.md).
- **Thay đổi phân công nếu có:** Phân công giữ nguyên theo kế hoạch ban đầu từ P0 đến P6, không có thay đổi phát sinh.

## 5. Xác nhận trước khi nộp

- [x] A xác nhận nhãn và export đúng phiên bản: Nguyễn Vũ Quang Minh / Khóa `lock.txt` (`C0E9-DE25`) và `lock2.txt` (`84DB-82C2`).
- [x] B xác nhận đã QA/QC độc lập trước reference và kiểm lại ca sửa: Quân / [qa_review.md](file:///d:/AI/repo/K4-L2-DAY11-NguyenVuQuangMinh-SVM/submission/r2_qa/qa_review.md) và đối soát nghiệm thu QC [delta.md](file:///d:/AI/repo/K4-L2-DAY11-NguyenVuQuangMinh-SVM/submission/rework/delta.md).
- [x] C xác nhận báo cáo đúng bản khóa, các file đầy đủ và check exit 0: Thạch / Xác nhận [manifest.json](file:///d:/AI/repo/K4-L2-DAY11-NguyenVuQuangMinh-SVM/submission/manifest.json) và lệnh `python3 lab11.py check` thoát exit code 0 (`✓ Hồ sơ hình thức đầy đủ`).
- [x] manifest.json tại commit chốt có failed_gates rỗng.
- [x] Repo nhóm Public, ảnh và các bằng chứng mở được.
- [x] C đã push và gửi link repo nhóm + commit qua kênh lớp công bố.

Chỉ đánh dấu việc đã kiểm thật. Nhóm nộp một hồ sơ chung; check không tự chấm đóng góp từng người. Giữ nguyên header/các cột enum của findings.csv; tên người được ghi trong tài liệu này hoặc phần note thích hợp.
