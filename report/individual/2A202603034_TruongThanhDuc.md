# Báo cáo cá nhân — HoVaTen-MSSV

Đổi tên file theo quy tắc `MSSV_HoTen.md`. Ví dụ: `20241234_NguyenVanAn.md`.

Dữ liệu: 12 ảnh người COCO val2017 (Lin et al., 2014) và 8 ảnh chân dung Wikimedia Commons, chỉ dùng trong lớp.

## 0. Thông tin học viên

- Họ tên: Trương Thành Đức 
- MSSV: 2A202603034
- Email: 26ai.ductt2@vinuni.edu.vn
- Link repository:https://github.com/thanhduc270998/K5-L2A-DAY04-TruongThanhDuc-2A202603034-KeypointPose.git
- Phiên bản CVAT (Help → About):2.74.1
- Máy: hệ điều hành Windows 10 · RAM 12 GB · chạy CVAT chế độ đầy đủ
- Bộ ảnh (`src/exports/assignment.json` do `lab.py setup` tạo): có
- Giờ bắt đầu:
- Giờ kết thúc:

## 1. Kiểm tra label (CP0)

| Label | Kiểu | Số điểm | Tên điểm đúng bảng của lab? | Đúng thứ tự? | Số cạnh |
|---|---|---:|---|---|---:|
| `person` | Skeleton |17 | Có | Có | 19 |
| `face` | Skeleton |13 | Có | Có | 13 |

Mẫu `person` dựng theo người quay mặt về camera (các điểm `left_*` nằm bên phải khung mẫu)? Có

Cách đổi cờ `occluded` / `outside` cho **một điểm** trên phiên bản CVAT này (một câu):

Để đổi cờ occluded hoặc outside cho một điểm riêng lẻ trong Skeleton trên CVAT, chỉ cần nhấp chuột phải trực tiếp vào điểm đó (hoặc di chuột qua và nhấn phím tắt Q cho occluded, O cho outside) rồi bật/tắt biểu tượng tương ứng

## 2. Số lượng đã gán và thời gian (CP1, CP2)

| Nhóm ảnh | Số ảnh | Số skeleton đã gán | Số người dưới 60 px (không gán) | Số poster / gương / màn hình bỏ qua | Thời gian (phút) |
|---|---:|---:|---:|---:|---:|
| `easy` | 5 | 5 | 2 | 1 | |
| `occluded` | 3 | 6 | 0 | 1 | |
| `back` | 2 | 4 | 1 | 0 | |
| `small` | 1 | 2 | 10 | 0 | |
| `crowd` | 1 | 3 | 2 | 0 | |
| `faces` | 8 | 8 | — | 0 | |

- Số ảnh người đã xong ở mốc phút 105: 10 / 10
- Thời gian thực tế (phút): CP1 _85_ · CP2 _26_ · CP3 _15_
- Ảnh tốn thời gian nhất là ảnh nào, vì sao?

Ảnh tốn thời gian nhất là ảnh `000000253584.jpg` do ban đầu tôi không biết cách đo pixel người trong ảnh nên tôi đã đánh skeleton cho 7 người nhìn thấy được trong bức ảnh , nhưng khi tôi chạy file check kết quả thì tôi nhận thấy mình đã không tuân thủ việc không đánh skeleton cho người dưới 60px (5 người). Sau khi biết mình mắc sai lầm thì tôi đã xóa đi và kiểm tra lại tốn khoảng 15 phút

## 3. Bảng ca khó (CP1, CP2)

Mỗi điểm bạn phân vân một dòng, nhất là phân vân giữa `v = 1` và `v = 0`, hoặc không chắc hướng người.

