---
layout: post
title: "Reading an analog value with continuous sampling in the BeagleBone Black"
author: wilmer
categories: [BeagleBone]
tags: [ADC]
image: assets/images/Post27/CoverPost.png
description: "Using the ADC Class for reading the voltage in a potentiometer with continuous sampling with the BeagleBone Black"
featured: false
hidden: false
rating: 4
date:   2021-01-22 08:00:00 -0600
---


In this post, I show a simple application to read an analog value running in the background with its own thread with the ADC C++ class implementation shown previously in <a href="{{ site.baseurl }}{% post_url 2021-01-16-Post24-BeagleBone_ADCClass %}"> this post</a>. Remembering that the BeagleBone has 7 analog inputs and ADC of 12 bits that let to de user to represent an analog signal within a range of 4096 values. It is important to remember that the reference for analog voltage is <font color="red">1.8V</font>. If the user provides a greater voltage, the BeagleBone could be damaged.  

## Circuit and components

The circuit can be seen in Figure 1. It consists of a Potentiometer with a power supplied using the analog ground pin **0V** located at the pin **P9_34** and to analog VDD pin at <font color="purple"><b>1.8V</b></font> located at the pin <font color="purple"><b>P9_32</b></font>. Finally, its output is connected to the **P9_39** pin.

The components are:
- 1 Potentiometer of 200KΩ
- Jumpers male-male to make the connections

<figure style="text-align: center; width:70%; 
              margin-left: auto; 
              margin-right: auto;">
    <img src="../assets/images/Post27/Circuit_bb.jpg"
    alt="Circuit.jpg" width="100%"/>
  <figcaption>
    Figure 1: Circuit to read an analog value from a potentiometer with continuous sampling with the BeagleBone Black.
  </figcaption>
</figure>

## Coding
  
First an `ADC` class object is declared, for example:

```cpp
ADC adcPin(P9_39);
```

An integer variable is declared and initialized to store the digital value. You can note the name contains the suffix `Out` because this variable is **passed by reference** to the corresponding method, after in the main program.

```cpp
int adcValueOut = 0;
```

An integer variable is declared and initialized to set the interval time between each reading.

```cpp
int sampleTime = 100;
```

The continuous sampling of digital values can be obtained through the next class method. It takes two arguments, the first is the variable, **passed by reference**, to store the digital value and the second is the interval between each reading. The sampling is made in the background through an internal thread execution. Besides, this method does not return any value.

```cpp
adcPin.ReadADCContinuousSampling(adcValueOut,sampleTime);
```

This method can be stopped through the next line of code:

```cpp
adcPin.StopReadADCContinuousSampling();
```

In the main program a `while loop` can be used to wait for a user key press to stop the continuous sampling method before finishing the program. 

```cpp
while (userInput != 'y')
{
  message = "Do you want to stop the readings on the pin? Enter 'y' for yes:";
  cout << RainbowText(message, "Blue")  << endl;
  cin >> userInput;
  if (userInput == 'y') 
  {
    adcPin.StopReadADCContinuousSampling();
  }
}
```

The complete code for this application is shown in the next listing together with its corresponding execution video.

### Listing_5.3
```cpp
#include <iostream>
#include "../../Sources/ADC.h"

using namespace std;

int main()
{
  string message = "Main program starting here...";
  cout << RainbowText(message,"Blue", "White", "Bold") << endl;
  
  message = "Setting ADC mode on a pin";
  cout << RainbowText(message, "Blue") << endl;
  ADC adcPin(P9_39);

  message = "Read continuously the ADC value on pin";
  cout << RainbowText(message, "Blue") << endl;

  int adcValueOut = 0;
  int sampleTime = 100;
  adcPin.ReadADCContinuousSampling(adcValueOut,sampleTime);
  
  char userInput = '\0';
  while (userInput != 'y')
  {
    message = "Do you want to stop the readings on the pin? Enter 'y' for yes:";
    cout << RainbowText(message, "Blue")  << endl;
    cin >> userInput;
    if (userInput == 'y') 
    {
      adcPin.StopReadADCContinuousSampling();
    }
  }

  message = "Main program finishes here...";
  cout << RainbowText(message,"Blue", "White","Bold") << endl;

  return 0;
}
```

### Execution of the program:
<figure style="text-align: center; width:100%; 
              margin-left: auto; 
              margin-right: auto;">
  <video width="100%" controls poster="../assets/images/Post27/VideoCover.png">
    <source src="../assets/images/Post27/Video.mp4" type="video/mp4">
  </video>
  <figcaption>
    Video: Execution of the program.
  </figcaption>
</figure>

Se you in the next post. 
