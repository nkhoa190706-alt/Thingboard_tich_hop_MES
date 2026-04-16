# 🚀 MES-Lite 3D - Hệ Thống Điều Hành & Giám Sát Hiệu Suất Lab In 3D

Đây là dự án **Nghiên cứu khoa học (NCKH)** về hệ thống điều hành sản xuất thu gọn (MES-Lite), tập trung vào việc giám sát thời gian thực và tối ưu hóa hiệu suất thiết bị (**OEE**) cho các máy in 3D.

## 🔗 Xem Demo Trực Tuyến
👉 [nkhoa190706-alt.github.io/Thingboard_tich_hop_MES/](https://nkhoa190706-alt.github.io/Thingboard_tich_hop_MES/)

## 🌟 Tính năng chính
* **Giám sát Real-time:** Theo dõi nhiệt độ, công suất và tiến độ in theo từng giây.
* **Quản trị OEE:** Tự động tính toán các chỉ số Khả dụng (A), Hiệu suất (P) và Chất lượng (Q).
* **Cấu hình động:** Kết nối linh hoạt với ThingsBoard qua IP và Token.

## 🏗 Kiến trúc kỹ thuật
1. **Edge Layer:** Chip ESP32 kết nối Serial với máy in 3D.
2. **Platform Layer:** ThingsBoard IoT Platform xử lý dữ liệu qua MQTT.
3. **Application Layer:** Dashboard phát triển bằng React.js và Tailwind CSS.
