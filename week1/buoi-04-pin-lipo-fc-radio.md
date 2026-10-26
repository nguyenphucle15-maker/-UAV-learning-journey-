# Buổi 4 – Pin LiPo, Flight Controller và hệ thống điều khiển vô tuyến
*Tuần 1 · Module: Hiểu drone · Nối tiếp Buổi 3 (khung, motor, cánh quạt, ESC)*

## Mục tiêu
- Đọc nhãn pin LiPo (vd `3S 5000mAh 30C`) và tính điện áp, Wh, dòng tối đa, thời gian bay ước lượng
- Giải thích flight controller làm gì; vẽ đường điện và đường tín hiệu trong drone
- Hiểu các channel (kênh) của tay điều khiển; viết SOP an toàn pin LiPo đầu tiên

## 1. Pin LiPo (Lithium Polymer)
Ví dụ đời thường: pin như bình xăng. **Điện áp** = áp lực đẩy, **mAh** = dung tích bình, **C-rating** = độ to của vòi xả.

### Điện áp một cell
| Trạng thái | V / cell |
|---|---|
| Sạc đầy | 4,20 |
| Danh định (in trên nhãn) | 3,70 |
| Cất giữ (storage) | ~3,80–3,85 |
| Nên hạ cánh (nghỉ, không tải) | ~3,5 |
| Tối thiểu tuyệt đối | 3,0 – thấp hơn là hỏng vĩnh viễn |

`S` = số cell mắc nối tiếp. 3S = 11,1 V danh định / 12,6 V đầy (loại dùng cho F450).

### Công thức
| Đại lượng | Công thức | Ví dụ 3S 5000mAh 30C |
|---|---|---|
| Dòng xả tối đa (A) | C × Ah | 30 × 5 = 150 A |
| Năng lượng (Wh) | V danh định × Ah | 11,1 × 5 = 55,5 Wh |
| Thời gian bay (phút) | (Ah × 0,8) ÷ dòng TB × 60 | 4 ÷ 16 × 60 ≈ 15 phút |

- Hệ số 0,8: chỉ dùng ~80% dung lượng để bảo vệ pin và giữ dự phòng.
- C-rating trên nhãn thường bị ghi cao hơn thực tế → coi là giới hạn trên.
- Wh quan trọng khi mang pin lên máy bay (hãng bay quy định theo Wh).

### Hai dây trên pin
- **Dây xả chính** (đầu XT60): cấp điện cho drone.
- **Dây cân bằng (balance lead)**: S+1 sợi (3S có 4 sợi), cắm vào sạc để sạc cân bằng từng cell.

Sạc cân bằng giữ các cell bằng nhau; một cell bị sạc quá áp (vd 4,35 V) có thể phồng hoặc cháy.

## 2. Flight Controller (FC – bộ điều khiển bay)
Ví dụ đời thường: người quản lý ca – liên tục nhận thông tin, so với mục tiêu, ra lệnh điều chỉnh, hàng trăm lần mỗi giây.

```
Cảm biến (IMU, baro, GPS, compass)  ─┐
                                     ├─► FC tính toán ─► Lệnh cho 4 ESC ─► 4 motor
Lệnh từ phi công (receiver)         ─┘   (lặp ~400 lần/giây)
```

| Bộ phận / cổng (Pixhawk) | Chức năng |
|---|---|
| Processor (vi xử lý) | Chạy firmware ArduPilot hoặc PX4 |
| IMU (khối đo quán tính) | Đo nghiêng và xoay |
| Barometer (cảm biến áp suất) | Ước tính độ cao |
| GPS / I2C | Cắm GPS kèm compass |
| RCIN | Nhận tín hiệu từ receiver |
| MAIN OUT | Xuất tín hiệu tới ESC |
| TELEM | Telemetry radio về máy tính |
| POWER | Nhận 5 V từ power module |

**Power module (mô-đun nguồn):** (1) hạ áp xuống 5 V nuôi FC; (2) đo điện áp và dòng của pin để FC biết khi nào pin yếu và tự RTL (Return To Launch).

**Pixhawk 2.4.8:** bản clone của Pixhawk 1 – rẻ, nhiều tài liệu, nhưng chất lượng không đồng đều; một số bản cần firmware `Pixhawk1-1M` trên ArduPilot. Sẽ so sánh với FC mới hơn ở Buổi 23.

### Sơ đồ đường điện (═) và tín hiệu (─)
```
Pin LiPo 3S ══► Power Module ══► PDB ══► 4 ESC ══► 4 Motor
                    │                       ▲
                    │ 5V + dữ liệu pin      │ tín hiệu PWM/DShot
                    ▼                       │
                  FC (Pixhawk) ─────────────┘
                    ▲      ▲          ▲
              Receiver  GPS+Compass  Telemetry radio
```

