# FINAL_CE
창의공학 종합과제 팀프로젝트 9팀</br>
박채연, 백지은, 박성우</br>
### 유튜브 링크
- y

## 종합 프로젝트 포스터(파일 있음)
![image](https://github.com/qcd126/FINAL_CE/assets/128008018/7e4d107b-c538-44bb-a932-7b167bded79c)

# [프로젝트 구성]
## 앱인벤터
![스크린샷 2023-12-10 152031](https://github.com/qcd126/FINAL_CE/assets/128008018/4f541f6c-bfdb-4509-9803-d3119391852e) ![스크린샷 2023-12-10 220135](https://github.com/qcd126/FINAL_CE/assets/128008018/b29a019f-75bb-4ef4-a733-0539115a2870)


## 아두이노 코드
```
const int relayPin = 9; // 릴레이 모듈을 제어하는 핀 번호
const int motorPin1 = 10; // 모터 드라이버의 IN1 핀
const int motorPin2 = 11; // 모터 드라이버의 IN2 핀

void setup() {
  pinMode(relayPin, OUTPUT);
  pinMode(motorPin1, OUTPUT);
  pinMode(motorPin2, OUTPUT);
  
  digitalWrite(relayPin, LOW); // 처음에는 릴레이 OFF
  
  Serial.begin(9600);
  Serial.println("Setup complete");
}

void loop() {
  if (Serial.available()) {
    char command = Serial.read();
    Serial.print("Received command: ");
    Serial.println(command);
    switch(command) {
      case '1':
        digitalWrite(relayPin, HIGH); // 릴레이 ON
        digitalWrite(motorPin1, HIGH); // 모터 방향 설정
        digitalWrite(motorPin2, LOW); // 모터 방향 설정
        Serial.println("Relay and motor turned ON");
        break;
      case '0':
        digitalWrite(relayPin, LOW); // 릴레이 OFF
        digitalWrite(motorPin1, LOW); // 모터 멈춤
        digitalWrite(motorPin2, LOW); // 모터 멈춤
        Serial.println("Relay and motor turned OFF");
        break;
      default:
        Serial.println("Invalid command");
        break;
    }
  }
}

```

## 프로세싱 코드
```
import processing.net.*;
import processing.serial.*;

Server server;
Serial myPort;

void setup() {
  server = new Server(this, 12345); // 12345번 포트에서 웹서버 시작
  myPort = new Serial(this, "COM8", 9600);
  println("Server setup complete");
}

void draw() {
  Client client = server.available();
  if (client != null) {
    String input = client.readStringUntil('\n'); // 줄바꿈 문자까지만 읽음
    if (input != null) {
      input = input.trim(); // 문자열 양쪽의 공백과 줄바꿈 문자 제거
      if (input.length() > 0) {
        char command = input.charAt(0);
        println("Received command: " + command);
        if (command == '1') {
          myPort.write('1'); // '1'을 받으면 릴레이 ON
          println("Sent '1' to Arduino");
        } else if (command == '0') {
          myPort.write('0'); // '0'을 받으면 릴레이 OFF
          println("Sent '0' to Arduino");
        } else {
          println("Invalid command");
        }
        client.stop(); // 클라이언트 연결을 끊습니다.
      }
    }
  }
}

```

