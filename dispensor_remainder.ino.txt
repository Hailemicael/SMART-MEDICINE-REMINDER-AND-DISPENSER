#include <DS3232RTC.h>
#include <LiquidCrystal.h> // includes the 
LiquidCrystal Library
#include <Keypad.h>
#include <Servo.h>
Appendix B: Code To Interface 
LCD, Keypad, and Others
LiquidCrystal lcd(12, 11, 5, 4, 3, 2); // 
Creates an LC object. Parameters: (rs, 
enable, d4, d5, d6, d7) 
const byte ROWS = 4; //four rows
const byte COLS = 3; //three columns
char keys[ROWS][COLS] = {
 { '1', '2', '3' },
 { '4', '5', '6' },
 { '7', '8', '9' },
 { '*', '0', '#' }
};
byte rowPins[ROWS] = {7,8,9,10}; 
//connect to the row pinouts of the keypad
byte colPins[COLS] = {6,1,0};
//connect to the column pinouts of the 
keypad
Keypad keypad = 
Keypad(makeKeymap(keys), rowPins, 
colPins, ROWS, COLS);
int buzzer = 13;
int storedHour;
int storedMinute;
Servo myservo;
int pos = 0;
void setup() {
 //Serial.begin(9600);
 myservo.attach(A3);
 setSyncProvider(RTC.get); 
 lcd.begin(16, 2);
 lcd.clear();
 pinMode(buzzer, OUTPUT);
 pinMode(buzzer, OUTPUT);
 digitalWrite(buzzer, LOW);
}
void loop() {
 char key;
 int gapTime;
 lcd.clear();
 lcd.setCursor(0, 0);
 lcd.print("Medicin Reminder");
 lcd.setCursor(0, 1);
 lcd.print("Now:");
 lcd.setCursor(5, 1);
 digitalClockDisplay();
 delay(2000);
 while(1){
 lcd.clear();
 lcd.setCursor(0, 0);
 lcd.print("Repeation / day?");
 lcd.setCursor(0, 1);
 while(1){
 key = keypad.getKey();
 if (key != '\0') break;
 }
 lcd.print(key);
 switch(key){
 case '1': gapTime = 24;
 lcd.clear();
 lcd.setCursor(0, 0);
 lcd.print("1 times a day?");
 lcd.setCursor(0, 1);
 lcd.print("*Cancel #Confirm");
 while(1){
 key = keypad.getKey();
 if (key == '#') break;

 if (key == '*') { key='\0'; break; 
}
 }
 break; 
 case '2': gapTime = 12;
 lcd.clear();
 lcd.setCursor(0, 0);
 lcd.print("2 times a day?");
 lcd.setCursor(0, 1);
 lcd.print("*Cancel #Confirm");
 while(1){
 key = keypad.getKey();
 if (key == '#') break;
 if (key == '*') { key='\0'; break; 
}
 }
 break;
 case '3': gapTime = 8;
 lcd.clear();
 lcd.setCursor(0, 0);
 lcd.print("3 times a day?");
 lcd.setCursor(0, 1);
 lcd.print("*Cancel #Confirm");
 while(1){
 key = keypad.getKey();
 if (key == '#') break;
 if (key == '*') { key='\0'; break; 
}
 }
 break; 
 case '4': gapTime = 6;
 lcd.clear();
 lcd.setCursor(0, 0);
 lcd.print("4 times a day?");
 lcd.setCursor(0, 1);
 lcd.print("*Cancel #Confirm");
 while(1){
 key = keypad.getKey();
 if (key == '#') break;
 if (key == '*') { key='\0'; break; 
}
 }
 break;
 default: lcd.print("Meet 
Doctor!"); 
 }
 if (key == '#') { key='\0'; break; }
 if (key == '*') key='\0';
 }
 int d=0;
 String beginHour;
 lcd.clear();
while(key != '#'){
 lcd.setCursor(0, 0);
 lcd.print("Set Begin Hour:");
 lcd.setCursor(4, 1);
 lcd.print(" #Ok *Clear");
 lcd.setCursor(0, 1);
 while(1){
 key = keypad.getKey();
 if (key != '\0') break;
 }
 beginHour += key;
 if(beginHour != '\0')
 lcd.print(beginHour);
 if (key == '*') { key='\0'; beginHour='\0'; 
d=0;
 lcd.clear();
 } 
 }
 
Alarm and Others
 int mediMinute = minute();
 int mediHour[4]; 
 for (int i=0; i<24; i=i+gapTime)
 mediHour[i/gapTime] = 
(beginHour.toInt()+i);
 delay(3000);
 int clk=0;
 while (1) {
 lcd.clear();

 
 storedHour = mediHour[clk/gapTime];
 storedMinute = mediMinute;
 int realHour = hour();
 int realMinute = minute();
 if ((realHour == storedHour) && 
(realMinute == storedMinute)) {
 soundAlarm();
 soundAlarm2();
 }
 lcd.setCursor(0, 0);
 lcd.print("Medication Time:");
 lcd.setCursor(0, 1);
 if(mediHour[clk/gapTime] < 10){
 lcd.print('0');
 lcd.setCursor(1, 1);
 lcd.print(mediHour[clk/gapTime]);
 } else lcd.print(mediHour[clk/gapTime]);
 lcd.setCursor(2, 1);
 lcd.print(":");
 lcd.setCursor(4, 1);
 lcd.print(mediMinute);
 delay(2000);
 clk = clk + gapTime;
 if (clk == 24) clk = 0;
 lcd.clear();
 lcd.setCursor(0, 0);
 lcd.print("Medicin Reminder");
 lcd.setCursor(0, 1);
 lcd.print("Now:");
 lcd.setCursor(5, 1);
 digitalClockDisplay();
 delay(2000);
 }
}
void digitalClockDisplay()
{
 // digital clock display of the time
 lcd.print(hour());
 printDigits(minute());
 printDigits(second());
}
void printDigits(int digits)
{
 // utility function for digital clock display: 
prints preceding colon and leading 0
 lcd.print(':');
 if(digits < 10)
 lcd.print('0');
 lcd.print(digits);
}
/* makes alarming beep sounds. */
void soundAlarm() {
 digitalWrite(buzzer, HIGH);
 digitalWrite(buzzer, HIGH);
 delay(100);
 digitalWrite(buzzer, LOW);
 digitalWrite(buzzer, LOW);
 lcd.clear();
 lcd.setCursor(0, 0);
 lcd.print("Take Medicine!");
 lcd.setCursor(0, 1);
 lcd.print("Now:");
 lcd.setCursor(5, 1);
 digitalClockDisplay();
 delay(2000);
 lcd.clear();
 digitalWrite(buzzer, HIGH);
 digitalWrite(buzzer, HIGH);
 delay(100);
 digitalWrite(buzzer, LOW);
 digitalWrite(buzzer, LOW);
 delay(100);
 digitalWrite(buzzer, HIGH);
 digitalWrite(buzzer, HIGH);
 delay(100);
 digitalWrite(buzzer, LOW);
 digitalWrite(buzzer, LOW); 
 delay(500);
// waits 15ms for the servo to reach the 
position
 }

  
void soundAlarm2() {
 pos = 0; 
 pos <= 180; 
 pos += 1; // goes from 0 degrees to 180 
degrees
 // in steps of 1 degree
 myservo.write(pos); 
// tell servo to go to position in variable 'pos'
 delay(150)
