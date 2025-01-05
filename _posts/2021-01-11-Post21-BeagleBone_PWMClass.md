---
layout: post
title: "Implementation of the PWM Class for BeagleBone Black"
author: wilmer
categories: [BeagleBone]
tags: [PWM]
image: assets/images/Post21/CoverPost.png
description: "Coding the PWM Class members and methods for the BeagleBone Black"
featured: false
hidden: false
rating: 5
date:   2021-01-11 08:00:00 -0600
---

In this post, I show a PWM C++ class implementation. Remembering the PWM is a technique that lets us emulate an analog signal in a digital pin. This is done by changing the time that a signal is On in a fixed period of time. The ratio between the time ON and OFF is named duty cycle. The more duty cycle, the more is the "analog" average voltage measured on the digital pin. 

The BeagleBone has six pins than can be dedicated to do PWM.  These are:
<ul>
  <li>P8_13</li>
  <li>P8_19</li>
  <li>P9_14</li>
  <li>P9_16</li>
  <li>P9_21</li>
  <li>P9_22</li>
</ul>


## Coding

The methods coded to the C++ class `PWM` are: 
- Private methods:
  - `WriteFile(string, string, int)`
  - `Enable()`
  - `Disable()`
 
- Public methods:
  - `int GetPeriod()`
  - `int GetDutyCycle()`
  - `int SetPeriod(int)`
  - `int SetDutyCycle(int)`
  - `void Delayms(int)`
  - `int DoUserFunction(callbackType)`
  - `void StopUserFunction()`


The `WriteFile(string, string, int)` method accesses to the file system path that controls the PWM behaviour on the pin at **"/sys/devices/platform/ocp/"**.

The `Enable()` method activates the PWM on the pin setting the enable property to 1 This method is called in the Constructor.

The `Disable()` method deactivates the PWM on the pin setting the enable property to 0. This method is called in the  Destructor.

The `GetPeriod()` method, returns the last set pin's period. 

The `GetDutyCycle()` method, returns the last set pin's duty cycle. 

The `SetPeriod(int)` method, receives an int number and set the period property of the pin in the sys file system. 

The `SetDutyCycle(int)` method, receives an integer number between 0 and 100 and converts it as a percentage of the period before to set the duty_cycle property of the pin in the sys file system. The method checks if the number is less or greater than that range, and set the duty_cycle property as 0 or 100, respectively.

The `DoUserFunction(callbackType)` method, receives a user function thought a function pointer from the main program to do something in background through the execution of a thread.

The `StopUserFunction()` method finishes the execution of the thread called by the user function that should be defined in the previous explained `void DoUserFunction(callbackType)` method.

## Class code

### PWM.h
```cpp
#ifndef PWM_H
#define PWM_H

#include <string>
#include <thread>
#include "RAINBOWCOLORS.h"

/* 
  Declare a type for a function pointer
  It is the construct for: using function_type = int (*) ()
    function_type:  the function name
    int: return type  
    (*): the dereference operator due to the address of the function name
    (): the arguments of the function, in this case void
  Stores the address of a function 
*/
using callbackType = int (*)();

const std::string PWM_PATH ("/sys/devices/platform/ocp/");
const std::string EHRPWM0_PATH = "48300000.epwmss/48300200.pwm/pwm/";
const std::string EHRPWM1_PATH = "48302000.epwmss/48302200.pwm/pwm/";
const std::string EHRPWM2_PATH = "48304000.epwmss/48304200.pwm/pwm/";

/* The pwm class internal number of the pin*/
enum ID {
  P8_13 = 0,
  P8_19 = 1,
  P9_14 = 2,
  P9_16 = 3,
  P9_21 = 4,
  P9_22 = 5, 
};

class PWM
{
  private:
    int id; /* The PWM number of the object */
    std::string idMap[6];
    std::string name;   /* The name of the PWM e.g. pwm-6:1 */
    std::string path;   /* The full path to the PWM e.g. /sys/class/pwm/pwm-6:1 */
    int period;
    int dutyCycle;
    int enable;
    bool stopDutyCycleFlag = false;

    std::thread functionThread;
    
    // Method to write the files
    virtual int WriteFile(std::string, std::string, int);
    // Method to enable the pwm on the pin
    virtual int Enable();
    // Method to enable the pwm on the pin
    virtual int Disable();

  public:

    // Default constructor
    PWM(int id = P8_13, int period = 500000);

    // Interface method to get the period
    virtual int GetPeriod();

    // Interface method to get the duty cycle
    virtual int GetDutyCycle();

    // Interface method to set the period
    virtual int SetPeriod(int);

    // Interface method to set the duty cycle
    virtual int SetDutyCycle(int);
    
    // Delay method in milliseconds
    virtual void Delayms(int);

    // Method to do execute an user function
    virtual int DoUserFunction(callbackType);

    // Method to stop the user function
    virtual void StopUserFunction();

    // Destructor
    virtual ~PWM();

};
#endif // PWM_H
```