## 3. Hệ thống điều khiển vô tuyến (RC)
- **Transmitter / Tx (tay phát)** – cầm trên tay; **Receiver / Rx (bộ thu)** – gắn trên drone, cắm vào RCIN.
- Hai thiết bị phải **bind** (ghép đôi). Phổ biến băng tần 2,4 GHz.

| Kênh | Điều khiển | Cần (Mode 2) |
|---|---|---|
| CH1 | Roll | Phải, ngang |
| CH2 | Pitch | Phải, dọc |
| CH3 | Throttle (ga) | Trái, dọc |
| CH4 | Yaw | Trái, ngang |
| CH5 | Flight mode | Công tắc |

- Giá trị mỗi kênh ~1000–2000 µs.
- Tập **Mode 2** (ga bên trái) ngay từ đầu.
- Giao thức Rx → FC: PWM, PPM, SBUS, iBus; hệ mới ELRS/CRSF. FlySky i6 + FS-iA6B (PPM/iBus) đủ cho Giai đoạn 2.
- **RC failsafe:** mất sóng → FC tự RTL hoặc hạ cánh. Không bao giờ bay thật khi chưa kiểm tra failsafe (học kỹ ở Buổi 16).
- **Telemetry radio** (433/915 MHz) truyền dữ liệu về Mission Planner. Kiểm tra quy định sử dụng tần số tại Việt Nam (Cục Tần số vô tuyến điện) trước khi mua/dùng.

## An toàn (áp dụng từ Giai đoạn 2)
- Tháo cánh quạt khi cắm pin để cấu hình trong nhà.
- **Cắm pin sau cùng, rút pin trước tiên.** Bật tay phát trước khi cắm pin drone; tắt tay phát sau khi rút pin.
- Sạc bằng sạc cân bằng, có người trông, trong túi chống cháy; để pin nguội sau bay rồi mới sạc.
- Không dùng pin phồng. Không dùng 2–3 ngày → xả/sạc về ~3,8 V/cell để cất.
- Tuân thủ quy định hiện hành của Việt Nam về UAV và xin phép vùng trời; kiểm tra văn bản pháp luật mới nhất.

## Cần nhớ
1. 1 cell LiPo: **4,2 V đầy · 3,7 V danh định · 3,8 V cất giữ · 3,5 V hạ cánh · không dưới 3,0 V**.
2. **Dòng max = C × Ah · Wh = V × Ah · Thời gian bay ≈ (Ah × 0,8) ÷ A × 60**.
3. FC là bộ não; power module vừa cấp điện vừa đo pin.
4. CH1 Roll · CH2 Pitch · CH3 Throttle · CH4 Yaw · Mode 2.
5. Failsafe pin và failsafe mất sóng phải được cấu hình trước mọi chuyến bay thật.

## Thực hành
- [ ] Bài 1 – Điền bảng đọc nhãn pin: `3S 5000mAh 30C`, `4S 1500mAh 100C`, `6S 10000mAh 25C`
- [ ] Bài 2 – Google Sheets `LiPo_Calculator` (công thức bên dưới), thử đổi dung lượng 3000/6000 mAh và ghi nhận xét
- [ ] Bài 3 – Viết `docs/SOP-01_LiPo.md` (quản lý pin LiPo: kiểm tra, sạc, sau bay, cất giữ, vận chuyển, sự cố, thải bỏ, nhật ký pin)
- [ ] Bài 4 – Simulator: gán 4 kênh Mode 2, chế độ Angle; hover 30 giây trôi không quá ~2 m, lặp 10 lần

Công thức Sheets (cột: A tên pin · B số cell · C mAh · D C-rating · E dòng hover A):
```
F2:  =B2*3.7                  // V danh định
G2:  =B2*4.2                  // V đầy
H2:  =F2*C2/1000              // Wh
I2:  =D2*C2/1000              // dòng max (A)
J2:  =(C2/1000*0.8)/E2*60     // thời gian bay (phút)
```
*Định dạng Việt Nam: dùng `3,7` và dấu `;` nếu công thức báo lỗi.*

## Kiểm tra
1. Pin 4S 3000mAh 40C: V danh định, Wh, dòng max?
2. Pin 3S sau bay: 3,92 / 3,90 / 3,61 V – có vấn đề gì, xử lý thế nào?
3. Power module có chức năng nào? (A chia điện ESC · B cấp 5 V cho FC và đo pin · C nhận tín hiệu tay điều khiển · D tăng áp)
4. Vì sao bắt buộc cấu hình RC failsafe trước khi bay thật?
5. Không dùng drone 2 tuần, pin đang 4,2 V/cell – xử lý thế nào?

## Nguồn đọc thêm
- ArduPilot – Power Module, Radio Control Systems (ardupilot.org/copter → First Time Setup)
- Oscar Liang – LiPo battery guides (oscarliang.com)
- Painless360 (YouTube) – LiPo và radio cho người mới
