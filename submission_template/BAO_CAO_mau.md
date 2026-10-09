# Báo cáo lab: chọn tracker cho 5 video

**Nhóm:** Cá nhân **Thành viên:** Tran Cao Quoc Dinh — MSSV: 2A202602939

Detector cố định: `yolo26n.pt`, ảnh 640 px, Re-ID `osnet_x0_25_msmt17`. Không đổi các mục này trong bài nộp chính.

## 1. Cấu hình đã chọn

Mỗi video: tracker bạn nộp, `conf`, `iou`, điều bạn **nhìn thấy** trên video, và một cấu hình đã thử rồi loại.

| Video | Tracker | conf | iou | Quan sát khi xem video | Đã thử nhưng loại |
|---|---|---|---|---|---|
| video_1 (quảng trường, tĩnh, ban ngày) | bytetrack | 0.3 | 0.5 | Chạy đủ 600 frame; camera tĩnh, kết quả có độ bao phủ ổn định nhưng vẫn bỏ sót nhiều người nhỏ. | Thử conf cao hơn sẽ dễ bỏ sót người nhỏ; giữ cấu hình 0.3/0.5. |
| video_2 (phố đêm, tĩnh, rất đông) | bytetrack | 0.3 | 0.5 | Chạy đủ 1050 frame; cảnh đông và tối nên ưu tiên giữ hộp phát hiện, chấp nhận một số ID đổi khi người cắt nhau. | conf cao hơn có nguy cơ mất người trong vùng tối. |
| video_3 (camera di động, ảnh nhỏ) | bytetrack | 0.3 | 0.5 | Chạy đủ 837 frame; ảnh nhỏ và camera di động làm track ngắn, cần ngưỡng vừa để tránh bỏ sót. | conf thấp hơn dễ tăng hộp nhiễu. |
| video_4 (trong nhà, camera di chuyển) | bytetrack | 0.3 | 0.5 | Chạy đủ 900 frame; chuyển động camera và phản chiếu làm ID có thể gián đoạn. | conf thấp hơn có thể giữ thêm nhiễu từ phản chiếu. |
| video_5 (trên xe bus, giao lộ đông) | bytetrack | 0.3 | 0.5 | Chạy đủ 750 frame; rung lắc và mật độ cao làm tăng nguy cơ đổi ID lúc các hộp giao nhau. | conf cao hơn dễ mất người ở frame rung. |

## 2. Số liệu video_1

Dán bảng HOTA / MOTA / IDF1 do `scripts/evaluate_practice.py` in ra.

```
HOTA: 26.911 · MOTA: 17.297 · IDF1: 25.714 (video_1, ByteTrack, conf=0.3, iou=0.5).
```

`video_2` đến `video_5` không có nhãn trong gói lab. Không điền số cho các video đó.

## 3. Phân tích

Ở video_1, ByteTrack có độ chính xác hộp cao trên các hộp đã phát hiện (precision 96.917%) nhưng độ bao phủ thấp (recall 17.932%), thể hiện qua MOTA 17.297 và HOTA 26.911. Cảnh camera tĩnh giúp theo chuyển động hoạt động ổn định, nhưng người nhỏ hoặc bị che khuất vẫn bị bỏ sót. Với video_2, cảnh tối và rất đông làm việc giữ hộp quan trọng hơn; conf=0.3 là mức cân bằng giữa bỏ sót và nhiễu. Các video camera di động và rung lắc có thể hưởng lợi từ tracker có Re-ID trong lần thử tiếp theo.

## 4. Nếu có thêm thời gian

Nếu có thêm thời gian, tôi sẽ so sánh ByteTrack với StrongSORT ở video_2 và video_5 để kiểm tra Re-ID có giảm đổi ID khi người đi ngang nhau hay không. Tôi cũng sẽ quét `conf` mịn hơn quanh 0.2–0.4 và xem các frame gây bỏ sót nhiều nhất.