| Ảnh | Người / mặt | Điểm | Cờ đã chọn | Phân vân giữa | Lý do chọn |
|---|---|---|---:|---|---|
| `000000056651.jpg` | Người phụ nữ mặc áo kẻ ở giữa (`#2`) | `right_wrist` | `v = 0` | `v = 1` và `v = 0` | Cổ tay phải bị che khuất phía sau bánh sinh nhật / khay bánh; khi thử đặt `v = 1` theo vị trí dự đoán (`khoanh tay` sau bánh) thì script kiểm tra cảnh báo cặp `wrist` ngược phía so với các cặp còn lại, đồng thời không ước lượng chắc chắn được tâm khớp bị che nên chuyển sang `v = 0` theo quy tắc không đoán bừa. |
| `000000311497.jpg` | Người phụ nữ áo tím (`#2`) | `left_knee`, `right_knee`, `left_ankle`, `right_ankle` | `v = 2` | `lr_swap` hay chân bắt chéo thật | Nhìn từ góc nghiêng 3/4 sang trái, chân trái bước lên trước bắt chéo qua phía trước chân phải (chân trước có băng gối chồng lên đùi sau và bàn chân lộ má ngoài bên trái, chân sau lùi về phải), ngược phía ngang với vai/tay nên script phát `[CẢNH BÁO]`; cả 4 khớp đều lộ rõ nên giữ nguyên `v = 2` và giữ nguyên bên trái/phải. |
| | | | | | |

## 4. Bảng tự review (CP3)

Đi từ ảnh cuối về ảnh đầu. 5 ảnh người (ít nhất 2 ảnh `back` hoặc `crowd`) và 3 ảnh mặt. Ít nhất 8 dòng, kể cả dòng `ok`. Ghi lỗi trước, sửa sau.

Mã lỗi dùng đúng một trong: `lr_swap`, `vis_flag`, `position`, `missing`, `extra`, `ok`.

| Ảnh | Người / mặt | Điểm | Mã lỗi | Ghi chú | Đã sửa? |
|---|---|---|---|---|---|
| 000000311497.jpg | người | 17 | lr_swap | cặp knee, ankle nằm ngược phía so với các cặp còn lại. Có thể đổi trái/phải một cặp (`lr_swap`), hoặc tay / chân bắt chéo thật: kiểm lại | Đã sửa cho cặp hông (left_hip chấm đỏ v = 2, right_hip chấm xanh nét đứt v = 1) hiện nằm gần ngang nhau đúng tầm khớp hông theo đường chỉ hồng, hai đường vai–hông song song tự nhiên theo thân và nối xuống đúng trục hai đùi |
| `000000056651.jpg` | Người phụ nữ mặc áo kẻ ở giữa (`#2`) | `right_wrist` | `vis_flag` | Cổ tay phải bị che khuất phía sau bánh sinh nhật / khay bánh; ban đầu thử đặt `v = 1` theo vị trí dự đoán (`khoanh tay` sau bánh) thì script kiểm tra cảnh báo cặp `wrist` ngược phía so với các cặp còn lại, đồng thời không ước lượng chắc chắn được tâm khớp bị che (phân vân giữa `v = 1` và `v = 0`). | Đã sửa (chuyển từ `v = 1` sang `v = 0` theo quy tắc không đoán bừa) |
| `000000253584.jpg` | Người nhỏ phía sau (`#2` cũ, cao 51 px) | Cả skeleton | `extra` | Gán nhầm người có chiều cao dưới ngưỡng 60 px (`51 px`), bị `check_submission.py` cảnh báo ở commit `18a5a41`. | Đã sửa (xóa skeleton ở commit `46314b2`) |
| `000000253584.jpg` | Người nhỏ phía sau (`#3` cũ, cao 29 px) | Cả skeleton | `extra` | Gán nhầm người có chiều cao dưới ngưỡng 60 px (`29 px`), bị `check_submission.py` cảnh báo ở commit `18a5a41`. | Đã sửa (xóa skeleton ở commit `46314b2`) |
| `000000253584.jpg` | Người nhỏ phía sau (`#5` cũ, cao 28 px) | Cả skeleton | `extra` | Gán nhầm người có chiều cao dưới ngưỡng 60 px (`28 px`), bị `check_submission.py` cảnh báo ở commit `18a5a41`. | Đã sửa (xóa skeleton ở commit `46314b2`) |
| `000000137767.jpg` | Người phụ nữ cầm đĩa bay (`#1`) | `left_wrist` | `vis_flag` | Cổ tay trái buông sau mép váy/nơ hồng bị che khuất nhưng ước lượng chắc được; ở commit `18a5a41` đặt nhầm `v = 2`. | Đã sửa (đổi `v = 2` → `v = 1` ở commit `46314b2`) |
| | | | | | |
| | | | | | |
| | | | | | |
| | | | | | |
| | | | | | |

