---
layout: post
title: "Updates to the C++ library for the BeagleBone Black"
author: wilmer
categories: [BeagleBone]
tags: [GPIO, LED, PWM, ADC]
image: assets/images/Post29/CoverPost.png
description: "Updates to the C++ library for the BeagleBone Black"
featured: false
hidden: false
rating: 4
date:   2021-03-04 06:00:00 -0600
---

In this post, I show some updates to the library explained previously in:
- <a href="{{ site.baseurl }}{% post_url 2020-12-22-Post10-BeagleBone_GPIOClass %}"> GPIO Class</a>
- <a href="{{ site.baseurl }}{% post_url 2020-12-26-Post13-BeagleBone_LEDClass %}"> LED Class</a>
- <a href="{{ site.baseurl }}{% post_url 2021-01-03-Post16-BeagleBone_BUTTONClass %}"> Button Class</a>
- <a href="{{ site.baseurl }}{% post_url 2021-01-11-Post21-BeagleBone_PWMClass %}"> PWM Class</a>
- <a href="{{ site.baseurl }}{% post_url 2021-01-16-Post24-BeagleBone_ADCClass %}"> ADC Class</a>



## Main Updates

- The first is the addition of a specific class to manage the access methods to de system files in the BeagleBone. It was called SYSFILEACCESS and contains writing and reading methods.
- A new header and application file were added. It was called RAINBOWCOLORS and contains an overloaded function to let the terminal output be colored.
- A new header file called BLACKPIN_ID was added. It contains an `enum` structure to identify the header GPIO pins location with the kernel GPIO location in the AM335x processor for the BeagleBone Black. In a possible future, an extension to the Green and Blue BeagleBones could be added with new files, and its the corresponding mapping between header and kernel pin's name.
- The code in the `Destructor` in each class was reduced to avoid unsetting the pin when it inheritance is used through the applications
  

The complete code for these files are shown in the next listings.

### SYSFILEACCESS.h
```cpp
#ifndef SYSFILEACCESS_H
#define SYSFILEACCESS_H

#include <string>

class SYSFILEACCESS  
{
  public:
    // Method to write a system file
    virtual int WriteFile(std::string, std::string, std::string);

    // Overload Method to write a system file
    virtual int WriteFile(std::string, std::string, int);

    // Method to read a system file
    virtual std::string ReadFile(std::string, std::string);

    // Overload method to read a system file
    virtual int ReadFile(std::string);
};

#endif // SYSFILEACCESS_H
```

