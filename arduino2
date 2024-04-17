#include <pm2008_i2c.h>
#include <SPI.h>
#include <WiFi.h>



char ssid[] = "seven"; //  your network SSID (name)
char pass[] = "12345678";    // your network password (use for WPA, or use as key for WEP)
int status = WL_IDLE_STATUS;
char g;
char gas;
char gas2;
WiFiServer server(80);  // 80포트를 사용하는 웹서버 선언

PM2008_I2C pm2008_i2c;
int total_grade2;
int i = 0;
int wifion = 0;
int speaker = 5;
int R = 6;
int G = 7;
int B = 8;
int pan = 16;
int led3 = 17;



void setup() {
  Serial.begin(9600);

   while (!Serial) {
    ; // wait for serial port to connect. Needed for native USB port only
  }
  pinMode(16,OUTPUT); //추가한 부분
  pinMode(1,OUTPUT);
   pinMode(17,OUTPUT);
  pm2008_i2c.begin();
  pinMode(speaker, OUTPUT);
pinMode(R, OUTPUT);
pinMode(G, OUTPUT);
pinMode(B, OUTPUT);
digitalWrite(R, LOW);
digitalWrite(G, LOW);
digitalWrite(B, HIGH);


  pm2008_i2c.command();


  if (WiFi.status() == WL_NO_SHIELD) { // 현재 아두이노에 연결된 실드를 확인
    Serial.println("WiFi shield not present"); 
    while (true);  // 와이파이 실드가 아닐 경우 계속 대기
  } 

  // 와이파이에 연결 시도
  while ( status != WL_CONNECTED) { //연결될 때까지 반복
    Serial.print("Attempting to connect to SSID: ");
    Serial.println(ssid);
    status = WiFi.begin(ssid, pass);  // WPA/WPA2 연결
  } 

  server.begin();

  printWifiStatus();  // 연결 성공시 연결된 네트워크 정보를 출력
}

