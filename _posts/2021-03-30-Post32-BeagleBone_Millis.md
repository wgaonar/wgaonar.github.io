---
layout: post
title: "Blinking a LED without Delay in the BeagleBone Black"
author: wilmer
categories: [BeagleBone]
tags: [GPIO]
image: assets/images/Post32/CoverPost.png
description: "Updates to the C++ library for the BeagleBone Black"
featured: false
hidden: false
rating: 4
date:   2021-03-30 08:00:00 -0600
---

In this post, I show how to blink a LED with using Delays routines. This is done through the implementation of some functions that retrieve the time and duration of time marks in `C++`. These functions are like the `millis()` and `micros()` in Arduino. In a <a href="{{ site.baseurl }}{% post_url 2020-12-23-Post11-BeagleBone_LedOn %}"> previous post </a> I showed how to make a blink on la LED using Delays, in contrast, in this post I show how to use the new functions to signal time events without the need to freeze the BeagleBone while it is doing the delay.

## Circuit and components

The circuit can be seen in Figure 1. Please keep in mind that the BeagleBone works at <font color="red">3.3V</font> and not 5V like microcontrollers as Arduino. It is so important to avoid damage to the board, especially when you are working with buttons or digital inputs in general. 


The components are:
- 1 Resistor of 1KΩ
- 1 LED of 3mm
- Jumpers male-male to make the connections

<figure style="text-align: center; width:70%; 
              margin-left: auto; 
              margin-right: auto;">
    <img src="../assets/images/Post32/Circuit_bb.png"
    alt="Circuit.jpg" width="100%"/>
  <figcaption>
    Figure 1: Circuit to make a delay on la LED with Millis() / Micros() / Nanos() function.
  </figcaption>
</figure>

## Coding
  
The function `Millis()` is defined. This takes the Coordinated Universal Time (UTC) and then the actual time. After that, the `elapsedTime` object is declared and initialized as de difference between the actual time and the initial UTC time in the BeagleBone, i.e., the interval of time between the actual time and the time since 00:00:00 UTC, Thursday, 1 January 1970. This is an object of a `duration` class from which the `count()` member can be used to obtain the time in milliseconds.

```cpp
unsigned long Millis()
{
  // Retrieve the  Unix Time (UTC), Thursday, 1 January 1970.
  time_point<system_clock> UTCTime = time_point<system_clock>{};

  // Retrieve the actual time
  time_point<system_clock> actualTime = system_clock::now();

  duration<double> elapsedTime = actualTime - UTCTime;

  return duration_cast<milliseconds>(elapsedTime).count();
}
```

In the same way, the `Micros()` the `Nanos()` functions are defined to signal time events in milliseconds and nanoseconds, respectively.

```cpp
unsigned long Micros()
{
  // Retrieve the  Unix Time (UTC), Thursday, 1 January 1970.
  time_point<system_clock> UTCTime = time_point<system_clock>{};

  // Retrieve the actual time
  time_point<system_clock> actualTime = system_clock::now();

  duration<double> elapsedTime = actualTime - UTCTime;

  return duration_cast<microseconds>(elapsedTime).count();
}
```

```cpp
unsigned long Nanos()
{
  // Retrieve the  Unix Time (UTC), Thursday, 1 January 1970.
  time_point<system_clock> UTCTime = time_point<system_clock>{};

  // Retrieve the actual time
  time_point<system_clock> actualTime = system_clock::now();

  duration<double> elapsedTime = actualTime - UTCTime;

  return duration_cast<nanoseconds>(elapsedTime).count();
}
```

The complete codes for the application of this functions are shown in the next listings together with its corresponding execution videos.


### Listing_1.4

```cpp
/************************************************************************
Listing_1.4.cpp
@wgaonar
28/03/2021
https://github.com/wgaonar/BeagleCPP

Blink a LED in milliseconds without Delayms()

Class: GPIO
************************************************************************/
#include <iostream>
#include "../../Sources/GPIO.h"

using namespace std;

int main()
{
  string message = "Main program starting here...";
  cout << RainbowText(message,"Blue", "Bold") << endl;
  
  GPIO ledPin(P8_12, OUTPUT);

  STATE ledState = LOW;
  unsigned long previousMillis = 0; // will store last time LED was updated
  const int interval = 1000;       // interval at which to blink (milliseconds)

  int count = 0;
  while (count < 10)
  {
    // Get the actual time
    unsigned long currentMillis = Millis();

    if ((currentMillis - previousMillis) >= interval) 
    {
      // save the last time you blinked the LED
      previousMillis = currentMillis;

      // if the LED is off turn it on and vice-versa:
      if (ledState == LOW) 
        ledState = HIGH;
      else
      { 
        ledState = LOW;
        count++;
        cout << "Blinking " << count << " times of 10\n";
      }

      // set the LED with the ledState of the variable:
      ledPin.DigitalWrite(ledState);
    }  
  }

  message = "Main program finishes here...";
  cout << RainbowText(message,"Blue", "Bold") << endl;
  return 0;
}
```

