# Thesis1
####ESP###
#define BLYNK_PRINT Serial
#include "uart.h"
#include <ESP8266WiFi.h>
#include <BlynkSimpleEsp8266.h>
#include <SimpleTimer.h>
#include <DHT.h>

char auth[] = "3c62d49c6f3743fdb693d37b0a9c569f";        //Token cua blynk
char ssid[] = "thuậnlm";        //Ten wifi
char pass[] = "leminhthuan122";         //Pass wifi
float t;
float h;
#define DHTPIN 4          // Pin ket noi voi DHT
#define DHTTYPE DHT11     // Su dung cam bien DHT11
DHT dht(DHTPIN, DHTTYPE); // Cau hinh chan DHT 

SimpleTimer timer;        // Su dung timer
void sendSensor()
{
  //float h = dht.readHumidity();     //Doc gia tri do am
  //float t = dht.readTemperature();  //Doc gia tri nhiet do

  // Gan du lieu vao bien virtual de hien thi len blynk
  // Chi nen gan 10 bien tro xuong
  delay(2000);
  delay(10);
  Blynk.virtualWrite(V0, h);
  Blynk.virtualWrite(V1, t);
  // Luu y nen ban khong du energy thi co the bo qua v2 va v3
}
void setup() {
  Serial.begin(9600);                   // Mo Serial
  Blynk.begin(auth, ssid, pass);        // Ket noi voi blynk
  dht.begin();                          // Khoi tao DHTchu
  timer.setInterval(1000L, sendSensor); //1s doc cam bien 1 lan
  
}
void loop() {
  if(Serial.available()>0)
{
  Serial.println("OK");
   String chuoi = Serial.readString();
  t = chuoi.substring(0,5).toFloat();
  h = chuoi.substring(5).toFloat();
  }
  
    delay(10);
  Blynk.virtualWrite(V0, h);
  Blynk.virtualWrite(V1, t);
  Blynk.run(); // Chay Blynk
  timer.run(); // Chay SimpleTimer
  
}
----------------------------------------------------------------
### Arduino ###
#include <Wire.h> 
#include <LiquidCrystal_I2C.h>
LiquidCrystal_I2C lcd(0x27,16,2);

#include "DHT.h"
//#include "UART_ARDUINO.h"
//UART Gui;
const int DHTPIN = 2; //Đọc dữ liệu từ DHT11 ở chân 2 trên mạch Arduino
const int DHTTYPE = DHT11; //Khai báo loại cảm biến, có 2 loại là DHT11 và DHT22
// float h = dht.re adHumidity(); //Đọc độ ẩm
//float t = dht.readTemperature(); //Đọc nhiệt độ
DHT dht(DHTPIN, DHTTYPE);

void setup() {



lcd.init();   
lcd.backlight();   //Bật đèn 
pinMode(3,OUTPUT);
pinMode(4,OUTPUT);
Serial.begin(9600);
dht.begin(); // Khởi động cảm biến
}

void loop() {
float h = dht.readHumidity(); //Đọc độ ẩm
float t = dht.readTemperature(); //Đọc nhiệt độ
//if(Serial.available()>0)
//{
//   t = Serial.readString();
//  Serial.println(t);
//  }





  if (isnan(t) || isnan(h)) { // Kiểm tra xem thử việc đọc giá trị có bị thất bại hay không. Hàm isnan bạn xem tại đây http://arduino.vn/reference/isnan
  } 
  else {
    lcd.setCursor(0,0);
    lcd.print(" ");
    lcd.print("Nhiet do:");
    lcd.print(round(t));
    lcd.print("oC ");

    lcd.setCursor(1,1);
    lcd.print("Do am   : ");  
    lcd.print(round(h));  
    lcd.print("% ");
  }


Serial.println(String("") + t + h); //Xuất nhiệt độ
 
 //Xuống hàng
delay(1000); //Đợi 1 giây

  if(t>33)
  {
   digitalWrite(3, HIGH);
   }
   else {
      
     digitalWrite(3,LOW);
     }
   if (h>80)

   {
   	digitalWrite(4, HIGH);
   }
      else {
      
     digitalWrite(3,LOW);
     }
}
//uno
