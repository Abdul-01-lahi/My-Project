# My-Project
#include <LiquidCrystal.h>
#include <Wire.h>
LiquidCrystal lcd( 9, 8, 7, 6, 5, 4);
const int trigPin = 11;
const int echoPin = 10;
long duration;
int distanceCm;
int b;
double alpha = 0.75;
int period = 20;
double refresh = 0.0;


void setup(void) {

  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);

  lcd.print("   Health Booth");
  delay(5000);
  pinMode(A0, INPUT);
  lcd.begin(16, 2);
  lcd.clear();
  lcd.setCursor(0, 0);
}

void loop(void) {

  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  duration = pulseIn(echoPin, HIGH);
  distanceCm = (duration * 0.034) / 2;
  b = 180 - distanceCm;
 
  lcd.setCursor(0, 2);
  lcd.print("Height:");
  lcd.print(b);
  lcd.print("cm");
  delay(500);

  
  static double oldValue = 0;
  static double oldrefresh = 0;

  int beat = analogRead(A0);

  double value = alpha * oldValue + (0 - alpha) * beat;
  refresh = value - oldValue;

  lcd.setCursor(0, 0);
  lcd.print(" Heart Rate:");
  lcd.print(beat / 10);
  oldValue = value;
  oldrefresh = refresh;
  delay(500);

}
