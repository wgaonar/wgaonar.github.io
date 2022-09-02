---
layout: post
title: "GPIO C++ Class"
author: wilmer
categories: [BeagleBone]
tags: [GPIO]
image: assets/images/Post10/CoverPost.png
description: "The develop of the C++ library to access the GPIO pins on the BeagleBone Black"
featured: false
hidden: false
rating: 4.5
date:   2020-12-22 08:00:00 -0600
---

This post will be the starting point about how to program the BeagleBone in C++.
Here, I show the code of a library to access and control the general purpose 
digital pins (GPIO) pins of the BeagleBone.

So the time before, I searched for a C++ library but, I did not find one. I have read
the book:  <a href="http://exploringbeaglebone.com/">"Exploring BeagleBone"</a> 
by Derek Molloy and I can say that this is an excellent source of information for 
doing great things with the Beagles. It contains a C++ library to control the pins 
but it was so complex for my prior C++ level of knowledge. Then I decided to learn 
more about this programming language and adapt or make my own C++ Library based on 
the work by Professor Molloy. This post is the beginning of others, I hope.

## Coding
<h3>Color text in the terminal</h3>

First of all, I ever liked to see the world in colors, and the programming does not
have to be the exception. For this reason I coded a file with an overloaded function
called: <span style="color:firebrick;font-weight: bold;">RainbowText()</span> 
to colorize text in the terminal. The header and the code file are shown here:


```cpp
#ifndef RAINBOWCOLORS_H
#define RAINBOWCOLORS_H

#include <string>
#include <map>

/*
General format to color the text in the terminal:
-> "\033[{format_attibute};{foreground_color};{background_color}m {TEXT} \033[{reset_format_attribute} m" 
*/

std::string RainbowText  (std::string, std::string);
std::string RainbowText  (std::string, std::string, std::string);
std::string RainbowText  (std::string, std::string, std::string, std::string);

#endif  // RAINBOWCOLORS_H
```

<h3>RAINBOWCOLORS.h</h3> 
```cpp
#ifndef RAINBOWCOLORS_H
#define RAINBOWCOLORS_H

#include <string>
#include <map>

/*
General format to color the text in the terminal:
-> "\033[{format_attibute};{foreground_color};{background_color}m {TEXT} \033[{reset_format_attribute} m" 
*/

std::string RainbowText  (std::string, std::string);
std::string RainbowText  (std::string, std::string, std::string);
std::string RainbowText  (std::string, std::string, std::string, std::string);

#endif  // RAINBOWCOLORS_H
```

<h4>RAINBOWCOLORS.cpp</h4> 
```cpp
#include <string>
#include <map>
#include "RAINBOWCOLORS.h"

std::map <std::string, std::string> mapForegroundColor= 
{
  { "Red", "38;5;160" },
  { "Orange", "38;5;202" },
  { "Yellow", "38;5;11" },
  { "Green", "38;5;76" },
  { "Blue", "38;5;20" },
  { "Indigo", "38;5;18" },
  { "Violet", "38;5;128" },
  { "Pink", "38;5;161" },
  { "Black", "38;5;0" },
  { "Gray", "38;5;246" },
  { "White", "38;5;15" },
  { "Default", "39" }
};

std::map <std::string, std::string> mapBackgroundColor = 
{
  { "Red", "48;5;160" },
  { "Orange", "48;5;202" },
  { "Yellow", "48;5;11" },
  { "Green", "48;5;76" },
  { "Blue", "48;5;20" },
  { "Indigo", "48;5;18" },
  { "Violet", "48;5;128" },
  { "Pink", "48;5;161" },
  { "Black", "48;5;0" },
  { "Gray", "48;5;246" },
  { "White", "48;5;15" },
  { "Default", "49" }
};

std::map <std::string, std::string> mapSetFormat =
{
  { "Default", "0" },
  { "Bold", "1" },
  { "Underlined", "4" },
  { "Blink", "5" },
  { "Reverse", "7" },
  { "Hidden", "8" }
};

std::string RainbowText (   std::string src,
                            std::string foregroundColor = "Default"
                        )
{
  auto pairFound = mapForegroundColor.find(foregroundColor);
  if (pairFound != mapForegroundColor.end())
      foregroundColor = pairFound->first;
  else
      foregroundColor = "Default";

  std::string backgroundColor = "Default";
  std::string format = "Default";

  std::string const escCode = "\033";
  
  std::string formattedText = escCode  + "[" +
                mapSetFormat.at(format) + ";" +
                mapForegroundColor.at(foregroundColor) + ";" +
                mapBackgroundColor.at(backgroundColor) + "m" +
                src + escCode + "[0m";
  return formattedText;
}

std::string RainbowText (   std::string src,
                            std::string foregroundColor = "Default",
                            std::string backgroundColor = "Default"
                        )
{
  auto pairFound = mapForegroundColor.find(foregroundColor);
  if (pairFound != mapForegroundColor.end())
      foregroundColor = pairFound->first;
  else
      foregroundColor = "Default";

  pairFound = mapBackgroundColor.find(backgroundColor);
  if (pairFound != mapBackgroundColor.end())
      backgroundColor = pairFound->first;
  else
      backgroundColor = "Default";
      
  std::string format = "Default";

  std::string const escCode = "\033";
  
  std::string formattedText = escCode  + "[" +
                mapSetFormat.at(format) + ";" +
                mapForegroundColor.at(foregroundColor) + ";" +
                mapBackgroundColor.at(backgroundColor) + "m" +
                src + escCode + "[0m";
  return formattedText;
}

std::string RainbowText (   std::string src,
                            std::string foregroundColor = "Default",
                            std::string backgroundColor = "Default",
                            std::string format = "Default"
                        )
{
  auto pairFound = mapForegroundColor.find(foregroundColor);
  if (pairFound != mapForegroundColor.end())
      foregroundColor = pairFound->first;
  else
      foregroundColor = "Default";

  pairFound = mapBackgroundColor.find(backgroundColor);
  if (pairFound != mapBackgroundColor.end())
      backgroundColor = pairFound->first;
  else
      backgroundColor = "Default";

  pairFound = mapSetFormat.find(format);
  if (pairFound != mapSetFormat.end())
      format = pairFound->first;
  else
      format = "Default";

  std::string const escCode = "\033";
  
  std::string formattedText = escCode  + "[" +
                mapSetFormat.at(format) + ";" +
                mapForegroundColor.at(foregroundColor) + ";" +
                mapBackgroundColor.at(backgroundColor) + "m" +
                src + escCode + "[0m";
  return formattedText;
}  
```

