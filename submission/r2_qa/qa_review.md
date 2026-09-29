# QA Review Độc Lập · B1-edge (P3)

- **Reviewer (Vai B):** Nguyễn Minh Quân (MSSV: 2A202602224, ID: `quan`)
- **Chủ nhãn (Vai A):** Lưu Lan Anh (MSSV: 2A202602266, ID: `lananh`)
- **Điều phối (Vai C):** Lê Quang Thịnh (MSSV: 2A202602316, ID: `thinh`)
- **Slice:** B1-edge
- **Mã khóa bản A đã khóa:** `FF66-769E`
- **File kiểm tra:** `submission/r1_craft/annotations.xml`
- **Phương thức QA:** QA mù (blind review) độc lập trên `submission/r2_qa/qa_overlay.html` và ảnh gốc, chưa đối chiếu reference/model.

---

## 1. Bảng đối chiếu nhận xét từng đối tượng (QA Checklist)

| frame | object_ref | rule_id | điều nhìn thấy | điều cần kiểm lại |
|---|---|---|---|---|
| adasind_001320.jpg | L2 (Bike) | R07, R09 | Vùng góc dưới trái (x: 0–242, y: 1097–1615) là gương/thân xe ego, không phải phương tiện xe máy lưu thông | Xóa box Bike đè lên thân xe ego; bổ sung polygon ignore_region với reason=ego_body theo R07 |
| adasind_001320.jpg | L1 (Pedestrian) | R03, R02 | Box L1 (Pedestrian, h=118.7px) nằm đè sát xe ba bánh L3 (ThreeWheeler) | Kiểm tra người đi bộ đứng ngoài hay người trong xe L3. Nếu người lái/ngồi trong xe thì không box riêng (R03); nếu người ngoài thì chỉnh lại biên để không giao cắt thân xe L3 |
| adasind_001320.jpg | [Chung frame] | R07 | Chưa có bất kỳ polygon ignore_region nào cho thân xe ego ở mép dưới | Bổ sung polygon ignore_region (reason: ego_body) cho phần thân xe ego nhìn thấy |
| adasind_014670.jpg | L1 (Bus) | R04, R05 | Đối tượng bị cắt biên trái (x: 0.4–83.1, y: 820.8–1087.7) có cabin đứng phẳng, thùng tải nhỏ chở hàng | Cần phân loại lại class từ Bus sang Truck theo quy tắc ánh xạ phương tiện R04 (xe tải nhỏ/pickup là Truck, chỉ minibus mới là Bus) |
| adasind_014670.jpg | L7 (Bike) | R07, R09 | Box Bike góc dưới trái (x: 0–295.5, y: 1138.2–1688.2) bao trùm phần thân/gương xe ego | Xóa box Bike ngoại lai, vẽ polygon ignore_region (reason: ego_body) bám sát đường cong thân xe ego |
| adasind_014670.jpg | L6 (Car) | R05 | Ô tô ở rìa phải chạm biên ảnh (xbr=1079.2), đã đánh truncated=true | Giữ nguyên truncated=true; kiểm tra thuộc tính edge_zone cho box cắt rìa kính fisheye |
| adasind_034080.jpg | L1, L2 (Bike) | R03, R05 | Hai box Bike lồng nhau (L2 x: 124–180, y: 1013–1117 nằm hoàn toàn trong tầm vóc xe L1 x: 116–276, y: 997–1301) | Kiểm tra kỹ: nếu L2 là xe máy phía sau bị che một phần thì giữ và đánh occluded=true; nếu là người ngồi trên xe L1 thì vi phạm R03 (người lái + xe = 1 box Bike), phải xóa L2 |
| adasind_034080.jpg | L9 (Bike) | R07, R09 | Box Bike góc dưới trái (x: 0–206.3, y: 1259.7–1634.7) vẽ trùm lên gương/thân xe ego | Xóa box Bike, vẽ polygon ego_body che toàn bộ vùng thân xe ego |
| adasind_034080.jpg | [Missing 3W] | R01, R04 | Khu vực giữa đường (x ~260–310, y ~1050–1100) có phương tiện ba bánh (ThreeWheeler) cao >40px bị che khuất một phần | Bổ sung box ThreeWheeler với thuộc tính occluded=true theo R01 |

---

## 2. Chi tiết nhận xét theo từng Frame

