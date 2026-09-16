# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 29 skeleton, trung bình 15.93 khớp có v > 0 mỗi người
- Tổng: v=2 333 | v=1 129 | v=0 31

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 24 | 5 | 0 | 17% |
| 1 | left_eye | 19 | 10 | 0 | 34% |
| 2 | right_eye | 23 | 6 | 0 | 21% |
| 3 | left_ear | 11 | 18 | 0 | 62% |
| 4 | right_ear | 17 | 12 | 0 | 41% |
| 5 | left_shoulder | 24 | 5 | 0 | 17% |
| 6 | right_shoulder | 28 | 1 | 0 | 3% |
| 7 | left_elbow | 23 | 6 | 0 | 21% |
| 8 | right_elbow | 25 | 4 | 0 | 14% |
| 9 | left_wrist | 19 | 10 | 0 | 34% |
| 10 | right_wrist | 20 | 8 | 1 | 28% |
| 11 | left_hip | 18 | 10 | 1 | 34% |
| 12 | right_hip | 18 | 10 | 1 | 34% |
| 13 | left_knee | 17 | 7 | 5 | 24% |
| 14 | right_knee | 18 | 6 | 5 | 21% |
| 15 | left_ankle | 16 | 4 | 9 | 14% |
| 16 | right_ankle | 13 | 7 | 9 | 24% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
