# Review bài của bạn cùng nhóm

Người gán được review: Không có (bài thực hiện độc lập, không có bạn cùng nhóm trong phiên lab này)

Người review: Hoàng Công Tùng (2A202602229)

Ngày: 16/09/2026

---

> Mục này không áp dụng vì bài thực hành được thực hiện độc lập. Không có bạn cùng nhóm để kiểm chéo trong phiên lab ngày 4.
>
> Thay vào đó, tôi đã tự thực hiện kiểm chéo với chính nhãn của mình qua 3 lượt:
> - **Lượt 1 (hình dáng):** Chạy `visualize_pose.py` kiểm tra xương cắt chéo, phát hiện và sửa chiều bộ xương ở train_02 ngay từ bước warm-up.
> - **Lượt 2 (đếm):** Chạy `check_pose_labels.py` và `visibility_report.py`, phát hiện 4 cảnh báo v=0 sai ở train_01, train_04, train_13.
> - **Lượt 3 (phóng to):** Zoom 200% kiểm tra 3 người có cảnh báo, xác nhận và sửa lại cờ visibility cho đúng luật lớp.

## Lỗi tìm được (trong bài của chính mình, qua tự kiểm)

| Ảnh | Người thứ | Khớp | Lỗi gì | Sửa thế nào |
| --- | ---: | --- | --- | --- |
| train_01.jpg | 1 | left_knee, right_knee, left_ankle, right_ankle | v=0 sai: các khớp này nằm gọn trong ảnh nhưng bị đánh Outside | Đổi thành v=1 và đặt chấm ước lượng đúng vị trí |
| train_01.jpg | 2 | left_knee, right_knee, left_ankle, right_ankle | v=0 sai: tương tự người số 1 | Đổi thành v=1 và đặt chấm ước lượng đúng vị trí |
| train_04.jpg | 2 | left_knee, right_knee, left_ankle, right_ankle | v=0 sai: người đứng gọn trong ảnh nhưng khớp chân bị đánh Outside | Đổi thành v=1 và đặt chấm ước lượng |
| train_13.jpg | 1 | Toàn bộ skeleton | Thiếu hẳn một người — gold có 3 người, chỉ gán 2 | Gán bổ sung bộ xương thứ 3 cho người bị bỏ sót |

## Hai câu kết luận

- Lỗi lặp đi lặp lại nhiều nhất của bài này: **Dùng v=0 (Outside) cho khớp đáng lẽ phải là v=1 (Occluded)** — xảy ra chủ yếu ở các khớp chân (đầu gối, cổ chân) khi người đứng gọn trong ảnh nhưng phần chân bị che bởi quần áo hoặc góc nhìn.
- Nó là lỗi **guideline chưa rõ** — ban đầu tôi chưa phân biệt rõ ràng giữa "khớp không nhìn thấy vì bị che" (v=1) và "khớp ra ngoài mép ảnh" (v=0). Sau khi đọc cảnh báo từ `check_pose_labels.py` và đối chiếu lại luật lớp, tôi đã hiểu và sửa đúng. Đây không phải lỗi thao tác mà là lỗi hiểu nhầm định nghĩa ban đầu.
