---
layout: post
title: "Implementation of the LED Class for BeagleBone Black"
author: wilmer
categories: [BeagleBone]
tags: [LED]
image: assets/images/Post13/CoverPost.png
description: "Source code for the C++ LED Class"
featured: false
hidden: false
rating: 4
date:   2020-12-23 08:00:00 -0600
---

In this post, I show a LED C++ class implementation which is derived from GPIO class and adds some methods for blink a led. 

## Coding

The methods coded to the C++ class `BUTTON` are:
- Private methods:
  - `void MakeBlink(int)`
  - `void MakeFlash(int, int)`
  - `void MakeHeartBeat(int, int)`
 
- Public methods:
  - `void TurnOn()`
  - `void TurnOff()`
  - `void Blink(int)`
  - `void Flash(int, int)`
  - `void HeartBeat(int, int)`
  - `void StopBlink()`
  - `void StopFlash()`
  - `void StopHeartBeat()`

The `TurnOn()` method sets the Led to On.
The `TurnOff()` method sets the Led to Off.

The `Blink(int)` method receives the duration in milliseconds of blinking from the main program. It calls the `MakeBlink(int)` method to do the blinking.

The `Flash(int, int)` method receives two values from the main program, the duration in milliseconds to turn the Led On and Off respectively. It calls the `MakeFlash(int, int)` method to do the flashing.

The `HeartBeat(int, int)` method receives two values from the main program. The first is the time in milliseconds to turn the Led On and Off two times, and the second is a ratio to keep the Led Off during a certain time. It calls the `MakeHeartBeat(int, int)` method to do the heart beat pattern.

The `MakeBlink()` method contains the routine to do the blinking which uses the `digitalWrite()` and `Delayms()` methods inherited from the GPIO class.  The most important contribution is about the way that blinking is performed. It uses a <b>thread</b> constructed by means of a function pointer to do the blink pattern. The advantage is that one o more blinking patterns can be done at the same same time in a **parallel way** on different LEDs.

The `MakeFlash()` method contains the routine to do the flashing. Likewise to the `MakeBlink()` method, it uses the `digitalWrite()` and `Delayms()` methods inherited from the GPIO class and it is performed through a <b>thread</b> constructed by means of a function pointer to do the flash pattern.

The `MakeHeartBeat()` method contains the routine to do "digital" heart beat beat pattern which consists in turning the Led On and Off two times and keep the Led Off during a time calculated as the multiplication of the time On by this ratio value. Likewise to the `MakeBlink()` method, it uses the `digitalWrite()` and `Delayms()` methods inherited from the GPIO class and it is performed through a <b>thread</b> constructed by means of a function pointer to do the heart beat pattern.

The `StopBlink()` method lets the user stops the blinking pattern from the main program. This method sets the private boolean variable `stopBlinkFlag` to true letting to the `MakeBlink()` method completes or finishes the constructed thread.

The `StopFlash()` method lets the user stops the flash pattern from the main program. This method sets the private boolean variable `stopFlashFlag` to true letting to the `MakeFlash()` method completes or finishes the constructed thread.

The `StopHeartBeat()` method lets the user stops the heart beat pattern from the main program. This method sets the private boolean variable `stopHeartBeatFlag` to true letting to the `MakeHeartBeat()` method completes or finishes the constructed thread.

### LED.h

```cpp
#ifndef LED_H
#define LED_H

#include <thread>
#include "GPIO.h"

class LED: public GPIO 
{
  private:
    bool stopBlinkFlag = false;
    bool stopFlashFlag = false;
    bool stopHeartBeatFlag = false;
    
    std::thread blinkThread;
    std::thread flashThread;
    std::thread heartBeatThread;
    
    void MakeBlink(int); 
    void MakeFlash(int, int);
    void MakeHeartBeat(int, int);
  public:
    // Overload constructor
    LED (int);

    // Method to turn on the Led
    void TurnOn();

    // Method to turn off the Led
    void TurnOff();

    // Method for doing a blinking pattern
    void Blink(int);

    // Method for doing a flashing pattern
    void Flash(int, int);

    // Method for doing a digital heart beat pattern
    void HeartBeat(int, int);

    // Method for stopping a blinking
    void StopBlink();

    // Method for stopping a flashing
    void StopFlash();
    
    // Method for stopping a digital heart beat
    void StopHeartBeat();
    
    // Destructor
    ~LED();
};

#endif // LED_H
```

