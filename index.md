# Fingerprint ID Safe with Keypad
Safes need the best security to protect your valuables, and this safe will contain two different measures of safety: the fingerprint sensor and keypad. No one shares the same fingerprint pattern, so access to the safe is exclusive to you. The keypad will provide another layer of protection as only you will have memory of the code.

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Miguel Perez | Millennium Brooklyn High School | Electrical Engineering | Incoming Senior

# Project Demontration


# Presentation Slideshow

<p align="center">
<iframe src="https://docs.google.com/presentation/d/1UHRe4euwXLA001kdskTZnMEoiOJf4JZp6baKYkGstNM/edit#slide=id.g143baba31e0_0_82" frameborder="0" frameborder="0" width="760" height="369" allowfullscreen="true" mozallowfullscreen="true" webkitallowfullscreen="true"></iframe>
</p>


# Final Milestone


My final milestone was to make the fingerprint sensor and the keypad work together to rotate the servo motor and provide double the protection for the safe. I wired the fingerprint sensor, keypad, and servo motor onto the Arduino board then I combined the codes from my previous milestones to make a final code that works everything together. A correct fingerprint and keypad password are required to rotate the servo motor and open the door, so it will not work when one of them is incorrect.

[![Final Milestone](https://res.cloudinary.com/marcomontalbano/image/upload/v1660246216/video_to_markdown/images/youtube--xr4aomYz9q4-c05b58ac6eb4c4700831b2b3070cd403.jpg)](https://youtu.be/xr4aomYz9q4 "Final Milestone"){:target="_blank" rel="noopener"}

# Second Milestone


My second milestone was setting up a password on the keypad to make the servo motor rotate. I first correctly wired both the keypad and the servo motor onto the Arduino board and entered the correct password on the code I used. The serial monitor will appear as "incorrect" when I enter the wrong password and will appear as "correct" when I enter the right password, which is 37A23, and cause the servo motor to rotate.

[![Milestone 2](https://res.cloudinary.com/marcomontalbano/image/upload/v1660245951/video_to_markdown/images/youtube--levklaeHm24-c05b58ac6eb4c4700831b2b3070cd403.jpg)](https://youtu.be/levklaeHm24 "Milestone 2"){:target="_blank" rel="noopener"}

# First Milestone
  

My first milestone was enrolling my fingerprint onto the fingerprint module to make the servo motor rotate. I first had to correctly wire the fingerprint sensor to the Arduino board and then upload code from the Adafruit Fingerprint Sensor Library to the Arduino board to enroll my fingerprint onto the sensor. I enrolled my fingerprint by saving it as "ID 1" and then placing my finger on the fingerprint sensor while it lit up. 

[![Milestone 1](https://res.cloudinary.com/marcomontalbano/image/upload/v1659388711/video_to_markdown/images/youtube--vZ3ouX8SA2c-c05b58ac6eb4c4700831b2b3070cd403.jpg)](https://youtu.be/vZ3ouX8SA2c "Milestone 1"){:target="_blank" rel="noopener"}

# Bill of Materials 

Total cost: $113.53

| Item | Qty | Price | Where to Buy |
| ------------- | ------------- | ------------- | ------------- |
| Arduino UNO REV3  | 1  | $24.49  | https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6  |
| Fingerprint Sensor  | 1  | $22.79  | https://www.amazon.com/Optical-Fingerprint-Reader-Arduino-Mega2560/dp/B077GKPWMN  |
| 4x4 Keypad  | 1  | $7.99 ($1.60 each)  | https://www.amazon.com/Matrix-Membrane-Switch-Keyboard-Arduino/dp/B07THCLGCZ  |
| Servo Motor  | 1  | $15.99  | https://www.amazon.com/ANNIMOS-Digital-Waterproof-DS3218MG-Control/dp/B076CNKQX4  |
| Jumper Wires  | 23  | $6.98 ($0.30 each)  | https://www.amazon.com/Elegoo-EL-CP-004-Multicolored-Breadboard-arduino/dp/B01EV70C78  |
| String  | 1  | $4.99  | https://www.amazon.com/White-Cotton-Butchers-Twine-String/dp/B09TQXBFYD  |
| Tape  | 1  | $9.84  | https://www.amazon.com/ScotchBlue-2098-36D-Platinum-Painters-Tape/dp/B01FIIXXQ6  |
| Cardboard  | 1  | $0.51  | https://www.amazon.com/Juvale-Corrugated-Cardboard-Sheets-Inches/dp/B07TDH6XG9  |
| Hot Glue Gun With Sticks  | 1  | $19.95  | https://www.amazon.com/Gorilla-8401509-Hot-Glue-Sticks/dp/B07K791YRP  |

# Fingerprint ID Safe with Keypad Code

``` 
#include <Keypad.h>
#define Password_Length 6
#include <Adafruit_Fingerprint.h>
#include <SoftwareSerial.h>
#include<Servo.h>
SoftwareSerial mySerial(12, 13);
Adafruit_Fingerprint finger = Adafruit_Fingerprint(&mySerial);

const byte ROWS = 4;
const byte COLS = 4;
char Data[Password_Length];
char Master[Password_Length] = "37A23";
byte data_count = 0, master_count = 0;
bool Pass_is_good;
char customKey;
Servo myServo;
char hexaKeys[ROWS][COLS] = {
  {'1', '2', '3', 'A'},
  {'4', '5', '6', 'B'},
  {'7', '8', '9', 'C'},
  {'*', '0', '#', 'D'}
};

byte rowPins[ROWS] = {9, 8, 7, 6};
byte colPins[COLS] = {5, 4, 3, 2};

Keypad customKeypad = Keypad(makeKeymap(hexaKeys), rowPins, colPins, ROWS, COLS);

void setup() {
  Serial.begin(9600);
  myServo.attach(10);
  while (!Serial);
  delay(100);
  Serial.println("fingertest");
  finger.begin(57600);
  if (finger.verifyPassword()) {
    Serial.println("Found fingerprint sensor!");
  }
  else
  {
    Serial.println("Did not find fingerprint sensor :(");
    while (1)
    {
      delay(1);
    }
  }
  finger.getTemplateCount();
  Serial.print("Sensor contains "); Serial.print(finger.templateCount); Serial.println(" templates");
  Serial.println("Waiting for valid finger...");
  // Serial.println("Enter Password:");
}

void loop() {
  getFingerprintIDez();

  char customKey = customKeypad.getKey();
  if (customKey) {
    Serial.print(customKey);
    Data[data_count] = customKey;
    data_count++;
  }

  if (data_count == Password_Length - 1) {

    if (!strcmp(Data, Master)) {
      Serial.println("\nCorrect Password");
      if (finger.fingerID == 1 ) {
        Serial.println("Valid fingerprint!");
        myServo.write(80);
        delay(5000);
      }
      else {
        Serial.println("Not a valid fingerprint!");
      }
      delay(5000);

    }
    else {
      Serial.println("\nInCorrect Password");
      delay(1000);
    }

    clearData();
  }

}


uint8_t getFingerprintID()
{
  uint8_t p = finger.getImage();
  switch (p)
  {
    case FINGERPRINT_OK:
      Serial.println("Image taken");
      break;
    case FINGERPRINT_NOFINGER:
      Serial.println("No finger detected");
      return p;
    case FINGERPRINT_PACKETRECIEVEERR:
      Serial.println("Communication error");
      return p;
    case FINGERPRINT_IMAGEFAIL:
      Serial.println("Imaging error");
      return p;
    default:
      Serial.println("Unknown error");
      return p;
  }
  // OK success!
  p = finger.image2Tz();
  switch (p)
  {
    case FINGERPRINT_OK:
      Serial.println("Image converted");
      break;
    case FINGERPRINT_IMAGEMESS:
      Serial.println("Image too messy");
      return p;
    case FINGERPRINT_PACKETRECIEVEERR:
      Serial.println("Communication error");
      return p;
    case FINGERPRINT_FEATUREFAIL:
      Serial.println("Could not find fingerprint features");
      return p;
    case FINGERPRINT_INVALIDIMAGE:
      Serial.println("Could not find fingerprint features");
      return p;
    default:
      Serial.println("Unknown error");
      return p;
  }
  // OK converted!
  p = finger.fingerFastSearch();
  if (p == FINGERPRINT_OK)
  {
    Serial.println("Found a print match!");
  } else if (p == FINGERPRINT_PACKETRECIEVEERR) {
    Serial.println("Communication error");
      p;
  } else if (p == FINGERPRINT_NOTFOUND) {
    Serial.println("Did not find a match");
    return p;
  }
  else
  {
    Serial.println("Unknown error");
    return p;
  }
  {
    digitalWrite(11, HIGH);
    delay(3000);
    digitalWrite(11, LOW);
    Serial.print("Not Found");
    Serial.print("Error");
    return finger.fingerID;
  }
  // found a match!
  Serial.print("Found ID #"); Serial.print(finger.fingerID);
  Serial.print(" with confidence of "); Serial.println(finger.confidence);
  Serial.print("Enter password");
  return finger.fingerID;
}
// returns -1 if failed, otherwise returns ID #
int getFingerprintIDez()
{
  uint8_t p = finger.getImage();
  if (p != FINGERPRINT_OK)  return -1;

  p = finger.image2Tz();
  if (p != FINGERPRINT_OK)  return -1;

  p = finger.fingerFastSearch();
  if (p != FINGERPRINT_OK)  return -1;
  {
    delay(3000);
    Serial.print("Found ID #"); Serial.print(finger.fingerID);
    Serial.print(" with confidence of "); Serial.println(finger.confidence);
  }
}


void clearData() {
  while (data_count != 0) {
    Data[data_count--] = 0;
  }
  return;
}
```
