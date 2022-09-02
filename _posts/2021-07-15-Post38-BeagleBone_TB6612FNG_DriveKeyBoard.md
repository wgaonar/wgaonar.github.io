---
layout: post
title: "Control a DC motor from the keyboard with the TB6612FNG driver and the BeagleBone Black"
author: wilmer
categories: [BeagleBone]
tags: [DCMotors, TB6612FNG]
image: assets/images/Post38/CoverPost.png
description: "Driving a DC motor through the keyboard using the TB6612FNG driver with the BeagleBone Black"
featured: false
hidden: false
rating: 4.5
date:   2021-07-15 08:00:00 -0500
---

In this post, I show how to drive a DC Motor from the keyboard using the BeagleBone and the <a href="https://toshiba.semicon-storage.com/ap-en/semiconductor/product/motor-driver-ics/brushed-dc-motor-driver-ics/detail.TB6612FNG.html">TB6612FNG</a> driver from Toshiba. In the <a href="{{ site.baseurl }}{% post_url 2021-07-11-Post37-BeagleBone_TB6612FNG_Drive %}">last entry</a>, you can read about how to drive a motor with this driver.

It is important to remember that the logic voltage for the BeagleBone is <font color="red">3.3V</font>. If the user provides a greater voltage, the BeagleBone could be damaged.

## Circuit and components

The circuit can be seen in Figure 1. It consists of a TB6612FNG driver, a low voltage DC Motor, 4 AA batteries, and the BeagleBone. 

<figure style="text-align: center; 
              margin-left: auto; 
              margin-right: auto;">
    <img src="../assets/images/Post38/Circuit.png" alt="circuit.png" width="100%"/>
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

Next, the user can change the motor speed using the key "W" and "S" to increase or decrease it, respectively. The class method `DCMotor::Drive()`, checks if the user has input a value for the speed beyond the limits of <font color="red">100 and -100`</font> and keep it between this range, but in this case, the user code can do that too. If the user presses the key "Y", the program finishes.

```cpp
message = "Enter an option 'y', 'w', 's': ";
cout << RainbowText(message, "Yellow");
cin >> userInput;

switch (userInput)
{
case 'w':
  motorSpeed += 10;
  if (motorSpeed >= 100)
    motorSpeed = 100;
  break;
case 's':
  motorSpeed -= 10;
  if (motorSpeed <= -100)
    motorSpeed = -100;
  break;
default:
  break;
}
```

The instruction to move the motors is `MotorA.Drive(i);` which is the core class method that receives as an argument the speed value and depending on that, it configures the desired rotation direction and speed for the motor. 

```cpp
  // Move the motor
  MotorA.Drive(motorSpeed);
```

Finally, the TB6612FNG has to be deactivated using the next line.

```cpp
// Deactivate the module
DeactivateTB6612FNG(standByPin);
```

The code uses a `while loop` to drive the motor increasing and decreasing the speed until the user press the key "Y". This is shown in the next listing together with its corresponding execution output.

### TB6612FNG_1.2.cpp
```cpp
/******************************************************************************
TB6612FNG_1.2.cpp
@wgaonar
04/07/2021
https://github.com/wgaonar/BeagleCPP

- Control de motor speed with 'w' and 's' keys while the user does not press 'y'

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
  // Activate the module
  ActivateTB6612FNG(standByPin);

  string message = "Main program starting here...";
  cout << RainbowText(message,"Blue", "White", "Bold") << endl;

  message = "If you want to stop the program, enter 'y' for yes";
  cout << RainbowText(message, "Yellow") << endl;
  message = "Or enter 'w' for increase speed or 's' for decrease it";
  cout << RainbowText(message, "Yellow") << endl;

  int motorSpeed = 0;
  char userInput = '\0';
  while (userInput != 'y')
  {
    message = "Enter an option 'y', 'w', 's': ";
    cout << RainbowText(message, "Yellow");
    cin >> userInput;

    switch (userInput)
    {
    case 'w':
      motorSpeed += 10;
      if (motorSpeed >= 100)
        motorSpeed = 100;
      break;
    case 's':
      motorSpeed -= 10;
      if (motorSpeed <= -100)
        motorSpeed = -100;
      break;
    default:
      break;
    }

    // Move the motor
    MotorA.Drive(motorSpeed);
  }

  // Deactivate the module
  DeactivateTB6612FNG(standByPin);  

  message = "Main program finishes here...";
  cout << RainbowText(message,"Blue", "White","Bold") << endl;

  return 0;
}
```

### Execution of the program:
<Figure style="text-align: center; width:100%; 
              margin-left: auto; 
              margin-right: auto;">
  <video width="100%" controls poster="../assets/images/Post38/VideoCover-TB6612FNG_1.2.png">
    <source src="../assets/images/Post38/TB6612FNG_1.2.mp4" type="video/mp4">
  </video>
  <figcaption>
    Video: Execution of the program.
  </figcaption>
</figure>

Se you in the next post. 
