//LedClass.ion
#include "MyClass.h"

LED blueLed;

int ledPin = 4;
float ledInterval = 5;
bool ledIsInInterval = false;
float ledStartTime;
bool value = true;

void setup()
{
  Serial.begin(9600);
  blueLed.attach(ledPin);
}

void loop()
{
  
}


//LedClass.cpp

#include "esp32-hal.h"
#include "esp32-hal-gpio.h"
#include "MyClass.h"
#include "Arduino.h"

LED::LED(){
  
}

LED::LED(int pin)
{
  attach(pin);
  _pin = pin;
}

void LED::attach(int pin)
{
  pinMode(pin, OUTPUT);
}


void LED::toggleOn(int pin)
{
  digitalWrite(pin, HIGH);
}

void LED::toggleOff(int pin)
{
  digitalWrite(pin, LOW);
}

float LED::MillisecondsToSeconds(int milliseconds) 
{
  float second = float(milliseconds) / 1000;
  return second;
}

bool LED::IsTimePassedFromInterval(float startTimeInSeconds, float intervalInSeconds)
{
  float endTimeSeconds = startTimeInSeconds + intervalInSeconds;
  unsigned long currentMillis = millis();
  float currentSeconds = MillisecondsToSeconds(currentMillis);

  if(currentSeconds < endTimeSeconds)
  {
    return true;
  }
  else
  {
    return false;
  }
}

void LED::Reverse(bool value, int pin)
{
  digitalWrite(pin, !value);
  state = !value;  
}

int LED::getState(bool state)
{
  if(state == 1)
  {
    return 1;
  }
  return 0;
}

void LED::turnOff(int pin)
{
  for(int i = 0; i < 180; i++)
  {
    digitalWrite(pin, HIGH);
  }
}
void LED::turnOn(int pin)
{
  for(int i = 180; i < 0; i--)
  {
    digitalWrite(pin, LOW);
  }
}


void LED::toggleSwitch(int array[], int arraySize, int frequency, int pin) {
  for (int i = 0; i < arraySize; i++) {
    if (array[i] == 1) 
    {
      toggleOff(pin);
    } 
    else if(array[i] == 0)
    {
      toggleOn(pin);
    }
    else {}
    delay(frequency); 
  }
}

//LedClass.h

#ifndef MyClass_h
#define MyClass_h

#include "Arduino.h"

class LED
{
  public:
    bool state;
    LED();
    LED(int pin);
    void attach(int pin);
    void toggleOn(int pin);
    void toggleOff(int pin);
    bool IsTimePassedFromInterval(float startTimeInSeconds, float intervalInSeconds);
    float MillisecondsToSeconds(int milliseconds);
    void Reverse(bool value, int pin);
    int getState(bool state);
    void turnOff(int pin);
    void turnOn(int pin);
    void toggleSwitch(int array[], int arraySize, int frequency, int pin);
  private:
   int _pin; 
};
#endif

