# UAV Learning Journey
Hành trình tự học UAV/Drone từ con số 0 – theo khung FPT Cranes
"UAV Systems & Autonomous Drone Operations".

**Người học:** Phúc – nền tảng quản lý vận hành & đào tạo nhân sự, chuyển hướng sang ngành UAV/drone.
**Định hướng nghề nghiệp:** Drone Operations & Mission Engineer · Drone Testing & Validation Engineer.

## Lộ trình Giai đoạn 1 (4 tuần, mô phỏng)
- [ ] Tuần 1: Nguyên lý bay, cấu tạo, LiPo, pháp lý & an toàn
- [ ] Tuần 2: Cảm biến, PID, ArduPilot/PX4, Ground Control Station
- [ ] Tuần 3: SITL, mission, geofence, failsafe, đọc log
- [ ] Tuần 4: Python, MAVLink/MAVSDK, script tự bay, dự toán phần cứng

Giai đoạn 2 (tháng 2–3): lắp ráp & bay drone thật F450 + Pixhawk.
Giai đoạn 3 (từ tháng 4): bay tự hành nâng cao, Computer Vision, payload, vận hành thương mại.

## Cấu trúc repo
| Thư mục | Nội dung |
|---|---|
| `week1/` … `week4/` | Ghi chú từng buổi học, sản phẩm đầu ra của tuần |
| `missions/` | File mission (`.waypoints`, `.plan`) chạy trên SITL |
| `scripts/` | Script Python (MAVSDK / pymavlink) |
| `logs/` | Log bay và báo cáo phân tích log |
| `docs/` | SOP vận hành, checklist an toàn, dự toán linh kiện |
| `JOURNAL.md` | Nhật ký học tập theo buổi |

## Sản phẩm đầu ra
| Tuần | Sản phẩm | Trạng thái |
|---|---|---|
| 1 | Sơ đồ cấu tạo drone + bảng chức năng linh kiện | ⏳ |
| 2 | Ghi chú "PID là gì" bằng lời của mình | ⏳ |
| 3 | Mission tự động 5–8 điểm chạy thành công trên SITL | ⏳ |
| 4 | Script tự cất cánh – bay ô vuông – hạ cánh + dự toán drone thật | ⏳ |
| Capstone | Video demo mission hoàn chỉnh + thuyết trình 5 phút | ⏳ |

Xem chi tiết tiến độ trong [JOURNAL.md](JOURNAL.md).
