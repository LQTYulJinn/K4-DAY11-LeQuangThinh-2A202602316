# Kế hoạch review từ lỗi quan sát được

Từ `findings.csv` và `zone_table.md`, chọn **hai lát cắt của bài ADASIND một camera** cần review trước. Bảng này
giải thích dữ liệu thật bạn vừa làm; nó không thay cho kế hoạch bốn camera giả lập ở `45_sampling_plan.csv`.

| Lát cắt / frame | Số ca và loại lỗi | Vì sao review trước | Bằng chứng cần giữ |
|---|---|---|---|
| `adasind_014670.jpg`, zone mid (vùng đối tượng L1/L2/R5/M3) | 4 dòng findings: 1 WRONG_CLASS chưa chốt (Bus/Truck, escalate), 1 SPURIOUS hóa ra là reference thiếu (escalate), 2 dòng model FP/FN quanh cùng khu vực | Đây là frame duy nhất trong slice có bất đồng chưa giải quyết (2 ticket escalate) và cũng là frame model yếu nhất (accuracy 0.667, FP=2, FN=1 theo `local_quality.md`) — review trước để không lan lỗi sang quyết định gold set | `submission/screenshots/adasind_014670.jpg_L1_bus_truck_dispute.png`, `adasind_014670.jpg_L2_threewheeler_missed.png`, `findings.csv` dòng r1_craft/r3_diag L1, L2, `30_escalation_ticket.md` |
| `adasind_034080.jpg`, zone center/mid (block ThreeWheeler R9, cặp Bike L1/L2) | 2 dòng findings: 1 MISSING đã xác nhận thật (R01, rework), 1 DUPLICATE do QA phát hiện (R03, rework) | Đây là frame duy nhất trong slice có lỗi thiếu box đã xác nhận bằng ảnh (không phải nghi ngờ), và có ca rider/duplicate cần A tự kiểm tra kỹ trước khi rework để tránh lặp lại lỗi giống vòng calib | `submission/screenshots/adasind_034080.jpg_R9_occluded_3w.png`, `adasind_034080.jpg_L1_L2_bike_overlap.png`, `findings.csv` dòng R9, L2 (round r2_qa) |

Giới hạn của kết luận từ ba frame ADASIND: chỉ 3 frame, 1 camera, 20 vật tham chiếu — không đủ để suy rộng thành "model yếu ở zone mid" hay "annotator hay bỏ sót vật bị che" cho toàn bộ 200 frame hay bốn camera thật; đây chỉ là tín hiệu ban đầu, không phải kết luận thống kê (xem thêm giới hạn ở `zone_table.md`).

## Chuyển sang kế hoạch bốn camera giả lập

Cách soát độ phủ của 200 frame ở `45_sampling_plan.csv`: đếm số ca **độc lập** theo camera × normal/hard chứ không đếm số frame liền kề trong cùng một cảnh (ví dụ 5 frame liên tiếp của cùng một xe cắt ngang chỉ tính là 1 ca "cắt ngang bất ngờ", không phải 5 ca). Kế hoạch bốn camera chỉ giúp **tìm ra ca cần soi kỹ hơn** (dựa trên loại rủi ro mỗi camera, xem `46_gold_set_plan.md`), chưa đo được tỷ lệ lỗi thật của cả hệ thống — muốn đo tỷ lệ lỗi cần một tập đã được gọi là gold (đã qua bước review độc lập + giải quyết bất đồng), không phải tập lấy mẫu ban đầu.
