---
layout: post
title: "Implementation of the BUTTON Class for BeagleBone Black"
author: wilmer
categories: [BeagleBone]
tags: [BUTTONS]
image: assets/images/Post16/CoverPost.png
description: "Source code for the C++ BUTTON Class"
featured: false
hidden: false
rating: 4
date:   2021-01-03 08:00:00 -0600
---

In this post, I show a Button C++ class implementation in order to read an attached button to a GPIO pin in the BeagleBone Black. The public methods written in the C++ class `BUTTON` are:  

- `int ReadButton()`
- `int WaitForButton()`
- `int WaitForButton(int)`
- `int WhenButtonWasPressed(callbackType)`
- `void StopWaitForButton()`

The `int ReadButton()` method checks that the GPIO has been set up as `INPUT` and returns the value of the pin that is attached to the button. This is done through a call to the `DigitalRead()` method of the GPIO class.

The `int WaitForButton()` first checks if the GPIO attached to a button has been set up as `INPUT` and then wait until the button has changed from a `LOW` state to a `HIGH` state. This means the button has been pressed.

The `int WaitForButton(int)` is an overload method, its receives <span style="color: red;font-weight: bold;">one</span> of the next types of transition on the pin:

- `RISING`
- `FALLING`
- `BOTH`

The `int WhenButtonWasPressed(callbackType)` method, receives a user customized function when the button has been pressed and creates a thread with a function pointer to that function which is executed in a **parallel** way to the main function in the program. In this user customized function any other methods of the class can be called, for example: `WaitForButton()` without affect the behavior of the main function.

The `void StopWaitForButton()` controls the execution of the `WaitForButton()` method. This is done through the bool member variable  `stopWaitForButtonFlag` that can be set with a the right timing to stop that method.

## Code

### BUTTON.h

```cpp
#ifndef BUTTON_H
#define BUTTON_H

#include <thread>
#include "GPIO.h"

/* 
  Declare a type for a function pointer
  It is the construct for: using function_type = int (*) ()
    function_type:  the function name
    int: return type  
    (*): the dereference operator due to the adddress of the function name
    (): the arguments of the function, in this case void
  Stores the address of a function 
*/
using callbackType = int (*)();

class BUTTON : public GPIO
{
  private:
    int valueOnPin;
    int previousValueOnPin; 
    bool stopWaitForButtonFlag = false;

    std::thread whenButtonWasPressedThread; 

  public:
    // Overload constructor
    BUTTON(int);

    // Interface method to get the GPIO pin state
    virtual int ReadButton();

    // Method for wait for a press on a button with default rising edge
    virtual int WaitForButton();

    // Overloaded Method for wait for a press on a button with an Edge
    virtual int WaitForButton(int);

    // Method to do execute an user function when the button will be pressed
    virtual int WhenButtonWasPressed(callbackType);

    // Method to stop the function executed whe the button was pressed
    virtual void StopWaitForButton(bool);

    // Destructor
    ~BUTTON();
};

#endif // BUTTON_H
```

### BUTTON.cpp

```cpp
#include <iostream>
#include <chrono>
#include <thread>

#include "BUTTON.h"

// Overload constructor
BUTTON::BUTTON(int newId) : GPIO(newId, INPUT) {}

/*
    Public method for reading the input from a button
    @return int:   The button state HIGH / LOW
                  -1 Error in the pin's mode
*/
int BUTTON::ReadButton()
{
    if (this->mode != INPUT)
    {
        perror("'ReadButton' method only works on INPUT mode");
        return -1;
    }
    valueOnPin = this->DigitalRead();
    return valueOnPin;
}

/*
    Public method for waiting a rising edge on the press of a button
    @return int:   1 The button was pressed
                  0 The button was not pressed
                  -1 Error in the pin's mode
*/
int BUTTON::WaitForButton()
{
  if (this->mode != INPUT)
  {
    perror("'waitForButton' method only works on INPUT mode");
    return -1;
  }
  string message;

  WriteFile(path, "edge", "rising");
  while (stopWaitForButtonFlag == false)
  {
    previousValueOnPin = ReadButton(); 
    if (previousValueOnPin == LOW)
      break; 
  }
  while (stopWaitForButtonFlag == false)
  {
    if (ReadButton() == HIGH)
      break; 
  }
  if (previousValueOnPin != valueOnPin)
  {
    message = "A RISING edge was detected!";
    cout << RainbowText(message, "Pink") << endl;
    return 1;
  }
  return 0;
}

/*
  Public overloaded method for waiting a specific type edge on the press of a button
  @param int: The desired edge type RISING / FALLING / BOTH
  @return int:  1 The button was pressed
                0 The button was not pressed
                -1 Error in the pin's mode
*/
int BUTTON::WaitForButton(int edge = RISING)
{
  if (this->mode != INPUT)
  {
    perror("'waitForButton' method only works on INPUT mode");
    return -1;
  }
  string message;
  switch (edge)
  {
  case RISING:
    WriteFile(path, "edge", "rising");
    while (stopWaitForButtonFlag == false)
    {
      previousValueOnPin = ReadButton(); 
      if (previousValueOnPin == LOW)
        break; 
    }
    while (stopWaitForButtonFlag == false)
    {
      if (ReadButton() == HIGH)
        break; 
    }
    if (previousValueOnPin != valueOnPin)
    {
      message = "A RISING edge was detected!";
      cout << RainbowText(message, "Pink") << endl;
      return 1;
    }
    break;
  case FALLING:
    WriteFile(path, "edge", "falling");
    while (stopWaitForButtonFlag == false)
    {
      previousValueOnPin = ReadButton(); 
      if (previousValueOnPin == HIGH)
        break; 
    }
    while (stopWaitForButtonFlag == false)
    {
      if (ReadButton() == LOW)
        break; 
    }
    if (previousValueOnPin != valueOnPin)
    {
      message = "A FALLING edge was detected!";
      cout << RainbowText(message, "Pink") << endl;
      return 1;
    }
    break;
  case BOTH:
    WriteFile(path, "edge", "both");
    previousValueOnPin = ReadButton();
    while (stopWaitForButtonFlag == false)
    {
      if (previousValueOnPin != ReadButton())
      {
        message = "A RISING OR FALLING edge was detected!";
        cout << RainbowText(message, "Yellow") << endl;
        return 1;
      }
    }
    break;
  }
  return 0;
}

/*
    Public callback method to do something when a button will be pressed
    @param callbackType: user function to execute
    @return int: 1 the user function was called
                -1 Error in the pin's mode
*/
int BUTTON::WhenButtonWasPressed(callbackType callbackFunction)
{
  if (this->mode != INPUT)
  {
    perror("'waitForButton' method only works on INPUT mode");
    return -1;
  }

  string message = "'WhenButtonWasPressed' method has been activated!!!";
  cout << RainbowText(message, "Orange") << endl;

  whenButtonWasPressedThread = thread(callbackFunction);

  return 1;
}

/*
    Public method to stop function executed whe the button was pressed
    @param bool: Flag to stop the thread
*/
void BUTTON::StopWaitForButton()
{
  stopWaitForButtonFlag = true;
}

// Destructor
BUTTON::~BUTTON()
{
  if (whenButtonWasPressedThread.joinable())
    whenButtonWasPressedThread.join();
}
```

Se you in the next post. 
