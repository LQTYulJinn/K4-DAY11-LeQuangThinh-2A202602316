# Exit ticket

Đọc `docs/10-svm360-reading-vi.md` trước khi trả lời câu 1–2. Các câu về zone, `why`, rework, parking và sampling
đã nằm trong file tương ứng nên không hỏi lại ở đây.

1. Một vật ở vùng seam giữa hai camera thật xuất hiện với hai box khác nhau: đó là lỗi `DUPLICATE` hay cần một quy
   tắc riêng? Vì sao?

   Không nên tính là `DUPLICATE` theo nghĩa lỗi gán nhãn. `DUPLICATE` trong bài lab này (xem ca `adasind_034080.jpg`
   L1/L2 hoặc calib L1/L8) nghĩa là **cùng một camera, cùng một vật, bị vẽ hai lần** — sửa bằng cách xóa bớt một box.
   Ở vùng seam, hai box nằm trên **hai ảnh khác nhau** (hai camera khác nhau), có hai hệ tọa độ, hai góc nhìn, và
   thường hai giá trị zone khác nhau (ví dụ `edge` ở camera này, `mid` ở camera kia) — đây không phải lỗi thao tác
   mà là đặc điểm hình học tất yếu của hệ bốn camera chồng vùng nhìn. Cần một attribute/quy tắc riêng, ví dụ
   `seam_pair` hoặc một trường liên kết hai object_id qua hai camera, để tầng sau (BEV/tracking) biết đây là **một
   vật vật lý, hai quan sát**, không phải hai vật hay một lỗi cần xóa bớt. Rule mới này nên nêu rõ: không tự động
   xóa box nào ở bước gán nhãn một-camera; việc hợp nhất chỉ làm ở tầng có timestamp, calibration và policy output
   (xem câu 2).

2. Một vật đi qua nhiều frame trên cùng camera: khi nào giữ cùng track ID, khi nào thêm keyframe hoặc trạng thái
   Outside? Nêu bằng chứng sẽ cần trước khi nối track qua hai camera.

   Trong một camera: giữ cùng track ID khi vật còn quan sát được liên tục và chỉ thay đổi hình học dần (di chuyển,
   xoay nhẹ); thêm **keyframe** khi hình học đổi đột ngột (ví dụ vật quay đầu, bị che rồi lộ lại ở tư thế khác) để
   không nội suy sai giữa hai trạng thái quá khác nhau; chuyển sang **Outside** khi vật rời hẳn khung hình hoặc bị
   `ignore_region` (ví dụ đi vào vùng `lens_border`) che hoàn toàn, thay vì xóa track — Outside giữ lại lịch sử để
   biết vật "đã từng ở đây" mà không ép model đoán vị trí khi không còn quan sát được.

   Nối track **qua hai camera** cần nhiều bằng chứng hơn một track trong-camera: (1) timestamp đồng bộ giữa hai
   camera (cùng một mốc thời gian, không lệch frame), (2) calibration/hình học lắp đặt của cả hai camera để biết
   vùng seam thật sự chồng ở đâu và chuyển tọa độ giữa hai ảnh, (3) policy về output đích — ví dụ hệ thống cuối cần
   một ID duy nhất trên BEV hay chấp nhận giữ hai quan sát riêng và để tầng fusion xử lý. Thiếu bất kỳ điều nào
   trong ba điều trên, việc tự ghép track là suy diễn không có căn cứ (xem `docs/10-svm360-reading-vi.md`).

3. Nhìn lại cả buổi: một chỗ bạn tin nhãn mình đúng nhưng reference hoặc người soát nghĩ khác (dẫn frame/`object_ref`),
   bạn đã xử lý thế nào, và nếu làm lại slice này bạn sẽ đổi gì trong cách làm?

   Ca rõ nhất là `adasind_014670.jpg`, object `L1` (Bus theo annotator). QA (Quân, blind review, chưa thấy reference)
   nghi là Truck; sau khi mở reference thì đúng là reference cũng ghi Truck (IoU=0.733, `mismatching_label`), còn
   model độc lập lại dự đoán Bus — khớp với lựa chọn ban đầu của annotator, không khớp reference. Tôi (vai C) đã
   không mặc định tin reference đúng: mở lại ảnh gốc, phóng to vùng vật bị cắt biên (chỉ còn 83px), nhưng vẫn không
   đủ chi tiết (không thấy đầu xe hay cửa sau) để tự tin chọn một phía — nên xếp `why=E5_unresolved` thay vì ép về
   `E1_annotator_error` hay `E0_reference_defect`, và mở ticket escalate thay vì tự quyết. Nếu làm lại slice này,
   tôi sẽ đổi hai điều: (1) chụp ảnh bằng chứng **ngay khi QA nêu nghi vấn**, trước khi mở reference, để có bản ghi
   độc lập tại thời điểm chưa bị ảnh hưởng bởi reference; (2) với vật bị cắt biên nặng (<100px chiều rộng còn lại),
   chủ động thử tìm frame lân cận có cùng vật ở góc nhìn đầy đủ hơn (nếu track được) thay vì chỉ phân tích trên một
   frame tĩnh, vì đây chính là nguyên nhân gốc khiến bất đồng không thể giải quyết dứt điểm trong 240 phút.