### SYSFILEACCESS.cpp
```cpp
#include <iostream>
  #include <fstream>
  #include <string>
  #include <chrono> // chrono::milliseconds()
  #include <thread> // this_thread::sleep_for()
  #include <exception>
  
  #include "SYSFILEACCESS.h"
  
  class SYSFILEACCESS_Exception : public std::exception 
  {
    private:
      std::string reason;
    public:
      SYSFILEACCESS_Exception (const char* why) : reason (why) {};
      virtual const char* what()
      {
        return reason.c_str();
      }
  };
  
  /*
    Public method that writes a string value to a file in the path provided
    @param String path: The file system path to be modified
    @param String feature: The name of file to be written
    @param string value: The value to be written to in the file
    @return int: 1 written has succeeded
  */
  int SYSFILEACCESS::WriteFile(std::string path, std::string feature, std::string value) 
  {
    std::string fileName = path + feature;
    std::ofstream file(fileName, std::ios_base::out);
    if (file.is_open())
    {
      file << value;
      file.close();
      std::this_thread::sleep_for(std::chrono::milliseconds(10));
      return 1; 
    } 
    else 
    {
      perror(("Error while opening file: " + fileName).c_str());
      return -1;
    }
  }
  
  /*
    Public method that writes a string value to a file in the path provided
    @param string path: The file system path to be modified
    @param string feature: The name of file to be written
    @param int value: The value to be written to in the file
    @return int: 1 written has succeeded
  */
  int SYSFILEACCESS::WriteFile(std::string path, std::string feature, int value) 
  {
    std::string fileName = path + feature;
    std::ofstream file(fileName, std::ios_base::out);
    if (file.is_open())
    {
      file << value;
      file.close();
      std::this_thread::sleep_for(std::chrono::milliseconds(10));
      return 1;
    } 
    else
    {
      perror(("Error while opening file: " + fileName).c_str());
      return -1;
    } 
  }
  
  /*
    Public method that read a file in the path provided
    @param String path: The sysfs path of the file to be read
    @param String feature: The file to be read to in that path
    @return string: The read value / "-1" if there was an error
  */
  std::string SYSFILEACCESS::ReadFile(std::string path, std::string feature) 
  {
    std::string fileName;
    fileName = path + feature;
    std::ifstream file(fileName, std::ios_base::in);
    if (!file.is_open()) 
    {
      perror(("Error while opening file: " + fileName).c_str());
      return "-1";
    }
    std::string value;
    getline(file,value);
    //if (file.bad())
    //  perror(("Error while reading file: " + fileName).c_str());
    file.close();
    return value;
  }
  
  /*
    Public method that read a file in the path provided
    @param String path: The sysfs path of the file to be read
    @return int: The read value / "-1" if there was an error
  */
  int SYSFILEACCESS::ReadFile(std::string path) 
  {
    std::string fileName;
    fileName = path;
    std::ifstream file(fileName, std::ios_base::in);
  
    if (file.is_open())
    {
      std::string value;
      getline(file,value);
      file.close();
      std::this_thread::sleep_for(std::chrono::milliseconds(10));
      return std::stoi(value);
    } 
    else
    {
      perror(("Error while opening file: " + fileName).c_str());
      return -1;
    }
  }
```

### RAINBOWCOLORS.h
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

### RAINBOWCOLORS.cpp
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
    { "Olive Green", "38;5;64" },
    { "Light Green", "38;5;107" },
    { "Light Blue", "38;5;123" },
    { "Light Red", "38;5;196" },
    { "Sky Blue", "38;5;75" },
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
    { "Olive Green", "48;5;64" },
    { "Light Green", "48;5;107" },
    { "Light Blue", "48;5;123" },
    { "Light Red", "48;5;196" },
    { "Sky Blue", "48;5;75" },
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

### BLACKPIN_ID.h
```cpp
#ifndef BLACKPIN_ID_H
#define BLACKPIN_ID_H

/* The gpio number of the pin*/
enum GPIO_ID {
  P8_08 = 67,
  P8_10 = 68,
  P8_11 = 45,
  P8_12 = 44,
  P8_14 = 26,
  P8_16 = 46,
  P8_17 = 27,
  P8_18 = 65,
  P8_20 = 63,
  P8_26 = 61,
};

#endif // BLACKPIN_ID_H
```

### GPIO.h
```cpp
#ifndef GPIO_H
#define GPIO_H

#include <string>
#include <map>

#include "RAINBOWCOLORS.h"
#include "BLACKPIN_ID.h"
#include "SYSFILEACCESS.h"

const std::string GPIO_PATH("/sys/class/gpio/");

/* The numeric mode for MODE: e.g. 0/1 for OUTPUT/INPUT */
enum MODE {
  OUTPUT = 0,
  INPUT = 1,
};

/* The numeric value for VALUE: e.g. 0/1 for LOW/HIGH */
enum VALUE {
  LOW = 0,
  HIGH = 1,
};

/* The numeric value for EDGE: e.g. 0/1/2 for RISING/FALLING/BOTH */
enum EDGE {
  RISING = 0,
  FALLING = 1,
  BOTH = 2,
};

class GPIO : public SYSFILEACCESS
{
  protected:
    GPIO_ID id;         /* Enum for the Kernel GPIO number of the object */
    MODE mode;          /* The GPIO mode e.g. 0/1 for OUTPUT/INPUT */
    int value;          /* The GPIO value e.g. 0/1 for LOW/HIGH */
    std::string name;   /* The name of the GPIO e.g. gpio44 */
    std::string path;   /* The full path to the GPIO e.g. /sys/class/gpio/gpio44 */
    
    /* Map to store the BeagleBone Black pin`s kernel number with its header name */
    std::map <int, std::string> blackPinIdMap; 

  public:
    // Default constructor
    GPIO ();

    // Overload constructor with the pin`s name
    GPIO (GPIO_ID);

    // Overload constructor with the pin id and mode
    GPIO (GPIO_ID, MODE);

    // Initialize the GPIO pin with the data provided by the constructor
    void InitGPIOPin();

    // Initialize the GPIO pin id map kernel's number -> header's name
    void InitPinIdMap(); 

    // Accessor method to get the kernel pin's number
    int GetPinKernelId();

    // Accessor method to get the header pin's name
    std::string GetPinHeaderId();

    // Mutator method to set the pin's mode
    int SetMode(MODE);

    // Method to export the GPIO pin
    virtual int ExportGPIO(int);

    // Method to unexport the GPIO pin
    virtual int UnexportGPIO(int);

    // Interface method to set the GPIO pin state
    virtual int DigitalWrite(int);

    // Overload Interface method to set the GPIO pin state adn printing the value
    virtual int DigitalWrite(int, bool);

    // Interface method to get the GPIO pin state
    virtual int DigitalRead();

    // Delay method in microseconds
    virtual void Delayus(int);

    // Delay method in milliseconds
    virtual void Delayms(int);

    // Destructor
    virtual ~GPIO ();    
};

