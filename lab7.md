# การทดลองที่ 7 เรื่อง การเขียนโปรแกรมอินพุทสัญญาณดิจิทัลเพื่อใช้ในการควบคุมการเชื่อมต่อสัญญาณwifi
## วัตถุประสงค์
1. เพื่อคิดค้นโปรแกรมใหม่ๆโดยผสมผสานระหว่างความคิดสร้างสรรค์กับความรู้พื้นฐานจากแลปครั้งก่อน
2. เพื่อศึกษาวิธีการเขียนโปรแกรมที่ถูกต้อง
3. เพื่อศึกษาการแสดงผลของINPUTต่างๆในรูปแบบของการเชื่อมต่อสัญญาณwifi
## อุปกรณ์ที่ใช้
1. ไมโครคอนโทรลเลอร์ (ESP01)
2. สายUSB(ไฟเลี้ยง)
3. programmer / usb to serial
4. adapter
5. หลอดไฟLEDเปล่งแสง
## ศึกษาข้อมูลเบื้องต้น
1. คลิปตัวอย่างการทดลองที่4และ5
* (4) https://www.youtube.com/watch?v=nFqoZT26U5k
* (5) https://www.youtube.com/watch?v=VX-QNQcO-b4
2. ตัวอย่างการเขียนโปรแกรมจาก https://github.com/choompol-boonmee/lab63b/tree/master/examples
3. ความรู้เพิ่มเติมจากอินเทอร์เน็ต
## วิธีการทำการทดลอง
1. ทำการต่อไฟเลี้ยงเข้าที่ตัวprogrammer และต่อไมโครคอนโทรลเลอร์ผ่านตัวadapterเข้าไปยังprogrammer (ดังภาพ)

![33](https://user-images.githubusercontent.com/80879818/112346747-d51a9800-8cf8-11eb-824c-b69084d3c0ad.jpg)

2. เปิดตัวอย่างโปรแกรม
* พิมพ์ vi src/main.cpp (เพื่อแสดงเนื้อหาโปรแกรม)
```javascript
#include <Arduino.h>
#include <ESP8266WiFi.h>
//#include <WiFiClient.h>
#include <ESP8266WebServer.h>

const char* ssid = "supreeya.4G";
const char* password = "0830611575";

ESP8266WebServer server(80);

int d = 1000//5;
int cnt = 0;

void setup()
{
	Serial.begin(115200);
	pinMode(0, INPUT);
	pinMode(2, OUTPUT);
	Serial.println("\n\n\n");
}

void loop()
{
	int val = digitalRead(0);
	Serial.printf("======= read %d\n", val);
	if(val==1) {
		digitalWrite(2, LOW);
      server.onNotFound([]() {
		    server.send(404, "text/plain", "Path Not Found");
	    });
      
	} else {
		digitalWrite(2, HIGH);
      WiFi.mode(WIFI_STA);
	    WiFi.begin(ssid, password);
  	  while (WiFi.status() != WL_CONNECTED) {
		  delay(5*100);
		  Serial.print(".");
	    }
	    Serial.print("\n\nIP address: ");
	    Serial.println(WiFi.localIP());

	    server.on("/", []() {
		    server.send(200, "text/plain");
	    });

	    server.begin();
	    Serial.println("HTTP server started");
	}
delay(d);
}
```
## การบันทึกผลการทดลอง
## อภิปรายผลการทดลอง
## ประโยชน์จากการทดลอง
## คำถามหลังการทดลอง
