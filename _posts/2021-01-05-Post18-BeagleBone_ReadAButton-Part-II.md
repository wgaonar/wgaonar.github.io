---
layout: post
title: "Reading a button in the BeagleBone Black PART II"
author: wilmer
categories: [BeagleBone]
tags: [BUTTONS]
image: assets/images/Post18/CoverPost.png
description: "Second series of examples about how to read Button with the BeagleBone Black"
featured: false
hidden: false
rating: 4
date:   2021-01-06 08:00:00 -0600
---

In this post, I will continue to show you how to read a button with the BeagleBone Black.  In the <a href="{{ site.baseurl }}{% post_url 2021-01-05-Post17-BeagleBone_ReadAButton-Part-I %}"> first entry</a> I showed how to read an input from a button doing *POLLING* in the code, i.e., checking continuously for the state of the button inside the main program. 

In this post, I show a new method named `WaitForButton()` to the C++ class `BUTTON` that stop the main program until a button has been pressed.

## Circuit and components

The circuit can be seen in Figure 1. Please keep in mind that the BeagleBone works at <font color="red">3.3V</font> and not 5V like microcontrollers as Arduino. It is so muy important to avoid damage to the board, especially when you are working with buttons or digital inputs in general. 

The components are:
<ul>
  <li>1 Resistor of 10KΩ as a pull-down resistor</li>
  <li>1 Push button of 12mm</li>
  <li>Jumpers male-male to make the connections</li>
</ul>

<figure style="text-align: center; width:70%; 
              margin-left: auto; 
              margin-right: auto;">
    <img src="../assets/images/Post18/Circuit_bb.jpg"
    alt="Circuit_bb.jpg" width="100%"/>
  <figcaption>
    Figure 1: Circuit to read the input from a button.
  </figcaption>
</figure>

## Coding

I added the corresponding methods to read a button into the class **BUTTON** who is derived from GPIO class. The methods are:
- `WaitForButton()`
- `WaitForButton(int)`

The `WaitForButton()` method, first checks if the GPIO is attached to a button has been set up as INPUT and then wait until the button has changed from a `LOW` state to a `HIGH` state. This means the button has been pressed.

```cpp
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
```
The second method `WaitForButton(int)` is an overload of the first. The difference consists that this method receives <font color="red">one</font> 
of the next types of transition on the pin:
- `RISING`
- `FALLING`
- `BOTH`

```cpp
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
```
The button can be declared as a `BUTTON` object
specifying the pin attached to it:

```cpp
BUTTON redButtonPin(P8_08);
```

In the main program, only a single line makes to wait for the press of a button, 
before continue with the next line of code:

```cpp
redButtonPin.WaitForButton();
```

### Listing_3.2
```cpp
#include <iostream>
#include "../../Sources/GPIO.h"
#include "../../Sources/BUTTON.h"

using namespace std;

int main()
{
  string message = "Main program starting here...";
  cout << RainbowText(message,"Blue", "White", "Bold") << endl;
  
  BUTTON redButtonPin(P8_08);
  message = "The program is waiting for a press on a Button\nPlease, press the red button!";
  cout << endl << RainbowText(message, "Red") << endl;

  redButtonPin.WaitForButton();
  message = "The red button was pressed!!!";
  cout << RainbowText(message, "Red") << endl;
  
  message = "Main program finishes here...";
  cout << RainbowText(message,"Blue", "White","Bold") << endl;
  return 0;
}
```

### Execution of the program:
<figure style="text-align: center; width:100%; 
              margin-left: auto; 
              margin-right: auto;">
  <video width="100%" controls poster="../assets/images/Post18/VideoCover.png">
    <source src="../assets/images/Post18/Video.mp4" type="video/mp4">
  </video>
  <figcaption>
    Video: Execution of the program.
  </figcaption>
</figure>

Se you in the next post.