### PWM.cpp
```cpp
#include <iostream>
#include <fstream>
#include <string>
#include <chrono> // chrono::milliseconds()
#include <thread> // this_thread::sleep_for()
#include <exception>
#include <mutex>
#include <cstdlib>// system()

#include "PWM.h"

using namespace std;

class CustomException : public exception 
{
  private:
    string reason;
  public:
    CustomException (const char* why) : reason (why) {};
    virtual const char* what() const noexcept 
    {
      return reason.c_str();
    }
};

// Overload constructor with 2 arguments
PWM::PWM(int pwmPin, int newPeriod)
{
  id = pwmPin;
  period = newPeriod;

  idMap[P8_13] = "P8.13";
  idMap[P8_19] = "P8.19";
  idMap[P9_14] = "P9.14";
  idMap[P9_16] = "P9.16";
  idMap[P9_21] = "P9.21";
  idMap[P9_22] = "P9.22";

  switch (pwmPin)
  {
  case P8_13:
    name = EHRPWM2_PATH + "pwmchip6/pwm-6:1/";
    break;
  case P8_19:
    name = EHRPWM2_PATH + "pwmchip6/pwm-6:0/";
    break;
  case P9_14:
    name = EHRPWM1_PATH + "pwmchip3/pwm-3:0/";
    break;
  case P9_16:
    name = EHRPWM1_PATH + "pwmchip3/pwm-3:1/";
    break;
  case P9_21:
    name = EHRPWM0_PATH + "pwmchip1/pwm-1:1/";
    break;
  case P9_22:
    name = EHRPWM0_PATH + "pwmchip1/pwm-1:0/";
    break;
  default:
    break;
  }

  path = PWM_PATH + name;

  cout  << RainbowText("Trying to enable the PWM pin: ","Pink") 
        << RainbowText(idMap[pwmPin], "Pink", "Default", "Bold") << endl;
  string commandString = "config-pin " + idMap[pwmPin] + " pwm";
  const char* command = commandString.c_str();
  system(command);

  // Set the period for the PWM behavior
  SetPeriod(newPeriod); 

  // Enabling the PWM behavior on the pin
  Enable();

  cout  << RainbowText("Setting the PWM pin with a period of ", "Pink")
        << RainbowText(to_string(GetPeriod()), "Pink") 
        << RainbowText("ns was a success!", "Pink") << endl; 
}

/*
    Private method that writes a string value to a file in the path provided
    @param string path: The system path of the file to be modified
    @param string feature: The name of file to be written
    @param int value: The value to be written to in the file
    @return int: 1 written has succeeded
*/
int PWM::WriteFile(string path, string feature, int value) 
{
  string fileName = path + feature;

  ofstream file(fileName, ios_base::out);
  if (!file.is_open()) 
  {
    perror(("Error while opening file: " + fileName).c_str());
    throw CustomException("Error in 'writeFile' method");
  } 
  file << value;
  file.close();
  Delayms(10);
  
  return 1;
}

/*
    Private method to enable the PWM on the pin.
    It is used in the constructor
    @return int: 1 set duty cycle has succeeded / throw an exception if not
*/
int PWM::Enable()
{
    if (WriteFile(path, "enable", 1) == 1)
      return 1;
    else 
    {
      perror("Error enabling the PWM on the pin");
      throw CustomException("Error in Enable method");
    }
}

/*
    Private method to disable the PWM on the pin.
    It is used in the constructor
    @return int: 1 set duty cycle has succeeded / throw an exception if not
*/
int PWM::Disable()
{
    if (WriteFile(path, "enable", 0) == 1)
      return 1;
    else 
    {
      perror("Error disabling the PWM on the pin");
      throw CustomException("Error in DisablePWM method");
    }
}

/*
    Public method to get the period 
    @return int: The pin's period
*/
int PWM::GetPeriod()
{
    return period;
}

/*
    Public method to get the period 
    @return int: The pin's duty cycle
*/
int PWM::GetDutyCycle()
{
    return dutyCycle;
}

/*
    Public method to set the period of the PWM
    The chosen period is in nanoseconds 
    @param int: The desired period
    @return int: 1 set period has succeeded / throw an exception if not
*/
int PWM::SetPeriod(int newPeriod)
{
    period = newPeriod;
    if (WriteFile(path, "period", period) == 1)
      return 1;
    else 
    {
      perror("Error setting the PWM period for the pin");
      throw CustomException("Error in WritePWMPeriod method");
    }
}

/*
    Public method to set the duty cycle of the PWM
    @param int: The desired duty cycle in percentage: 0-100
    @return  int: 1 set duty cycle has succeeded / throw an exception if not
            int: 0 if the user decides to stop this method
*/
int PWM::SetDutyCycle(int newDutyCycle)
{
  while (stopDutyCycleFlag == false)
  {
    if (newDutyCycle >= 0 && newDutyCycle <= 100)
      dutyCycle = static_cast<int>(newDutyCycle / 100.0 * period);
    else if (newDutyCycle < 0)
      dutyCycle = 0;
    else
      dutyCycle = 100;
    
    if (WriteFile(path, "duty_cycle", dutyCycle) == 1)
      return 1;
    else 
    {
      perror("Error setting the PWM duty cycle for the pin");
      throw CustomException("Error in WritePWMDutyCycle method");
    }
  }
  return 0;
}

/*
  Public method to do a delay in milliseconds
  @param int: duration of the delay
*/
void PWM::Delayms(int millisecondsToSleep) 
{
  this_thread::sleep_for(chrono::milliseconds(millisecondsToSleep));
}

/*
  Public callback method to do a user customized function when is called
  @param callbackType: user function pointer to execute 
  @return int: 1 the user function was called      
*/

int PWM::DoUserFunction (callbackType callbackFunction)
{
  string message = "'UserFunction' method has been activated!";
  cout << RainbowText(message, "Orange") << endl;

  functionThread = thread(callbackFunction);

  return 1;
}

/*
    Public method to stop the user function execution
*/
void PWM::StopUserFunction()
{
  stopDutyCycleFlag = true;
}

// Destructor
PWM::~PWM() 
{
  if (functionThread.joinable())
    functionThread.join();

  // Turn Off the duty cycle on the pin
  this->SetDutyCycle(0);

  // Disabling the pwm on the pin
  this->Disable();
  this->Delayms(10);
  cout  << RainbowText("Destroying the GPIO_PIN with path: ","Gray")
        << RainbowText(path, "Gray", "Default", "Bold") << endl;
}
```

Se you in the next post. 
