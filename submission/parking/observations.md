# Quan sát vạch ô đỗ

- **Hai vạch `parking_line` đã vẽ (mô tả vị trí trong ảnh):** đã vẽ 27 đoạn polyline theo các vệt sơn trắng chia ô
  đỗ, trải từ hàng gần (góc dưới ảnh, ví dụ đoạn từ khoảng (30, 680) đến (26, 720) và đoạn từ (405, 652) đến
  (527, 720) — hai vạch chéo rõ nét ở tiền cảnh) cho tới hàng xa gần đường chân trời/hàng cây (các đoạn ngắn quanh
  y≈463–511, bị nén hình học do góc máy thấp nhìn xa). Toàn bộ các đoạn này bám theo vệt sơn trắng thật phân chia
  từng ô đỗ, dừng đúng chỗ vệt sơn mờ đi hoặc bị xe/góc khuất che.
- **Một vạch/dấu sơn hoặc biên không vẽ, và vì sao:** dải sơn màu vàng chạy dọc theo chân hàng rào/hàng cây ở biên
  xa của bãi đỗ (phía trên cùng vùng có thể quan sát, dọc theo `free_space` phía xa) **không** được vẽ thành
  `parking_line`. Đây là vạch biên/curb đánh dấu ranh giới bãi đỗ với khu vực ngoài (gần hàng rào gỗ), không phải
  vệt sơn chia giữa hai ô đỗ cạnh nhau — đúng theo hướng dẫn "không lấy vạch làn/lối xe chạy hay mép đường làm
  `parking_line`" ở `docs/11-parking-lines-vi.md`.
- **Polygon `free_space` dừng ở đâu; có phần bị che nào không:** polygon trải từ mép dưới ảnh lên tới dải sơn vàng
  biên xa vừa nêu (không vẽ tràn qua dải sơn đó ra ngoài bãi đỗ), và có một chỗ khuyết rõ ràng ở khoảng x≈193–230,
  y≈463–479 để **loại trừ vùng chiếc xe con màu đỏ đang đỗ gần trung tâm ảnh** — polygon không vẽ xuyên qua thân xe.
- **Ca chưa chắc cần hỏi người soát (nếu không có, ghi "không có"):** các đoạn polyline ngắn ở hàng xa (gần đường
  chân trời, y≈463–511) bị nén rất nhỏ do góc máy thấp và ảnh có phần cháy sáng/ám trắng ở nền trời — khó khẳng
  định 100% từng đoạn ngắn là vệt sơn thật hay là nhiễu/lóa sáng trên mặt đường. Đề xuất người soát mở ảnh gốc ở độ
  phân giải đầy đủ (không qua bản nén) để xác nhận lại nhóm đoạn này trước khi tính là đã xong.
