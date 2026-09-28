# Đề xuất gold set theo camera — tình huống giả lập

**Đầu bài:** 50.000 frame từ bốn camera SVM, ngân sách chọn 200 frame để review/gold. Đây là tình huống trên slide,
**không phải** 50.000 frame có trong repo. Phân bổ đúng 200 ở `45_sampling_plan.csv` cho bốn camera, mỗi camera có
normal và hard slice. “Gold set” ở đây là **kế hoạch tạo** reference sau kiểm chứng, không phải teaching reference
ADASIND hoặc nhãn vừa vẽ. Nếu cần, dùng `notebooks/day11-svm360-colab.ipynb` để thử tổng phân bổ; notebook
không làm thay phần lý do.

| camera_id | Hard case cần chọn | Vì sao dễ sai | Annotation space / calibration cần giữ | Cách review trước khi gọi là gold |
|---|---|---|---|---|
| front | Xe cắt đầu ở cự ly $<2\text{ m}$ (cut-in), người đi bộ băng cắt nhanh ngang nắp capo, ánh sáng ngược nắng cực mạnh (sun glare) | Biến dạng thấu kính fisheye cực đại ở cự ly gần; bóng đổ nắp capo trùng với bóng xe; lóa sáng làm mờ chân người và vệt tiếp đất của lốp xe | Không gian pixel ảnh fisheye gốc $[u, v]$, thông số nội vi $K$, vector méo thấu kính $[k_1, k_2, k_3, k_4]$, tư thế ngoại vi $[R \mid T]$ so với tâm cầu xe, và ranh giới `lens_border` chuẩn | Hai Senior Annotator gán nhãn mù độc lập (blind annotation). Nếu IoU $\ge 0.85$ tự động chấp thuận; nếu có xung đột, Technical Lead đối soát với đám mây điểm 3D LiDAR/Bird's-Eye-View |
| rear | Chướng ngại vật thấp sát cản sau ($<1.5\text{ m}$), trẻ em/động vật nhỏ rơi vào điểm mù góc thấp, vạch ô đỗ bị trầy xước kết hợp bóng nước phản chiếu | Camera lùi hướng chúi xuống mặt đường làm biến dạng vật thể thấp theo phương ngang; các vệt lóa đèn lùi trên sàn bãi đỗ dễ bị gán nhầm thành `parking_line` | Hệ tọa độ fisheye nguyên bản, ma trận homography mặt phẳng đất phẳng ($H_{ground}$) để xác định đáy box tiếp đất thực tế và ranh giới `free_space` sát cản sau | QA chuyên gia rà soát chéo độc lập với log cảm biến siêu âm (ultrasonic proximity sensors); chỉ chốt Gold khi đáy bounding box tiếp xúc chuẩn xác với mặt phẳng đỗ |
| left | Xe máy luồn lách sát gương sườn trái, vật thể di chuyển cắt ngang đường ghép mí giữa camera trước và camera hông (front-left seam line) | Thân phương tiện bị phân tách qua hai góc nhìn quang học khác nhau; vật thể bị kéo giãn hình học theo phương tiếp tuyến của vành thấu kính fisheye | Tọa độ pixel ảnh gốc, ma trận đồng bộ thời gian microsecond (Timestamp Synchronization Matrix) và mặt nạ vùng ghép mí (Seam Mask Coordinates) | Hiển thị đồng thời hai màn hình: ảnh thấu kính đơn và ảnh ghép toàn cảnh 360 Top-View; kiểm tra tính liên tục hình học 3D trước khi phê duyệt Gold |
| right | Người đi bộ hoặc xe đạp băng ra từ ngõ khuất góc phải khi xe chuẩn bị rẽ; vạch ô đỗ xe bị che khuất bởi bồn cây hoặc bó vỉa hè | Tỷ lệ che khuất cao ($>50\%$), ranh giới vỉa hè gồ ghề dễ gây nhầm lẫn với diện tích trống di chuyển được nếu không xác định đúng `free_space` | Tọa độ ảnh fisheye gốc, bảng căn chỉnh thấu kính gương phụ, vector pháp tuyến mặt phẳng đường địa phương | Thẩm định chéo giữa 2 QA chuyên gia độc lập; đối chiếu với bản đồ vạch kẻ đỗ xe độ phân giải cao (HD Map) để loại trừ triệt để sai sót |

