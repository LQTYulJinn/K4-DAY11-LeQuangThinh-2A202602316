# Sensor context

- **Rig:** ADASIND không kèm tài liệu rig chi tiết, ghi theo quan sát trực tiếp trên ảnh của slice B1-edge. Camera
  gắn trên một xe hai bánh (xe máy/xe đạp điện) — không phải ô tô — vì `R07` gọi rõ phần ego là "thân xe/gương/**tay
  lái**", và ở frame `adasind_014670.jpg` vùng góc dưới trái (x: 0–295, y: 1138–1688) thấy rõ tay lái, gương chiếu
  hậu, và một phần cánh tay/thân/chân người — nhiều khả năng là camera gắn ở ngực hoặc trên tay lái của chính người
  lái xe ghi hình, không phải một xe hai bánh khác đi phía trước. Đây chính là căn cứ để xác nhận nhận xét P0 của QA
  (`qa_review.md`: box `L7` bị nhầm là `Bike` ngoại lai) là đúng — xem `submission/screenshots/adasind_019560.jpg_calib_L7_L8_duplicate_rider.png`
  cho một ca liên quan (rider) ở vòng calib.
- **`ego_body` nhìn thấy ở đâu trong frame:** ở góc dưới-trái khung hình (dưới đường chân trời của vòng kính,
  khoảng y > 1100 trên ảnh 1080×1920), gồm tay lái, gương, và một phần thân/tay/chân của người lái xe ghi hình —
  không phải toàn bộ nửa dưới khung hình. Ba frame của slice (`adasind_001320.jpg`, `adasind_014670.jpg`,
  `adasind_034080.jpg`) đều có vùng này nhưng r1_craft hiện **chưa vẽ polygon `ignore_region` (reason=ego_body)**
  cho vùng đó ở cả 3 frame — đây là lỗi P0 cần A bổ sung khi rework theo `qa_review.md`.
- **Vòng kính (lens circle) nằm ở vị trí nào trong ảnh, chiếm khoảng bao nhiêu phần khung hình:** theo
  `assets/frames.csv`, tâm vòng kính của 3 frame trong slice dao động quanh `cx≈456–606` (khoảng 42–56% chiều rộng
  1080px, hơi lệch trái so với tâm ảnh), `cy≈891–922` (khoảng 46–48% chiều cao 1920px, gần giữa theo chiều dọc),
  bán kính `r≈810–812` (khoảng 75% chiều rộng ảnh). Vì ảnh 1080×1920 cao hơn nhiều so với đường kính vòng kính
  (~1620px), vòng kính bị cắt ở hai bên trái/phải và chừa khoảng đen (`lens_border` ignore_region) ở dải trên cùng
  và dưới cùng khung hình — đúng như hai polygon `lens_border` đã được công cụ import sẵn trong mỗi frame (R08).
