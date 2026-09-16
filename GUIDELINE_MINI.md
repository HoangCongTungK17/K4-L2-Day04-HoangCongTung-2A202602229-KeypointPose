# Mini guideline - nhóm: Độc lập  |  người gán: Hoàng Công Tùng  |  ngày: 16/09/2026

> Điền file này **trong lúc** gán nhãn, không phải sau khi xong. Mỗi lần bạn dừng lại
> hơn 10 giây để phân vân, đó là một dòng phải ghi vào đây.

## 1. Luật bắt buộc (đã thống nhất cả lớp - không sửa)

- Bộ 17 điểm COCO, đúng tên, đúng thứ tự. Lấy từ file `.SVG` chung.
- Mọi người trong ảnh đều có **đủ 17 điểm**. Điểm không dùng được thì gắn cờ, không xoá.
- Trái/phải tính theo **cơ thể người**, không theo bức ảnh.
- Bị che, còn trong khung -> `v = 1`, **vẫn đặt chấm** ở vị trí ước lượng.
- Ra ngoài mép ảnh -> `v = 0`, **không** đặt chấm.
- Không dùng `Hidden` (`h`) - nó không được lưu vào file.

## 2. Luật của nhóm bạn

| Tình huống | Luật nhóm bạn chọn | Vì sao |
| --- | --- | --- |
| Hông của người mặc quần áo dài | v=1, đặt chấm ước lượng theo giải phẫu (ngang đỉnh xương chậu, cách cột sống ~15-20cm mỗi bên) | Hông luôn nằm trong khung trừ khi người bị cắt từ hông trở xuống. Dù không có mốc bề mặt nào, vẫn phải ước lượng theo vị trí giải phẫu. |
| Tai bị tóc hoặc mũ bảo hiểm che một phần | v=1, đặt chấm ở vị trí tai thực theo giải phẫu dù tóc/mũ che khuất bề mặt tai | Tai vẫn nằm trong khung, chỉ bị che bởi tóc hoặc vật phụ. Căn cứ vào vị trí đầu, mắt và hàm để ước lượng chính xác hơn. |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | Hông, đầu gối, cổ chân: v=0 (ra ngoài mép ảnh, không đặt chấm). Vai, khuỷu tay, cổ tay: tuỳ theo từng bên có bị cắt không. | Phần thân bị lưỡi kéo mép ảnh cắt đi hoàn toàn không thể ước lượng vị trí. Áp dụng quy tắc "ra ngoài mép ảnh → v=0" nghiêm ngặt. |
| Cổ tay nằm sau tay lái / sau thân mình | v=1, đặt chấm ước lượng tại vị trí cổ tay dựa vào hướng của cẳng tay | Cổ tay vẫn nằm trong khung hình, chỉ bị che bởi vật thể hoặc phần thân khác. Kéo dài đường thẳng từ khuỷu tay qua cẳng tay để ước lượng vị trí cổ tay. |
| Hai người chồng lên nhau | Gán xong hoàn toàn người phía trước rồi mới sang người phía sau. Khớp của người sau bị người trước che → v=1, ước lượng theo giải phẫu. | Tránh lỗi "nhầm người" (đặt điểm của người này sang cơ thể người kia). Làm xong một người, khoá lại, mới sang người tiếp theo. |
| Người nhỏ đến mức nào thì không gán nữa | Gán tất cả người có thể phân biệt được ít nhất vai và hông trong ảnh (kể cả người nhỏ ở góc ảnh) | Bộ dữ liệu này đã được chọn lọc, mọi người trong ảnh đều đủ lớn để gán nhãn. Nếu phân vân → gán, vì bỏ sót người bị trừ điểm nặng hơn gán không hoàn hảo. |

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `train_02`, người thứ `1`, khớp `nose / left_eye / right_eye / left_ear / right_ear`

