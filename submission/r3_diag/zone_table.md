# Zone table (slice của bạn)

Lệnh `python3 lab11.py model` tự ghi bảng số (cùng cách đếm với `r1_craft/compare.md` và `model_compare.md`); chạy lại lệnh sẽ cập nhật bảng và giữ nguyên phần nhận xét. Bạn chỉ viết mục Nhận xét.

| Zone | n_ref | L missing | L spurious | M missing (`LR_noM` + `R_only`) | M thừa (`LM_noR` + `M_only`) | Lỗi L chính (`what`) |
|---|---:|---:|---:|---:|---:|---|
| center | 7 | 1 | 1 | 2 | 4 | SPURIOUS (1) |
| mid | 9 | 1 | 1 | 6 | 7 | WRONG_CLASS (1) |
| edge | 4 | 0 | 0 | 1 | 1 | — |

## Nhận xét

- Zone gãy nhiều nhất là **mid**: model bỏ sót 6 vật mà người+reference đều xác nhận (`LR_noM`+`R_only` = 5+1), và tự báo thêm 7 vật không ai khác thấy (`LM_noR`+`M_only` = 1+6) — cao nhất ở cả hai chiều so với center (2 và 4) và edge (1 và 1). Zone `edge` có số liệu "sạch" nhất (1/1) nhưng `n_ref` chỉ có 4 vật trong 3 frame, nên đây nhiều khả năng là mẫu quá nhỏ để kết luận model xử lý rìa ảnh tốt, không phải bằng chứng đáng tin.
- Giả thuyết: zone `mid` là vùng có mật độ vật cao nhất trong 3 frame (nhiều xe/người/xe máy đứng gần nhau — xem ca nghi trùng `L1/L2 Bike` và `L2+R8` ở `adasind_034080.jpg`), nên model có khả năng bị nhiễu bởi chồng lấn (occlusion) nhiều hơn là bị ảnh hưởng trực tiếp bởi méo fisheye (méo lẽ ra rõ nhất ở `edge`). Giới hạn quan trọng: slice chỉ có 3 frame / 20 đối tượng tham chiếu — không đủ để suy rộng thành kết luận "model yếu ở zone mid" cho toàn bộ 4 camera SVM hay cho production; đây chỉ là tín hiệu ban đầu cần kiểm thêm ở slice khác trước khi đưa vào kế hoạch gold set.
