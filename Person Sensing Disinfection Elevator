#include <LiquidCrystal.h>
LiquidCrystal lcd(12, 11, 2, 3, 4, 5);

extern volatile unsigned long timer0_millis; 
unsigned long timeVal;  
unsigned long readTime;  
int hour, min, sec;
boolean state=false;

void setup()
{
  Serial.begin(9600);
  lcd.begin(16, 2); 
  pinMode(6, INPUT);   // PIR센서 
  pinMode(7, INPUT);   // 전면 버튼 터치센서
  pinMode(8, INPUT);   // 측면 터치센서
  pinMode(5, OUTPUT);  // 전면 버튼 UVC LED  
  pinMode(9, OUTPUT);   // 가습기
  pinMode(10, OUTPUT);  // 스피커
  pinMode(13, OUTPUT);  // 측면 버튼 UVC LED  
}



void loop()
{
 String pir_value;  // 시리얼 모니터를 통해 오류를 파악하기 위해 설정
 int savetime;    // 소독 후 시간 저장
 int currenttime = 3600*hour+60*min+sec;   // 현재 시간 저장
 int savehour;
 int savemin;
 int savesec;
 int pir = 0;
 bool act = false;
 int touch1 = digitalRead(7);  // 전면 버튼
 int touch2 = digitalRead(8);  // 측면 버튼
 bool lamp1 = false;
 bool lamp2 = false;

 
  if(Serial.available()){                                 // 오픈소스를 통해 얻은 시간 코드 (시리얼 모니터에 "시 : 분 : 초" 형식으로 현재 시각 입력)
    String inString = Serial.readStringUntil('\n');    
    int index1 = inString.indexOf(':'); 
    int index2 = inString.indexOf(':',index1+1);   
    int index3 = inString.length();
    
    hour = inString.substring(0, index1).toInt();
    min = inString.substring(index1+1,index2).toInt();
    sec = inString.substring(index2+1,index3).toInt();
  
    
    timer0_millis = ((long)hour*3600+min*60+sec)*1000;
    state=true;
    timeVal=millis();
  }
  if(state==true){  
    
    if(millis()-timeVal>=1000){  
     readTime = millis()/1000;
      
     if(millis()>=86400000){
       timer0_millis=0;
     }
     timeVal = millis();
   
     sec = readTime%60;
     min = (readTime/60)%60;
     hour = (readTime/(60*60))%24;



 

 

 


 // pir센서  
 pir = digitalRead(6);  
 
 if (pir == HIGH) {
  currenttime = 3600*hour+60*min+sec;
  while (pir == HIGH) {                  // PIR센서에 감지가 되는 동안 실행
      pir_value = "탑승";
      Serial.println(pir_value);
      pir = digitalRead(6);
      delay(3000);

{
//터치패드
if(digitalRead(7)== HIGH) {
   lamp1 = true;
   Serial.println("touch1 감지");
   for( int i=1; i<8; i++) {
      lcd.setCursor(7,0);
      lcd.print(i);
      delay(2000);
   }
}
 
else if(digitalRead(8) == HIGH) {
  lamp2 = true;
  Serial.println("touch2 감지");
     for( int i=1; i<8; i++) {             //  터치센서를 7층으로 설정했으므로 인식되면 LCD에 7층까지 올라가는 것을 표시
      lcd.setCursor(7,0);
      lcd.print(i);
      delay(2000);
   }
  }       
}
  } 
   if(pir == LOW) {                       // PIR센서가 HIGH이다가 LOW로 변경되었을 때
      pir_value = "하차";
      Serial.println(pir_value);
      act = true;
       {
        if(act == true) {
           digitalWrite(9, HIGH);        // 가습기 ON
           { 
            if(lamp1 == true) {          // 전면 터치센서가 감지되었던 경우
              digitalWrite(5, HIGH);
              delay(10000);
              digitalWrite(5, LOW); }
            
            else if(lamp2 == true) {     // 측면 터치센서가 감지되었던 경우
               digitalWrite(13, HIGH); 
               delay(10000);
               digitalWrite(13, LOW); } 

            else if(lamp1 == true && lamp2 == true) {    // 전면, 측면 터치센서 모두 감지되었던 경우
              digitalWrite(5, HIGH);
              digitalWrite(13, HIGH);
              delay(10000);
              digitalWrite(5, LOW);
              digitalWrite(13, LOW);
            }
           }
           digitalWrite(9,LOW); 
           savetime = currenttime;     // 소독 종료 후 시간 저장 및 출력
           lcd.clear();
           savehour = hour;
           savemin = min;
           savesec = sec;
           lcd.setCursor(4,1);
           lcd.print(savehour);
           lcd.print(" : ");
           lcd.print(savemin);
        }
      }
      delay(5000);
      act = false;
    }
  delay(1000);
 }
 
else if( pir == LOW) {                     // PIR센서가 인식되지 않는 경우 LOW값 가짐
  currenttime = 3600*hour+60*min+sec;
    pir_value = "비어있음";
    Serial.println(pir_value);
    act = false;
    delay(1000);   
}



//소독 경고  
if (savetime + 1800 < currenttime) {        // 마지막 소독 진행 시간에서 30분이 지난 경우 소독 경고 알림 실행
    lcd.setCursor(2,0);
    lcd.print("Need");                      // LCD에 소독이 필요하다는 시각적 알림
    lcd.setCursor(2,1);
    lcd.print("disinfection");
    digitalWrite(10, HIGH);                 // 스피커를 통해 소독 필요하다는 음성 알림 
    delay(10);
    digitalWrite(10, LOW);
    delay(10000);
    digitalWrite(5, HIGH);                  // 가습기, 전면, 측면 UVC LED 작동
    digitalWrite(13, HIGH);
    digitalWrite(9,HIGH);
    delay(10000); 
    digitalWrite(5,LOW);
    digitalWrite(13,LOW);
    digitalWrite(9,LOW);
    delay(1000);
    currenttime = 3600*hour+60*min+sec;    // 소독 후 현재 시간 저장
    savehour = hour;
    savemin = min;
    savesec = sec;
    lcd.clear();
    lcd.setCursor(4,1);
    lcd.print(savehour);
    lcd.print(" : ");
    lcd.print(savemin):
    savetime = currenttime;
    delay(5000);
    }
}
 }
 }
}
