---
layout: post
title: "Drive any DC Motors with the L298N driver and the BeagleBone Black"
author: wilmer
categories: [BeagleBone]
tags: [DCMotors, L298N]
image: assets/images/Post53/CoverPost.png
description: "Drive any numbers of DC motors forward and backward with the L298N driver and the BeagleBone Black"
featured: false
hidden: false
rating: 4.5
date:   2022-05-05 08:00:00 -0500
categories: BeagleBone
---

In this post, I show how to control **any** number of DC Motors to move forward or backward using the BeagleBone and the driver <a href="https://www.sparkfun.com/datasheets/Robotics/L298_H_Bridge.pdf">L298N</a>. In practice, this driver can be found in a popular red-colored module that has been designed to make it easy to play and interact with DC motors as this <a href="https://lastminuteengineers.com/l298n-dc-stepper-driver-arduino-tutorial/">tutorial</a> shows. 

In the <a href="{{ site.baseurl }}{% post_url 2022-05-04-Post52-BeagleBone_L298N_Drive2Motors %}">last entry</a>, you can read about how to drive a pair of DC motors with this driver.

The remarkable aspect is the use of **C++ vectors** to drive simultaneously the number of DC motors the user wants in the same rotation direction. You have to take into account that the L298N driver only can manage 2 DC motors. If you need more, you have to add more drivers. In this post, I show how to use the STL `std::vector` to drive 2 DC motors.

It is important to remember that the logic voltage for the BeagleBone is <font color="red">3.3V</font>. If the user provides a greater voltage, the BeagleBone could be damaged.


## Circuit and components

  The circuit can be seen in Figure 1. It consists of a L298N driver module , 2 low voltage DC Motors, batteries, and the BeagleBone. 


<figure style="text-align: center; 
              margin-left: auto; 
              margin-right: auto;">
    <img src="../assets/images/Post53/Circuit.png" alt="circuit.png" width="100%"/>
  <figcaption>
    Figure 1: Circuit to drive a pair of DC motor with the L298N driver.
  </figcaption>
</figure>

The components are:
- 1 DC Motor driver TB6612FNG
- 2 DC Motor 3.0 - 6.0V
- 4 AA Batteries 
- 1 Protoboard mini
- Jumpers male-male to make the connections

The pins used for control the driver and the motors are:
- The pins used for control the **motor A** are:
  - GPIO **P8_12** and **P8_14** to control the motor rotation direction
  - PWM **P8_13** to control the speed
- The pins used for control the **motor B** are:
  - GPIO **P8_17** and **P8_18** to control the motor rotation direction
  - PWM **P8_19** to control the speed

## Coding
  
First, two `DCMotor` objects are declared. For that, `GPIO` and `PWM` objects are declared with global scope to initialize each one of both motors that will be driven. 

These objects are named `AIN1`, `AIN2`, `PWMA`, `BIN1`, `BIN2`, and `PWMB` for the `MotorA` and `MotorB` objects, respectively. These are `DCMotor` objects declared as `MotorLeft` and `MotorRight` and contain methods to set the speed and the spin direction to drive the DC motor and to stop it, as well as, a fourth boolean initialization parameter `true/false` to invert by software the motor direction rotation instead of inverting the motor's jumpers physically.

As is shown next, I have used this parameter to invert the rotation direction of the `MotorA`, setting the last parameter to `true`, instead of inverting the jumpers physically. This feature can be useful when you do not have access to the circuit or the motor directly.  


```cpp
// Declaring the pins for MotorA 
GPIO AIN1 (P8_12);
GPIO AIN2 (P8_14);
PWM PWMA (P8_13);

// Declare the MotorA
DCMotor MotorLeft (AIN1, AIN2, PWMA, true);

// Declaring the  pins for MotorB
GPIO BIN1 (P8_17);
GPIO BIN2 (P8_18);
PWM PWMB (P8_19);

// Declare the MotorB
DCMotor MotorRight (BIN1, BIN2, PWMB);
```

These `DCMotor` objects are used to initialize the `L298N` object with two of them. This inheritance structure has the goal to encapsulate the corresponding methods for any generic DC motor in the `DCMotor` object avoiding repeat code in the `L298N` object and focusing on the methods to **drive / brake** not only one, but the two motors at the same time and in a different direction if it desired. 

```cpp
// Declare the L298N Module
L298N myL298NModule (MotorLeft, MotorRight);
```

After that, I declare a **vector of pointers** to the `L298N` objects and initialize it with the object declared previously but passing these by **reference** instead of by value. This avoids creating a copy for each object and working directly with the L298N modules. Is here, where the user can add the number of modules desired.

```cpp
// Declare the vector of pointers to L298N objects
vector<L298N *> vectorOfL298N = {&myL298NModule};
```

To control the speed of both motors, the user can change it using the keys "W" and "S" to increase or decrease it, respectively. The class method `Drive()` checks if the user has input a value for the speed beyond the limits of <font color="red">100 and -100</font> and keeps it between this range, but in this case, the user code can do that too. If the user presses the key "Y", the program finishes.