Tổng hợp số lỗi theo mã:

| `lr_swap` | `vis_flag` | `position` | `missing` | `extra` | `ok` |
|---:|---:|---:|---:|---:|---:|
| 1 | 2 | 0 | 0 | 3 | 0 |

Điểm còn phân vân giữa `v = 1` và `v = 0` (ghi cả hai lựa chọn và lý do chọn):

- `000000056651.jpg` — người phụ nữ mặc áo kẻ ở giữa (`#2`), điểm `right_wrist`: phân vân giữa **`v = 1`** (dự đoán khoanh tay ẩn phía sau bánh sinh nhật, nhưng `check_submission.py` cảnh báo cặp `wrist` ngược phía) và **`v = 0`** (do khớp bị che hoàn toàn không ước lượng chắc chắn được tâm khớp nên chọn `v = 0` theo quy tắc không đoán bừa).

## 5. Kiểm file xuất và kết quả tự kiểm (CP4)

Tự kiểm tay trong file JSON:

| File | Người / mặt kiểm (ảnh, thứ tự) | Số phần tử `keypoints` | Điểm `outside` ra `v = 0`, `occluded` ra `v = 1`? | `num_keypoints` khớp? |
|---|---|---:|---|---|
| `TruongThanhDuc_persons.zip` | `000000056651.jpg` (`#2`, `id=4`) | 51 | Có (`2` điểm `v = 0`, `3` điểm `v = 1`, `12` điểm `v = 2`) | Có (`15` = `12 + 3`) |
| `TruongThanhDuc_persons.zip` | `000000311497.jpg` (`#1`, `id=15`) | 51 | Có (`2` điểm `v = 0`, `2` điểm `v = 1`, `13` điểm `v = 2`) | Có (`15` = `13 + 2`) |
| `TruongThanhDuc_faces.zip` | `face_324513.jpg` (`#1`, `id=5`) | 39 | Có (`8` điểm `occluded` ra `v = 1`, `5` điểm `v = 2`, không có `outside`) | Có (`13` = `5 + 8`) |

Sau khi xuất, CVAT ghi `occluded` thành `v = 1` và `outside` thành `v = 0`. Tọa độ của điểm `outside` trong file là `0, 0` hay tọa độ cũ? **tọa độ cũ** (ví dụ `right_wrist` ở `000000056651.jpg #2` có `v = 0` vẫn mang tọa độ cũ `(371.15, 363.8)`).

Hai job đã chuyển sang completed trên CVAT? Có / Không

Dán phần **TÓM TẮT** in ra từ `src/check_submission.py`:

```text
- File person: TruongThanhDuc_persons.zip
  Số ảnh: 12 · số skeleton `person`: 20 · mỗi annotation 51 số
  Skeleton theo ảnh: 000000043500=2, 000000056651=2, 000000078170=2, 000000137767=1, 000000194027=1, 000000201702=1, 000000253584=2, 000000309169=3, 000000311497=2, 000000365099=1, 000000390585=1, 000000480582=2
  Cờ v: v=2 237 (70%) · v=1 61 (18%) · v=0 42 (12%)
  num_keypoints trung bình: 14.9
- File face: TruongThanhDuc_faces.zip
  Số ảnh: 8 · số skeleton `face`: 8 · mỗi annotation 39 số
  Skeleton theo ảnh: face_137371=1, face_147624=1, face_185015=1, face_249336=1, face_324513=1, face_562485=1, face_575921=1, face_85624=1
  Cờ v: v=2 96 (92%) · v=1 8 (8%) · v=0 0 (0%)
  num_keypoints trung bình: 13.0
- Ghi chú: 42 điểm v = 0 vẫn mang tọa độ cũ (CVAT 2.x xuất như vậy; chuẩn COCO ghi 0, 0).
- Số [LỖI]: 0 · số [CẢNH BÁO]: 1
Kết quả: ĐẠT (đọc lại từng cảnh báo ở trên)
```