- **Mơ hồ ở chỗ nào:** Người trong ảnh đứng quay lưng hoàn toàn về phía ống kính. Toàn bộ khuôn mặt bị che bởi chính gáy và tóc của người đó. Không có bất kỳ đường viền nào của mắt, mũi, tai có thể nhìn thấy.
- **Bạn quyết thế nào:** Đánh v=1 (Occluded) cho tất cả 5 điểm trên mặt. Đặt chấm ước lượng "xuyên thấu qua gáy" — mũi ở chính giữa gáy, hai mắt hai bên mũi ngang tầm mắt, hai tai ở rìa đầu.
- **Vì sao:** Các điểm này vẫn hoàn toàn nằm bên trong khung hình (không bị cắt ra ngoài mép ảnh), chỉ bị che bởi gáy và tóc của chính người đó. Theo luật lớp: bị che + còn trong khung = v=1, bắt buộc ước lượng.
- **Nếu người khác quyết ngược lại (đánh v=0):** Model sẽ học rằng người quay lưng thì không có mặt → bỏ qua hoàn toàn vùng đầu khi người đứng quay lưng. Nếu sau này model gặp ảnh chụp phía sau người trong nhóm đông người, nó sẽ không phát hiện được khuôn mặt tiềm ẩn đó.

### Ca 2 - ảnh `train_10`, người thứ `1`, khớp `left_ear / left_knee / left_ankle / right_ankle`

- **Mơ hồ ở chỗ nào:** Người bị cắt ở mép ảnh bên trái — không nhìn thấy tai trái và một phần chân trái. Khó xác định: phần nào "bị che trong ảnh" (v=1) và phần nào "đã ra ngoài mép ảnh hoàn toàn" (v=0).
- **Bạn quyết thế nào:** Tai trái: v=0 (đã bị mép ảnh cắt mất, không còn trong khung). Đầu gối và cổ chân: xem kỹ mép ảnh — nếu phần chân đó đã lọt ra ngoài viền ảnh thì v=0, nếu còn trong ảnh dù bị che thì v=1.
- **Vì sao:** Tiêu chí quyết định duy nhất là "điểm đó còn trong ranh giới pixel của ảnh không?". Không phải "tôi có nhìn thấy không?" mà là "nó có tồn tại trong khung không?".
- **Nếu người khác quyết ngược lại (đánh v=1 cho tai):** Model sẽ học cách đoán tọa độ của khớp ra ngoài mép ảnh — điều không bao giờ có dữ liệu thực tế để kiểm chứng. Dữ liệu huấn luyện sẽ bị nhiễu bởi các tọa độ giả tưởng.

### Ca 3 - ảnh `train_03`, người thứ `1 và 2`, khớp `left_hip / right_hip`

- **Mơ hồ ở chỗ nào:** Hai người đứng gần nhau, một người che một phần thân người kia. Hông của người đứng phía sau bị che bởi người đứng trước. Khó xác định hông đang ở chỗ nào vì không có mốc nào quan sát được (quần áo che + người khác che thêm).
- **Bạn quyết thế nào:** v=1 cho cả hai bên hông của người đứng sau, đặt chấm ước lượng dựa vào vị trí vai và đùi của người đó để tính vị trí hông theo tỉ lệ giải phẫu chuẩn.
- **Vì sao:** Dù bị che hai lớp (quần áo + người khác), hông vẫn chắc chắn nằm trong khung hình vì phần thân trên và chân của người đó đều nhìn thấy. Chấm lệch vài chục pixel (lệch nhẹ) vẫn tốt hơn bỏ trống (vi phạm luật lớp).
- **Nếu người khác quyết ngược lại (đánh v=0):** Model học rằng khi hai người chồng lên nhau, hông người phía sau không tồn tại → bỏ qua hoàn toàn khớp đó trong tình huống đông người. Đây là lỗi nghiêm trọng vì trong thực tế đám đông, hầu hết các khớp đều bị người khác che một phần.

## 4. Sau khi so visibility report với bạn cùng nhóm

- Khớp lệch `%v=1` nhiều nhất: Không áp dụng (thực hiện độc lập, không có bạn cùng nhóm)
- Nguyên nhân là **guideline chưa rõ** hay **một trong hai bên gán sai**: Không áp dụng
- Luật mới bổ sung vào mục 2 sau khi thống nhất: Sau khi tự kiểm tra bằng `check_pose_labels.py`, phát hiện sai sót ở train_01, train_04, train_13 — đã bổ sung luật rõ ràng hơn: hông/đầu gối/cổ chân bị quần áo che nhưng còn trong ảnh → bắt buộc v=1, không được dùng v=0.
