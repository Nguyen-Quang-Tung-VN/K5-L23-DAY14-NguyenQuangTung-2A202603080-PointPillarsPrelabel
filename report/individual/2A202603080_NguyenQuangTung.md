# Báo cáo Day 14 — Nguyễn Quang Tùng — 2A202603080

- Mã học viên dùng với `--student`: `2A202603080`
- `seed_tag` trong `frames_A.txt`: `cb0f42154c`

## 1. Dự đoán trước sweep (bước 5b)

- Tham số tôi nghĩ giảm công sửa nhiều nhất: **`score_thr`** (tăng ngưỡng điểm tin cậy từ mức gốc `0.1` lên khoảng `0.3–0.4`), kết hợp với **`min_points`** (ví dụ `5`) để loại bỏ các hộp dự đoán có quá ít điểm LiDAR.
- Vì sao (nhìn ảnh BEV tập A: box thừa nhiều hay box sót nhiều?):
  - Quan sát 3 ảnh tổng hợp camera + BEV trong `png_A/` (`s01f13`, `s03f55`, `s04f41`) và số đo cấu hình gốc trên tập A, **box thừa (false positive) nhiều áp đảo so với box sót**: mô hình tạo ra nhiều hộp nhiễu điểm thấp ở hàng cây, bụi rậm, rào chắn, dải phân cách và biển báo/trạm xe buýt (cụ thể ở lượt đo gốc trên 3 frame tập A: 59 hộp thừa phải xóa, chiếm `59 × 0.3 = 17.7` điểm trong tổng `21.7` điểm chi phí, trong khi chỉ có 3 xe Bus bị sót = `3.0` điểm và 2 hộp cần sửa = `1.0` điểm).
  - Các vật thể bị bỏ sót chủ yếu là xe `Bus` (lớp mà trọng số KITTI chưa được huấn luyện trực tiếp) nên việc đổi `score_thr` hay `min_points` không thể tự tìm thêm xe Bus, nhưng tăng `score_thr` (và `min_points`) sẽ cắt được lượng lớn hộp nhiễu điểm thấp.
  - Tuy nhiên cần giữ ngưỡng ở mức vừa phải (không đặt quá cao như `0.5` trở lên) vì chi phí thêm 1 vật bị sót (`1.0`) nặng gấp hơn 3 lần xóa 1 hộp thừa (`0.3`), nếu lọc quá tay làm mất các xe ở xa hoặc `Pedestrian`/`Cyclist` thì tổng chi phí sẽ tăng ngược trở lại.

## 2. Tập A: sửa tay và tune

| Cấu hình | Xóa | Thêm | Sửa | Chi phí |
|---|---:|---:|---:|---:|
| Gốc | | | | |
| `best_config.json` | | | | |

- Tham số đã đổi trong `best_config.json`: ...
- Kết quả leave-one-out (dán các dòng `scripts/pp_sweep.py` in ra): ...
- Cấu hình có ổn định trên từng frame không? Vì sao tôi vẫn chọn (hoặc không chọn) nó: ...

## 3. Trước/sau (bước 8)

![so sánh](../../d14_2A202603080/compare/<frame>.jpg)

- `scripts/pp_compare.py` báo giảm (hoặc tăng) ...% công sửa trên tập A.
- Dự đoán ở mục 1 đúng hay sai? Giải thích bằng số box xóa/thêm/sửa: ...

## 4. Tập B

Số đo từ `scripts/pp_prelabel.py --frames d14_2A202603080/frames_B.txt --config d14_2A202603080/best_config.json --gt d14_2A202603080/ref_B.json`:

| Xóa | Thêm | Sửa | Chi phí |
|---:|---:|---:|---:|
| | | | |

- Loại box tốn công sửa nhất ở tập B: ...

## 5. Giới hạn

- Nêu ít nhất 2 giới hạn (ví dụ: chỉ 3 frame tune; model KITTI không có Truck/Bus; chi phí là công thức, chưa phải thời gian sửa thật): ...