```cpp
// Update the motors speed
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

To move the motors, the code uses the public template functions `Forward()` and `Backward()` defined in a separate **template file** which can be called directly from the main implementation code.

These `Forward()` and `Backward()` functions, take as arguments a vector of pointers to `typename Module` objects passed by reference, the speed value that will be used to drive the motors, the duration of the movement, and the action to do after the movement has been executed. Inside of these functions, a `for loop` iterates on each `Module` object, i.e., on each DC motor driver, and runs it in the same rotation direction at the desired speed.

It is important to note that, these functions receive <font color="red">any DC motor module </font> object and call the corresponding methods to drive the DC motors according to the respective module. For example the TB6612FNG module, which was shown in <a href="{{ site.baseurl }}{% post_url 2021-07-11-Post37-BeagleBone_TB6612FNG_Drive %}">this entry</a>.

```cpp
/*
  Function to drive FORWARD a robot with ANY number of Module drivers 
  during certain time
  @param std::vector<Module *>: The vector of pointers to Module objects  
  @param int speed: The desired speed (0,100). It set up the correct value if the user enters 
                    a negative value.
  @param int duration:  The desired duration in milliseconds with 0 as 
                        default value
  @param STOPMODE action <brake / idle>:  Action on the motor after driving it with 
                                        <idle> as default action.  
*/
template <typename Module>
void Forward (std::vector<Module *> vectorOfModules, int speed, int duration, STOPMODE action)
{
  if (speed < 0)
    speed *= -1;

  for (auto module : vectorOfModules)
  {
    if (module->GetMotorAisUsed())
      module->MotorA.Drive(speed);
    if (module->GetMotorBisUsed())
      module->MotorB.Drive(speed);
  }

  if (duration > 0)
  {
    DelayMilliseconds(duration);
    
    if (action == idle)
      Idle(vectorOfModules);
    else
      Brake(vectorOfModules);
  }
}
```

```cpp
/*
  Function to drive BACKWARD a robot with ANY number of Module drivers 
  during certain time
  @param std::vector<Module *>: The vector of pointers to Module objects
  @param int speed: The desired speed (-100,0). It set up the correct value if
                    the user enters a positive value.
  @param int duration:  The desired duration in milliseconds with 0 as 
                        default value
  @param STOPMODE action <brake / idle>:  Action on the motor after driving it with 
                                        <idle> as default action. 
*/
template <typename Module>
void Backward (std::vector<Module *> vectorOfModules, int speed, int duration, STOPMODE action)
{
  if (speed > 0)
    speed *= -1;

  for (auto module : vectorOfModules)
  {
    if (module->GetMotorAisUsed())
      module->MotorA.Drive(speed);
    if (module->GetMotorBisUsed())
      module->MotorB.Drive(speed);
  }

  if (duration > 0)
  {
    DelayMilliseconds(duration);
    
    if (action == idle)
      Idle(vectorOfModules);
    else
      Brake(vectorOfModules);
  }
}
```

In order to complement the behavior to control any number of DC Motors with some DC motor drivers,  the `Brake()` and `Idle()` functions are defined too. It brakes or set the Idle mode any number of motors in the vector of pointers to  `typename Module` objects. 

```cpp
/*
  Function to BRAKE a robot with ANY number of motors
  @param std::vector<Module *>: The vector of pointers to Module objects
*/
template <typename Module>
void Brake (std::vector<Module *> vectorOfModules)
{
  for (auto module : vectorOfModules)
    module->Brake();
}
```

```cpp
/*
  Function to IDLE a robot with ANY number of motors
  @param std::vector<Module *>: The vector of pointers to Module objects 
*/
template <typename Module>
void Idle (std::vector<Module *> vectorOfModules)
{
  for (auto module : vectorOfModules)
    module->Idle();
}
```

In the implementation code according to the entered option, the user can decide when calling one, other, or one-third option to move forward, backward, or brake the motors with the `Brake()` function. **Note** how the first parameter used here is a `vector<L298N *>` fits in the template function without any problem. The main advantage is that these functions can be used with any other DC motor driver without the necessity of writing again the same code. 
     
```cpp
// Move the motors
if (motorSpeed > 0)
  Forward(vectorOfL298N, motorSpeed);
else if (motorSpeed < 0)
  Backward(vectorOfL298N, motorSpeed);
else
  Brake(vectorOfL298N);
```

The complete code for this application uses a `while loop` to control both motors increasing and decreasing the speed until the user press the key "Y". This code is shown in the next listing.

### L298N_1.5.cpp
```cpp
/******************************************************************************
L298N_1.5.cpp
@wgaonar
26/03/2022
https://github.com/wgaonar/BeagleCPP

- Drive a 2 or more motors through a vector

Class: L298N
******************************************************************************/
#include <iostream>
#include "../../../Sources/L298N.h"
#include "../../../Sources/DCMOTOR_UTILS.h"
#include "../../../Sources/DCMOTOR_UTILS.cpp"

using namespace std;

// Declaring the pins for MotorA 
GPIO AIN1 (P8_12);
GPIO AIN2 (P8_14);
PWM PWMA (P8_13);

// Declare the MotorA
DCMotor MotorLeft (AIN1, AIN2, PWMA, true);

// Declaring the  pins for MotorB
GPIO BIN1 (P8_17);
GPIO BIN2 (P8_18);
PWM PWMB (P8_19);

// Declare the MotorB
DCMotor MotorRight (BIN1, BIN2, PWMB);

// Declare the L298N Module
L298N myL298NModule (MotorLeft, MotorRight);

// Declare the vector of pointers to L298N objects
vector<L298N *> vectorOfL298N = {&myL298NModule};

int main()
{
  string message = "Main program starting here...";
  cout << RainbowText(message,"Blue", "White", "Bold") << endl;

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

    // Update the motors speed
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

    // Move the motors
    if (motorSpeed > 0)
      Forward(vectorOfL298N, motorSpeed);
    else if (motorSpeed < 0)
      Backward(vectorOfL298N, motorSpeed);
    else
      Brake(vectorOfL298N);
  }

  message = "Main program finishes here...";
  cout << RainbowText(message,"Blue", "White","Bold") << endl;

  return 0;
}
```

Se you in the next post. 
