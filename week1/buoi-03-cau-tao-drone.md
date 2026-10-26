# Buổi 3 – Cấu tạo drone: khung, motor, cánh quạt, ESC
*Tuần 1 · Module: Hiểu drone · Nối tiếp Buổi 2 (nguyên lý bay)*

## Mục tiêu
- Nhận diện và nêu chức năng của khung, motor, cánh quạt, ESC
- Đọc được thông số cơ bản: motor 2212 920KV, cánh quạt 1045, ESC 30A
- Biết linh kiện nào phải "khớp" với nhau

## Sơ đồ đường đi của năng lượng và lệnh
```
Pin LiPo ──► PDB (bảng chia điện) ──► ESC ×4 ──► Motor ×4 ──► Cánh quạt
                                         ▲
Flight Controller (bộ điều khiển bay) ───┘ (gửi tín hiệu tốc độ)
```

## Linh kiện
| Linh kiện | Ví dụ đời thường | Chức năng | Thông số cần biết |
|---|---|---|---|
| Frame (khung) | Bộ xương | Giữ mọi linh kiện, chịu va đập | Kích thước tính theo đường chéo motor–motor: F450 = 450 mm |
| Brushless motor (động cơ không chổi than) | Cơ bắp | Quay cánh quạt tạo lực đẩy | `2212` = stator rộng 22 mm, cao 12 mm; `920KV` = 920 vòng/phút cho mỗi 1 V |
| Propeller (cánh quạt) | Mái chèo | Đẩy không khí xuống | `1045` = đường kính 10 inch, bước (pitch) 4,5 inch; có cặp CW và CCW |
| ESC (bộ điều tốc động cơ) | Chân ga | Nhận lệnh từ flight controller, điều chỉnh dòng điện cho motor | `30A` = chịu dòng liên tục 30 A; phải lớn hơn dòng tối đa của motor |

## Quy tắc kết hợp
- **KV thấp + cánh to** (vd 920KV + 10 inch): lực đẩy mạnh, hiệu suất cao → drone chụp ảnh, khảo sát (F450).
- **KV cao + cánh nhỏ** (vd 2400KV + 5 inch): quay nhanh, linh hoạt → drone đua/FPV.
- ESC luôn chọn dư khoảng 20–30% so với dòng tối đa của motor.
- Đảo chiều motor bằng cách đổi 2 trong 3 dây motor–ESC (hoặc cấu hình trong phần mềm).

## An toàn
- Luôn **tháo cánh quạt** khi test motor, cấu hình hoặc kiểm tra trong nhà.
- Cánh quạt 10 inch quay ~8.000 vòng/phút có thể gây thương tích nặng.
- Lắp đúng cánh CW/CCW theo sơ đồ; lắp sai drone sẽ lật ngay khi cất cánh.

## Cần nhớ
1. Thông số motor `XXYY` = kích thước stator; `KV` = vòng/phút trên mỗi volt.
2. Thông số cánh quạt `1045` = 10 inch đường kính, 4,5 inch bước.
3. ESC phải chịu được dòng lớn hơn dòng tối đa motor kéo.
4. Motor KV thấp đi với cánh lớn, KV cao đi với cánh nhỏ.
