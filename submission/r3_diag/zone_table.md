# Zone table (slice của bạn)

Lệnh `python3 lab11.py model` tự ghi bảng số (cùng cách đếm với `r1_craft/compare.md` và `model_compare.md`); chạy lại lệnh sẽ cập nhật bảng và giữ nguyên phần nhận xét. Bạn chỉ viết mục Nhận xét.

| Zone | n_ref | L missing | L spurious | M missing (`LR_noM` + `R_only`) | M thừa (`LM_noR` + `M_only`) | Lỗi L chính (`what`) |
|---|---:|---:|---:|---:|---:|---|
| center | 4 | 0 | 0 | 2 | 2 | — |
| mid | 9 | 1 | 3 | 2 | 7 | SPURIOUS (2) |
| edge | 7 | 0 | 0 | 1 | 1 | — |

## Nhận xét

- **Zone nào người (L) và model (M) gãy nhiều nhất, dẫn số ở bảng trên**:
  - Cả Người gán nhãn (L) và Mô hình (M) đều gặp nhiều lỗi nhất ở **vùng `mid`** ($0.35 \le r/R < 0.6$).
  - Cụ thể: Vùng `mid` chiếm toàn bộ lỗi của L với **1 missing** và **3 spurious** (trong khi ở `center` và `edge`, L đạt độ chính xác tuyệt đối với 0 missing và 0 spurious).
  - Đối với Mô hình (M): Vùng `mid` bị gãy nặng nhất với **2 missing** và tới **7 box thừa** (`LM_noR`=1, `M_only`=6), cao hơn hẳn so với `center` (thừa 2) và `edge` (thừa 1).

- **Giả thuyết vì sao và giới hạn của slice ba frame**:
  - **Giả thuyết nguyên nhân**:
    1. *Đặc trưng bối cảnh*: Vùng `mid` là nơi tập trung phần lớn mật độ giao thông ở khoảng cách tầm trung (tiêu biểu là frame `adasind_258420.jpg` có phố xá đông đúc, các đối tượng bị che khuất một phần lẫn nhau, ranh giới giữa xe và người phức tạp).
    2. *Lệch miền dữ liệu mô hình (Domain Shift)*: Mô hình YOLO26m đóng băng được huấn luyện trên ảnh phối cảnh phẳng (pinhole), khi gặp độ cong quang học fisheye ở vùng `mid` bắt đầu nhận diện sai nhiều cụm chi tiết nền thành vật thể (dẫn tới 7 false positives).
    3. *Sai lệch từ Reference (E0)*: Điển hình cặp `L10+M6` (chiếc Car ở hậu cảnh) cả Người và Model đều thấy (`LM_noR`) nhưng Reference lại bỏ sót, cho thấy một phần "lỗi thừa" thực chất là do chất lượng bản tham chiếu chưa hoàn hảo.
  - **Giới hạn của slice ba frame**:
    - Dữ liệu chỉ có 3 frame ($N_{ref} = 20$ vật thể), toàn bộ lỗi của L đều nằm tập trung ở đúng một frame phức tạp (`258420`). Kích thước mẫu quá nhỏ, không đủ tính đại diện thống kê để đánh giá toàn diện độ tin cậy của thuật toán hay gán nhãn viên trên toàn bộ hệ thống 4 camera SVM.