## Write and Read to and from the digital pins 
<p>
  The next step is access to the digital pins to read and write. It can be done by 
  reading and writing files in "/sys/class/gpio" path in the BeagleBone to set the 
  mode and then read and write the digital pin's state.
</p>

<p>
  I wrote some functions to set up the pin's state:
  <ul>
    <li>WriteFile()</li>
    <li>ReadFile()</li>
    <li>ExportGPIO()</li>
    <li>UnexportGPIO()</li>
    <li>SetMode()</li>
  </ul>
  Besides, I wrote other functions read and write the pin's state:
  <ul>
    <li>int DigitalWrite(int)</li>
    <li>int DigitalRead()</li>
    <li>Delayms(int)</li>
  </ul>
</p>

These functions are coded in the GPIO.h and GPIO.cpp files, which are shown here:

<h3>GPIO.h</h3>
```cpp
#ifndef GPIO_H
#define GPIO_H

#include <string>
#include <thread>
#include "RAINBOWCOLORS.h"

using namespace std;

const string GPIO_PATH("/sys/class/gpio/");

/* The gpio number of the pin*/
enum ID {
   P8_08 = 67,
   P8_10 = 68,
   P8_11 = 45,
   P8_12 = 44,
   P8_14 = 26,
   P8_16 = 46,
   P8_17 = 27,
   P8_18 = 65,
   P8_19 = 22,
   P8_20 = 63,
   P8_26 = 61,
};

/* The mode e.g. 0/1 for OUTPUT/INPUT */
enum MODE {
   OUTPUT = 0,
   INPUT = 1,
};

/* The value e.g. 0/1 for LOW/HIGH */
enum VALUE {
   LOW = 0,
   HIGH = 1,
};

/* The value e.g. 0/1 for LOW/HIGH */
enum EDGE {
   RISING,
   FALLING,
   BOTH,
};

class GPIO  
{
  protected:
    int id;        /* The GPIO number of the object */
    int mode;      /* The GPIO mode e.g. 0/1 for OUTPUT/INPUT */
    int value;     /* The GPIO value e.g. 0/1 for LOW/HIGH */
    string name;   /* The name of the GPIO e.g. gpio44 */
    string path;   /* The full path to the GPIO e.g. /sys/class/gpio/gpio44 */
    
    // Helper methods
    virtual int WriteFile(string, string, string);
    virtual string ReadFile(string, string);
    virtual int ExportGPIO();
    virtual int UnexportGPIO();
    virtual int SetMode(int);
   
  public:
    // Overload constructor
    GPIO (int, int);

    // Interface method to set the GPIO pin state
    virtual int DigitalWrite(int);

    // Interface method to get the GPIO pin state
    virtual int DigitalRead();

    // Delay method in milliseconds
    virtual void Delayms(int);

    // Destructor
    virtual ~GPIO ();    
};

#endif // GPIO_H
```