### Frame 1: `adasind_001320.jpg`
- **Tình trạng tổng quan:** Gán nhãn cơ bản các xe ThreeWheeler, Truck, Bike ở trung tâm tương đối tốt.
- **Vấn đề nghiêm trọng (P0):** Thiếu hoàn toàn polygon `ignore_region` (`ego_body`). Box `L2` (Bike) bị gán nhầm lên vùng thân xe ego của chính xe ghi hình ở góc dưới bên trái ảnh.
- **Vấn đề cần rà soát (P1):** Box `L1` (Pedestrian) và `L3` (ThreeWheeler) có vùng bounding box chồng lấn lớn. Cần phân định rõ người đi bộ độc lập hay người ngồi trên xe.

### Frame 2: `adasind_014670.jpg`
- **Tình trạng tổng quan:** Nhận diện tốt các xe `Car` (`L6`), `Pedestrian` (`L3`, `L4`) và `ThreeWheeler` (`L2`, `L5`).
- **Sai nhãn (P1):** Đối tượng `L1` ở mép trái được gán nhãn `Bus` nhưng quan sát hình dáng cabin và thùng xe thì đây là xe tải nhỏ chở hàng (`Truck`). Theo rule `R04`, cần sửa nhãn thành `Truck`.
- **Vấn đề phạm vi (P0):** Tương tự frame 1, box `L7` (Bike) ở góc dưới trái tiếp tục là vùng thân xe ego chưa được vẽ `ignore_region` (`ego_body`).

### Frame 3: `adasind_034080.jpg`
- **Tình trạng tổng quan:** Các đối tượng người đi bộ `L7`, `L8` và ô tô `L4`, `L5`, `L6` vẽ chuẩn xác.
- **Rà soát Rider / Duplicate (P1):** Cặp box `L1` và `L2` đều là `Bike`. Box `L2` nằm ngay vị trí người lái của `L1`. Cần kiểm tra kỹ ảnh phóng to: nếu là xe thứ hai di chuyển song song phía sau thì giữ 2 box riêng và gán `occluded=true` cho `L2`; nếu không thấy xe thứ hai mà chỉ là rider thì phải xóa `L2` và giữ `L1` theo quy tắc R03.
- **Bỏ sót đối tượng (P1):** Phát hiện một xe ba bánh ở cự ly trung bình (x: 259–307, y: 1055–1097, H≈42px) đủ ngưỡng `H ≥ 40px` (R01) nhưng bị bỏ sót.
- **Vấn đề phạm vi (P0):** Box `L9` tiếp tục bị vẽ đè lên thân xe ego ở góc dưới trái.

---

## 3. Ảnh bằng chứng (Evidence Screenshots)

Các ảnh crop bằng chứng đã được lưu trữ trong `submission/screenshots/`:
- `submission/screenshots/adasind_001320.jpg_L1_L3.png`: Giao cắt giữa Pedestrian L1 và ThreeWheeler L3.
- `submission/screenshots/adasind_001320.jpg_L2_ego.png`: Box L2 Bike đè lên thân xe ego.
- `submission/screenshots/adasind_014670.jpg_L1_bus.png`: Box L1 Bus ở rìa trái (nghi vấn là Truck).
- `submission/screenshots/adasind_014670.jpg_L7_ego.png`: Box L7 Bike đè lên thân xe ego.
- `submission/screenshots/adasind_034080.jpg_L1_L2_bike_overlap.png`: Chồng lấn giữa 2 box Bike L1 và L2.
- `submission/screenshots/adasind_034080.jpg_L9_ego.png`: Box L9 Bike đè lên thân xe ego.

---

## 4. Kết luận bàn giao QA từ B sang C và A

1. **Số nhận xét:** 9 nhận xét (trong đó có 3 ca P0 về phạm vi thân xe ego, 3 ca P1 về class/rider/thiếu box, 3 ca rà soát hình học và zone).
2. **Ca ưu tiên rework:**
   - [P0] Bổ sung polygon `ignore_region` (`reason=ego_body`) ở cả 3 frame và xóa bỏ các box Bike giả lập đè lên thân xe ego (`L2` frame 1, `L7` frame 2, `L9` frame 3).
   - [P1] Điều chỉnh class `L1` ở frame `adasind_014670.jpg` từ `Bus` sang `Truck`.
   - [P1] Rà soát cặp `L1`/`L2` ở `adasind_034080.jpg` đảm bảo đúng luật R03.
   - [P1] Bổ sung xe ba bánh bị sót ở giữa đường trong `adasind_034080.jpg`.
3. **Trạng thái:** QA độc lập đã chốt hoàn tất. Bàn giao kết quả cho C để mở reference/chẩn đoán (P4) và A chuẩn bị phương án sửa (P5).
