# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Hoàng Công Tùng (2A202602229)    Ngày: 16/09/2026

## 1. Nhãn của tôi

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 28 |
| v=2 / v=1 / v=0 | 314 / 129 / 33 |
| Thời gian trung bình mỗi ảnh | 5-6 phút |

Ba khớp có `%v=1` cao nhất (hay bị che nhất):
1. left_ear (61%)
2. right_ear (46%)
3. left_wrist / left_hip (36%)

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

Có, đây đúng là những khớp khó nhất khi gán nhãn. **Tai** thường bị tóc, mũ hoặc góc chụp che khuất — nhiều ảnh chụp nghiêng khiến tai ở phía xa ống kính bị che hoàn toàn. **Hông** không bao giờ nhìn thấy được trực tiếp trên người mặc quần áo; tọa độ hông luôn là ước lượng giải phẫu, không có mốc bề mặt cụ thể để bám vào. **Cổ tay** dễ bị che khi người đang cầm đồ vật hoặc đặt tay sau lưng. Đây là sự khác biệt giữa "bị che" (còn trong khung, ước lượng được, đánh v=1) và "khó xác định vị trí giải phẫu chính xác" (hông không có mốc bề mặt nào nhìn thấy).

## 2. Chấm với gold

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.926 | 0.926 |
| OKS@0.50 | 0.966 | 0.966 |
| OKS@0.75 | 0.966 | 0.966 |
| Lỗi `dao_trai_phai` | 0 | 0 |
| Lỗi `nham_nguoi` | 0 | 0 |
| Lỗi `xoa_khop_bi_che` | 0 | 0 |

**Tôi đã sửa gì giữa hai lần chạy**:
- train_13.jpg — người thứ 1 — toàn bộ skeleton — Gán bổ sung bộ xương bị bỏ sót: gold có 3 người nhưng bài ban đầu chỉ gán 2 người. Người bị bỏ sót là người đứng khuất một phần sau người khác, khó phát hiện khi nhìn tổng thể. Sau khi thêm skeleton, điểm OKS không thay đổi vì người đó đã bị tính OKS = 0 trong lần đầu.

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?**

Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh. Trong bước warm-up ở ảnh train_02, bộ xương ban đầu bị thả theo chiều không khớp với cơ thể người, nhưng tôi đã phát hiện và tự sửa ngay bằng cách kéo từng điểm về đúng bên cơ thể trước khi tiếp tục gán các ảnh còn lại.

## 3. Kiểm chéo

Bạn cùng nhóm: Không có (thực hiện độc lập)

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm: Không áp dụng vì không có thành viên nhóm để so sánh.

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất: Sau khi tự kiểm tra bằng `check_pose_labels.py`, phát hiện một số khớp bị đánh v=0 sai (khớp vẫn trong ảnh nhưng bị che). Đã bổ sung luật: hông, đầu gối, cổ chân bị che bởi quần áo nhưng còn trong khung ảnh → bắt buộc dùng v=1 kèm ước lượng vị trí, không dùng v=0.

## 4. Model

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.8450 | 0.8450 | 0.0000 |
| pose_mAP50-95 | 0.6853 | 0.6908 | +0.0055 |
| pose_precision | 0.9734 | 0.9792 | +0.0058 |
| pose_recall | 0.8462 | 0.8462 | 0.0000 |
| box_mAP50-95 | 0.8119 | 0.8041 | -0.0078 |

### Trả lời năm câu hỏi ở cuối notebook

1. `pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?

`pose_mAP50-95` tăng nhẹ +0.0055 (từ 0.6853 lên 0.6908). Mức tăng rất nhỏ là hoàn toàn hợp lý vì 20 ảnh quá ít so với dataset COCO gốc (hàng trăm nghìn ảnh). Nhãn của tôi có tỉ lệ v=1 cao hơn gold (đặc biệt ở tai và hông), điều này giúp model học thêm cách ước lượng khớp bị che khuất, dẫn đến cải thiện nhỏ trên tập test. Precision cũng tăng nhẹ (+0.0058), cho thấy model ít đoán sai vị trí hơn sau fine-tune.

2. `box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm *khớp* dễ hơn? Vì sao?

`box_mAP50-95` = 0.8041 cao hơn hẳn `pose_mAP50-95` = 0.6908, chênh nhau 0.113 điểm. Model tìm *người* (bounding box) dễ hơn nhiều so với định vị *khớp* chính xác. Lý do: bounding box chỉ cần bao phủ đúng người trong ảnh, sai vài chục pixel vẫn được chấp nhận; còn tọa độ 17 khớp phải chính xác đến từng pixel, đồng thời model phải xử lý tốt các tình huống che khuất mà không có thông tin hình ảnh trực tiếp để học.

3. Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43:

Ảnh test_06: model gặp lỗi **lệch nhẹ** — các điểm khớp được đặt đúng vùng cơ thể nhưng lệch khỏi tọa độ giải phẫu chuẩn vài chục pixel, đặc biệt ở phần hông và đầu gối. Pose tổng thể vẫn nhận ra được nhưng chưa đủ chính xác. Đây là lỗi ít nguy hiểm nhất trong bốn loại, không gây model học ngược như lỗi đảo trái/phải.

4. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?

Ảnh test_06 có OKS thấp nhất giữa nhãn và model. Các khớp mà model đặt lệch (hông, đầu gối) là vị trí bị che khuất bởi quần áo — không có mốc bề mặt nào để model bám vào. Gold của COCO để v=0 cho các khớp này (không gán), trong khi nhãn của tôi dùng v=1 theo luật lớp. Tôi cho rằng nhãn của tôi phản ánh đúng quy tắc lab hơn; model học theo phân phối COCO nên ước lượng khớp bị che kém hơn so với kỳ vọng của bộ nhãn lớp mình dùng.

5. Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó nói gì về bức ảnh đó?

Ảnh tôi gán có OKS thấp nhất so với gold là train_11 (OKS = 0.861) và ảnh model đoán tệ nhất là test_06. Đây là hai ảnh khác nhau, cho thấy lỗi gán nhãn của tôi và lỗi của model không phát sinh từ cùng một nguyên nhân. Lỗi của tôi ở train_11 là lệch vị trí khớp cổ tay (lệch ~34px), trong khi lỗi của model ở test_06 đến từ tư thế người khó và khớp bị che nhiều. Điều này có nghĩa ảnh test_06 là ảnh khó khách quan với model, còn train_11 là ảnh tôi gán chưa cẩn thận đủ.

## 5. Một rule evidence bạn đã dùng

Ảnh train_01, người thứ 1, khớp `left_hip` (hông trái).

Bằng chứng nhìn thấy: Người phụ nữ mặc tạp dề màu đỏ thẫm che toàn bộ vùng bụng và hông. Không có điểm mốc bề mặt nào quan sát được (không thấy dây lưng quần, không thấy viền khớp hông). Tuy nhiên, phần đùi bên dưới và phần vai bên trên đều còn nhìn thấy rõ ràng và nằm trong ảnh — điều đó chứng minh chắc chắn hông đang ở bên trong khung hình, chỉ là bị tạp dề che khuất. Do đó tôi chọn v=1 (Occluded) và đặt chấm ước lượng tại vị trí giải phẫu học cho thấy hông người phải nằm ở đó: ngang bằng đỉnh xương chậu, hai bên cột sống. Nếu chọn v=0 sẽ loại khớp này khỏi phép tính OKS và vi phạm luật lớp quy định "bị che, còn trong khung → v=1, vẫn đặt chấm".
