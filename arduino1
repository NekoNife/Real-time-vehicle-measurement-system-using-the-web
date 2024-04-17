#include <SoftwareSerial.h>
#include <DHT.h>
#include <Servo.h>
#include <LiquidCrystal_I2C.h>
#include <Wire.h>// LCD 제어

#define BT_RXD 13 // 블루투스
#define BT_TXD 12
#define DHTPIN A2  //온습도
#define DHTTYPE DHT11


// 0x27 I2C 주소를 가지고 있는 16x2 LCD객체를 생성합니다.(I2C 주소는 LCD에 맞게 수정해야 합니다.)
LiquidCrystal_I2C lcd(0x27, 16, 2);
/*LCD글자 폰트 만듬*/
byte onChar[] = {
  B01110,
  B10001,
  B10001,
  B01110,
  B00100,
  B11111,
  B01000,
  B01110
};
byte doChar[] = {
  B00000,
  B11111,
  B10000,
  B10000,
  B11111,
  B00100,
  B11111,
  B00000
};

byte guChar[] = {
  B00000,
  B00001,
  B11101,
  B00111,
  B01001,
  B10001,
  B00001,
  B00000
};
byte riChar[] = {
  B00001,
  B11101,
  B00101,
  B11101,
  B10001,
  B11101,
  B00001,
  B00000
};
byte maChar[] = {
 B00000,
  B11110,
  B10010,
  B10010,
  B10010,
  B10010,
  B11110,
  B00000
};
byte aChar[] = {
 B00000,
  B10000,
  B10000,
  B10000,
  B11000,
  B10000,
  B10000,
  B00000
};
byte moChar[] = {
  B00000,
  B11111,
  B10001,
  B10001,
  B11111,
  B00100,
  B11111,
  B00000
};

int LEDt = 2;
int Flame;
int FlamePin = 11;
int Buzzer = 3;
DHT dht(DHTPIN, DHTTYPE);
int servoPin = A0;


int echoPin = 8;
int trigPin = 9;
int i;

int pinNum = A1;  //ir 센서
float irdistance = 0.0; 

int waterPin = A3;    //수위감지
int waterval = 0; 




SoftwareSerial bluetooth(BT_RXD, BT_TXD);
Servo servo; 


void setup(){
  /*시리얼*/
  Serial.begin(9600);
  bluetooth.begin(9600);
  
/*LCD*/
   // I2C LCD를 초기화 합니다..
  lcd.init();
  // I2C LCD의 백라이트를 켜줍니다.
  lcd.backlight();
/*인 아웃*/
  pinMode(LEDt, OUTPUT);

    pinMode(FlamePin, INPUT);
    pinMode(Buzzer, OUTPUT);
   
      pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode (pinNum, INPUT);
 pinMode(waterPin,INPUT);

  
}
 