### LED.cpp

```cpp
#include <iostream>
#include <chrono>
#include <thread>

#include "LED.h"

// Overload constructor
LED::LED(int newId) : GPIO(newId, OUTPUT) {}

/*
  Public method to turn on the Led attached to the pin 
*/
void LED::TurnOn()
{
  this->DigitalWrite(HIGH);
}

/*
  Public method to turn on the Led attached to the pin 
*/
void LED::TurnOff()
{
  this->DigitalWrite(LOW);
}

/*
  Public method to make a blink on the pin 
  @param int: The desired duration in milliseconds
*/
void LED::Blink(int duration)
{
  string message; 
  message = "Blinking has been activated with duration of: ";
  message += to_string(duration) + "ms on pin: " + to_string(id);
  
  std::cout << RainbowText(message, "Pink", "Default", "Bold") << endl; 
  blinkThread = std::thread(&LED::MakeBlink, this, duration);
}

/*
  Private method that contains the routine to blink 
  @param int: The desired duration in milliseconds
*/
void LED::MakeBlink(int duration)
{
  while (this->stopBlinkFlag == false)
  {
    DigitalWrite(HIGH);
    Delayms(duration);
    DigitalWrite(LOW);
    Delayms(duration);
  }
}

/*
  Public method to stop the blinking on the pin 
*/
void LED::StopBlink ()
{
  stopBlinkFlag = true;
}

/*
  Public method to make a flash on the pin 
  @param int: The desired time ON in milliseconds
  @param int: The desired time OFF in milliseconds
*/
void LED::Flash(int timeOn, int timeOff)
{
  string message 
  {
    "Flashing has been activated time on: "
    + to_string(timeOn) + "ms and time off: " 
    + to_string(timeOff) + "ms on pin: " + to_string(id)
  };
  std::cout << RainbowText(message, "Pink", "Default", "Bold") << endl; 
  flashThread = std::thread(&LED::MakeFlash, this, timeOn, timeOff);
}

/*
  Private method that contains the routine to flash
  @param int: The desired time ON in milliseconds
  @param int: The desired time OFF in milliseconds
*/
void LED::MakeFlash(int timeOn, int timeOff)
{
  while (this->stopFlashFlag == false)
  {
    DigitalWrite(HIGH);
    Delayms(timeOn);
    DigitalWrite(LOW);
    Delayms(timeOff);
  }
}

/*
  Public method to stop the flash on the pin 
*/
void LED::StopFlash ()
{
  stopFlashFlag = true;
}

/*
    Public method to make a digital heart beat on the pin 
    @param int: The desired time On of the pulse in milliseconds
    @param int: The desired ratio between the pulses and the pause in the pattern
*/
void LED::HeartBeat(int timeOn, int ratio)
{
  string message 
  {
    "Heart beat has been activated with a time ON of: "
    + to_string(timeOn) + "ms on pin: " + to_string(id)
    + " with a ratio pulse/pause of: " + to_string(ratio)
  };
  std::cout << RainbowText(message, "Pink", "Default", "Bold") << endl; 
  heartBeatThread = std::thread(&LED::MakeHeartBeat, this, timeOn, ratio);
}

/*
    Private method that contains the routine to do the digital heart beat 
    @param int: The desired time On of the pulse in milliseconds
    @param int: The desired ratio between the pulses and the pause in the pattern
*/
void LED::MakeHeartBeat(int timeOn, int ratio)
{
  while (this->stopHeartBeatFlag == false)
  {
    for (int i = 0; i < 2; i++)
    {
      DigitalWrite(HIGH);
      Delayms(timeOn);
      DigitalWrite(LOW);
      Delayms(timeOn);
    }
    Delayms(timeOn*ratio);
  }
}

/*
  Public method to stop the digital heart beat on the pin 
*/
void LED::StopHeartBeat ()
{
    stopHeartBeatFlag = true;
}

// Destructor
LED::~LED()
{
  if (blinkThread.joinable())
    blinkThread.join();
  if (flashThread.joinable())
    flashThread.join();
  if (heartBeatThread.joinable())
    heartBeatThread.join();
}
```

Se you in the next post.
