![esp32devkit-esp32](https://github.com/user-attachments/assets/10166062-7652-4c6a-b63d-9ca7c049f466)
![st7735](https://github.com/user-attachments/assets/c5dc0dd4-224e-4e41-9187-60ff677dd679)
![vn-11134207-7r98o-ly8496d3ea19f8-tn](https://github.com/user-attachments/assets/332e54d7-695e-49b4-b789-14f173c3b9d7)
![Good-Sound-Mini-Speaker-2-Inch-Bass-Sound-Horn-4ohm-3-Watt-Multimedia-Speaker-50mm](https://github.com/user-attachments/assets/e67e1b05-4ddd-494f-972c-cdb4dd8b84e6)
<img width="557" height="385" alt="Thiết kế chưa có tên" src="https://github.com/user-attachments/assets/430e4d17-54a4-4c0c-a557-a90798cc1da3" />
# ESP32 Bluetooth Speaker — MAX98357A + ST7735 + 3 nút

> Máy loa Bluetooth chuyên dụng dùng **ESP32** + **MAX98357A (I2S DAC & amp)** + **ST7735 1.8"** + **3 nút**. Dự án này tối ưu cho học tập và chế tạo: đầy đủ code `.ino`, sơ đồ nối dây, ảnh demo và hướng dẫn dùng.

---

## Mục lục

1. [Tổng quan](#tổng-quan)
2. [Tính năng nổi bật](#tính-năng-nổi-bật)
3. [Demo & ảnh / GIF](#demo--ảnh--gif)
4. [Phần cứng cần thiết (BOM)](#phần-cứng-cần-thiết-bom)
5. [Sơ đồ nối dây (Wiring)](#sơ-đồ-nối-dây-wiring)
6. [Cách lắp nhanh (Breadboard / PCB)](#cách-lắp-nhanh-breadboard--pcb)
7. [Phần mềm & thư viện](#phần-mềm--thư-viện)
8. [Cài đặt và nạp](#cài-đặt-và-nạp)
9. [Hướng dẫn sử dụng](#hướng-dẫn-sử-dụng)
10. [Chi tiết chức năng đặc biệt](#chi-tiết-chức-năng-đặc-biệt)
11. [Tinh chỉnh âm lượng / GAIN](#tinh-chỉnh-âm-lượng--gain)
12. [Debug & xử lý sự cố](#debug--xử-lý-sự-cố)
13. [Mở rộng & nâng cấp](#mở-rộng--nâng-cấp)
14. [Cài đặt Git & đẩy lên GitHub nhanh](#cài-đặt-git--đẩy-lên-github-nhanh)
15. [License & Credits](#license--credits)

---

# Tổng quan

Dự án này là một **loa Bluetooth di động/để bàn** do ESP32 điều khiển. ESP32 đóng vai trò **A2DP Sink** nhận luồng âm thanh từ điện thoại và xuất tín hiệu I2S tới module **MAX98357A** để khuếch đại và drive loa. Màn **ST7735 1.8"** hiển thị trạng thái, tên thiết bị, tên bài (nếu có metadata), cùng 1 UI nhỏ và bàn phím 3 nút để đổi tên Bluetooth.

Mục tiêu: _dễ build, dễ hiểu, hoạt động ổn định trên bo ESP32 phổ thông, có UI cơ bản phù hợp để demo trên GitHub_.

---

# Tính năng nổi bật

- ESP32 làm **Bluetooth A2DP Sink** (tên mặc định `Bach_LoaBluetooth`).
- Dùng **MAX98357A** (I2S → Class-D amp) để xuất ra loa 4–8Ω.
- Màn **ST7735 1.8"** hiển thị:
  - Tên Bluetooth
  - Trạng thái kết nối
  - Tên bài/metadata khi nhận được
  - Biểu tượng nốt nhạc + dấu chấm than khi đang phát
- **3 nút**: Play/Pause (STOP), Next, Prev (kết hợp để bàn phím rename).
- **Bàn phím 3 nút** trên màn hình: dùng PREV/NEXT để di chuyển chọn ký tự, STOP để chọn. Giữ STOP để hoàn thành.
- **Giữ STOP >= 1.5s** đưa vào chế độ đặt tên (Rename). **Giữ STOP >= 3s** là nút bí mật để tắt/mở màn hình (tiết kiệm điện).
- Phát **1 nốt nhạc** ngắn khi bắt đầu Play (buzzer) — có thể bật/tắt phần này trong code.
- Hỗ trợ đổi tên Bluetooth tại runtime (sử dụng `esp_bt_dev_set_device_name()` và restart A2DP).

---

# Demo & ảnh / GIF

> Thêm file ảnh/gif vào thư mục `images/` rồi chèn vào README như sau:

```md
![Demo toàn bộ](images/demo_all.png)
```

_Nên chuẩn bị: `images/demo_all.png` (ghép ảnh), `images/schematic.png` (sơ đồ nối dây), `images/demo_video.gif` (nếu muốn)._

---

# Phần cứng cần thiết (BOM)

- 1 × **ESP32** (WROOM hoặc tương đương)
- 1 × **MAX98357A module** (I2S DAC + Class-D amp)
- 1 × **ST7735 1.8 inch SPI display**
- 3 × **push button (momentary)**
- 1 × **Passive buzzer (tùy chọn)** — để phát nốt thông báo
- 1 × **Loa 4–8Ω** (công suất tùy chọn, 3W–5W recommended)
- Wires, breadboard hoặc PCB, transistor/MOSFET nhỏ nếu muốn điều khiển backlight an toàn
- (Tùy chọn) 100µF electrolytic cap cho nguồn module MAX98357A

---

# Sơ đồ nối dây (Wiring)

**MAX98357A ⇄ ESP32 (I2S)**

```
ESP32            MAX98357A
3V3   ----------> VIN
GND   ----------> GND
GPIO25 ----------> DIN (SDIN)
GPIO26 ----------> BCLK
GPIO22 ----------> LRCLK (WS)
SPK+  <---------> OUT+ (Loa +)
SPK-  <---------> OUT- (Loa -)
```

**ST7735 ⇄ ESP32 (SPI)**

```
ESP32            ST7735
3V3   ----------> VCC
GND   ----------> GND
GPIO18 ----------> SCK
GPIO19 ----------> MOSI (SDA)
GPIO5  ----------> CS
GPIO21 ----------> DC   (đã đổi từ 16)
GPIO23 ----------> RST  (đã đổi từ 17)
GPIO4  ----------> BL   (Backlight control, qua transistor nếu cần)
```

**Buttons** (pull-down to GND via button, code dùng `INPUT_PULLUP`):

```
GPIO32 --- button --- GND  (STOP)
GPIO33 --- button --- GND  (NEXT)
GPIO27 --- button --- GND  (PREV)
```

**Buzzer (optional)**

```
GPIO14 (BUZ_PIN) --> Passive buzzer --> GND
```

> Lưu ý: nếu backlight tiêu thụ > 20–30 mA, hãy dùng MOSFET / transistor để điều khiển nó — KHÔNG kéo trực tiếp từ GPIO.

---

# Cách lắp nhanh (Breadboard / PCB)

1. Kết nối ESP32 với MAX98357A theo bảng I2S ở trên. Đảm bảo GND chung.  
2. Nối ST7735 theo SPI pins. Nếu dùng wiring khác, chỉnh `#define` pin trong `main.ino`.  
3. Nối 3 nút: một chân nút ra GPIO, chân kia ra GND. (Code dùng `INPUT_PULLUP`).  
4. Nối loa vào OUT+ / OUT- của MAX98357A. Không nối loa trực tiếp vào GND.  
5. Cấp nguồn ổn định 3.3V hoặc 5V cho MAX98357A (5V sẽ cho công suất lớn hơn).  
6. Tải code `main.ino` (file `src/main.ino`) vào ESP32.

---

# Phần mềm & thư viện

- Arduino IDE hoặc PlatformIO
- Thư viện cần cài (Library Manager hoặc git):
  - `Adafruit GFX Library`
  - `Adafruit ST7735 and ST7789 Library`
  - `BluetoothA2DPSink` (ESP32 A2DP library)

> Nếu dùng PlatformIO, thêm dependencies tương ứng vào `platformio.ini`.

---

# Cài đặt và nạp

1. Mở Arduino IDE → `File > Preferences` → thêm board ESP32 (nếu chưa có) theo hướng dẫn Espressif.
2. Cài các thư viện trong Library Manager.
3. Mở `src/main.ino` (hoặc file `.ino` trong root) trong Arduino IDE.
4. Chọn board: ví dụ `ESP32 Dev Module` (hoặc WROOM), chọn đúng COM port.
5. Upload. Nếu gặp lỗi bộ nhớ, thử giảm buffer hoặc dùng phiên bản A2DP khác.

---

# Hướng dẫn sử dụng

- Bật ESP32 → màn hiện tên Bluetooth (mặc định `Bach_LoaBluetooth`).
- Trên điện thoại: quét Bluetooth → kết nối với `Bach_LoaBluetooth`.
- Play nhạc trên điện thoại → ESP32 nhận và phát qua loa.
- Controls:
  - **STOP** (GPIO32): nhấn ngắn — Play / Pause; nhấn giữ >= 1.5s → Rename; nhấn giữ >= 3s → toggle màn (secret)
  - **NEXT** (GPIO33): next track
  - **PREV** (GPIO27): previous track
- Rename: vào chế độ rename, dùng PREV/NEXT để chọn ký tự, STOP để chọn ký tự. Kết thúc: nhấn PREV+NEXT đồng thời hoặc giữ STOP để hoàn tất.

---

# Chi tiết chức năng đặc biệt

- **Icon nốt nhạc + chấm than**: khi `connected && isPlaying` màn sẽ hiển thị biểu tượng nhỏ (top-right) để biểu thị đang phát.
- **Buzzer beep khi Play**: khi bật lại playback từ nút STOP, thiết bị sẽ phát 1 nốt ngắn (điều chỉnh tham số tần số/thời lượng trong code nếu cần).
- **Đổi tên Bluetooth runtime**: sử dụng `esp_bt_dev_set_device_name()` để cập nhật tên, sau đó restart A2DP sink để áp dụng tên mới.

---

# Tinh chỉnh âm lượng / GAIN

- MAX98357A có chân **GAIN** để chọn mức khuếch đại (một số breakout board có jumper hoặc pad):
  - Float (mặc định): ~+9 dB
  - GND: +12 dB
  - VCC hoặc pull-up: mức thấp hơn (tham khảo datasheet breakout cụ thể)
- Nếu cần volume control phần mềm, bạn có thể implement attenuation bằng cách xử lý buffer PCM trước khi gửi (phức tạp hơn).  
- Khuyến nghị: bắt đầu với gain mặc định, nếu méo tiếng khi tăng to thì giảm gain hoặc dùng nguồn 5V cho MAX98357A.

---

# Debug & xử lý sự cố

**Không có âm thanh**:
- Kiểm tra VCC/GND, dây DIN/BCLK/LRCLK.
- Đảm bảo MAX98357A có nguồn đủ; thử cấp 5V nếu đang cấp 3.3V.  
- Kiểm tra loa nối giữa OUT+ và OUT- (không phải OUT- tới GND).  

**Màn hình không hiển thị**:
- Kiểm tra VCC/GND, chân RST, chân CS/DC/SCK/MOSI đúng.
- Kiểm tra `tft.initR(INITR_BLACKTAB)` phù hợp với module ST7735.
- Nếu dùng backlight control, thử bật trực tiếp BL lên HIGH tạm để test.

**Nút không hoạt động**:
- Kiểm tra dây GND chung và cài `INPUT_PULLUP` đã active.
- Kiểm tra chân không bị dùng cho chức năng đặc biệt (avoid GPIO6..11).

---

# Mở rộng & nâng cấp

- **Stereo**: dùng 2 × MAX98357A (một cho kênh L, một cho kênh R). Cần cấu hình SD/SD_MODE để board nhận L/R tương ứng; cả hai board dùng chung BCLK & LRCLK nhưng cần DIN khác hoặc chế độ chia slot. (Chi tiết ở phần advanced của repo).
- **Volume bằng nút**: thêm 2 nút nữa cho tăng/giảm volume và xử lý PCM attenuation.
- **Pinout tuỳ biến**: thay đổi `#define` trong code cho phù hợp bo mạch.
- **Tự động reconnect**: lưu last paired device và auto reconnect khi bật.

---

# Cài đặt Git & đẩy lên GitHub nhanh

```bash
git init
git add .
git commit -m "Initial commit: ESP32 Bluetooth Speaker"
git branch -M main
git remote add origin https://github.com/USERNAME/ESP32-Bluetooth-Speaker-ST7735.git
git push -u origin main
```

Thay `USERNAME` bằng tên GitHub của bạn.

---

# License & Credits

- Code trong repo: **MIT License** (tùy bạn chọn — sửa `LICENSE` nếu muốn GPL/Apache...).
- Thư viện & tài liệu tham khảo: Adafruit (ST7735), ESP32 A2DP library, datasheet MAX98357A.

---

## Ghi chú cuối

Nếu cần mình có thể:
- Tạo file `main.ino` sẵn (đã có trong `src/`) và upload ZIP để bạn kéo vào Arduino IDE.
- Vẽ schematic PNG (Fritzing) cho ảnh `images/schematic.png` để dán vào README.
- Tạo GIF demo `images/demo_all.gif` ghép sẵn các ảnh bạn gửi.

Nói mình biết muốn thêm phần nào — mình sẽ cập nhật README ngay.

