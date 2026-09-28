# Error analysis card

## Zone × block

| zone | block | what | count |
|---|---|---|---:|
| center | B4 | MISSING | 2 |
| center | B4 | SPURIOUS | 2 |
| center | C0 | MISSING | 1 |
| edge | B2 | ATTRIBUTE | 2 |
| edge | B4 | MISSING | 1 |
| edge | B4 | SPURIOUS | 1 |
| mid | B2 | BOX_GEOMETRY | 1 |
| mid | B4 | BOX_GEOMETRY | 1 |
| mid | B4 | MISSING | 2 |
| mid | B4 | SPURIOUS | 11 |
| mid | C0 | MISSING | 2 |
| unknown | B2 | MISSING | 1 |

## Top defects
- SPURIOUS: 14 (ví dụ frame adasind_258420.jpg)
- MISSING: 9 (ví dụ frame adasind_102750.jpg)
- ATTRIBUTE: 2 (ví dụ frame adasind_069450.jpg)

## Phân tích của bạn

Hai bảng trên do `python3 lab11.py card` tính từ `findings.csv`; chạy lại lệnh sẽ cập nhật bảng và giữ nguyên mục này. Viết cho lỗi nổi bật nhất, dẫn frame/`object_ref`.

- **Nguyên nhân khả dĩ (`why`) và vì sao bạn nghĩ vậy**:
  - Lỗi nổi bật nhất là **SPURIOUS** (14 ca) và **MISSING** (9 ca), tập trung chủ yếu ở **vùng `mid`** (11 ca SPURIOUS trên block B4, frame tiêu biểu `adasind_258420.jpg`).
  - Đối với Mô hình YOLO26m: Nguyên nhân chính là **`E4_model_domain`** do sự lệch miền dữ liệu (Domain Shift). Mô hình được huấn luyện trên ảnh phối cảnh phẳng (pinhole perspective), khi suy luận trực tiếp trên ảnh mắt cá (fisheye) chưa qua nắn chỉnh (undistort), các đặc trưng ở vùng trung gian và rìa bị méo phi tuyến, khiến mô hình nhầm lẫn bóng xe, biển hiệu, bụi cây ven đường thành phương tiện (sinh ra hàng loạt box giả M2, M4, M5, M7, M8, M10, M11).
  - Đối với Annotator: Xuất hiện lỗi **`E1_annotator_error`** trên `adasind_258420.jpg` (L1, L3) do nhận định sai ranh giới của các đối tượng bị che khuất ở cự ly xa hoặc box nhỏ dưới ngưỡng $H=40\text{ px}$.
  - Đối với Reference: Có ca **`E0_reference_defect`** trên frame `258420` (`L10+M6`), một chiếc ô tô con (`Car`) ở hậu cảnh mà cả Người gán nhãn L và Mô hình M đều phát hiện rõ ràng nhưng bản Reference R lại bỏ sót.

- **Cách sửa và ai nhận việc (`owner`)**:
  - Với lỗi model domain (`E4_model_domain`): Giao cho **`ai_team`** thu thập thêm dữ liệu huấn luyện fisheye đa dạng và tinh chỉnh (fine-tune) mô hình trực tiếp trên miền fisheye hoặc áp dụng spatial transformation / deformable convolution thích ứng với biến dạng góc rộng.
  - Với lỗi gán nhãn (`E1_annotator_error`): Giao cho **`annotator`** thực hiện rework trên CVAT, đo đạc kỹ chiều cao pixel theo quy tắc R01 ($H \ge 40\text{ px}$) trước khi tạo box.
  - Với lỗi reference (`E0_reference_defect`): Giao cho **`data_ops`** thẩm định lại mẫu gán nhãn chuẩn qua quy trình multi-annotator consensus để cập nhật gold standard.

- **Bằng chứng (ảnh trong `screenshots/`, dòng findings, rule)**:
  - Bằng chứng hình ảnh: Xem ảnh chụp đối chiếu so sánh mô hình `submission/screenshots/model_compare_mid.png` và overlay `submission/screenshots/escalation_adasind_258420.png`.
  - Dòng findings: Các dòng `r3_diag` trên frame `adasind_258420.jpg` (`L10+M6` với `E0_reference_defect`, các dòng `M_only` với `E4_model_domain`).
  - Quy tắc liên quan: Quy tắc **R01** (Ngưỡng chiều cao $H \ge 40\text{ px}$) và **R02** (Vẽ bám sát ảnh gốc fisheye).
