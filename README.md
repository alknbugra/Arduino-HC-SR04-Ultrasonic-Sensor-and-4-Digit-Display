# Arduino-HC-SR04-Ultrasonic-Sensor-and-4-Digit-Display

#include <Arduino.h>

#include <TM1637Display.h>

#include <SoftwareSerial.h>


#define CLK 2

#define DIO 3


#define TEST_DELAY 50


TM1637Display display(CLK, DIO);

const int trigger_pin = 12;

const int echo_pin = 13;


long duration;

int distance;

SoftwareSerial mySerial(1, 0);


void setup() {

  pinMode(trigger_pin, OUTPUT);
  
  pinMode(echo_pin, INPUT);
  
  pinMode(10, OUTPUT);
  
  pinMode(11, OUTPUT);
  
  mySerial.begin(9600);
  
}

void loop() {

  char ch = mySerial.read();
  
  display.setBrightness(0x0f);

  uint8_t data[] = { 0x0, 0x0, 0x0, 0x0 };
  
  display.setSegments(data);

  digitalWrite(trigger_pin, LOW);
  
  delayMicroseconds(2);
  
  digitalWrite(trigger_pin, HIGH);
  
  delayMicroseconds(10);
  
  digitalWrite(trigger_pin, LOW);
  
  duration = pulseIn(echo_pin, HIGH);
  
  distance = duration * 0.034 / 2;
  
  display.showNumberDec(distance, false, 2, 1);
  
  delay(TEST_DELAY);
  
  if (distance > 0 && distance < 60) {
  
    digitalWrite(10, HIGH);
    
    digitalWrite(11, LOW);
    
  }
   else {
   
    digitalWrite(10, LOW);
    
    digitalWrite(11, HIGH);
    
  }

  delay(TEST_DELAY);
  
}
