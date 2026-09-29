# Quan sát vạch ô đỗ

- Hai vạch `parking_line` đã vẽ (mô tả vị trí trong ảnh):
  1. Vạch sơn trắng tiền cảnh ở trung tâm phía dưới (polyline tọa độ từ khoảng [404.8, 651.7] đến [526.8, 720.0]): Vạch tạo ranh giới thực sự chia giữa hai ô đỗ xe liền kề ở hàng tiền cảnh, hướng xiên chéo từ lòng bãi đỗ xuống đáy ảnh.
  2. Vạch sơn trắng ở tiền cảnh bên trái (polyline tọa độ từ khoảng [29.6, 679.6] đến [25.7, 720.0]): Vạch phân chia ranh giới ô đỗ ngoài cùng bên trái hàng tiền cảnh, bám sát phần sơn nhìn thấy được trên mặt đường.
  (Ngoài ra trong bản export còn có các vạch chia ô riêng lẻ rõ ràng ở dãy giữa như vạch xiên từ [169.9, 519.9] đến [250.2, 564.5] và [276.9, 515.8] đến [420.0, 553.7]).

- Một vạch/dấu sơn hoặc biên **không** vẽ, và vì sao:
  - Dải sơn dài chạy ngang gần như toàn bộ chiều rộng bãi đỗ (chạy từ x≈2.6 qua x≈697.3 đến x≈959.7 tại cao độ y≈507–541): Đây là vạch phân cách giới hạn giữa làn lưu thông nội bộ (lối xe chạy / driving aisle) và các dãy ô đỗ, KHÔNG PHẢI là vạch phân chia từng ô đỗ riêng lẻ (`parking_line`). Theo quy định tài liệu `docs/11-parking-lines-vi.md`, các dải sơn dài chỉ lối xe chạy, mép đường hay vạch qua đường không được gắn nhãn `parking_line` chỉ vì nó nằm trong bãi đỗ. (Lưu ý: Trong file export nháp của Lan Anh có chứa polyline này tại index 4, nhóm thống nhất ghi nhận đây là vạch mép lối xe chạy, không tính vào vạch chia ô đỗ theo rubric).

- Polygon `free_space` dừng ở đâu; có phần bị che nào không:
  - Vùng `free_space` đã vẽ bao phủ diện tích mặt đường nhìn thấy được của lối xe chạy chính trong bãi đỗ (trải rộng từ đáy ảnh y=720 lên đến ranh giới dãy ô đỗ phía trên ở y≈463–472).
  - Về mép biên và vật cản: Polygon dừng lại ngay sát chân mép bồn cây/curb và hàng rào viền bãi đỗ phía trên. Đặc biệt tại vùng tọa độ x≈194.0–220.6, y≈462.9–479.0, polygon đã được vẽ lõm vào để né hoàn toàn gờ curb/chân cột tối màu nhô ra, đảm bảo tuyệt đối không xuyên qua curb, vật cản hay cây cối. Do bãi đỗ trong ảnh core trống hoàn toàn ở lối đi nên không có hiện tượng xuyên qua xe cộ hay vùng bị che khuất.

- Ca chưa chắc cần hỏi người soát (nếu không có, ghi “không có”):
  - Nhờ người soát làm rõ quy ước: Dải sơn dài chạy ngang bãi (phân định mép lối đi và ô đỗ) có nên xóa hẳn khỏi nhãn CVAT để tránh nhầm lẫn với vạch chia ô đỗ riêng lẻ hay không, hay chỉ cần chú thích phân biệt rõ trong observations.md.