- **Khi nào cần refresh gold set (đổi camera, calibration hoặc rule):**
  1. *Thay đổi phần cứng/cảm biến:* Khi thay đổi model camera, thay ống kính có trường nhìn (FOV) khác, hoặc thay đổi vị trí/góc nghiêng ngàm lắp đặt camera trên thân xe.
  2. *Cập nhật hiệu chuẩn (Re-calibration):* Khi xe được bảo dưỡng, căn chỉnh lại hình học hệ thống (Rig Recalibration) khiến ma trận nội vi/ngoại vi thay đổi.
  3. *Cập nhật quy chế gán nhãn (Guideline Revision):* Khi ban hành phiên bản quy tắc mới (ví dụ v1.1.0 bổ sung Rule R12 về ngưỡng che khuất xa, hoặc điều chỉnh định nghĩa class/vùng ignore).
  4. *Trôi dạt phân phối dữ liệu (Data Drift):* Khi xe vận hành trong miền môi trường hoạt động mới (ví dụ sang mùa mưa tuyết ngập lụt, công trường gồ ghề) làm thay đổi căn bản đặc tính quang học của hình ảnh.

- **Một ca seam/cross-camera cần policy và evidence trước khi ghép hai box:**
  - *Tình huống:* Một xe máy di chuyển ở góc $45^\circ$ sườn trước bên trái, một phần đầu xe nằm trong camera Front và thân/đuôi xe nằm trong camera Left.
  - *Bằng chứng (Evidence) bắt buộc trước khi ghép:*
    1. Bằng chứng đồng bộ thời gian: Hai frame trích xuất từ 2 camera có độ lệch thời gian $\le 5\text{ ms}$ (được xác thực bởi hardware trigger timestamp).
    2. Bằng chứng hình học 3D (Extrinsic Consistency): Đám mây điểm hoặc tia chiếu từ hai tâm quang học giao nhau tại cùng một tọa độ thực tế trong không gian xe với độ sai lệch vị trí $< 10\text{ cm}$.
  - *Quy tắc (Policy):* Trên từng camera đơn lẻ, mỗi camera chỉ vẽ bounding box cho phần nhìn thấy trong trường nhìn của mình và bật cờ `truncated="true"`, tuyệt đối không vẽ box dự đoán vượt ra ngoài biên nhìn thấy. Ở tầng hợp nhất dữ liệu (360 Fusion Top-View), hệ thống chỉ liên kết thành một track ID duy nhất khi thỏa mãn đầy đủ hai bằng chứng trên.

- **Vì sao peer agreement hoặc quality report trên ảnh một camera chưa chứng minh gold set đúng cho cả bốn camera:**
  - Chỉ số thỏa thuận ngang hàng (Peer Agreement) cao hay điểm `local-quality` cao trên một camera đơn lẻ (như lát cắt `B4-edge`) chỉ chứng minh các annotator hiểu giống nhau về các quy tắc 2D cục bộ trên một mặt phẳng quan sát nhất định.
  - Điều này **hoàn toàn chưa chứng minh được** Gold Set đúng cho hệ thống 4 camera SVM vì:
    1. Thiên kiến hệ thống chung (Common Systematic Bias): Cả hai annotator có thể cùng bỏ sót một lỗi do thấu kính hoặc cùng hiểu sai một ca che khuất (như thực tế ca xe Car `L10+M6` bị Reference R5 bỏ sót).
    2. Bất tương thích hình học toàn cảnh: Một bounding box được gán nhãn rất đẹp trên ảnh camera sườn có thể vi phạm chiều cao hình học hoặc điểm tiếp đất khi chiếu sang tọa độ của camera trước.
    3. Hệ thống SVM là sự kết hợp của 4 luồng thị giác 360 độ. Đánh giá cục bộ trên một camera không thể phát hiện các lỗi sai lệch thời gian, lỗi ghép mí và sự đứt gãy thực thể xuyên camera.
