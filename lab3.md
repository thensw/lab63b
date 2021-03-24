# การทดลองที่ 3 เรื่อง การเขียนโปรแกรมเอ้าพุทสัญญาณดิจิทัล
## วัตถุประสงค์

## อุปกรณ์ที่ใช้
1. ไมโครคอนโทรลเลอร์ (ESP01)
2. สายUSB
3. programmer / usb to serial
4. adapter
5. หลอดไฟLEDเปล่งแสง
6. relay
## ศึกษาข้อมูลเบื้องต้น
คลิปตัวอย่างการทดลอง https://www.youtube.com/watch?v=CCnN1WJsXQY
## วิธีการทำการทดลอง
1. ต่อadapterเข้ากับพอร์ท0(สายสีขาว),1(สายสีเหลือง) และนำพอร์ทต่อเข้ากับหลอดLEDเปล่งแสง (เพื่อดูผลลัพธ์จากการทดลอง)

![4](https://user-images.githubusercontent.com/80879818/112313689-b4dbe080-8cda-11eb-87ca-6766a18c049f.jpg)


2. ต่อไมโครคอนโทรลเลอร์ผ่านadapterเข้าไปที่ตัวusb to serial

![5](https://user-images.githubusercontent.com/80879818/112314208-56633200-8cdb-11eb-93c1-f1e51f3d3936.jpg)

3. เปิดโปรแกรมตัวอย่าง (ในที่นี้เลือกโปรแกรมที่3)
* พิมพ์ cd pattani (ชื่อโฟลเดอร์ที่จะเปิด)
* พิมพ์ cd 03_Output-Port
* พิมพ์ vi src/main.cpp (เพื่อแสดงเนื้อหาโปรแกรม)
```javascript
#include <Arduino.h>
#include <ESP8266WiFi.h>

int cnt = 0;

void setup()
{
	Serial.begin(115200);
	pinMode(0, OUTPUT);
	Serial.println("\n\n\n");
}

void loop()
{
	cnt++;
	if(cnt % 2) {
		Serial.println("========== ON ===========");
		digitalWrite(0, HIGH);
	} else {
		Serial.println("========== OFF ===========");
		digitalWrite(0, LOW);
	}
	delay(500);
}
```
> จากโค้ดเบื้องต้น
> * บรรทัดที่ 9 'pinMode(0, OUTPUT)' เป็นการเซตให้พอร์ท0เป็นพอร์ทOUTPUT
> * cnt++ เป็นการนับเพิ่มขึ้นไปเรื่อยๆ 
>   * ถ้า'cntเป็นเลขคี่ (cnt%2)'ให้ทำการ ON
>   * นอกเหนือจากนี้ คือ 'cntเป็นเลขคู่' ให้ทำการ OFF
> * delay(500) เป็นคำสั่งให้วนลูปทุกครึ่งวินาที หรือ 500 ms
## การบันทึกผลการทดลอง

## อภิปรายผลการทดลอง

## คำถามหลังการทดลอง