<h3>GPIO.cpp</h3>
```cpp
#include <iostream>
#include <fstream>
#include <string>
#include <chrono> // chrono::milliseconds()
#include <thread> // this_thread::sleep_for()
#include <exception>
#include <mutex>

#include "GPIO.h"
#include "RAINBOWCOLORS.h"

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

// Constructor
GPIO::GPIO (int id, int mode) 
{
    this->id = id;
    this->mode = mode;
    this->name = "gpio" + to_string(id);
    this->path = GPIO_PATH + name + "/";
    
    cout  << RainbowText("Trying to set up the GPIO pin: ","Gray") 
          << RainbowText(to_string(id), "Gray", "Default", "Bold") << endl;
    UnexportGPIO();
    ExportGPIO();
    SetMode(mode);
    cout << RainbowText("Setting the GPIO pin was a success!", "Green") << endl;
}

/*
    Private method that writes a string value to a file in the path provided
    @param String path: The system path of the file to be modified
    @param String feature: The name of file to be written
    @param string value: The value to be written to in the file
    @return int: 0 written has succeeded
*/
int GPIO::WriteFile(string path, string feature, string value) 
{
    string fileName = path + feature;

    ofstream file(fileName, ios_base::out);
    if (!file.is_open()) 
    {
      perror(("Error while opening file: " + fileName).c_str());
      throw CustomException("Error in 'WriteFile' method");
    } 
    file << value;
    file.close();
    Delayms(10); 
    return 0;
}

/*
    Private method that read a file in the path provided
    @param String path: The sysfs path of the file to be modified
    @param String feature: The file to be written to in that path
    @param string value: The value to be written to in the file
    @return int: 0 written has succeeded / -1 written has failed 
*/
string GPIO::ReadFile(string path, string feature) 
{
    string fileName;
    fileName = path + feature;
    ifstream file(fileName, ios_base::in);
    if (!file.is_open()) {
      perror(("Error while opening file: " + fileName).c_str());
      throw CustomException("Error in 'ReadFile' method");
    }
    string value;
    getline(file,value);
    if (file.bad())
      perror(("Error while reading file: " + fileName).c_str());
    file.close();
    return value;
}

/*
    Private method to export the GPIO pin
    @return int: 0 export has succeeded / -1 export has failed 
*/
int GPIO::ExportGPIO()
{
    if (WriteFile(GPIO_PATH, "export", to_string(id)) != 0)
      throw CustomException("Error to export the pin");
    return 0;
}

/*
    Private method to unexport the GPIO pin
    @return int: 0 unexport has succeeded / -1 unexport has failed 
*/
int GPIO::UnexportGPIO() 
{
    if (WriteFile(GPIO_PATH, "unexport", to_string(id)) != 0)
      throw CustomException("Error to unexport the pin");
    return 0;
}

/*
    Private method that set the pin Mode
    @param int: The desired mode 0/1 for OUTPUT/INPUT
    @return int: 0 set Mode has succeeded / -1 set Mode has failed 
*/
int GPIO::SetMode(int mode) 
{
    switch (mode) 
    {
      case OUTPUT:
          if (WriteFile(path, "direction", "out") != 0) 
            throw "Error to set the pin direction as OUTPUT";
          else
            cout << RainbowText("Set the pin direction as DIGITAL OUTPUT", "Orange") << endl;
          break;
      case INPUT:
          if (WriteFile(path, "direction", "in") != 0) 
            throw "Error to set the pin direction as INPUT";
          else
            cout << RainbowText("Set the pin direction as DIGITAL INPUT", "Yellow") << endl;
          break;   
    }
    return 0;
}

/*
    Public method to set/clear the pin value
    @param int: The desired value 1 for HIGH and 0 for low
    @return int: 0 set value has succeeded / -1 set value has failed 
*/
int GPIO::DigitalWrite(int newValue) 
{
    switch (newValue) 
    {
      case HIGH:
          //cout << "Setting the pin value as: " << "HIGH" << endl;
          if (WriteFile(this->path, "value", "1") == 0)
            return 0;
          break;
      case LOW:
          //cout << "Setting the pin value as: " << "LOW" << endl;
          if (WriteFile(this->path, "value", "0") == 0)
            return 0;
          break;
    }   
    return -1;
}

/*
    Public method to get the pin value
    @return GPIO::VALUE: The value of the pin LOW/HIGH
*/
int GPIO::DigitalRead() 
{
    string value = ReadFile(path, "value");
    if (value == "0")
      return LOW;
    else
      return HIGH;
}

/*
    Public method to do a delay in milliseconds
    @param int: duration of the delay
*/
void GPIO::Delayms(int millisecondsToSleep) 
{
    this_thread::sleep_for(chrono::milliseconds(millisecondsToSleep));
}

// Destructor
GPIO::~GPIO() 
{
    if (this->mode == OUTPUT)
      this->DigitalWrite(LOW);
    Delayms(10);
  
    this->UnexportGPIO();
    cout  << RainbowText("Destroying the GPIO_PIN with path: ","Gray")
          << RainbowText(path, "Gray", "Default", "Bold") << endl;
}
```

Se you in the next post.