#endif // GPIO_H
```


### GPIO.cpp
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

  message = "Read continuously the voltage on pin";
  cout << RainbowText(message, "Blue") << endl;

  float adcVoltageOut = 0.0;
  int sampleTime = 100;
  adcPin.ReadVoltageContinuosSampling(adcVoltageOut,sampleTime);
  
  char userInput = '\0';
  while (userInput != 'y')
  {
    message = "Do you want to stop the readings on the pin? Enter 'y' for yes:";
    cout << RainbowText(message, "Blue") << endl;
    cin >> userInput;
    if (userInput == 'y') 
    {
      adcPin.StopReadVoltageContinuosSampling();
    }
  }

  message = "Main program finishes here...";
  cout << RainbowText(message,"Blue", "White","Bold") << endl;

  return 0;
}
```

### LED.h
```cpp
#ifndef LED_H
#define LED_H

#include <thread>
#include <vector>
#include "GPIO.h"

class LED: public GPIO 
{
  private:
    bool stopBlinkFlag = false;
    bool stopFlashFlag = false;
    bool stopHeartBeatFlag = false;
    
    std::thread blinkThread;
    std::thread flashThread;
    std::thread heartBeatThread;
    
    void MakeBlink(int); 
    void MakeFlash(int, int);
    void MakeHeartBeat(int, int);

  public:
    // Overload constructor
    LED (GPIO_ID);

    // Method to turn on the Led
    void TurnOn();

    // Method to turn off the Led
    void TurnOff();

    // Method to toggle Led
    void Toggle();

    // Method for doing a blinking pattern
    void Blink(int);

    // Method for doing a flashing pattern
    void Flash(int, int);

    // Method for doing a digital heart beat pattern
    void HeartBeat(int, int);

    // Method for stopping a blinking
    void StopBlink();

    // Method for stopping a flashing
    void StopFlash();
    
    // Method for stopping a digital heart beat
    void StopHeartBeat();
    
    // Destructor
    ~LED();
};

#endif // LED_H
```

