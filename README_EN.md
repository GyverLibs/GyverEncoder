This is an automatic translation, may be incorrect in some places. See sources and examples!

## WARNING, THE LIBRARY IS OUT OF DATE!
### USE THE [EncButton] LIBRARY(https://github.com/GyverLibs/EncButton)
It is much lighter, has more features and uses less CPU time!

```






```

# GyverEncoder
Library for extended work with encoder
- Practicing encoder rotation
- Working out "pressed turn"
- Working out "quick turn"
- Several encoder polling algorithms
- Choice of encoder connection pull-up
- Work with two types of encoders
- Work with an external encoder (through a pin expander, etc.)
- Practicing pressing / holding a button with anti-bounce

### Compatibility
Compatible with all Arduino platforms (using Arduino functions)

### Documentation
The library has [extended documentation](https://alexgyver.ru/encoder/)

## Content
- [Install](#install)
- [Initialization](#init)
- [Usage](#usage)
- [Example](#example)
- [Versions](#versions)
- [Bugs and feedback](#feedback)

<a id="install"></a>
## Installation
- The library can be found by the name **GyverEncoder** and installed through the library manager in:
    - Arduino IDE
    - Arduino IDE v2
    - PlatformIO
- [Download library](https://github.com/GyverLibs/GyverEncoder/archive/refs/heads/main.zip) .zip archive for manual installation:
    - Unzip and put in *C:\Program Files (x86)\Arduino\libraries* (Windows x64)
    - Unpack and put in *C:\Program Files\Arduino\libraries* (Windows x32)
    - Unpack and put in *Documents/Arduino/libraries/*
    - (Arduino IDE) automatic installation from .zip: *Sketch/Include library/Add .ZIP library…* and specify the downloaded archive
- Read more detailed instructions for installing libraries [here] (https://alexgyver.ru/arduino-first/#%D0%A3%D1%81%D1%82%D0%B0%D0%BD%D0%BE% D0%B2%D0%BA%D0%B0_%D0%B1%D0%B8%D0%B1%D0%BB%D0%B8%D0%BE%D1%82%D0%B5%D0%BA)

<a id="init"></a>
## Initialization
```cpp
encoder enc; // not tied to a pin
Encoder enc(pin CLK, pin DT); // encoder without button (quick polling)
Encoder enc(pin CLK, pin DT, pin SW); // encoder with button
Encoder enc(pin CLK, pin DT, pin SW, type); // encoder with button and type indication
Encoder enc(pin CLK, pin DT, ENC_NO_BUTTON, type); // encoder without button and with type indication
```

<a id="usage"></a>
## Usage
```cpp
void tick(); // encoder poll, must be called permanently or in an interrupt
void setType(boolean type); // TYPE1 / TYPE2 - encoder type TYPE1 single step, TYPE2 two step. If your encoder is acting strange, change the type
void setTickMode(boolean tickMode); // MANUAL / AUTO - manual or automatic polling of the encoder with the tick() function. (default manual)
void setDirection(boolean direction); // NORM / REVERSE - encoder rotation direction
void setFastTimeout(int timeout); // set fast rotation timeout
void setPinMode(bool mode); // encoder connection type, pull-up HIGH_PULL (internal) or LOW_PULL (external to GND)
void setBtnPinMode(bool mode); // button connection type, pull-up HIGH_PULL (internal) or LOW_PULL (external to GND)
 
boolean isTurn(); // returns true on any rotation, resets itself to false
boolean isRight(); // returns true when turning right, resets itself tocranberry false
boolean isLeft(); // returns true when turning left, resets itself to false
boolean isRightH(); // returns true when holding down the button and turning right, resets itself to false
boolean isLeftH(); // returns true when holding the button and turning left, resets itself to false
boolean isFastR(); // returns true on fast turn
boolean isFastL(); // returns true on fast turn
 
boolean isPress(); // returns true when the button is clicked, resets itself to false
boolean isRelease(); // returns true when the button is released, resets itself to false
boolean isClick(); // returns true when the button is pressed and released, resets itself to false
boolean isHolded(); // returns true when the button is held down, resets itself to false
boolean isHold(); // returns true when the button is held down, DOES NOT CLEAR
boolean isSingle(); // returns true on single click (after timeout), resets itself to false
boolean isDouble(); // returns true on double click, resets itself to false
void resetStates(); // resets all is flags
```

<a id="example"></a>
## Example
See **examples** for other examples!
```cpp
#define CLK 2
#define DT 3
#define SW 4

#include "GyverEncoder.h"
Encoder enc1(CLK, DT, SW); // for working with a button

void setup() {
  Serial.begin(9600);
  enc1.setType(TYPE2);
}

void loop() {
// mandatory processing function. Must be constantly asked
  enc1.tick();
  
  if (enc1.isTurn()) { // if a turn was made (turn indicator in either direction)
    // your code
  }
  
  if (enc1.isRight()) Serial.println("Right"); // if there was a turn
  if (enc1.isLeft()) Serial.println("Left");
  
  if (enc1.isRightH()) Serial.println("Right held"); // if there was a hold + turn
  if (enc1.isLeftH()) Serial.println("Left held");
  
  //if (enc1.isPress()) Serial.println("Press"); // button click (+ debounce)
  //if (enc1.isRelease()) Serial.println("Release"); // same as isClick
  
  if (enc1.isClick()) Serial.println("Click"); // single click
  if (enc1.isSingle()) Serial.println("Single"); // single click (with timeout for double click)
  if (enc1.isDouble()) Serial.println("Double"); // double click
  
  
  if (enc1.isHolded()) Serial.println("Holded"); // if it was held and the enc didn't rotate
  //if (enc1.isHold()) Serial.println("Hold"); // returns the state of the button
}
```

<a id="versions"></a>
## Versions
- v3.6 from 09/16/2019 - Default settings returned
- v4.0 from 11/13/2019
        - Optimized code
        - Fixed bugs
        - Added other polling algorithms
        - Added the ability to completely remove the button (memory saving)
        - Added the ability to connect an external encoder
        - Added pin tightening setting
- v4.1: Fixed changing suspenders
- v4.2
        - Added TYPE1 support for PRECISE_ALGORITHM algorithm
        - Added double click processing: isSingle / isDouble
- v4.3: Fixed false isSingle
- v4.4: Added resetStates method, resets all is-flags and counters
- v4.5: Improved BINARY_ALGORITHM algorithm (thanks to Yaroslav Kurus)
- v4.6: BINARY_ALGORITHM fixed for TYPE1, isReleaseHold added
- v4.7: Fixed accidental pressed rotation in BINARY_ALGORITHM
- v4.8: increased performance for AVR Arduino
- v4.9: fast turn disabled if button is held down
- v4.10: fixed setDirection()

<a id="feedback"></a>
## Bugs and feedback
When you find bugs, create an **Issue**, or better, immediately write to the mail [alex@alexgyver.ru](mailto:alex@alexgyver.ru)
The library is open for revision and your **Pull Request**'Cranberries ow!