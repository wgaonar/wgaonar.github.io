---
layout: post
title: "Control a DC motor from the keyboard with the L298N driver and the BeagleBone Black"
author: wilmer
categories: [BeagleBone]
tags: [DCMotors, L298N]
image: assets/images/Post50/CoverPost.png
description: "Driving a DC motor through the keyboard using the L298N driver with the BeagleBone Black"
featured: false
hidden: false
rating: 4.5
date:   2022-05-02 08:00:00 -0500
---

In this post, I show how to drive a DC Motor from the keyboard using the driver <a href="https://www.sparkfun.com/datasheets/Robotics/L298_H_Bridge.pdf">L298N</a> which can drive two DC motors. In practice, this driver can be found in a popular red-colored module that has been designed to make it easy to play and interact with DC motors as this <a href="https://lastminuteengineers.com/l298n-dc-stepper-driver-arduino-tutorial/">tutorial </a> shows. 
  
In the <a href="{{ site.baseurl }}{% post_url 2022-05-01-Post49-BeagleBone_L298N_Drive %}">last entry</a>, you can read about how to drive a motor with this driver.

It is important to remember that the logic voltage for the BeagleBone is <font color="red">3.3V</font>. If the user provides a greater voltage, the BeagleBone could be damaged.


## Circuit and components

The circuit can be seen in Figure 1. It consists of a L298N driver module, a low voltage DC Motor, batteries, and the BeagleBone. 

<figure style="text-align: center; 
              margin-left: auto; 
              margin-right: auto;">
    <img src="../assets/images/Post50/Circuit.png" alt="circuit.png" width="100%"/>
  <figcaption>
    Figure 1: Circuit to drive a DC motor with the L298N driver.
  </figcaption>
</figure>

The components are:
- 1 DC Motor driver module L298N
- 1 DC Motor 5.0 - 6.0V
- 4 AA Batteries o 6 AA rechargeable batteries at 1.2V
- 1 Protoboard mini
- Jumpers male-male to make the connections

The pins used for control the motor are:
- GPIO **P8_12** and **P8_14** to control the motor rotation direction
- PWM **P8_13** to control the speed

## Coding
  
First, a `DCMotor` object is declared. For that, two `GPIO` and one `PWM` objects are declared with global scope to initialize the motor that will be driven. 

The `GPIO`objects are named `AIN1 and AIN2`, while the `PWM`object is `PWMA` and are used to initialize the `DCMotor` object named `MotorLeft` which contains the methods to set the speed, the spin direction and to drive or stop the DC motor. 

In this `DCMotor` object, the user can include a fourth boolean initialization parameter `true/false` to invert by software the motor direction rotation instead of inverting the motor's jumpers physically.
 
This `DCMotor` object is used to initialize the `L298N` object. This inheritance structure has the goal to encapsulate the corresponding methods for any generic DC motor avoiding repeat code in the `L298N` object and focusing on the methods to **drive / brake** the DC motor, and not only one, but the two motors at the same time and in a different direction if it desired. 

```cpp
// Declaring the pins for motor
GPIO AIN1 (P8_12);
GPIO AIN2 (P8_14);
PWM PWMA (P8_13);

// Declare the motor object
DCMotor MotorLeft (AIN1, AIN2, PWMA);

// Declare the L298N Module
L298N myL298NModule (MotorLeft);
```

Next, the user can change the motor speed using the key "W" and "S" to increase or decrease it,respectively. The class method `DCMotor::Drive()`, checks if the user has input a value for the speed beyond the limits of <font color="red">100 and -100`</font> and keep it between this range, but in this case, the user code can do that too. If the user presses the key "Y", the program finishes.

In the main program, some <span class="coding">for loops` can be used to drive the motor by increasing and decreasing the speed and driving it in both directions.

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

To make turn the DC motor, the method is `L298N::DCMotor::Drive` can be used. This receives two parameters:
- Speed in percentage, i.e. integer values between <font color="red">100 and -100</font>
- Time of the spin duration in milliseconds
  
A **positive** speed value sets the motor to turn in the **CW** direction, while, a **negative** speed value sets the motor to turn in the **CCW** direction.

```cpp
myL298NModule.MotorA.Drive(speed,duration);
```

The complete code for this application is shown in the next listing together with its corresponding execution output.

### L298N_1.2.cpp
```cpp
/******************************************************************************
L298N_1.2.cpp
@wgaonar
04/07/2021
https://github.com/wgaonar/BeagleCPP

- Control de motor speed with 'w' and 's' keys while the user does not press 'y'

Class: L298N
******************************************************************************/
#include <iostream>
#include "../../../Sources/L298N.h"

using namespace std;

// Declaring the pins for motor
GPIO AIN1 (P8_12);
GPIO AIN2 (P8_14);
PWM PWMA (P8_13);

// Declare the motor object
DCMotor MotorLeft (AIN1, AIN2, PWMA);

// Declare the L298N Module
L298N myL298NModule (MotorLeft);

int main()
{
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
    myL298NModule.MotorA.Drive(motorSpeed);
  }

  message = "Main program finishes here...";
  cout << RainbowText(message,"Blue", "White","Bold") << endl;

  return 0;
}
```

Se you in the next post. 