### LED.cpp
```cpp
#include <iostream>
  #include <chrono>
  #include <thread>
  
  #include "LED.h"
  
  // Overload constructor
  LED::LED(GPIO_ID newId) : GPIO(newId, OUTPUT) 
  {
    std::cout  << RainbowText("Led object was created on pin: ", "Light Blue") 
          << RainbowText(this->GetPinHeaderId(), "Light Blue", "Default", "Bold") 
          << std::endl << std::endl;
  }
  
  /*
    Public method to turn on the Led attached to the pin 
  */
  void LED::TurnOn()
  {
    this->DigitalWrite(HIGH);
  }
  
  /*
    Public method to turn on the Led attached to the pin 
  */
  void LED::TurnOff()
  {
    this->DigitalWrite(LOW);
  }
  
  /*
    Public method to toggle the Led attached to the pin 
  */
  void LED::Toggle()
  {
    if (this->DigitalRead() == LOW)
      this->DigitalWrite(HIGH);
    else
      this->DigitalWrite(LOW);
  }
  
  /*
    Public method to make a blink on the pin 
    @param int: The desired duration in milliseconds
  */
  void LED::Blink(int duration)
  {
    std::string message
    {
      "Blinking has been activated with duration of: "
      + std::to_string(duration) + "ms on pin: " + std::to_string(id)
    };
    std::cout << RainbowText(message, "Light Blue", "Default", "Bold") 
              << std::endl << std::endl; 
    blinkThread = std::thread(&LED::MakeBlink, this, duration);
  }
  
  /*
    Private method that contains the routine to blink 
    @param int: The desired duration in milliseconds
  */
  void LED::MakeBlink(int duration)
  {
    while (this->stopBlinkFlag == false)
    {
      this->DigitalWrite(HIGH);
      this->Delayms(duration);
      this->DigitalWrite(LOW);
      this->Delayms(duration);
    }
  }
  
  /*
    Public method to stop the blinking on the pin 
  */
  void LED::StopBlink ()
  {
    this->stopBlinkFlag = true;
  }
  
  /*
    Public method to make a flash on the pin 
    @param int: The desired time ON in milliseconds
    @param int: The desired time OFF in milliseconds
  */
  void LED::Flash(int timeOn, int timeOff)
  {
    std::string message 
    {
      "Flashing has been activated time on: "
      + std::to_string(timeOn) + "ms and time off: " 
      + std::to_string(timeOff) + "ms on pin: " + std::to_string(id)
    };
    std::cout << RainbowText(message, "Light Blue", "Default", "Bold") << std::endl;
    flashThread = std::thread(&LED::MakeFlash, this, timeOn, timeOff);
  }
  
  /*
    Private method that contains the routine to flash
    @param int: The desired time ON in milliseconds
    @param int: The desired time OFF in milliseconds
  */
  void LED::MakeFlash(int timeOn, int timeOff)
  {
    while (this->stopFlashFlag == false)
    {
      this->DigitalWrite(HIGH);
      this->Delayms(timeOn);
      this->DigitalWrite(LOW);
      this->Delayms(timeOff);
    }
  }
  
  /*
    Public method to stop the flash on the pin 
  */
  void LED::StopFlash ()
  {
    this->stopFlashFlag = true;
  }
  
  /*
    Public method to make a digital heart beat on the pin 
    @param int: The desired time On of the pulse in milliseconds
    @param int: The desired ratio between the pulses and the pause in the pattern
  */
  void LED::HeartBeat(int timeOn, int ratio)
  {
    std::string message 
    {
      "Heart beat has been activated with a time ON of: "
      + std::to_string(timeOn) + "ms on pin: " + std::to_string(id)
      + " with a ratio pulse/pause of: " + std::to_string(ratio)
    };
    std::cout << RainbowText(message, "Light Blue", "Default", "Bold") << std::endl;
    heartBeatThread = std::thread(&LED::MakeHeartBeat, this, timeOn, ratio);
  }
  
  /*
     Private method that contains the routine to do the digital heart beat 
     @param int: The desired time On of the pulse in milliseconds
     @param int: The desired ratio between the pulses and the pause in the pattern
  */
  void LED::MakeHeartBeat(int timeOn, int ratio)
  {
    while (this->stopHeartBeatFlag == false)
    {
      for (int i = 0; i < 2; i++)
      {
        this->DigitalWrite(HIGH);
        this->Delayms(timeOn);
        this->DigitalWrite(LOW);
        this->Delayms(timeOn);
      }
      this->Delayms(timeOn*ratio);
    }
  }
  
  /*
    Public method to stop the digital heart beat on the pin 
  */
  void LED::StopHeartBeat ()
  {
    this->stopHeartBeatFlag = true;
  }
  
  // Destructor
  LED::~LED()
  {
    if(this->DigitalRead() == HIGH)
      this->DigitalWrite(LOW);
  
    if (blinkThread.joinable())
      blinkThread.join();
    if (flashThread.joinable())
      flashThread.join();
    if (heartBeatThread.joinable())
      heartBeatThread.join();
  }
```

