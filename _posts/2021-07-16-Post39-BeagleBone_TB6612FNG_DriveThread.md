---
layout: post
title: "Using threads to drive a DC motor with the TB6612FNG driver in the BeagleBone Black"
author: wilmer
categories: [BeagleBone]
tags: [DCMotors, TB6612FNG]
image: assets/images/Post39/CoverPost.png
description: "Using threads to drive a DC motor with the TB6612FNG driver in the BeagleBone Black while this is doing some other stuff"
featured: false
hidden: false
rating: 4.5
date:   2021-07-16 08:00:00 -0500
---

<p align="center">
Pink spool icon made by <a href="https://www.flaticon.com/authors/iconixar" title="iconixar">iconixar</a> from <a href="https://www.flaticon.com/" title="Flaticon">www.flaticon.com</a>
</p>

In this post, I show how to drive a DC Motor from the keyboard using the BeagleBone and the <a href="https://toshiba.semicon-storage.com/ap-en/semiconductor/product/motor-driver-ics/brushed-dc-motor-driver-ics/detail.TB6612FNG.html">TB6612FNG</a> driver from Toshiba. 

The remarkable aspect is the use of <font color="red">threads</font> to drive the motor, an aspect that lets the program does some other things at the same time that the motor is running. In the <a href="{{ site.baseurl }}{% post_url 2021-07-11-Post37-BeagleBone_TB6612FNG_Drive %}"> first entry</a> dedicated to this driver, you can read about how to drive a motor with it.

It is important to remember that the logic voltage for the BeagleBone is <font color="red">3.3V</font>. If the user provides a greater voltage, the BeagleBone could be damaged.


## Circuit and components

The circuit can be seen in Figure 1. It consists of a TB6612FNG driver, a low voltage DC Motor, 4 AA batteries, and the BeagleBone. 


<figure style="text-align: center; 
              margin-left: auto; 
              margin-right: auto;">
    <img src="../assets/images/Post39/Circuit.png" alt="circuit.png" width="100%"/>
  <figcaption>
    Figure 1: Circuit to drive a DC motor with the TB6612FNG driver.
  </figcaption>
</figure>

The components are:
- 1 DC Motor driver TB6612FNG
- 1 DC Motor 3.0 - 6.0V
- 4 AA Batteries 
- 1 Protoboard mini
- Jumpers male-male to make the connections

The pins used for control the motor are:
- GPIO **P8_12** and **P8_14** to control the motor rotation direction
- PWM **P8_13** to control the speed
- GPIO **P8_16** to activate / deactivate de driver

## Coding
  
First, some `GPIO` and `TB6612FNG` objects are declared with global scope to initialize the digital pin to activate/deactivate the driver, the control pins, and the Motor that will be driven. These `GPIO` objects are used as the parameters to construct the `TB6612FNG` object. 

Remember that the user can include a boolean initialization parameter `true/false` to invert by software the motor rotation direction instead of inverting the motor's jumpers physically.

```cpp
// Declare the pin to activate / deactivate the TB6612FNG module
GPIO standByPin(P8_16);

// Declaring the pins for motor
GPIO AIN1 (P8_12);
GPIO AIN2 (P8_14);
PWM PWMA (P8_13);

// Declare the motor object
TB6612FNG MotorA (AIN1, AIN2, PWMA, false);
```

The first thing to do is to activate the TB6612FNG driver. This can be done with the next line:

```cpp
// Activate the module
ActivateTB6612FNG(standByPin);
```

In this code, the method `DriveThread()` is used to create a thread each time the motor is driven. These threads will be store in a `vector` of threads inside the code. This method is shown in the next listing:

```cpp
/*
  Public method to drive the motor during a certain time inside a thread
  @param int: the desired speed (-100,100)
  @param int: The desired duration in milliseconds
  @param ACTION: Confirm to brake / stop / Nothing the motor after driving it.    
*/
void TB6612FNG::DriveThread(int speed, int duration, ACTION action)
{
  std::thread motorThread = std::thread(&TB6612FNG::MakeDriveThread, this, speed, duration, action);
  vectorDriveThreads.push_back(std::move(motorThread));
}
```

The method `MakeDriveThread()` takes each thread and calls the method `Drive()` to drive the motor during a certain time and `stop` or `brake` this after the movement. This method is:

```cpp
/*
  Private method that contains the routine to drive 
  the motor during a certain time
  @param int: the desired speed (-100,100)  
*/
void TB6612FNG::MakeDriveThread(int speed, int duration, ACTION action)
{
  Drive(speed, duration, action);
}
```

On the other hand, in the implementation, the user can change the motor speed using the key "W" and "S" to increase or decrease it, respectively. The library does not let to set a speed beyond the limits of 100 and -100. If the user presses the key "Y", the program finishes. Immediately, the method `DriveThread()` is used to create a thread each time the motor is driven. These threads will be store in a `vector` of threads inside the code. While the motor is running, a message will be printed on the screen 100 times.

```cpp
// Drive the motors and printing messages on the terminal
switch (userInput)
{
case 'w':
  motorSpeed += 10;
  MotorA.DriveThread(motorSpeed,5000, stop);
  for(int i = 0; i < 100; i++)
    cout << "Doing something else while the motor is running" << endl;
  break;
case 's':
  motorSpeed -= 10;
  MotorA.DriveThread(motorSpeed,5000, stop);
  for(int i = 0; i < 100; i++)
    cout << "Doing something else while the motor is running" << endl;
  break;
default:
  break;
}
```

The instruction to move the motors is `MotorA.DriveThread(motorSpeed,5000,stop)`. It creates the thread and calls the `Drive()` method to drive the motor for 5 seconds at the desired speed and stops it after the movement.

```cpp
  // Move the motor
  MotorA.DriveThread(motorSpeed,5000, stop);
```

Finally, the TB6612FNG has to be deactivated using the next line.

```cpp
// Deactivate the module
DeactivateTB6612FNG(standByPin);
```

The complete code for this application using a `while loop` to drive the motor increasing and decreasing the speed until the user press the key "Y" and printing the messages on the screen. This code is shown in the next listing together with its corresponding execution output.


### TB6612FNG_1.3.cpp
```cpp
/******************************************************************************
TB6612FNG_1.3.cpp
@wgaonar
03/07/2021
https://github.com/wgaonar/BeagleCPP

- Drive a motor in a thread and printing messages in the terminal at the same 
  time

Class: TB6612FNG
******************************************************************************/
#include <iostream>
#include "../../../Sources/TB6612FNG.h"

using namespace std;

// Declare the pin to activate / deactivate the TB6612FNG module
GPIO standByPin(P8_16);

// Declaring the pins for motor
GPIO AIN1 (P8_12);
GPIO AIN2 (P8_14);
PWM PWMA (P8_13);

// Declare the motor object
TB6612FNG MotorA (AIN1, AIN2, PWMA, false);

int main()
{
  string message = "Main program starting here...";
  cout << RainbowText(message,"Blue", "White", "Bold") << endl;

  // Activate the module
  ActivateTB6612FNG(standByPin);

  message = "If you want to stop the program, enter 'y' for yes";
  cout << RainbowText(message, "Blue") << endl;
  message = "Or enter 'w' for increase speed or 's' for decrease it";
  cout << RainbowText(message, "Blue") << endl;

  int motorSpeed = 0;
  char userInput = '\0';
  while (userInput != 'y')
  {
    message = "Enter an option 'y', 'w', 's': ";
    cout << RainbowText(message, "Blue");
    cin >> userInput;

    // Drive the motors and printing messages on the terminal
    switch (userInput)
    {
    case 'w':
      motorSpeed += 10;
      MotorA.DriveThread(motorSpeed,5000, stop);
      for(int i = 0; i < 100; i++)
        cout << "Doing something else while the motor is running" << endl;
      break;
    case 's':
      motorSpeed -= 10;
      MotorA.DriveThread(motorSpeed,5000, stop);
      for(int i = 0; i < 100; i++)
        cout << "Doing something else while the motor is running" << endl;
      break;
    default:
      break;
    }
  }

  // Deactivate the module
  DeactivateTB6612FNG(standByPin);  

  message = "Main program finishes here...";
  cout << RainbowText(message,"Blue", "White","Bold") << endl;

  return 0;
}
```

### Execution of the program:
<figure style="text-align: center; width:100%; 
              margin-left: auto; 
              margin-right: auto;">
  <video width="100%" controls poster="../assets/images/Post39/VideoCover-TB6612FNG_1.3.png">
    <source src="../assets/images/Post39/TB6612FNG_1.3.mp4" type="video/mp4">
  </video>
  <figcaption>
    Video: Execution of the program.
  </figcaption>
</figure>

Se you in the next post. 
