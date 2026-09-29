# Escalation ticket

## Ticket 1 — Bất đồng class Bus/Truck (L1, adasind_014670.jpg)

- **Frame:** `adasind_014670.jpg` (slice B1-edge)
- **Ảnh chụp:** `submission/screenshots/adasind_014670.jpg_L1_bus_truck_dispute.png`
- **Expected impact:** Nếu không giải quyết, mọi thống kê class Bus/Truck của `local-quality` (hiện Bus fp=1, Truck fn=1, Truck recall chỉ 0.500) sẽ bị gán sai cho annotator trong khi đây có thể là lỗi reference hoặc một ranh giới R04 chưa đủ rõ cho vật bị cắt biên nhiều. Ảnh hưởng đến cách đọc mục 5–6 của rubric nếu người chấm mặc định đây là lỗi annotator.
- **Owner:** `qa` (Quân đã blind-review và cần phối hợp với Thịnh/Lan Anh mở lại CVAT phóng to trước khi chốt)
- **Recommendation:** Mở lại task CVAT, phóng to phần còn lại của vật (nếu track được ở frame lân cận) để tìm đầu xe/cửa sau trước khi quyết định. Nếu vẫn không thể phân biệt, giữ nguyên Bus (theo annotator + model, 2/4 nguồn) nhưng ghi rõ trong `40_decision_log.csv` là quyết định có điều kiện, và đề xuất Lab Coach xác nhận lại nhãn Truck trong teaching reference cho frame này.

## Ticket 2 — Reference bỏ sót xe ba bánh có thật (L2, adasind_014670.jpg)

- **Frame:** `adasind_014670.jpg` (slice B1-edge)
- **Ảnh chụp:** `submission/screenshots/adasind_014670.jpg_L2_threewheeler_missed.png`
- **Expected impact:** Cả reference và model đều bỏ sót một vật có thật (ThreeWheeler, 37×50px, vừa qua ngưỡng H=40) mà annotator đã vẽ đúng. Nếu không sửa, dòng SPURIOUS này sẽ tiếp tục làm sai lệch precision/recall của cả reference lẫn model cho những vật nhỏ/xa ở ngưỡng biên — ảnh hưởng trực tiếp đến cách đọc `local_quality.md` (ThreeWheeler đang là nhãn yếu nhất: accuracy 0.905, FP=1).
- **Owner:** `guideline` (người tạo teaching reference cần bổ sung box này)
- **Recommendation:** Thêm box ThreeWheeler (~37×50px) vào teaching reference của `adasind_014670.jpg` tại vị trí đã xác nhận bằng ảnh; cập nhật ghi chú QC riêng cho các vật gần ngưỡng H=40 để tránh bỏ sót lặp lại (xem đề xuất liên quan trong `20_guideline_patch.md`).