void loop(){

    
 
/*받을 정보들 */
  char btval = bluetooth.read();

  
  if (bluetooth.available()) {
    Serial.write(bluetooth.read());
  }
  if (Serial.available()) {
    bluetooth.write(Serial.read());
  }

  int h = dht.readHumidity();
   int t = dht.readTemperature();
    int angle = 0;
   int sener1;
lcd.init();
lcd.backlight();
lcd.createChar(0, onChar);
lcd.createChar(1, doChar);
lcd.createChar(2, guChar);
lcd.createChar(3, riChar);
lcd.createChar(4, maChar);
lcd.createChar(5, aChar);
lcd.createChar(6, moChar);

    Serial.print("humidity:");          // ‘시리얼 플로터’ 사용위해 이 부분 주석 필요
    Serial.println(h);                  // 습도 값 출력
    Serial.print("temperature:");       // ‘시리얼 플로터’ 사용위해 이 부분 주석 필요
    Serial.println(t);                  // 온도 값 출력 
  
     lcd.setCursor(0,0); 
     lcd.write(0);
      lcd.setCursor(2,0); 
          lcd.write(1);
             lcd.setCursor(3,0); 
             lcd.print(":");
  lcd.print(t);

 /*초음파 거리 */
     digitalWrite(trigPin, LOW);
    digitalWrite(echoPin, LOW);
    delayMicroseconds(2);
    digitalWrite(trigPin, HIGH);
    delayMicroseconds(10);
    digitalWrite(trigPin, LOW);
  // echoPin 이 HIGH를 유지한 시간을 저장 한다.
    unsigned long duration = pulseIn(echoPin, HIGH); 
  // HIGH 였을 때 시간(초음파가 보냈다가 다시 들어온 시간)을 가지고 거리를 계산 한다.
  float distance = ((float)(340 * duration) / 10000) / 2;  
  Serial.print(distance);
  Serial.println("cm");
    lcd.setCursor(0,1); 
     lcd.write(2);
      lcd.setCursor(2,1); 
     lcd.write(3);
        lcd.setCursor(3,1); 
            lcd.print(":");
    lcd.print(distance); 
 delay(500);
  lcd.clear();

 int data = analogRead(pinNum); //ir 센서
  int volt = map(data, 0, 1023, 0, 5000);
  irdistance = (22.5/(volt-0.1696))*1000;

  if(irdistance > 13){
     lcd.setCursor(0,0); 
     lcd.write(4);
      lcd.setCursor(1,0); 
     lcd.write(5);
           lcd.setCursor(2,0); 
     lcd.write(6);
        lcd.setCursor(4,0);    
     lcd.write(1);
       lcd.setCursor(5,0); 
            lcd.print(":");
    lcd.print("Caution"); 
  }else if(irdistance > 14){
      lcd.setCursor(0,0); 
     lcd.write(4);
      lcd.setCursor(1,0); 
     lcd.write(5);
           lcd.setCursor(2,0); 
     lcd.write(6);
        lcd.setCursor(4,0);    
     lcd.write(1);
       lcd.setCursor(5,0); 
            lcd.print(":");
    lcd.print("Dangerous"); 
  }

 Serial.print(" 거리센서 LOW 데이터 : "); Serial.print(data);
  Serial.print(" LOW 데이터 범위조정 : "); Serial.print(volt);
  Serial.print(" 측정거리 : "); Serial.print(irdistance);Serial.print("cm");
  Serial.println();

 lcd.setCursor(0,1); 
     lcd.write(4);
      lcd.setCursor(1,1); 
     lcd.write(5);
           lcd.setCursor(2,1); 
     lcd.write(6);
        lcd.setCursor(4,1);    
     lcd.write(1);
       lcd.setCursor(5,1); 
            lcd.print(":");
    lcd.print(irdistance); 
 delay(500);



  waterval = analogRead(waterPin);   // analogPin 의 변화 값(전류 값)을 읽음  
 Serial.print("물감지센서");      // 시리얼모니터에 전류 값 표시

  Serial.println(waterval);      // 시리얼모니터에 전류 값 표시

if (waterval > 400)
{
    digitalWrite(Buzzer, HIGH);
       digitalWrite(LEDt,HIGH);
  }
  else if (waterval < 300)
  {
    
      digitalWrite(Buzzer, LOW);
         digitalWrite(LEDt,LOW);
    }




 Flame = digitalRead(FlamePin);
  if (Flame == HIGH)
  {
    Serial.println("HIGH FLAME");
    digitalWrite(Buzzer, HIGH);
    servo.attach(servoPin);
      for(angle = 0; angle < 75; angle++) 
  { 
    servo.write(angle); 
    delay(10); 
  }
  servo.detach();
   
  }
  else
  {
    Serial.println("No flame");
    digitalWrite(Buzzer, LOW);
  }






if(btval == '1')
  {
   servo.attach(servoPin);
      for(angle = 0; angle < 75; angle++) 
  { 
    servo.write(angle); 
    delay(10); 
  }
  servo.detach();
  }

  if(btval == '2')
  {
    servo.attach(servoPin); 
   for(angle = 75; angle > 0 ; angle--) 
  { 
    servo.write(angle); 
    delay(10); 
  }
  
servo.detach();
  }

 if(btval == 'a')
  {
    digitalWrite(LEDt,HIGH);
   
  }

  if(btval == 'b')
  {
    digitalWrite(LEDt, LOW);
  }
  if(btval == 't')
  {
 bluetooth.print(t);
  }
    if(btval == 'h')
  {

 bluetooth.print(h);
  }





 if(btval == 'r')
  {
    digitalWrite(LEDt,HIGH);

    
       digitalWrite(LEDt,LOW);
  
  }

 if(btval == 'o')
  {
    servo.attach(servoPin);
   for(angle = 2; angle < 75; angle++) 
  { 
    servo.write(angle); 
    delay(30); 
 /*초음파 거리 */
     digitalWrite(trigPin, LOW);
    digitalWrite(echoPin, LOW);
    delayMicroseconds(2);
    digitalWrite(trigPin, HIGH);
    delayMicroseconds(10);
    digitalWrite(trigPin, LOW);
  // echoPin 이 HIGH를 유지한 시간을 저장 한다.
    unsigned long duration = pulseIn(echoPin, HIGH); 
  // HIGH 였을 때 시간(초음파가 보냈다가 다시 들어온 시간)을 가지고 거리를 계산 한다.
  float distance = ((float)(340 * duration) / 10000) / 2;  
   if(distance < 5){
   digitalWrite(LEDt,HIGH);
   delay(3000);
    digitalWrite(LEDt,LOW);
   break;
   }
   
  } 
servo.detach();
  }


  
}