### PWM.h
```cpp
#ifndef PWM_H
#define PWM_H

#include <string>
#include <thread>

#include "RAINBOWCOLORS.h"
#include "SYSFILEACCESS.h"

/* 
  Declare a type for a function pointer
  It is the construct for: using function_type = int (*) ()
    function_type:  the function name
    int: return type  
    (*): the dereference operator due to the address of the function name
    (): the arguments of the function, in this case: void
  Stores the address of a function 
*/
using callbackType = int (*)();

const std::string PWM_PATH ("/sys/devices/platform/ocp/");
const std::string EHRPWM0_PATH = "48300000.epwmss/48300200.pwm/pwm/";
const std::string EHRPWM1_PATH = "48302000.epwmss/48302200.pwm/pwm/";
const std::string EHRPWM2_PATH = "48304000.epwmss/48304200.pwm/pwm/";

/* The pwm internal class number of the pin*/
enum PWM_ID {
  P8_13 = 0,
  P8_19 = 1,
  P9_14 = 2,
  P9_16 = 3,
  P9_21 = 4,
  P9_22 = 5, 
};

class PWM : public SYSFILEACCESS
{
  private:
    int id; /* The PWM number of the object */
    std::string idMap[6];
    std::string name;   /* The name of the PWM e.g. pwm-6:1 */
    std::string path;   /* The full path to the PWM e.g. /sys/class/pwm/pwm-6:1 */
    int period;
    int dutyCycle;
    int enable;
    
    // Helper method to enable the pwm on the pin
    virtual int Enable();

    // Helper method to enable the pwm on the pin
    virtual int Disable();

  public:
    // Default constructor
    PWM();

    // Overload constructor with pin's id
    PWM(int);

    // Overload constructor with the pin id and period
    PWM(int, int);

    // Initialize the PWM pin with the data provided by the constructor
    void InitPWMPin();

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
  #include <cstdlib>  // system()
  
  #include "PWM.h"
  
  using namespace std;
  
  class PWM_Exception : public exception 
  {
    private:
      string reason;
    public:
      PWM_Exception (const char* why) : reason (why) {};
      virtual const char* what() 
      {
        return reason.c_str();
      }
  };
  
  // Default constructor
  PWM::PWM() {}
  
  // Overload constructor with the pin's id
  PWM::PWM(int newPWMPin)
  {
    id = newPWMPin;
    period = 500000;
    InitPWMPin();
    SetPeriod(period);
  }
  
  // Overload constructor 
  PWM::PWM(int pwmPin, int newPeriod)
  {
    id = pwmPin;
    period = newPeriod;
    InitPWMPin();
    SetPeriod(newPeriod); 
    cout  << RainbowText("Setting the PWM pin with a period of ", "Pink")
          << RainbowText(to_string(GetPeriod()), "Pink") 
          << RainbowText("ns was a success!", "Pink") << endl; 
  }
  
  // Public method to initialize the PWM pin
  void PWM::InitPWMPin()
  {
    idMap[P8_13] = "P8.13";
    idMap[P8_19] = "P8.19";
    idMap[P9_14] = "P9.14";
    idMap[P9_16] = "P9.16";
    idMap[P9_21] = "P9.21";
    idMap[P9_22] = "P9.22";
  
    switch (id)
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
          << RainbowText(idMap[id], "Pink", "Default", "Bold") << endl;
    string commandString = "config-pin " + idMap[id] + " pwm";
    const char* command = commandString.c_str();
    system(command);
  
    Enable();
  }
  
  /*
     Private method to enable the PWM on the pin.
     It is used in the constructor
     @return int: 1 set duty cycle has succeeded / throw an exception if not
  */
  int PWM::Enable()
  {
    if (WriteFile(path, "enable", 1) != 1)
    {
      perror("Error trying to enable the PWM on the pin");
      throw PWM_Exception("Error in the 'Enable' method");
    }
    else 
    {
      return 1;
    }
  }
  
  /*
     Private method to disable the PWM on the pin.
     It is used in the constructor
     @return int: 1 set duty cycle has succeeded / throw an exception if not
  */
  int PWM::Disable()
  {
    if (WriteFile(path, "enable", 0) != 1)
    {
      perror("Error trying to disable the PWM on the pin");
      throw PWM_Exception("Error in the 'Disable' method");
    }
    else 
    {
      return 1;
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
    if (WriteFile(path, "period", period) != 1)
    {
      perror("Error setting the PWM period for the pin");
      throw PWM_Exception("Error in the 'SetPeriod' method");
    }
    else 
    {
      return 1;
    }
  }
  
  /*
     Public method to set the duty cycle of the PWM
     @param int: The desired duty cycle in pertentage: 0-100
     @return  int: 1 set duty cycle has succeeded / throw an exception if not
  */
  int PWM::SetDutyCycle(int newDutyCycle)
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
      throw PWM_Exception("Error in WritePWMDutyCycle method");
    }
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
  
    std::thread functionThread(callbackFunction);
    functionThread.detach();
    return 1;
  }
  
  // Destructor
  PWM::~PWM() 
  {
    this->SetDutyCycle(0);
  }
```

