### ***Intelligent Automatic Watering Can using Arduino***

<hr>

Intelligent Automatic Watering Cans, powered by Arduino, revolutionize the way we nurture our plants. 
They combine the precision of technology with the nurturing touch of nature. 
In this project, we'll embark on a journey to create a device that waters plants intelligently, ensuring they thrive and flourish.
Mastering this fundamental step will serve as the cornerstone for more advanced experiments in automated gardening. 
With the Intelligent Automatic Watering Can, we're not just building a device; we're cultivating a connection between technology and the natural world.

***Components Required:***
- 1 x Arduino Uno R3
- 1 x 4x4 Keyboard
- 1 x LCD 16 x 2
- 1 x Positional Micro servo
- 1 x 1 kΩ Resistor

<br>

<hr>

***Circuit Diagram***

<img src="![Alt text](image.png)" width="650">

<hr>

***Arduino Code***

```cpp

#include <LiquidCrystal.h>

// definir os pinos entre o arduino e lcd
#include <Keypad.h>
#include <LiquidCrystal.h>
#include <Servo.h>

// Setting the password length
#define Password_Length 5

// Initializing objects
Servo myservo;
LiquidCrystal lcd(A0, A1, A2, A3, A4, A5);

int pos = 0;

char Data[Password_Length]; // Array to store the password entered
char Master[Password_Length] = "1234"; // Master password
byte data_count = 0, master_count = 0;

bool Pass_is_good;
bool door = false;
char customKey;

// Configuring the keyboard layout
const byte ROWS = 4;
const byte COLS = 4;
char keys[ROWS][COLS] = {
  {'1', '2', '3', 'A'},
  {'4', '5', '6', 'B'},
  {'7', '8', '9', 'C'},
  {'*', '0', '#', 'D'}
};

byte rowPins[ROWS] = {0, 1, 2, 3};
byte colPins[COLS] = {4, 5, 6, 7};

Keypad customKeypad( makeKeymap(keys), rowPins, colPins, ROWS, COLS);

// Function to display loading message
void loading (char msg[]) {
  lcd.setCursor(0, 1);
  lcd.print(msg);

  for (int i = 0; i < 9; i++) {
    delay(1000);
    lcd.print(".");
  }
}

// Function to clear password data
void clearData()
{
  while (data_count != 0)
  { 
    Data[data_count--] = 0;
  }
  return;
}

// Door closing function
void ServoClose()
{
  for (pos = 90; pos >= 0; pos -= 10) { 
    myservo.write(pos);
  }
}

// Door opening function
void ServoOpen()
{
  for (pos = 0; pos <= 90; pos += 10) {
    myservo.write(pos);  
  }
}

// Main function
void setup()
{
  myservo.attach(9, 2000, 2400);
  ServoClose();
  lcd.begin(16, 2);
  lcd.print("Protected Door");
  loading("Loading");
  lcd.clear();
}

void loop()
{
  if (door == true)
  {
    customKey = customKeypad.getKey();
    if (customKey == '#')
    {
      lcd.clear();
      ServoClose();
      lcd.print("Door is closed");
      delay(3000);
      door = false;
    }
  }
  else
    Open();
}

// Password entry function
void Open()
{
  lcd.setCursor(0, 0);
  lcd.print("Enter Password");
  
  customKey = customKeypad.getKey();
  if (customKey)
  {
    Data[data_count] = customKey;
    lcd.setCursor(data_count, 1);
    lcd.print(Data[data_count]);
    data_count++;
  }

  if (data_count == Password_Length - 1)
  {
    if (!strcmp(Data, Master))
    {
      lcd.clear();
      ServoOpen();
      lcd.print(" Door is Open ");
      door = true;
      delay(5000);
      loading("Waiting");
      lcd.clear();
      lcd.print(" Time is up! ");
      delay(1000);
      ServoClose();
      door = false;      
    }
    else
    {
      lcd.clear();
      lcd.print(" Wrong Password ");
      door = false;
    }
    delay(1000);
    lcd.clear();
    clearData();
  }
}

```
