# Error analysis card

## Zone × block

| zone | block | what | count |
|---|---|---|---:|
| center | B1 | MISSING | 3 |
| center | B1 | SPURIOUS | 6 |
| center | C0 | SPURIOUS | 2 |
| edge | B1 | IGNORE_SCOPE | 3 |
| edge | B1 | MISSING | 1 |
| edge | B1 | SPURIOUS | 1 |
| mid | B1 | MISSING | 6 |
| mid | B1 | SPURIOUS | 7 |
| mid | B1 | WRONG_CLASS | 1 |
| unknown | B1 | DUPLICATE | 1 |
| unknown | B1 | IGNORE_SCOPE | 2 |
| unknown | B1 | WRONG_CLASS | 1 |

## Top defects
- SPURIOUS: 16 (ví dụ frame adasind_019560.jpg)
- MISSING: 10 (ví dụ frame adasind_034080.jpg)
- IGNORE_SCOPE: 5 (ví dụ frame adasind_001320.jpg)

## Phân tích của bạn

Hai bảng trên do `python3 lab11.py card` tính từ `findings.csv`; chạy lại lệnh sẽ cập nhật bảng và giữ nguyên mục này. Viết cho lỗi nổi bật nhất, dẫn frame/`object_ref`.

- **SPURIOUS (16, nổi bật nhất):** đa số (11/16) là `M_only` — model tự báo thêm vật mà cả người lẫn reference đều không thấy (`why=E4_model_domain`, `owner=ai_team`), tập trung ở zone `mid` của `adasind_014670.jpg`/`adasind_034080.jpg`. Ca đáng chú ý khác là 2 dòng ở round `calib` (`adasind_019560.jpg` L7, L8): không phải model mà là chính annotator vẽ thừa — L7 là box Pedestrian đè lên người đang ngồi lái xe máy (vi phạm R03), L8 là box Bike trùng lặp trên cùng một xe đạp mà box L1 đã bao trùm. Đã xác nhận bằng ảnh gốc, xem `findings.csv` dòng calib và `20_guideline_patch.md`.
- **MISSING (10):** 6/10 ở zone `mid`, phần lớn là model bỏ sót vật mà người+reference đều xác nhận (`LR_noM`, `why=E4_model_domain`). Ca thật của annotator là `adasind_034080.jpg` R9 (ThreeWheeler bị người lái xe máy phía trước che gần hết, chỉ lộ mảng tối nhỏ ~42px) — `why=E1_annotator_error`, `rule_id=R01`, `owner=annotator`, `action=rework`.
- **WRONG_CLASS (`adasind_014670.jpg` L1, Bus/Truck):** không mặc định là lỗi annotator. Đã đối chiếu 4 nguồn độc lập: annotator (Bus) và model (Bus) nghiêng một hướng; reference và QA blind-review (Quân) nghiêng Truck. Vật bị cắt biên chỉ còn 83px nên không đủ để phân biệt chắc chắn — xếp `why=E5_unresolved`, `owner=qa`, `action=escalate` (xem `30_escalation_ticket.md` Ticket 1, ảnh `screenshots/adasind_014670.jpg_L1_bus_truck_dispute.png`).
- **IGNORE_SCOPE (5):** toàn bộ là các box Bike đè lên vùng thân xe ego chưa được vẽ `ignore_region` (do QA — Quân — phát hiện trong blind review: `adasind_001320.jpg` L2/L1, `adasind_014670.jpg` L7, `adasind_034080.jpg` L9). `why` để trống theo đúng quy tắc round `r2_qa`; `owner=annotator`, `action=rework` — A cần vẽ polygon `ego_body` (R07) và xóa box thừa.
- **Cách sửa và ai nhận việc:** A rework 4 ca P0/P1 đã liệt kê trong `qa_review.md` mục 4 (ego_body ×3, class Bus→Truck theo QA — nhưng xem ticket escalate ở trên trước khi sửa, rider/duplicate L1-L2 ở `adasind_034080.jpg`, bổ sung R9); C theo dõi 2 ticket escalate (Ticket 1, 2) và cập nhật `40_decision_log.csv` khi có quyết định cuối; B xác nhận lại sau khi A sửa xong (đối chiếu `delta.md`).
- **Bằng chứng:** `submission/screenshots/` (4 ảnh mới: `adasind_014670.jpg_L1_bus_truck_dispute.png`, `adasind_014670.jpg_L2_threewheeler_missed.png`, `adasind_034080.jpg_R9_occluded_3w.png`, `adasind_019560.jpg_calib_L7_L8_duplicate_rider.png`), `findings.csv`, `submission/r2_qa/qa_review.md`, `submission/r3_diag/model_compare.md`.
