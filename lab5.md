# การทดลองที่ 5 เรื่อง การเขียนโปรแกรมเชื่อมต่อไวไฟและเว็บเซอร์เวอร์
## วัตถุประสงค์
1. เพื่อทดสอบความสามารถของโปรแกรมในการรันข้อมูลบนคอมพิวเตอร์
2. เพื่อศึกษาวิธีการเขียน/อ่านค่าของโปรแกรมที่ใช้ในการทดลอง
## อุปกรณ์ที่ใช้
1. ไมโครคอนโทรลเลอร์ (ESP01)
2. สายUSB(ไฟเลี้ยง)
3. programmer / usb to serial
## ศึกษาข้อมูลเบื้องต้น
คลิปตัวอย่างการทดลอง https://www.youtube.com/watch?v=VX-QNQcO-b4
## วิธีการทำการทดลอง
1. เปิดโปรแกรมตัวอย่าง (ในที่นี้เลือกโปรแกรมที่5)
* พิมพ์ cd pattani (ชื่อโฟลเดอร์ที่จะเปิด)
* พิมพ์ cd 05_Wifi-Web-Server
* พิมพ์ vi src/main.cpp (เพื่อแสดงเนื้อหาโปรแกรม)
```javascript
#include <ESP8266WiFi.h>
//#include <WiFiClient.h>
#include <ESP8266WebServer.h>

const char* ssid = "HI_BMFWIFI_2.4G";
const char* password = "0819110933";

ESP8266WebServer server(80);

int cnt = 0;

void setup(void){
	Serial.begin(115200);

	WiFi.mode(WIFI_STA);
	WiFi.begin(ssid, password);
	while (WiFi.status() != WL_CONNECTED) {
		delay(500);
		Serial.print(".");
	}
	Serial.print("\n\nIP address: ");
	Serial.println(WiFi.localIP());

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
> จากโค้ดเบื้องต้น แบ่งข้อมูลเป็น2ส่วน ดังนี้
> * set up(บรรทัดที่15-22) 
>   * เป็นการเชื่อมต่อกับwifiที่เลือกไว้
>   * set up web server
>     * เริ่มต้นด้วย 'Hello cnt: '
>     * cnt++ เป็นการนับเพิ่มขึ้นไปเรื่อยๆ
> * loop
> * บรรทัดที่ 5และ6 ต้องใส่ชื่อและรหัสwifi
2. ต่อไมโครคอนโทรลเลอร์เข้ากับตัวprogrammer

![34](https://user-images.githubusercontent.com/80879818/112374420-a7dbe300-8d14-11eb-986b-0d5a6e19da30.jpg)

3. อัปโหลดโปรแกรมเข้าสู่ไมโครคอนโทรลเลอร์
* พิมพ์ pio run -t upload
  * ในขณะที่โปรแกรมกำลังอัปโหลด `กดปุ่มสีดำบนตัวprogrammer` เพื่อให้ตัวไมโครคอนโทรลเลอร์รับโปรแกรมใหม่เข้าไป
  * จากนั้น `กดปุ่มสีแดงบนตัวprogrammer` เพื่อทำการรีเซต
4. แสดงผลลัพธ์จากโปรแกรมผ่านคอมพิวเตอร์
* พิมพ์ pio device monitor
* กดปุ่ม reset เพื่อเริ่มการทำงานใหม่ โดยจะเริ่มจากการค้นหาwifi , เชื่อมต่อwifi และ`แสดงผลip address`
5. ทดสอบโปรแกรม
* copy ip address ไปที่ web browser 
> โปรแกรมแสดงผล 'Hello 1'
* กดปุ่ม reset
> โปรแกรมแสดงผล 'Hello 2' , 'Hello 3' , ... ไปเรื่อยๆ
## การบันทึกผลการทดลอง
* ในการแก้โปรแกรมจะต้องเขียนด้วยภาษาC++และจะต้องอัปโหลดลงไมโครคอนโทรลเลอร์ใหม่ทุกครั้ง
* โปรแกรมจากไมโครคอนโทรลเลอร์สามารถรันข้อมูลผ่านweb browserซึ่งเป็นช่องทางของคอมพิวเตอร์ขนาดกลางโดยใช้ ip address ได้
## อภิปรายผลการทดลอง
ในการทดลองครั้งนี้สามารถทดสอบโปรแกรมได้โดยการนำ ip address จากการรันไมโครคอนโทรลเลอร์มาใส่ที่ web browser เพื่อทดสอบโปรแกรมได้
## คำถามหลังการทดลอง
บรรทัดที่เท่าไรในเนื้อหาโปรแกรมที่ให้ใส่ชื่อwifi

Ans. บรรทัดที่21