### Execution of the program:

<figure style="text-align: center; width:100%; 
              margin-left: auto; 
              margin-right: auto;">
  <video width="100%" controls poster="../assets/images/Post32/VideoCover-Listing_1.4.png">
    <source src="../assets/images/Post32/Listing_1.4.mp4" type="video/mp4">
  </video>
  <figcaption>
    Video: Execution of the program.
  </figcaption>
</figure>

### Listing_1.5

```cpp
/************************************************************************
Listing_1.5.cpp
@wgaonar
28/03/2021
https://github.com/wgaonar/BeagleCPP

Blink a LED in microseconds without Delayus()

Class: GPIO
************************************************************************/
#include <iostream>
#include "../../Sources/GPIO.h"

using namespace std;

int main()
{
  string message = "Main program starting here...";
  cout << RainbowText(message,"Blue", "Bold") << endl;
  
  GPIO ledPin(P8_12, OUTPUT);

  STATE ledState = LOW;
  unsigned long previousMicros = 0; // will store last time LED was updated
  const int interval = 1000000;     // interval at which to blink (microseconds)

  int count = 0;
  while (count < 10)
  {
    // Get the actual time
    unsigned long currentMicros = Micros();

    if ((currentMicros - previousMicros) >= interval) 
    {
      // save the last time you blinked the LED
      previousMicros = currentMicros;

      // if the LED is off turn it on and vice-versa:
      if (ledState == LOW) 
        ledState = HIGH;
      else
      { 
        ledState = LOW;
        count++;
        cout << "Blinking " << count << " times of 10\n";
      }

      // set the LED with the ledState of the variable:
      ledPin.DigitalWrite(ledState);
    }  
  }

  message = "Main program finishes here...";
  cout << RainbowText(message,"Blue", "Bold") << endl;
  return 0;
}
```

### Execution of the program:

<figure style="text-align: center; width:100%; 
              margin-left: auto; 
              margin-right: auto;">
  <video width="100%" controls poster="../assets/images/Post32/VideoCover-Listing_1.5.png">
    <source src="../assets/images/Post32/Listing_1.5.mp4" type="video/mp4">
  </video>
  <figcaption>
    Video: Execution of the program.
  </figcaption>
</figure>

### Listing_1.6

```cpp
/************************************************************************
Listing_1.6.cpp
@wgaonar
28/03/2021
https://github.com/wgaonar/BeagleCPP

Blink a LED in nanoseconds  

Class: GPIO
************************************************************************/
#include <iostream>
#include "../../Sources/GPIO.h"

using namespace std;

int main()
{
  string message = "Main program starting here...";
  cout << RainbowText(message,"Blue", "Bold") << endl;
  
  GPIO ledPin(P8_12, OUTPUT);

  STATE ledState = LOW;
  unsigned long previousNanos = 0;  // will store last time LED was updated
  int interval = 1000000000;        // interval at which to blink (nanoseconds)

  int count = 0;

  // First while loop with a 1000000000ns (1s) for a delay
  while (count < 10)
  {
    // Get the actual time
    unsigned long currentNanos = Nanos();

    if ((currentNanos - previousNanos) >= interval) 
    {
      // save the last time you blinked the LED
      previousNanos = currentNanos;

      // if the LED is off turn it on and vice-versa:
      if (ledState == LOW) 
        ledState = HIGH;
      else
      { 
        ledState = LOW;
        count++;
        cout << "Blinking " << count << " times of 10\n";
      }

      // set the LED with the ledState of the variable:
      ledPin.DigitalWrite(ledState);
    }  
  }

  // seconde while loop for a 100ns
  interval = 500;
  count = 0;
  while (count < 1000)
  {
    // Get the actual time
    unsigned long currentNanos = Nanos();

    if ((currentNanos - previousNanos) >= interval) 
    {
      // save the last time you blinked the LED
      previousNanos = currentNanos;

      // if the LED is off turn it on and vice-versa:
      if (ledState == LOW) 
        ledState = HIGH;
      else
      { 
        ledState = LOW;
        count++;
        cout << "Blinking " << count << " times of 1000\n";
      }

      // set the LED with the ledState of the variable:
      ledPin.DigitalWrite(ledState);
    }  
  }

  message = "Main program finishes here...";
  cout << RainbowText(message,"Blue", "Bold") << endl;
  return 0;
}
```

### Execution of the program:

<figure style="text-align: center; width:100%; 
              margin-left: auto; 
              margin-right: auto;">
  <video width="100%" controls poster="../assets/images/Post32/VideoCover-Listing_1.6.png">
    <source src="../assets/images/Post32/Listing_1.6.mp4" type="video/mp4">
  </video>
  <figcaption>
    Video: Execution of the program.
  </figcaption>
</figure>

Se you in the next post. 
