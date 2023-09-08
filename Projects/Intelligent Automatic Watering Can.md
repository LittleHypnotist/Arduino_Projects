### ***Intelligent Automatic Watering Can using Arduino***

<hr>

Intelligent Automatic Watering Cans, powered by Arduino, revolutionize the way we nurture our plants. <p>
They combine the precision of technology with the nurturing touch of nature. <p>
In this project, we'll embark on a journey to create a device that waters plants intelligently, ensuring they thrive and flourish.<p>
Mastering this fundamental step will serve as the cornerstone for more advanced experiments in automated gardening. <p>
With the Intelligent Automatic Watering Can, we're not just building a device; we're cultivating a connection between technology and the natural world.<p>

<br>

***Components Required:***
- 1 x Arduino Uno R3
- 1 x DC motor
- 1 x 100 Ω Potentiometer
- 1 x Soil moisture sensor
- 1 x LCD 16 x 2
- 1 x H-bridge motor drive

<br>

***Note:*** It is imperative to use an H-bridge motor drive due to the requirements of the DC motor. This component enables precise control over the motor's direction and speed, crucial for the success of this project.

<hr>

***Circuit Diagram***

![image](https://github.com/LittleHypnotist/Arduino_Projects/assets/75622692/92e80172-3af0-4a49-abf7-6c3f18ca2400)


<hr>

***Arduino Code***

```cpp

#include <LiquidCrystal.h>

// define the pins between the arduino and lcd
const int rs = 12, en = 11, d4 = 5, d5 = 4, d6 = 3, d7 = 2;
LiquidCrystal lcd(rs, en, d4, d5, d6, d7);


const int pinoSensor = A0;
const int soloSeco = 65; // percentage
const int tempoRega = 5; // seconds
int humidadeSolo = 0;

void setup() {
  
  // set lcd size
  lcd.begin(16, 2);
  
}

void loop() {
  
    lcd.clear();
    lcd.print("Rega Automatica: ");
  
  for(int i=0; i < 5; i++) {   
    
    // positions the lcd cursor in column 0 row 1
    lcd.setCursor(0, 1);
    lcd.print("Humidade:");
    // humidity sensor reading
    humidadeSolo = analogRead(pinoSensor);
    // converts sensor variation from 0 to 876 to 0 to 100, to be used as a percentage
    humidadeSolo = map(humidadeSolo, 0, 876, 0, 100);
    
    lcd.print(humidadeSolo);
    lcd.print("%");
    
    delay(1000);
  }
  
  if(humidadeSolo < soloSeco) {
    
    lcd.clear();
    lcd.print("Rega Automatica: ");
    
    // positions the LCD cursor in column 0 row 1
    lcd.setCursor(0, 1);
    lcd.print(" A Regar...");
    
    // motor watering  
    digitalWrite(9, LOW);
    digitalWrite(8, HIGH);
    
    delay(tempoRega*1000);
    
  }
  else {
    
    lcd.clear();
    lcd.print("Rega Automatica: ");
    
    // positions the LCD cursor in column 0 row 1
    lcd.setCursor(0, 1);
    
    // engine not watering
    digitalWrite(9, HIGH);
    digitalWrite(8, HIGH);
    
    lcd.print("Solo regado");
    
    delay(3000);
  }
}

```

Link to the project on thinkercad: https://www.tinkercad.com/things/7bA13K3hDtL-intelligent-automatic-watering-can
