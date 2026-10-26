# Buổi 2 – Nguyên lý bay
*Tuần 1 · Module: Hiểu drone · Nối tiếp Buổi 1*

## Mục tiêu
- Giải thích 4 lực tác động lên drone
- Mô tả cách multirotor điều khiển roll, pitch, yaw bằng tốc độ motor
- Tính và hiểu thrust-to-weight ratio

## 4 lực cơ bản
| Lực | Ý nghĩa | Với multirotor |
|---|---|---|
| Thrust (lực đẩy) | Lực cánh quạt đẩy không khí xuống, drone bị đẩy lên | Lực chính giữ drone trên không |
| Weight (trọng lực) | Trái Đất kéo drone xuống | Gồm khung, pin, linh kiện, payload |
| Lift (lực nâng) | Lực hướng lên do luồng khí | Với multirotor, gần như chính là thrust |
| Drag (lực cản) | Không khí cản chuyển động | Tăng khi bay nhanh, gió mạnh |

Bay treo (hover): thrust = weight. Thrust > weight → bay lên; thrust < weight → hạ xuống.

## 3 trục chuyển động
```
        Mũi (trước)
     M1 (CW)   M2 (CCW)
          \     /
           [FC]
          /     \
     M4 (CCW)  M3 (CW)
```
| Trục | Chuyển động | Cách làm |
|---|---|---|
| Roll (lăn) | Nghiêng trái/phải | Tăng motor bên này, giảm bên kia |
| Pitch (chúc) | Chúc mũi lên/xuống | Tăng motor phía sau, giảm phía trước (để bay tới) |
| Yaw (xoay) | Xoay quanh trục đứng | Tăng cặp motor quay cùng chiều, giảm cặp còn lại |

Motor chéo nhau quay cùng chiều, hai cặp quay ngược chiều nhau → mô-men xoắn triệt tiêu, drone không tự xoay.

## Thrust-to-weight ratio (tỉ lệ lực đẩy/trọng lượng)
```
TWR = Tổng lực đẩy tối đa của các motor ÷ Trọng lượng cất cánh
```
- Ví dụ: 4 motor × 900 g = 3600 g; drone nặng 1500 g → TWR = 2,4
- Tối thiểu nên ≥ 2:1 cho drone chụp ảnh/khảo sát; drone đua thường 5:1 trở lên.

## Cần nhớ
1. Multirotor điều khiển mọi chuyển động chỉ bằng cách thay đổi tốc độ từng motor.
2. Motor chéo nhau quay cùng chiều để cân bằng yaw.
3. TWR < 2 → drone ì, khó giữ ổn định khi có gió.
