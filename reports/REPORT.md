# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Hồ Minh Trí   Nhóm: T049   Ngày: 16/9/2026

## 1. Nhãn của tôi

<!-- Lấy số từ reports/visibility_report.md hoặc outputs/visibility_report.json sau Chặng 4.
Số ảnh phải là 20; số skeleton là tổng số người trong 20 ảnh. Thời gian trung bình = tổng
thời gian gán / 20. -->

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 29 |
| v=2 / v=1 / v=0 | 333 | 129 | 31 |
| Thời gian trung bình mỗi ảnh | 2.4p (+0.5 after rework) |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. `left_ear` 
2. `right_ear`
3. `left_eye`

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.
Các khớp không khó gán do đều là đặc điểm phần đầu, dễ xác định vị trí giải phẫu. Lý do có nhiều v=1 nhất tại các khớp này là vì trong dataset đã cho, nhiều hình ảnh chứa người xoay đầu theo các góc camera không bắt được các điểm nhận dạng, hoặc người đội mũ, có tóc che tai, dẫn đến các đặc điểm nêu trên hay bị che.

<!-- Trả lời 2–4 câu. Phân biệt “hay bị che” với “khó xác định vị trí giải phẫu”; nêu bằng
chứng nhìn thấy thay vì chỉ nêu cảm giác. -->

## 2. Chấm với gold

<!-- Lấy hai cột từ outputs/eval_vs_gold.json: một lần ngay khi protected release mở và một
lần sau rework. Đếm số phần tử trong từng danh sách lỗi, không tự làm tròn. -->

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.961 | 0.966 |
| OKS@0.50 | 0.966 | 1.000 |
| OKS@0.75 | 0.966 | 1.000 |
| Lỗi `dao_trai_phai` | 0 | 0 |
| Lỗi `nham_nguoi` | 0 | 0 |
| Lỗi `xoa_khop_bi_che` | 2 | 0 |

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

<!-- Mỗi dòng phải có: tên ảnh + người thứ mấy + keypoint + thao tác sửa. Không viết “đã sửa
lại một số lỗi”. -->

- train_13 người #1: thiếu người -> thêm pose cho người theo guidline 
- train_10 người #1: xóa khớp bị che [`left_hip`, `right_hip`] -> thêm khớp, chỉnh lại vị trí chấm cho đúng vị trí ước lượng của khớp

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ,
bạn nghĩ vì sao mình vẫn sai?

<!-- Nếu không có lỗi, ghi rõ “Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh.” -->

Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh.

## 3. Kiểm chéo

Bạn cùng nhóm: ______

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| | | | | |
| | | | | |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

<!-- Viết một rule kiểm chứng được: điều kiện nhìn thấy/căn cứ vị trí → chọn v=1 hoặc v=0.
Không chỉ ghi “cẩn thận hơn khi gán”. -->

-

## 4. Model

<!-- Chép số từ outputs/eval_model.json sau Chặng 6. “Chênh” = sau fine-tune trừ baseline;
đây là quan sát trên tập test, không phải chất lượng sản phẩm. -->

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.8450 | 0.8450 | +0.0000 |
| pose_mAP50-95 | 0.6853 | 0.6908 | +0.0055 |
| pose_precision | 0.9734 | 0.9792 | +0.0058 |
| pose_recall | 0.8462 | 0.8462 | +0.0000 |
| box_mAP50-95 | 0.8119 | 0.8041 | -0.0078 |

### Trả lời năm câu hỏi ở cuối notebook

> Mỗi câu cần trỏ tới ảnh/chỉ số cụ thể. Một con số thấp không tự chứng minh nhãn sai;
> kiểm lại bằng bằng chứng thị giác và kết quả gold.

1. `pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model
   điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?
   
   pose_mAP50-95 tăng từ 0.6853 lên 0.6908 (+0.0055). 
   Nếu pose_mAP50-95 giảm, điều đó sẽ có nghĩa là 20 ảnh fine-tune đang khiến model khớp keypoint kém chính xác hơn, đặc biệt ở các ngưỡng đánh giá chặt.

2. `box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm
   *khớp* dễ hơn? Vì sao?

   Sau fine-tune, tại box_mAP50-95 = 0.8041, pose_mAP50-95 = 0.6908, chênh nhau 0.1133. Điều này cho thấy trong bộ đánh giá đã cho, box detection có điểm cao hơn pose estimation -> model xác định người trong bbox dễ hơn việc xác định chính xác vị trí các khớp.

3. Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43
   (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn):

   Ảnh: test_02
   Lỗi: nhầm người - model predict bóng của con chim thành người

4. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?

   Ảnh có OKS thấp nhất: train_06
   Tôi đúng, do model không xác định được nửa người bên phải của người lãi xe do không đủ thông tin hình ảnh, dẫn đến bộ nhãn của tôi bị thừa nhiều khớp so với model. Tôi vẫn xác định vị trí các khớp khuất do theo context ảnh có thể thấy rõ ràng vị trí mũi chân phải của người, từ đó suy ra được vị trí khớp `right_ankle` và `right_knee`. Đồng thời từ vị trí `right_shoulder` và chiều rộng xe máy, có thể suy ra được khớp `right_elbow` và `right_wrist`. Còn lại các chấm mặt có thể tự ước tính theo giải phẫu mặt và góc xoay của đầu.  

5. Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó
   nói gì về bức ảnh đó?

   Không. Dữ liệu gold pre-rework cho thấy ảnh có vấn đề về annotation đáng chú ý không trùng với ảnh có OKS model-vs-nhãn thấp nhất.
   Ảnh có OKS trung bình thấp nhất là: train_13.jpg
   Do: Thiếu hẳn 1 người chưa được đánh nhãn
   Nếu có tồn tại 1 ảnh cả tôi và model đều đoán tệ nhất, điều đó có nghĩa là ảnh không rõ ràng, thiếu thông tin, hoặc cũng có thể ground-truth sai trong trường hợp người label.
   

## 5. Một rule evidence bạn đã dùng

Chọn một keypoint trong ảnh core mà bạn phải quyết định giữa `v=1` và `v=0`. Nêu ảnh, người,
khớp, bằng chứng nhìn thấy và lý do chọn trạng thái đó trong 3-5 câu.

<!-- Cấu trúc gợi ý: (1) train_XX + người thứ mấy + keypoint; (2) căn cứ thị giác như phần cơ
thể liền kề, trang phục hoặc vật che; (3) vì sao khớp còn trong khung (v=1) hay đã ra khỏi
khung (v=0). -->

train_11 / person 235 / [`right_knee`, `left_knee`]
Dù trong ảnh, phần khớp có thể vẫn nằm trong khung hình, tuy nhiên không đủ bằng chứng từ các bộ phận liền kề, như phần hông, đùi, để xác định được vị trí ước lượng của khớp. Do đó thay vì ước tính vị trí và đánh v=1, tôi quyết định đánh v=0.
