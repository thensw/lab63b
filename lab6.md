# การทดลองที่ 6 เรื่อง การเขียนโปรแกรมสร้างไวไฟแอคเซสพอยต์ (Wifi AP)
## วัตถุประสงค์
1. เพื่อศึกษาวิธีการสร้างwifi access point
2. เพื่อศึกษาวิธีการเขียน/อ่านค่าของโปรแกรมที่ใช้ในการทดลอง
## อุปกรณ์ที่ใช้
1. ไมโครคอนโทรลเลอร์ (ESP01)
2. สายUSB(ไฟเลี้ยง)
3. programmer / usb to serial
## ศึกษาข้อมูลเบื้องต้น
คลิปตัวอย่างการทดลอง https://www.youtube.com/watch?v=T26DVHePlTs
## วิธีการทำการทดลอง
1. เปิดโปรแกรมตัวอย่าง (ในที่นี้เลือกโปรแกรมที่6)
พิมพ์ cd pattani (ชื่อโฟลเดอร์ที่จะเปิด)
พิมพ์ cd 06_Wifi-AP-Web-Server
พิมพ์ vi src/main.cpp (เพื่อแสดงเนื้อหาโปรแกรม)
```javascript
#include <ESP8266WiFi.h>
//#include <WiFiClient.h>
#include <ESP8266WebServer.h>

const char* ssid = "MY-ESP8266";
const char* password = "choompol";

IPAddress local_ip(192, 168, 1, 1);
IPAddress gateway(192, 168, 1, 1);
IPAddress subnet(255, 255, 255, 0);

ESP8266WebServer server(80);

int cnt = 0;

void setup(void){
	Serial.begin(115200);

	WiFi.softAP(ssid, password);
	WiFi.softAPConfig(local_ip, gateway, subnet);
	delay(100);

	server.onNotFound([]() {
		server.send(404, "text/plain", "Path Not Found");
	});

	server.on("/", []() {
		cnt++;
		String msg = "Hello cnt: ";
		msg += cnt;
		server.send(200, "text/plain", msg);
	});

	server.begin();
	Serial.println("HTTP server started");
}

void loop(void){
  server.handleClient();
}
```
* ตั้งชื่อwifiและรหัสผ่านที่ต้องการ ที่บรรทัดที่ 4,5
* กำหนดค่า IPAddress ที่บรรทัด 7-9

![25](https://user-images.githubusercontent.com/80879818/112378711-d8724b80-8d19-11eb-931f-7dd7cb192b61.jpg)

> * เปิด web server ที่พอร์ท80
> จากโค้ดเบื้องต้น แบ่งข้อมูลออกเป็น2ส่วน คือ
> * set up
>   * เตรียม web server ไว้1ตัว
>   * สร้างaccess pointโดยการรันคำสั่ง`solfAP`จากนั้นกำหนดssidและpassword
>   * ใส่ค่าIPAddress ที่กำหนดไว้
>   
>   ![33](https://user-images.githubusercontent.com/80879818/112379475-c93fcd80-8d1a-11eb-9e27-9b95114df222.jpg)
> * loop
2. ต่อไมโครคอนโทรลเลอร์เข้ากับตัวprogrammer
3. อัปโหลดโปรแกรมเข้าสู่ไมโครคอนโทรลเลอร์
* พิมพ์ pio run -t upload
  * ในขณะที่โปรแกรมกำลังอัปโหลด `กดปุ่มสีดำบนตัวprogrammer` เพื่อให้ตัวไมโครคอนโทรลเลอร์รับโปรแกรมใหม่เข้าไป
  * จากนั้น `กดปุ่มสีแดงบนตัวprogrammer` เพื่อทำการรีเซต
4. แสดงผลลัพธ์จากโปรแกรมผ่านคอมพิวเตอร์
* พิมพ์ pio device monitor
* กดปุ่ม reset เพื่อทดสอบการทำงานอีกครั้ง
5. ใช้โทรศัพท์มือถือหรือเครื่องมือสื่อสารต่างๆในการค้นหาwifiตัวที่พึ่งสร้าง เพื่อทดสอบว่าwifiที่ทำการทดลองนั้นถูกสร้างขึ้นมาจริงหรือไม่
## การบันทึกผลการทดลอง
จากการทดลองนี้พบว่าwifiสามารถสร้างขึ้นเองได้จริงและสามารถเชื่อมต่อกับอุปกรณ์สื่อสารได้ ดังภาพ

![ไฟ](https://user-images.githubusercontent.com/80879818/112380564-183a3280-8d1c-11eb-8624-f25014203328.jpg)

## อภิปรายผลการทดลอง
* คำสั่ง softAP ใช้ในการสร้าง access point
* การทดลองที่6 นี้จะแตกต่างจากการทดลองที่5 ตรงที่การทดลองที่5ใช้wifiที่มีอยู่แล้วในการทดลอง แต่ในการทดลองที่6นี้เป็นการสร้างwifiขึ้นมาใหม่
## คำถามหลังการทดลอง
ไม่ว่าใครก็สามารถสร้างwifi access pointใช้เองได้ใช่ไหม

Ans. ใช่ ถ้าหากเขียนโปรแกรมถูกต้องและมีอุปกรณืในการทำครบถ้วน
