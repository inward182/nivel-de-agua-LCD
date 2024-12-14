# Nivel de agua LCD

## Descripción
### En este repositorio se muestra como  podemos programar un pantalla LCD que nos indique el porcentaje de nivel de agua medido a traves de un sensor ultrasonico de distancia, mientras que 4 leds nos indican lo mismo.
## Material Necesario
- wokwi
- ESP32
- Ultrasonic Sensor
- LCD-1602
- Relay Module Reference
## Programación
```
// defines pins numbers
#include <LiquidCrystal_I2C.h>
#define I2C_ADDR    0x27
#define LCD_COLUMNS 20
#define LCD_LINES   4
LiquidCrystal_I2C lcd(I2C_ADDR, LCD_COLUMNS, LCD_LINES);

const int trigPin = 4;
const int echoPin = 15;
const int led1 = 12;
const int led2 = 14;
const int led3 = 26;
const int led4 = 25;

// defines variables
long duration;
int distance;
int safetyDistance;


void setup() {
  
pinMode(trigPin, OUTPUT); // Sets the trigPin as an Output
pinMode(echoPin, INPUT); // Sets the echoPin as an Input
pinMode(led1, OUTPUT);
pinMode(led2, OUTPUT);
pinMode(led3, OUTPUT);
pinMode(led4, OUTPUT);

Serial.begin(9600); 
lcd.init();
lcd.backlight();
 // Starts the serial communication
}


void loop() {
// Clears the trigPin
digitalWrite(trigPin, LOW);
delayMicroseconds(2);

// Sets the trigPin on HIGH state for 10 micro seconds
digitalWrite(trigPin, HIGH);
delayMicroseconds(10);
digitalWrite(trigPin, LOW);

// Reads the echoPin, returns the sound wave travel time in microseconds
duration = pulseIn(echoPin, HIGH);

// Calculating the distance
distance= duration*0.034/2;

safetyDistance = distance;
if (safetyDistance>=1 && safetyDistance<=5)
{
  digitalWrite(led1, LOW);
  digitalWrite(led2, LOW);
  digitalWrite(led3, LOW);
  digitalWrite(led4, LOW);
  lcd.clear();
  lcd.setCursor(0,4);
  lcd.print("Tanque 0%");
  delay(200);
}
else if(safetyDistance>=5 && safetyDistance<=100) 
{
  digitalWrite(led1, HIGH);
  digitalWrite(led2, LOW);
  digitalWrite(led3, LOW);
  digitalWrite(led4, LOW);
  lcd.clear();
  lcd.setCursor(-3,4);
  lcd.print("Tanque 25%");
  delay(200);
}
else if (safetyDistance>=101 && safetyDistance<=200)
{
 digitalWrite(led1,  HIGH);
  digitalWrite(led2, HIGH);
  digitalWrite(led3, LOW);
  digitalWrite(led4, LOW);
  lcd.clear();
  lcd.setCursor(-6,4);
  lcd.print("Tanque 50%");
  delay(200);
}
else if (safetyDistance>=201 && safetyDistance<=300)
{
 digitalWrite(led1,  HIGH);
  digitalWrite(led2, HIGH);
  digitalWrite(led3, HIGH);
  digitalWrite(led4, LOW);
  lcd.clear();
  lcd.setCursor(-6,4);
  lcd.print("Tanque 75%");
  delay(200);
}
else 
{
 digitalWrite(led1,  HIGH);
  digitalWrite(led2, HIGH);
  digitalWrite(led3, HIGH);
  digitalWrite(led4, HIGH);
  lcd.clear();
  lcd.setCursor(-6,4);
  lcd.print("Tanque lleno");
  delay(200);
}


// Prints the distance on the Serial Monitor
Serial.print("Distance: ");
Serial.println(distance);
delay (2000);
}
 ```
## Librerías

1. DHT Sensor Library for EXPx
2. LiquidCrystal I2C

## Conexión

![image](https://github.com/user-attachments/assets/e84bd045-153e-431b-8489-aa8187282d12)

##Instrucciones de operación 

1. Iniciar Simulador
2. Visualizar los datos en la pantalla LCD
3. Visualizar lo que nos indican los Leds en los modulos de referencia
4. Subir y Bajar la distancia dando doble click al sensor UltraSonic

## Resultados

Los valores serán mostrados en la pantalla LCD, cada 2 segundos se actualizará, mostrará el porcentaje de agua en el tanque basado en la distancia que marca el sensor, y también si ninguna luz se enciende, significa que esta vacio, una sola luz esta al 25%, dos luces al 50%, tres luces al 75% y las 4 cuando esta lleno el tanque. 

![image](https://github.com/user-attachments/assets/53cc6daa-cdf1-4f84-a60f-0610c0ee9970)
![image](https://github.com/user-attachments/assets/b7c67f04-8bad-49de-8fa8-a27b6ba0718e)
![image](https://github.com/user-attachments/assets/974673bf-60e1-40bd-b437-ca73eee7ce98)
![image](https://github.com/user-attachments/assets/1969a4fe-5867-43ca-98a8-614f089ffe3f)
![image](https://github.com/user-attachments/assets/76172d17-ac14-445e-bd14-f06f428f9cd8)