Các `[CẢNH BÁO]` còn giữ nguyên và lý do:

- `[CẢNH BÁO] 000000311497.jpg #1: cặp knee, ankle nằm ngược phía so với các cặp còn lại. Có thể đổi trái/phải một cặp (lr_swap), hoặc tay / chân bắt chéo thật: kiểm lại.`
  Lý do giữ nguyên: Người phụ nữ trong ảnh đang bước nghiêng 3/4 về bên trái khung hình, chân trái bước lên trước bắt chéo qua phía trước chân phải ở sau, khiến `left_knee`, `left_ankle` nằm lệch sang trái ảnh so với `right_knee`, `right_ankle` (ngược phía ngang với vai/tay/hông). Cả 4 khớp đều lộ rõ (`v = 2`) và đã gán đúng bên trái/phải của người (đồng thời đã kiểm tra, chỉnh lại cặp `left_hip`, `right_hip` cho đúng vị trí), nên cảnh báo này là chân bắt chéo thật, không sửa.

## 6. Ảnh chụp màn hình

1. Label `person` trong label constructor (đủ 17 điểm có tên):

   ![Label person](images/1_label_person.png)

2. Một người quay lưng đã gán, cờ đã đặt:

   ![Người quay lưng](images/2_nguoi_quay_lung.png)

3. Một khuôn mặt đã gán 13 điểm, phóng to vùng mắt:

   ![Khuôn mặt phóng to](images/3_mat_phong_to.png)

4. Một đoạn JSON xuất của một người:

   ![JSON export](images/4_json_export.png)

Nguồn ảnh: COCO val2017 / Wikimedia Commons.

## 7. Tự đánh giá

1. Lỗi bạn mắc nhiều nhất hôm nay, và cách bạn sẽ tránh ở dự án thật:

   - **Lỗi thực tế đã gặp:**
     - **Sai hướng / ngược bên (`lr_swap` / chéo vai–hông):** ở `000000311497.jpg` (người phụ nữ đứng nghiêng), cặp `left_hip` / `right_hip` ban đầu bị đặt chéo ngược phía so với cặp vai (ở commit `18a5a41` và `46314b2`) rồi mới sửa lại song song ở commit `d16794e`.
     - **Gán thừa người dưới 60 px (`extra`) và nhầm cờ `v` (`vis_flag`):** ở `000000253584.jpg` đã xóa 3 skeleton nhỏ dưới 60 px ở commit `46314b2`, và ở `000000137767.jpg` đã sửa `left_wrist` từ `v = 2` → `v = 1`.
   - **Cách tránh ở dự án thật:** Xác định rõ hướng mặt/hông người trước khi kéo điểm; bật hiển thị tên điểm để kiểm cặp `left_*` / `right_*`; soi các đường nối vai–hông–chân xem có vắt chéo bất thường không trước khi `Ctrl+S`; và đo nhanh chiều cao bbox (`>= 60 px`) trước khi gán.

2. Một quy tắc bạn đề xuất thêm vào guideline (kèm ảnh ví dụ nếu có):

   - Bổ sung bước **"soi đường nối vai–hông cho người đứng nghiêng"** vào checklist (đề phòng trường hợp độ lệch ngang giữa `left_hip` và `right_hip` nhỏ hơn ngưỡng của script tự kiểm nên không có `[CẢNH BÁO]`, như ở `000000311497.jpg`).
   - Bổ sung mẹo **"vẽ khung bbox đo chiều cao trước khi đặt từng điểm"** để kiểm tra ngưỡng `>= 60 px` ngay từ đầu, tránh mất thời gian gán xong mới phát hiện người dưới 60 px phải xóa như ở `000000253584.jpg`.

