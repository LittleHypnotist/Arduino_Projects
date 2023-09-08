### ***Intelligent Automatic Watering Can using Arduino***

<hr>

Intelligent Automatic Watering Cans, powered by Arduino, revolutionize the way we nurture our plants. 
They combine the precision of technology with the nurturing touch of nature. 
In this project, we'll embark on a journey to create a device that waters plants intelligently, ensuring they thrive and flourish.
Mastering this fundamental step will serve as the cornerstone for more advanced experiments in automated gardening. 
With the Intelligent Automatic Watering Can, we're not just building a device; we're cultivating a connection between technology and the natural world.

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

<img src="![Alt text](image.png)" width="650">

<hr>

***Arduino Code***

```cpp

#include <LiquidCrystal.h>

// definir os pinos entre o arduino e lcd
const int rs = 12, en = 11, d4 = 5, d5 = 4, d6 = 3, d7 = 2;
LiquidCrystal lcd(rs, en, d4, d5, d6, d7);


const int pinoSensor = A0;
const int soloSeco = 65; //percentagem
const int tempoRega = 5; //segundos
int humidadeSolo = 0;

void setup() {
  
  // define o tamanho do lcd
  lcd.begin(16, 2);
  
}

void loop() {
  
    lcd.clear();
    lcd.print("Rega Automatica: ");
  
  for(int i=0; i < 5; i++) {   
    
    //posiciona o cursor do lcd na coluna 0 linha 1
    lcd.setCursor(0, 1);
    lcd.print("Humidade:");
    //leitura do sensor de humidade
    humidadeSolo = analogRead(pinoSensor);
    //converte variação do sensor de 0 a 876 para 0 a 100, para ser usado a percentagem
    humidadeSolo = map(humidadeSolo, 0, 876, 0, 100);
    
    lcd.print(humidadeSolo);
    lcd.print("%");
    
    delay(1000);
  }
  
  if(humidadeSolo < soloSeco) {
    
    lcd.clear();
    lcd.print("Rega Automatica: ");
    
    //posiciona o cursor do LCD na coluna 0 linha 1
    lcd.setCursor(0, 1);
    lcd.print(" A Regar...");
    
    //motor rega 
    digitalWrite(9, LOW);
    digitalWrite(8, HIGH);
    
    delay(tempoRega*1000);
    
  }
  else {
    
    lcd.clear();
    lcd.print("Rega Automatica: ");
    
    //posiciona o cursor do LCD na coluna 0 linha 1
    lcd.setCursor(0, 1);
    
    //motor não rega
    digitalWrite(9, HIGH);
    digitalWrite(8, HIGH);
    
    lcd.print("Solo regado");
    
    delay(3000);
  }
}

```