### ADC.h
```cpp
#ifndef ADC_H
#define ADC_H

#include <string>
#include <thread>

#include "RAINBOWCOLORS.h"
#include "SYSFILEACCESS.h"

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

class ADC : public SYSFILEACCESS
{
  private:
    int id; /* The ADC number of the object */
    std::string idMap[7];
    std::string path; /* The full path to the ADC e.g. /sys/bus/iio/devices/iio:device0/in_voltage0_raw */
    std::string name; /* The name of the ADC e.g. in_voltage0_raw */
    int adcValue;

    bool stopReadADCFlag = false;
    bool stopReadVoltageFlag = false;

    std::thread ReadADCThread;
    std::thread ReadVoltageThread;

    void MakeReadADC(int &, int);
    void MakeReadVoltage(float &, int);

  protected:
    // Helper method to get the ADC value on pin
    virtual int GetADC();

  public:
    // Default constructor
    ADC(int);

    // Interface method to read one time the adc value on the pin
    virtual void ReadADC(int &);

    // Overload interface method for reading the ADC value with a delay on the pin
    virtual void ReadADC(int &, int);

    // Interface method for reading the ADC value continuosly on the pin
    virtual void ReadADC(int &, int, bool);

    // Interface method to read one time the voltage on the pin
    virtual void ReadVoltage(float &);

    // Overload interface method for reading the voltage with a delay on the pin
    virtual void ReadVoltage(float &, int);

    // Interface method for reading the voltage continuosly on the pin
    virtual void ReadVoltage(float &, int, bool);
    
    // Method to do execute an user function
    virtual int DoUserFunction(callbackType);

    // Method to stop the ADC continuos sampling 
    virtual void StopReadADC();

    // Method to stop the voltage continuos sampling
    virtual void StopReadVoltage();
    
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
  
  class ADC_Exception : public exception 
  {
    private:
      string reason;
    public:
      ADC_Exception (const char* why) : reason (why) {};
      virtual const char* what()
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
  
    GetADC();
    cout  << RainbowText("Setting the ADC pin was a success!", "Violet") << endl; 
  }
  
  /*
    Protected method to get the ADC value on pin 
    @return int: The pin's value between 0 - 4095
  */
  int ADC::GetADC()
  {
    adcValue = ReadFile(path);
    if ( adcValue == -1)
    {
      perror("Error trying to read the ADC on the pin");
      throw ADC_Exception("Error in the 'ReadADC' method");
    }
    else
    {
      return adcValue;
    }
  }
  
  /*
     Public method to get the ADC value on pin 
     @return int: The pin's value between 0 - 4095
  */
  void ADC::ReadADC(int &adcValueOut)
  {
    adcValueOut = GetADC();
  }
  
  /*
    Public method to get one the ADC value on pin and wait a time Interval
    @param int: Reference output for the ADC value between 0 - 4095
    @param int: The time interval between each sample
  */
  void ADC::ReadADC(int &adcValueOut, int timeInterval)
  {
    ReadADC(adcValueOut);
    Delayms(timeInterval);
  }
  
  /*
    Public method to get continuosly the ADC value on pin 
    @param int: Reference output for the ADC value between 0 - 4095
    @param int: The time interval between each sample
  */
  void ADC::ReadADC(int &adcValueOut, int timeInterval, bool runInBackground)
  {
    if (runInBackground == true)
    {
      std::string message = "Read ADC input has been activated";
      cout << RainbowText(message, "Violet", "Default", "Bold") << endl;
      ReadADCThread = std::thread(&ADC::MakeReadADC, this, std::ref(adcValueOut),timeInterval);
    }
  }
  
  /*
    Private method that contains the routine to make the ADC read 
    @param int: A reference variable to store The pin's value between 0 - 4095
  */
  void ADC::MakeReadADC(int &adcValueOut, int timeInterval)
  {
    while (stopReadADCFlag == false)
    {
      this->ReadADC(adcValueOut);
      std::string message = "ADC value on pin " + idMap[id] + ": " + to_string(adcValueOut);
      cout << RainbowText(message, "Violet") << endl;
      Delayms(timeInterval);
    }
  }
  
  /*
    Public method to stop reading the ADC 
  */
  void ADC::StopReadADC()
  {
    stopReadADCFlag = true;
  }
  
  /*
    Public method to get the voltage on the pin
    @param float: Reference output for the ADC value between 0 - 1.8
  */
  void ADC::ReadVoltage(float &voltageOut)
  {
    voltageOut = GetADC() * 1.8 / 4095;
  }
  
  /*
    Public method to get the voltage on the pin
    @param float: Reference output for the ADC value between 0 - 1.8
    @param int: The time interval between each sample
  */
  void ADC::ReadVoltage(float &voltageOut, int timeInterval)
  {
    this->ReadVoltage(voltageOut);
    Delayms(timeInterval);
  }
  
  /*
    Public method to get continuosly the voltage value on pin 
    @param float: Reference output for the ADC value between 0 - 1.8
    @param int: The time interval between each sample
  */
  void ADC::ReadVoltage(float &voltageOut, int timeInterval, bool runInBackground)
  {
    std::string  message = "Read voltage in a thread has been activated";
    cout << RainbowText(message, "Violet", "Default", "Bold") << endl;
    ReadVoltageThread = std::thread(&ADC::MakeReadVoltage, this, std::ref(voltageOut),timeInterval);
  }
  
  /*
    Private method that contains the routine to make the ADC read 
    @param int: A reference variable to store The pin's value between 0 - 4095
  */
  void ADC::MakeReadVoltage(float &voltageOut, int timeInterval)
  {
    while (stopReadVoltageFlag == false)
    {
      this->ReadVoltage(voltageOut);
      std::string message = "Voltage on pin " + idMap[id] + ": " + to_string(voltageOut);
      cout << RainbowText(message, "Violet") << endl;
      Delayms(timeInterval);
    }
  }
  
  /*
    Public method to stop reading the voltage 
  */
  void ADC::StopReadVoltage()
  {
    stopReadVoltageFlag = true;
  }
  
  /*
    Public callback method to do a user Customized function when is called
    @param callbackType: user function pointer to execute 
    @return int: 1 the user function was called      
  */
  
  int ADC::DoUserFunction (callbackType callbackFunction)
  {
    std::string message = "'UserFunction' method has been activated!";
    cout << RainbowText(message, "Violet", "Default", "Bold") << endl;
  
    std::thread functionThread(callbackFunction);
    functionThread.detach();
    return 1;
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
  
    // Waiting for the last reading on the pin
    this->Delayms(10);
    cout  << RainbowText("Stopping the reading on the ADC PIN: ","Violet")
          << RainbowText(idMap[id], "Violet", "Default", "Bold") << endl;
  }
```

Se you in the next post. 
