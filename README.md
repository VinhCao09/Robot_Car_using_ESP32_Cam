# 🚙Robot Car using ESP32 Cam. Upload code via OTA📷
In this project, we are going to build an ESP32 Cam Surveillance Car—a practical and accessible starting point to start learning about remote-controlled and ESP32 projects. This project leverages the versatility of the ESP32 camera module, offering a budget-friendly, energy-efficient and easy-to-make solution. Designed with simplicity and functionality in mind, it's an ideal starting point for newcomers to start learning web controls and the WiFi functionality of ESP32.
## Components
- ESP32-CAM Module camera
- Camera OV2640
- 4Pcs DC Electric Motor 3-6V Dual Shaft Geared TT Magnetic Gearbox Engine with 4P
- L298 DC Motor Driver 
- Battery 18650 3.7V x2
- BUZZER 5V 85dB
- Switch (ON/OFF)
- 4 Wheel 2 Layer Robot Smart Car Chassis Kits with Speed Encoder for Arduino DIY

## Overview

![images](https://github.com/VinhCao09/Robot_Car_using_ESP32_Cam/blob/main/images/1.jpg)
![images](https://github.com/VinhCao09/Robot_Car_using_ESP32_Cam/blob/main/images/2.jpg)

## Schematic
![images](https://github.com/VinhCao09/Robot_Car_using_ESP32_Cam/blob/main/images/4.jpg)

## OTA with Project
### OTA là gì?
Bạn có thể tham khảo bài viết sau để hiểu về OTA và cách vận hành OTA: https://www.iotzone.vn/esp32/esp32-ota-lap-trinh-qua-mang/

### Tại sao tôi lại dùng OTA?

Không giống như Arduino, ESP32, ESP8266,... hay một số vi điều khển khác. ESP32 Cam không được tích hợp sẵn mạch nạp trong module mà có một module khác hỗ trợ cho việc nạp code gọi là FTDI adapter. Việc này gây sự khó khăn cho việc update code vì khi ta đã gắn ESP32 Cam vào hệ thống Robot Car thì lúc này việc tháo ra nạp code rồi gắn vào lại khá phức tạp. Vì vậy trong chương trình nạp code lần đầu tiên, mình đã sử dụng đoạn code gốc kết hợp với một đoạn mã để khởi tạo OTA. Sau đó, chúng ta triển khai các đoạn mã OTA trong tất cả các đoạn code cần nạp vào ESP32 qua mạng cục bộ.

*Lưu ý: Nếu không khai báo mã OTA trong code, chúng ta không thể upload code mới vào ESP32 qua mạng cục bộ được nữa.*

## Version Recommend
*Version Arduino IDE:*
`2.3.2`

*Version Board:* esp32 by Espressif Systems - `2.0.6`

## How to use

Upload the code to the Arduino

`⚠️Board version esp32 3.0 or higher may cause ledc error. I use version 2.0.6 which is quite good`

Change the wifi configuration

```bash
    // Connect to Wi-Fi
    WiFi.begin("YOUR_SSID", "YOUR_PASSWORD");
    while (WiFi.status() != WL_CONNECTED) {
        delay(500);
        Serial.print(".");
    }
```
Change Static IP address configuration

*Ở đây tôi sử dụng IP tĩnh là 192.168.1.99, điều này có nghĩa rằng EPS32 Cam sẽ được cấp địa chỉ IP tĩnh này thay vì IP động, Nếu không có đoạn mã này, ESP32 Cam được cấp IP động một cách ngẫu nhiên khiến việc kết nối trở nên khó khăn*

```bash
   // Static IP address configuration
    IPAddress local_IP(192, 168, 1, 99);
    IPAddress gateway(192, 168, 1, 1);
    IPAddress subnet(255, 255, 255, 0);
    IPAddress primaryDNS(8, 8, 8, 8);   // optional
    IPAddress secondaryDNS(8, 8, 4, 4); // optional

    if (!WiFi.config(local_IP, gateway, subnet, primaryDNS, secondaryDNS)) {
        Serial.println("STA Failed to configure");
    }
```
Change the Pin configuration 
```bash
#define MOTOR_1_PIN_1    14
#define MOTOR_1_PIN_2    15
#define MOTOR_2_PIN_1    13
#define MOTOR_2_PIN_2    12
#define LED_PIN          4
#define BUZZER           2
#define LEDC_CHANNEL     0
#define LEDC_TIMER       0
    }
```

## Deploy

After uploading the code, you can access the same local network as the ESP32 Cam and access the IP address: `192.168.1.99`

![images](https://github.com/VinhCao09/Robot_Car_using_ESP32_Cam/blob/main/images/6.png)

## How to update via OTA?

Sau khi nạp code lần đầu bằng dây cáp, trong bộ code này đã bao gồm cả đoạn mã hỗ trợ nạp OTA qua Wifi Local. Bạn cần chắc chắn rằng kết nối của ESP32 Cam và từ máy tính bạn vào wifi là chung một mạng. Bạn cần lưu ý sơ đồ sau để tránh nạp code lỗi. 

![images](https://simplyexplained.com/uploads/2021-11-02-esp-idf-store-aws-iot-certificates-in-nvs-partition/esp32-factory-app-two-ota-definitions.svg)

Nếu muốn nạp code qua phương thức chứa OTA, bạn cần định nghĩa lại phân vùng. Khoảng 2MB sẽ được cấp để chúng ta có thể tải chương trình mã binary vào phân vùng này, sau khi nạp code sau, mã này sẽ được ESP32 Cam nạp vào bộ nhớ Flash và nó sẽ tự reset rồi tiến hành chạy lại đoạn code. 

Sau lần nạp code thông qua kết nối vật lý, lúc này bạn có thể nhìn trên Arduino IDE có thiết bị mang IP 192.168.1.99, bạn có thể nạp code không dây qua địa chỉ IP này. Hãy đảm bảo chọn đúng loại bo nhé!
![images](https://github.com/VinhCao09/Robot_Car_using_ESP32_Cam/blob/main/images/7.jpg)

Sau đó, bạn hãy chia lại phân vùng
![images](https://github.com/VinhCao09/Robot_Car_using_ESP32_Cam/blob/main/images/8.jpg)

Tiến hành nhấn vào biểu tượng upload code và nhập password, ở lần đầu nạp code bằng dây, mình đã set password là admin. Nếu bạn có sự thay đổi mật khẩu code thì nhập đúng mật khẩu nhé!

![images](https://github.com/VinhCao09/Robot_Car_using_ESP32_Cam/blob/main/images/9.jpg)

## 🚀 About Me
Hello 👋I am Vinh. I'm studying HCMC University of Technology and Education

**Major:** Electronics and Telecommunication

**Skill:** 

*- Microcontroller:* ESP32/8266 - ARDUINO - PIC - Raspberry Pi - PLC Rockwell Allen Bradley

*- Programming languages:* C/C++/HTML/CSS/PHP/SQL and
related Frameworks (Bootstrap)

*- Communication Protocols:* SPI, I2C, UART, CAN

*- Data Trasmissions:* HTTP, TCP/IP, MQTT
## Authors

- [@my_fb](https://www.facebook.com/vcao.vn)
- [@my_email](contact@vinhcaodatabase.com)

## Demo

👉Click on the icon below to watch the demo video:

[![Watch the video](https://media3.giphy.com/media/A7LF3J4uMJQ4r8ApLg/giphy.gif?cid=6c09b95275l1l3krhehcppcrgllmv64r7jd6py964efin2av&ep=v1_internal_gif_by_id&rid=giphy.gif&ct=s)](https://www.tiktok.com/@vinhcaoplay/video/7382913249139756306?lang=vi-VN)


https://github.com/user-attachments/assets/f18a0c5d-1bc3-4c33-acfe-f1d9587329c1


## Datasheet

ESP32 Cam: https://www.handsontec.com/dataspecs/module/ESP32-CAM.pdf 

![Logo](https://codingninja.asia/images/codeninjalogo.png)
