

# 7-Segment Display Clock

## Introduction
This project demonstrates how to build a digital clock using a 7-segment display module and an Arduino. The clock displays the current time in hours and minutes using four 7-segment displays.

## Components Required
- Arduino Uno
- 4-digit 7-segment display module (common cathode or common anode)
- DS3231 RTC (Real-Time Clock) module
- 220-ohm resistors (if needed, depending on your display module)
- Breadboard and jumper wires
- Power supply (e.g., USB or battery)

## Circuit Diagram
1. **Connect the DS3231 RTC module to the Arduino:**
   - VCC to 5V
   - GND to GND
   - SDA to A4
   - SCL to A5

2. **Connect the 7-segment display module to the Arduino:**
   - Connect segment pins (a, b, c, d, e, f, g, dp) to Arduino digital pins
   - Connect common pins (DIG1, DIG2, DIG3, DIG4) to Arduino digital pins

## Arduino Code

```cpp
#include <Wire.h>
#include <RTClib.h>

RTC_DS3231 rtc;

const int segmentPins[8] = {2, 3, 4, 5, 6, 7, 8, 9}; // a, b, c, d, e, f, g, dp
const int digitPins[4] = {10, 11, 12, 13}; // DIG1, DIG2, DIG3, DIG4

const byte segmentDigits[10] = {
  0b00111111, // 0
  0b00000110, // 1
  0b01011011, // 2
  0b01001111, // 3
  0b01100110, // 4
  0b01101101, // 5
  0b01111101, // 6
  0b00000111, // 7
  0b01111111, // 8
  0b01101111  // 9
};

void setup() {
  Wire.begin();
  rtc.begin();

  for (int i = 0; i < 8; i++) {
    pinMode(segmentPins[i], OUTPUT);
  }
  for (int i = 0; i < 4; i++) {
    pinMode(digitPins[i], OUTPUT);
    digitalWrite(digitPins[i], LOW); // Turn off all digits
  }
}

void loop() {
  DateTime now = rtc.now();
  
  int hours = now.hour();
  int minutes = now.minute();
  
  displayNumber(hours / 10, 0);   // Tens place of hours
  delay(5);
  displayNumber(hours % 10, 1);   // Units place of hours
  delay(5);
  displayNumber(minutes / 10, 2); // Tens place of minutes
  delay(5);
  displayNumber(minutes % 10, 3); // Units place of minutes
  delay(5);
}

void displayNumber(int num, int digit) {
  for (int i = 0; i < 8; i++) {
    digitalWrite(segmentPins[i], bitRead(segmentDigits[num], i));
  }
  
  for (int i = 0; i < 4; i++) {
    digitalWrite(digitPins[i], i == digit ? HIGH : LOW);
  }
}
```

## Usage
1. Connect the components according to the circuit diagram.
2. Upload the provided code to the Arduino.
3. The clock will display the current time in hours and minutes.

## Troubleshooting
- Ensure all connections are secure.
- Verify the correct common type (cathode or anode) of your 7-segment display and adjust the wiring/code if necessary.
- Make sure the DS3231 RTC module is working and correctly set up.

## License
This project is open-source and available under the MIT License.

