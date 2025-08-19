![esp32devkit-esp32](https://github.com/user-attachments/assets/10166062-7652-4c6a-b63d-9ca7c049f466)
![st7735](https://github.com/user-attachments/assets/c5dc0dd4-224e-4e41-9187-60ff677dd679)
![vn-11134207-7r98o-ly8496d3ea19f8-tn](https://github.com/user-attachments/assets/332e54d7-695e-49b4-b789-14f173c3b9d7)
![Good-Sound-Mini-Speaker-2-Inch-Bass-Sound-Horn-4ohm-3-Watt-Multimedia-Speaker-50mm](https://github.com/user-attachments/assets/e67e1b05-4ddd-494f-972c-cdb4dd8b84e6)
<img width="557" height="385" alt="Thiết kế chưa có tên" src="https://github.com/user-attachments/assets/430e4d17-54a4-4c0c-a557-a90798cc1da3" />
🎵 ESP32 Bluetooth Speaker with MAX98357A + ST7735
🇻🇳 Loa Bluetooth ESP32 + MAX98357A + ST7735
📌 Introduction / Giới thiệu

This project turns an ESP32 into a portable Bluetooth speaker with a 1.8" ST7735 TFT screen to display status, support renaming via on-screen keyboard, and music control with 3 physical buttons.
Dự án này biến ESP32 thành một loa Bluetooth mini với màn hình TFT ST7735 1.8" hiển thị trạng thái, hỗ trợ đổi tên Bluetooth trực tiếp, và điều khiển nhạc bằng 3 nút bấm vật lý.

⚡ Features / Tính năng

Bluetooth A2DP connection (music streaming).

Audio output via MAX98357A I2S DAC.

TFT screen shows device name, status, song info, and 🎵❗ symbol when playing.

3 buttons:

STOP → Play/Pause (hold 1.5s: rename Bluetooth, hold 3s: toggle screen).

NEXT → Next track.

PREV → Previous track.

On-screen virtual keyboard for renaming.

Power saving: turn screen ON/OFF manually.

🛠️ Hardware / Phần cứng

ESP32 DevKit (WROOM/WROVER).

MAX98357A I2S amplifier.

ST7735 1.8" TFT display (SPI).

3 push buttons.

4–8 Ω speaker (3W–5W).

5V USB power or Li-ion battery + charger module.

🔌 Wiring / Kết nối
MAX98357A (I2S)
MAX98357A	ESP32
LRC (WS)	GPIO 22
BCLK	GPIO 26
DIN	GPIO 25
GND	GND
VIN	5V

(GAIN pin sets default volume: GND=12 dB, Float=9 dB, VCC=6 dB, etc.)

TFT ST7735 (SPI)
ST7735	ESP32
CS	GPIO 5
DC	GPIO 16
RST	GPIO 17
SDA	GPIO 23
SCK	GPIO 18
VCC	3.3V
GND	GND
BL	GPIO 4
Buttons / Nút bấm
Button	ESP32
STOP	GPIO 32
NEXT	GPIO 33
PREV	GPIO 27
🚀 Usage / Hướng dẫn sử dụng

Flash firmware via Arduino IDE / PlatformIO.

Default Bluetooth name: Bach_LoaBluetooth.

Pair with phone/laptop and start playback.

Use buttons to control music.

Hold STOP 1.5s → rename Bluetooth (via virtual keyboard).

Hold STOP 3s → toggle screen (power saving).

📷 Demo

(Add wiring diagram, real build photo, or demo GIF here)

📜 License / Giấy phép

MIT License – Free to modify and use for personal or commercial projects.
