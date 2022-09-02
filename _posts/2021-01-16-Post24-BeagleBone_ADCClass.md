---
layout: post
title: "Implementation of the ADC Class for BeagleBone Black"
author: wilmer
categories: [BeagleBone]
tags: [ADC]
image: assets/images/Post24/CoverPost.png
description: "Coding the ADC Class members and methods for the BeagleBone Black"
featured: false
hidden: false
rating: 5
date:   2021-01-16 08:00:00 -0600
---

In this post, I show an analog to digital converter (ADC) C++ class implementation. Remembering that the BeagleBone has 7 analog inputs and ADC of 12 bits that lets to de user to represent an analog signal within a range of 4096 values. It is important to remember that the reference for analog voltage is <font color="red">1.8V</font>. If the user provides a greater voltage, the BeagleBone could be damaged. 

The BeagleBone has seven pins than can be dedicated to the ADC.  These are:
- P9_33
- P9_35
- P9_36
- P9_37
- P9_38
- P9_39
- P9_40


## Coding

The methods coded to the C++ class `ACD are:  
- Private methods:  
  - `int ReadFile(std::string)`
  - `MakeReadADCContinuousSampling(int &, int)`
  - `MakeReadVoltageContinuousSampling(float &, int)`
  
- Public methods: 
  - `int ReadADC()`
  - `float ReadVoltage()`
  - `void ReadADCContinuousSampling(int &, int)`
  - `void ReadVoltageContinuousSampling(float &, int)`
  - `void DoUserFunction(callbackType)`
  - `void StopReadADCContinuousSampling()`
  - `void StopReadVoltageContinuousSampling()`
  - `void StopUserFunction()`
  - `void Delayms(int);`

The `int ReadFile(std::string)` method accesses to the file system path that has the values of the ADC module the PWM behavior on the pins at **"/sys/bus/iio/devices/iio:device0/"** and it returns the raw value ADC on pin, i.e. a `int` value between <font color="red">0 - 4095</font>.

The `MakeReadVoltageContinuousSampling(float &, int)` method, when is activated, runs on a thread to read the analog voltage on the pin and pass this value through a reference variable to the main program. 

The `MakeReadADCContinuousSampling(int &, int)` method, when is activated, runs on a thread to read the ADC value on the pin and pass this value through a reference variable to the main program. 

The `int ReadADC()` method reads the corresponding system file and returns the read ADC value on the pin.

The `float ReadVoltage()` method reads the corresponding system file and returns the ADC value on the pin converted to voltage ina a range between 0 - 1.8V.

The `void ReadADCContinuousSampling(int &, int)` method, can be invoked from the main program to do a continuous sampling on the ADC value. It calls the private <span class="coding">MakeReadADCContinuousSampling(int &, int)` method to do this.

The `void ReadVoltageContinuousSampling(int &, int)` method, can be invoked from the main program to do a continuous sampling on the pin's voltage. It calls the private <span class="coding">MakeReadVoltageContinuousSampling(int &, int)` method to do this.

The `DoUserFunction(callbackType)` method, receives a user function thought a function pointer from the main program to do something in background through the execution of a thread.

The `StopReadADCContinuousSampling()` method finishes the execution of the thread called by the `void ReadADCContinuousSampling(int &, int)` method.

The `StopReadVoltageContinuousSampling()` method finishes the execution of the thread called by the `void ReadVoltageContinuousSampling(int &, int)` method.

The `StopUserFunction()` method finishes the execution of the thread called by the user defined function `void DoUserFunction(callbackType)` method.

## Class code

### ADC.h
```cpp
#ifndef ADC_H
#define ADC_H

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

const std::string ADC_PATH = "/sys/bus/iio/devices/iio:device0/";

/* The pwm class internal number of the pin*/
enum ID {
  P9_39 = 0,
  P9_40 = 1, 
  P9_37 = 2,
  P9_38 = 3,
  P9_33 = 4,
  P9_36 = 5,
  P9_35 = 6,
};

class ADC
{
  private:
    int id; /* The ADC number of the object */
    std::string idMap[7];
    std::string path; /* The full path to the ADC e.g. /sys/bus/iio/devices/iio:device0/in_voltage0_raw */
    std::string name; /* The name of the ADC e.g. in_voltage0_raw */
    std::string message; /* The variable to output messages on the terminal*/

    bool stopReadADCFlag = false;
    bool stopReadVoltageFlag = false;

    std::thread ReadADCThread;
    std::thread ReadVoltageThread;
    std::thread functionThread;

    void MakeReadADCContinuousSampling(int &, int);
    void MakeReadVoltageContinuousSampling(float &, int);
    
    // Method to read files
    virtual int ReadFile(std::string);

  public:

    // Default constructor
    ADC(int);

    // Interface method to get one ADC value
    virtual int ReadADC();

    // Interface method to get the voltage on the pin
    virtual float ReadVoltage();

    // Interface method for reading the ADC value continuously on the pin
    virtual void ReadADCContinuousSampling(int &, int);

    // Interface method for reading the Voltage continuously on the pin
    virtual void ReadVoltageContinuousSampling(float &, int);
    
    // Method to do execute an user function
    virtual int DoUserFunction(callbackType);

    // Method to stop the ADC continuous sampling 
    virtual void StopReadADCContinuousSampling();

    // Method to stop the voltage continuous sampling
    virtual void StopReadVoltageContinuousSampling();
    
    // Method to stop the user function
    virtual void StopUserFunction();

    // Delay method in milliseconds
    virtual void Delayms(int);

    // Destructor
    virtual ~ADC();
};
#endif // ADC_H
```

