# Thành viên và phân vai — Day11 SVM 360 Fisheye

## 1. Thông tin nhóm

- Khóa/lớp: K4
- Tên nhóm: LaLaLaLand
- Repo Public: https://github.com/LQTYulJinn/K4-DAY11-LeQuangThinh-2A202602316
- Máy giữ hồ sơ chính / người quản lý: Lê Quang Thịnh (vai C)
- Slice chung lấy từ mode.json: B1-edge
- Tên định danh vai A dùng cho --self: lananh
- Kênh trao đổi nội bộ: [Điền]
- Đại diện nộp (vai C): Lê Quang Thịnh
- Commit chốt bài: [Điền sau khi push bản nộp cuối]

## 2. Ba vai chính

| Vai | Họ và tên | MSSV | Tên định danh trong mode | Trách nhiệm | Bằng chứng đóng góp |
|---|---|---|---|---|---|
| A · Gán nhãn | Lưu Lan Anh | 2A202602266 | lananh | Parking/C0/slice, self-QC, lock, rework | [Link file/commit và mô tả phần đã làm] |
| B · QA độc lập | Nguyễn Minh Quân | 2A202602224 | quan | Review trước reference, finding QA, kiểm lại ca sửa | [submission/r2_qa/qa_review.md](file:///c:/Users/pc/Desktop/learn/AI/K4-DAY11-LeQuangThinh-2A202602316/submission/r2_qa/qa_review.md), [submission/findings.csv](file:///c:/Users/pc/Desktop/learn/AI/K4-DAY11-LeQuangThinh-2A202602316/submission/findings.csv) (4 dòng r2_qa), 6 screenshots |
| C · Chẩn đoán & điều phối | Lê Quang Thịnh | 2A202602316 | thinh | Báo cáo, phân xử, kế hoạch, tích hợp, check và nộp | [Link file/commit và mô tả phần đã làm] |

Bảng này xác định vai của nhóm. Vòng QA tự sinh trong team.json thuộc quy trình nhiều hồ sơ của CLI; nhóm dùng một slice chung (B1-edge) và quy trình A → B → C đã nêu trong hướng dẫn.

## 3. Bàn giao theo pha

| Mốc | Người giao → nhận | File / commit / mã khóa | Người nhận đã kiểm gì? | Trạng thái / vướng mắc |
|---|---|---|---|---|
| P0 · Chốt môi trường và vai | C → A, B | mode.json, slice=B1-edge, phân vai (file này) | Đã kiểm tra môi trường, phân công vai A (lananh), B (quan), C (thinh) và chốt slice B1-edge | Khởi tạo thành công |
| P2 · Khóa bản đầu | A → B, C | submission/r1_craft/annotations.xml, lock.txt, code=FF66-769E, commit 433433b | Kiểm tra mã khóa FF66-769E khớp file XML, đủ 3 frame slice B1-edge (23 box, 6 polygon) | Bản khóa r1_craft hợp lệ, sẵn sàng bàn giao cho B |
| P3 · Chốt QA mù | B → C, A | submission/r2_qa/qa_review.md, findings.csv (4 dòng r2_qa), 6 ảnh screenshots | C kiểm tra đủ 3 frame, 9 nhận xét, không còn TODO, 4 dòng finding r2_qa đúng chuẩn cell=L_only why rỗng; A nhận danh sách ca rà soát | QA mù đã chốt; sẵn sàng mở reference (P4) |
| P4 · Quyết định sửa | C → A, B | [finding, decision log, commit] | [Điền] | [Điền] |
| P5 · Kiểm bản sửa | A → B → C | [v2, lock2, review kiểm lại, delta] | [Điền] | [Điền] |
| P6 · Chốt nộp | A, B → C | [manifest, commit chốt] | [Điền] | [Điền] |

## 4. Bất đồng và phối hợp

- Một ca đã phân xử: [Frame/object/rule; ý kiến A/B; bằng chứng; quyết định và link]
- Ca còn mở: [Nội dung, người theo dõi, phép kiểm tiếp theo; nếu không còn thì ghi rõ]
- Đóng góp của A/B/C vào kế hoạch và exit ticket: [Điền phần việc thực tế]
- Thay đổi phân công nếu có: [Thời điểm, lý do, người nhận; nếu không đổi thì ghi rõ]

## 5. Xác nhận trước khi nộp

- [ ] A xác nhận nhãn và export đúng phiên bản: [Tên / bằng chứng]
- [x] B xác nhận đã QA độc lập trước reference và kiểm lại ca sửa: Nguyễn Minh Quân — Đã review độc lập 3 frame trên qa_overlay.html, lập bảng 9 nhận xét trong submission/r2_qa/qa_review.md, bổ sung 4 dòng finding r2_qa và 6 ảnh bằng chứng trong screenshots/
- [ ] C xác nhận báo cáo đúng bản khóa, các file đầy đủ và check exit 0: [Tên / bằng chứng]
- [ ] manifest.json tại commit chốt có failed_gates rỗng.
- [ ] Repo nhóm Public, ảnh và các bằng chứng mở được.
- [ ] C đã push và gửi link repo nhóm + commit qua kênh lớp công bố.
