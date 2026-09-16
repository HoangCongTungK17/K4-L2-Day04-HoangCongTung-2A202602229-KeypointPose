# Reviewer checklist - điền khi kiểm bài người khác

Người gán: Hoàng Công Tùng   Người kiểm: Hoàng Công Tùng (tự kiểm)   Ngày: 16/09/2026

> **Ghi chú:** Bài thực hiện độc lập, không có bạn cùng nhóm. Checklist dưới đây phản ánh kết quả tự kiểm tra bài của chính mình.

Chạy trước khi soi bằng mắt:

```bash
python3 tools/check_pose_labels.py --images dataset/images/train --labels dataset/labels/train
python3 tools/visualize_pose.py --images dataset/images/train --labels dataset/labels/train --out outputs/vis_train
python3 tools/visibility_report.py --labels dataset/labels/train --out outputs/visibility_report.json
```

| | Mục kiểm | Đạt? | Ghi chú / ảnh nào |
| --- | --- | --- | --- |
| 1 | Mọi người trong ảnh đều có đủ 17 điểm, không ai bị thiếu | ✅ | Đủ 17 điểm cho tất cả 28 skeleton sau rework. train_13 ban đầu thiếu 1 người, đã bổ sung. |
| 2 | Bật đường nối: không có xương nào cắt chéo ở vai hoặc hông | ✅ | Đã kiểm qua visualize_pose.py. Phát hiện và sửa chiều xương ở train_02 ngay từ bước warm-up. |
| 3 | Không có xương nào kéo dài sang một cơ thể khác | ✅ | check_pose_labels.py không báo lỗi nham_nguoi. |
| 4 | Khớp bị che dùng `v = 1` **và có chấm**, không phải `v = 0` | ✅ | Sau rework: đã sửa 4 cảnh báo v=0 sai ở train_01, train_04, train_13. |
| 5 | `v = 0` chỉ xuất hiện ở khớp thật sự ra ngoài mép ảnh | ✅ | Đã rà soát lại toàn bộ sau khi đọc cảnh báo từ check_pose_labels.py. |
| 6 | Không có dấu hiệu dùng `Hidden` (điểm `v = 2` nằm ở chỗ vô lý) | ✅ | Không sử dụng phím `h` trong toàn bộ quá trình gán nhãn. |
| 7 | Export đúng **COCO Keypoints 1.0**: mảng `keypoints` có 51 số mỗi người | ✅ | File person_keypoints_default.json xuất từ CVAT đúng định dạng COCO Keypoints 1.0. |
| 8 | Bản YOLO Pose: mỗi dòng 56 số, `kpt_shape: [17, 3]` | ✅ | check_pose_labels.py báo "ĐẠT định dạng", 0 lỗi. |
| 9 | Visibility report đã nộp, và hai bảng đã được đặt cạnh nhau | ✅ | reports/visibility_report.md đã có. Không có bạn nhóm để so sánh chéo. |
| 10 | Mọi ca không rõ đều được ghi trong `GUIDELINE_MINI.md` | ✅ | Ghi đủ 3 ca: train_02 (đầu quay lưng), train_10 (người bị cắt mép ảnh), train_03 (hai người chồng nhau). |
| 11 | `check_pose_labels.py` chạy 0 lỗi | ✅ | Kết quả: "ĐẠT định dạng. 0 lỗi." sau khi sửa toàn bộ cảnh báo. |

## Lỗi tìm được

Chép sang `reports/review_partner.md`. Mỗi dòng một lỗi, đủ bốn cột - người sửa phải
mở đúng chỗ đó được mà không cần hỏi lại.

| Ảnh | Người thứ | Khớp | Lỗi gì | Sửa thế nào |
| --- | ---: | --- | --- | --- |
| train_01.jpg | 1 | left_knee, right_knee, left_ankle, right_ankle | v=0 sai (khớp trong ảnh bị đánh Outside) | Đổi v=1, đặt chấm ước lượng |
| train_04.jpg | 2 | left_knee, right_knee, left_ankle, right_ankle | v=0 sai (tương tự train_01) | Đổi v=1, đặt chấm ước lượng |
| train_13.jpg | 1 | Toàn bộ skeleton | Thiếu hẳn một người | Gán bổ sung skeleton thứ 3 |

## Hai câu kết luận

- Lỗi lặp đi lặp lại nhiều nhất của bài này: Dùng v=0 cho khớp đáng lẽ phải là v=1 (xảy ra ở các khớp chân khi người đứng gọn trong ảnh nhưng khớp bị che bởi quần áo).
- Nó là lỗi **guideline chưa rõ** — ban đầu chưa phân biệt rõ "bị che" (v=1) với "ra ngoài mép ảnh" (v=0). Đã hiểu và sửa đúng sau khi đọc output của check_pose_labels.py.