3. Nhãn của bạn đi vào model ở đâu? Chọn một lỗi bạn đã sửa và nói nó sẽ làm model học sai thế nào (heatmap, trọng số trong loss, hay bước lật ảnh):

   - **Nhãn đi vào model:** Mỗi điểm `v > 0` tạo một đỉnh Gauss trên **heatmap đích** của kênh khớp tương ứng; cờ `v` quy định **trọng số trong loss** (`v = 0` có trọng số `0`, `v = 1` và `v = 2` có trọng số `1`); và khi tăng cường dữ liệu bằng **lật ngang ảnh (`x' = W − x`)**, code huấn luyện sẽ tự đổi tên cặp `left ↔ right`.
   - **Lỗi ở ảnh `000000311497.jpg` (commit `d16794e`) — cơ chế lật ảnh và heatmap:** Khi huấn luyện có bước lật ngang ảnh (`x' = W − x`) kèm đổi tên cặp `left ↔ right`, nếu `left_hip` và `right_hip` bị đặt ngược phía nhau thì cả ảnh gốc lẫn ảnh lật đều dạy model nhãn mâu thuẫn (hông trái thành hông phải), đồng thời đỉnh heatmap đích của hai kênh hông bị đặt lệch sang sai nửa thân người, làm giảm OKS.

## 8. Khó khăn và lỗi kỹ thuật

- **Ước lượng kích thước người dưới ngưỡng 60 px:** Ban đầu chưa quen cách kiểm tra chiều cao pixel của người trên CVAT nên ở ảnh `000000253584.jpg` đã gán thừa cả những người nhỏ ở phía sau; sau khi chạy `src/check_submission.py` thấy cảnh báo `< 60 px` (`51 px`, `29 px`, `28 px` ở commit `18a5a41`) mới quay lại xóa trên CVAT và rà soát lại (tốn thêm khoảng 15 phút).
- **Khó phân biệt ranh giới `v = 1` / `v = 0` và cảnh báo `lr_swap` ở ca che khuất / bước chéo:** Ở `000000056651.jpg`, khớp `right_wrist` bị bánh sinh nhật che khuất nên thử ước lượng `v = 1` thì bị `check_submission.py` cảnh báo cặp `wrist` ngược phía, phải đổi về `v = 0` theo quy tắc không đoán bừa; ở `000000311497.jpg`, hai chân bước chéo thật làm script cảnh báo cặp `knee, ankle`, trong khi cặp `left_hip` / `right_hip` bị chéo nhẹ ngược chiều vai lại nhỏ hơn ngưỡng cảnh báo của script nên phải soi trực tiếp đường nối trên CVAT mới phát hiện để sửa.
- **Định dạng file JSON xuất từ CVAT 2.74.1:** File `annotations/person_keypoints_default.json` trong zip xuất ra bị nén trên một dòng dài (`11 974` ký tự), gây khó khăn khi mở tự kiểm tay và chụp ảnh `4_json_export.png` (phải tách file tạm để format lại trong VS Code rồi xóa file tạm). Ngoài ra, các điểm `outside` (`v = 0`) vẫn giữ nguyên tọa độ `(x, y)` cũ thay vì `0, 0` (được `check_submission.py` xác nhận là bình thường, không phải lỗi).
- **Lỗi hệ thống CVAT:** Không gặp lỗi mất dữ liệu hay lỗi không xuất được file từ CVAT trong quá trình làm bài.

## Bonus (nếu làm)

### B1 — 3 ảnh người khó thêm

| Ảnh | Số skeleton | Số người dưới 60 px | Ca khó nhất |
|---|---:|---:|---|
| | | | |
| | | | |
| | | | |

Kết quả `src/check_submission.py` cho `HoVaTen_bonus_persons.zip`:

```text
(dán ở đây)
```

### B2 — 5 ảnh khuôn mặt đen trắng / hồng ngoại

Ảnh không màu khó hơn ở điểm nào? Kính râm trên ảnh hồng ngoại có còn là `v = 1` không?

...

### B3 — Model so với nhãn của tôi (không tính điểm)

- Demo đã dùng:
- Ảnh đã thử:

1. Model có trả điểm cho khớp bị che, hay khớp ngoài ảnh không? Confidence ở các điểm đó thế nào?

   ...

2. Với người quay lưng, model có đổi trái/phải không?

   ...

3. Bộ điểm của model có giống 17 điểm COCO không?

   ...

## 9. Tài liệu tham khảo và công cụ AI đã dùng

...