void loop() {

 
      

  uint8_t ret = pm2008_i2c.read();
  uint8_t pm1p0_grade = 0;
  uint8_t pm2p5_grade = 0;
  uint8_t pm10p_grade = 0;
  uint8_t total_grade = 0;

int gas = analogRead(A1);
Serial.print ("배기가스: " );
Serial.println(gas);
int gas2 = analogRead(A0);
Serial.print ("차량안 일산화탄소: " );
Serial.println(gas2);
delay(500);

if (gas2 >= 500) {
tone(speaker, 330);
digitalWrite(R, HIGH);
digitalWrite(G, LOW);
digitalWrite(B, LOW);
delay(150);
noTone(speaker);
digitalWrite(R, LOW);
digitalWrite(G, LOW);
digitalWrite(B, LOW);
delay(150);
} else {
noTone(speaker);
digitalWrite(R, LOW);
digitalWrite(G, LOW);
digitalWrite(B, HIGH);
}

Serial.print("미세먼지:");
  total_grade2 = max(pm2008_i2c.pm1p0_grimm, pm2008_i2c.pm2p5_grimm);
    total_grade2 = max(total_grade2, pm2008_i2c.pm10_grimm);
 Serial.println(total_grade2);
  if (ret == 0) {
    // PM 1.0
    if (pm2008_i2c.pm1p0_grimm < 10) {
      pm1p0_grade = 1;
    } else if (pm2008_i2c.pm1p0_grimm < 15) {
      pm1p0_grade = 2;
    } else if (pm2008_i2c.pm1p0_grimm < 20) {
      pm1p0_grade = 3;
    } else {
      pm1p0_grade = 4;
    }
    // PM 2.5
    if (pm2008_i2c.pm2p5_grimm < 10) {
      pm2p5_grade = 1;
    } else if (pm2008_i2c.pm2p5_grimm < 15) {
      pm2p5_grade = 2;
    } else if (pm2008_i2c.pm2p5_grimm < 20) {
      pm2p5_grade = 3;
    } else {
      pm2p5_grade = 4;
    }
    // PM 10
    if (pm2008_i2c.pm10_grimm < 20) {
      pm10p_grade = 1;
    } else if (pm2008_i2c.pm10_grimm < 30) {
      pm10p_grade = 2;
    } else if (pm2008_i2c.pm10_grimm < 40) {
      pm10p_grade = 3;
    } else {
      pm10p_grade = 4;
    }
    // Get worst grade
    total_grade = max(pm1p0_grade, pm2p5_grade);
    total_grade = max(total_grade, pm10p_grade);
    switch (total_grade) {
      case 1: {
          Serial.println("Good!");
          digitalWrite(pan,LOW); //추가한 부분
  
    char g = "Good";
          break;
        }
      case 2: {
          Serial.println("Normal!");
          digitalWrite(pan,LOW); //추가한 부분
  char g = "Normal";
         break;
        }
      case 3: {
          Serial.println("Bad!");
    
  digitalWrite(pan,HIGH);  
    char g = "Bad";
          break;
        }
      case 4: {
          Serial.println("Worst!"); 
          digitalWrite(pan,HIGH);             
        char g = "Worst";
          break;
        }
      default:
        break;
    }
  }

 
  
 WiFiClient client = server.available();  // 들어오는 클라이언트를 수신한다.
  if (client) {  // 클라이언트를 수신 시
    Serial.println("new client");  // 클라이언트 접속 확인 메시지 출력
    boolean currentLineIsBlank = true;

    while (client.connected ()) { 
      if (client.available()) {
        char c = client.read();
        // 문자의 끝을 입력 받으면 http 요청이 종료되고, 답신을 보낼 수 있습니다.
        if (c == '\n' && currentLineIsBlank) {
          client.println("HTTP/1.1 200 OK");
          client.println("Content-Type: text/html");
          client.println("Connection: close");
          client.println("Refresh: 1"); // 1초당 페이지 refresh
          client.println();
          client.println("<!DOCTYPE HTML>");
          client.println("<meta charset=utf-8/>");
          client.print("<meta name=view content=width=device-width, ");
          client.println("initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0, user-scalable=no />");
          client.println("<html>");
          client.println("<head>");  
          client.println("<title>Many Sensor</title>");
          client.println("</head>");
          client.println("<h1>Many Sensor</h1>");
          for(i=0; i < 2; i++)
          {
          client.println("<div data-role=content>");
          client.print("m : ");
          client.print(total_grade2);  // flex 센서 값 출력
              client.println(g);
          client.println("<br>");
          client.println("<br>");
          client.print("co : ");
          client.println(gas);  // 조도 센서 값 출력
          client.println("<br>");
           client.println("<br>");
          client.print("car co : ");
          client.println(gas2);  // 조도 센서 값 출력
          client.println("<br>");
           client.println("<br>"); 
              client.print("address:192.168.43.235");
           client.println("<br>");
          client.println("</div>");
          client.println("</body>");
          }
          client.println("</html>");
          break;
        }
     if (c == '\n') {
          // you're starting a new line
          currentLineIsBlank = true;
        } else if (c != '\r') {
          // you've gotten a character on the current line
          currentLineIsBlank = false;
        }
      }
    }
    delay(1);
    client.stop();
    Serial.println("client disonnected");
    // 클라이언트와 연결을 끊는다.
  }
}


void printWifiStatus() {
  // print the SSID of the network you're attached to:
  Serial.print("SSID: ");
  Serial.println(WiFi.SSID());

  // print your WiFi shield's IP address:
  IPAddress ip = WiFi.localIP();
  Serial.print("IP Address: ");
  Serial.println(ip);

  // print the received signal strength:
  long rssi = WiFi.RSSI();
  Serial.print("signal strength (RSSI):");
  Serial.print(rssi);
  Serial.println(" dBm");
}
