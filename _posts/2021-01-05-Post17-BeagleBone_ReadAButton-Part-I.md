---
layout: post
title: "Reading a button in the BeagleBone Black PART I"
author: wilmer
categories: [BeagleBone]
tags: [BUTTONS]
image: assets/images/Post17/CoverPost.png
description: "First series of examples about how to read Button with the BeagleBone Black"
featured: false
hidden: false
rating: 4
date:   2021-01-05 08:00:00 -0600
---

In this post, I will show you how to read a button with the BeagleBone Black. I have written a <a href="{{ site.baseurl }}{% post_url 2020-12-24-Post12-BeagleBone_ButtonLed %}"> previous post</a> that shows the same behavior. The difference with this post is that here, I have written a C++ class that contains specific methods to work with buttons. This class is part of the C++ library to access and control the general purpose digital pins (GPIO) pins of the BeagleBone that I started to work in and described in these posts <a href="{{ site.baseurl }}{% post_url 2020-12-22-Post10-BeagleBone_GPIOClass %}"> BeagleBone GPIO C++ class </a> and <a href="{{ site.baseurl }}{% post_url 2021-01-03-Post16-BeagleBone_BUTTONClass%}"> BeagleBone Button C++ Class</a>

# Circuit and components

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
    <img src="../assets/images/Post17/Circuit_bb.jpg"
    alt="Circuit_bb.jpg" width="100%"/>
  <figcaption>
    Figure 1: Circuit to read the input from a button.
  </figcaption>
</figure>

# Coding

The corresponding method to read a button into the class <b>BUTTON</b> 
who is derived from GPIO class mentioned earlier in the introduction. The method is:

- `ReadButton()`

The `ReadButton()` method checks that the GPIO has been set up as INPUT
and returns the value of the pin that is attached to the button. This is done through a call to the 
`DigitalRead()` method of the GPIO class.

```cpp
int BUTTON::ReadButton()
{
  if (this->mode != INPUT)
  {
    perror("'ReadButton' method only works on INPUT mode");
    return -1;
  }
  return this->DigitalRead();
}
```
The button can be declared as a `BUTTON` object
specifying the pin attached to it:

```cpp
BUTTON redButtonPin(P8_08);
```

In the main program a while loop can wait for the press of the button:

```cpp
while (redButtonPin.ReadButton() == 0)
{
  if (redButtonPin.ReadButton() == 1)
  {
    message = "The red button was pressed!!!";
    cout << RainbowText(message, "Red") << endl;
  }
}
```

## Complete code

The code is shown in the next listings:

### BUTTON.h

```cpp
#ifndef BUTTON_H
#define BUTTON_H

#include "GPIO.h"

class BUTTON : public GPIO
{
  public:
    // Overload constructor
    BUTTON(int);

    // Interface method to get the GPIO pin state
    virtual int ReadButton();

    ~BUTTON();
};

#endif // BUTTON_H
```

### BUTTON.cpp
```cpp
#include <iostream>

#include "BUTTON.h"
#include "RAINBOWCOLORS.h"

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
  return this->DigitalRead();
}

// Destructor
BUTTON::~BUTTON()
{}
```

### Listing_3.1
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
  message = "Please, press the red button!";
  cout << endl << RainbowText(message, "Red") << endl;

  while (redButtonPin.ReadButton() == 0)
  {
    if (redButtonPin.ReadButton() == 1)
    {
      message = "The red button was pressed!!!";
      cout << RainbowText(message, "Red") << endl;
    }
  }
  
  message = "Main program finishes here...";
  cout << RainbowText(message,"Blue", "White","Bold") << endl << endl;
  return 0;
}
```

### Execution of the program:
<figure style="text-align: center; width:100%; 
              margin-left: auto; 
              margin-right: auto;">
  <video width="100%" controls poster="../assets/images/Post17/VideoCover.png">
    <source src="../assets/images/Post17/Video.mp4" type="video/mp4">
  </video>
  <figcaption>
    Video: Execution of the program.
  </figcaption>
</figure>

Se you in the next post.