### ADC.cpp
```cpp
#include <iostream>
#include <fstream>
#include <string>
#include <chrono> // chrono::milliseconds()
#include <thread> // this_thread::sleep_for()
#include <exception>
#include <mutex>

#include "ADC.h"

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
ADC::ADC(int adcPin)
{
  id = adcPin;

  idMap[P9_39] = "P9_39";
  idMap[P9_40] = "P9_40";
  idMap[P9_37] = "P9_37";
  idMap[P9_38] = "P9_38";
  idMap[P9_33] = "P9_33";
  idMap[P9_36] = "P9_36";
  idMap[P9_35] = "P9_35";

  name = "in_voltage" + to_string(id) + "_raw";
  path = ADC_PATH + name;

  cout  << RainbowText("Trying to enable the ADC on pin: ","Violet") 
        << RainbowText(idMap[id], "Violet", "Default", "Bold") << endl;

  int ADCValue = ReadADC();
  if (ADCValue >= 0 && ADCValue <= 4095)
    cout  << RainbowText("Setting the ADC pin was a success!", "Violet") << endl; 
}

/*
  Private method that read a file in the path provided
  @param String path: The sysfs path of the file to be read
  @return int: the read value 
*/
int ADC::ReadFile(string newPath) 
{
  string fileName;
  fileName = path;
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

  return stoi(value);
}

/*
    Public method to get the ADC value on pin 
    @return int: The pin's value between 0 - 4095
*/
int ADC::ReadADC()
{
  return ReadFile(path);
}

/*
    Public method to get the voltage on the pin 
    @return float: The pin's value between 0 - 1.8
*/
float ADC::ReadVoltage()
{
  return ( ReadFile(path) * 1.8 / 4095 );
}

/*
  Public method to get continuously the ADC value on pin 
  @param int: Reference output for the ADC value between 0 - 4095
  @param int: The time interval between each sample
*/
void ADC::ReadADCContinuousSampling(int &adcValueOut, int sampleTime)
{
  message = "Read ADC input has been activated";
  cout << RainbowText(message, "Violet", "Default", "Bold") << endl;

  ReadADCThread = std::thread(&ADC::MakeReadADCContinuousSampling, this, std::ref(adcValueOut),sampleTime);
}

/*
  Private method that contains the routine to make the ADC read 
  @param int: A reference variable to store The pin's value between 0 - 4095
*/
void ADC::MakeReadADCContinuousSampling(int &adcValueOut, int sampleTime)
{
  while (stopReadADCFlag == false)
  {
    adcValueOut = this->ReadADC();
    message = "ADC value on pin " + idMap[id] + ": " + to_string(adcValueOut);
    cout << RainbowText(message, "Violet") << endl;
    // cout << "ADC value on pin " << idMap[id] << ": " << adcValueOut << endl;
    Delayms(sampleTime);
  }
}

/*
  Public method to stop reading the ADC 
*/
void ADC::StopReadADCContinuousSampling()
{
  stopReadADCFlag = true;
}

/*
  Public method to get continuously the voltage value on pin 
  @param float: Reference output for the ADC value between 0 - 1.8
  @param int: The time interval between each sample
*/
void ADC::ReadVoltageContinuousSampling(float &voltageOut, int sampleTime)
{
  message = "Read voltage has been activated";
  cout << RainbowText(message, "Violet", "Default", "Bold") << endl;

  ReadVoltageThread = std::thread(&ADC::MakeReadVoltageContinuousSampling, this, std::ref(voltageOut),sampleTime);
}

/*
  Private method that contains the routine to make the ADC read 
  @param int: A reference variable to store The pin's value between 0 - 4095
*/
void ADC::MakeReadVoltageContinuousSampling(float &voltageOut, int sampleTime)
{
  while (stopReadVoltageFlag == false)
  {
    voltageOut = this->ReadVoltage();
    message = "Voltage on pin " + idMap[id] + ": " + to_string(voltageOut);
    cout << RainbowText(message, "Violet") << endl;
    //cout << "Voltage on pin " << idMap[id] << ": " <<  << endl;
    Delayms(sampleTime);
  }
}

/*
  Public method to stop reading the voltage 
*/
void ADC::StopReadVoltageContinuousSampling()
{
  stopReadVoltageFlag = true;
}

/*
  Public callback method to do a user customized function when is called
  @param callbackType: user function pointer to execute 
  @return int: 1 the user function was called      
*/

int ADC::DoUserFunction (callbackType callbackFunction)
{
  message = "'UserFunction' method has been activated!";
  cout << RainbowText(message, "Violet", "Default", "Bold") << endl;

  functionThread = thread(callbackFunction);

  return 1;
}

/*
    Public method to stop the user function execution
*/
void ADC::StopUserFunction()
{
  stopReadADCFlag = true;
}

/*
  Public method to do a delay in milliseconds
  @param int: duration of the delay
*/
void ADC::Delayms(int millisecondsToSleep) 
{
  this_thread::sleep_for(chrono::milliseconds(millisecondsToSleep));
}

// Destructor
ADC::~ADC() 
{
  if (ReadADCThread.joinable())
    ReadADCThread.join();
  if (ReadVoltageThread.joinable())
    ReadVoltageThread.join();
  if (functionThread.joinable())
    functionThread.join();

  // Waiting for the last reading on the pin
  this->Delayms(10);
  cout  << RainbowText("Stoping the reading on the ADC PIN: ","Violet")
        << RainbowText(idMap[id], "Violet", "Default", "Bold") << endl;
}
```

Se you in the next post. 
