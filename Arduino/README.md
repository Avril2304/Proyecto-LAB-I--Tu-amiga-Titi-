# Arduino
##### Proyecto LAB I 
### Por Bianconi Clara y Ogas Avril:
En esta carpeta encontraremos el codigo que realizamos para el funcionamiento del Arduino de nuestro proyecto "Tu amiga Titi":

## CODIGO SIN TERMINAR 29/10/2024

#include <LCDI2C_Viet.h>  // Biblioteca para display
#include <ROM_Standard_JP.h>  
#include <SoftwareSerial.h>  // Biblioteca para Bluetooth
#include <SimpleKalmanFilter.h>  // Biblioteca para el filtro Kalman
#include "DFRobotDFPlayerMini.h"//bibloteca dfplayer
 
#if (defined(ARDUINO_AVR_UNO) || defined(ESP8266))   // Using a soft serial port
#define FPSerial mySerial
#else
#define FPSerial Serial1
#endif
 
LiquidCrystal_I2C_Viet lcd(0x27, 20, 4);
#define SENSOR_1 8  // Sensor táctil 1
#define SENSOR_2 9  // Sensor táctil 2
 
SoftwareSerial mySerial(2, 3);  // Cables Bluetooth
DFRobotDFPlayerMini myDFPlayer;
void printDetail(uint8_t type, int value);//mostrar los detalles del dfplayer
 
/*------VARIABLES-------*/
const int sensorLuz = A1;
String datos = "";
float lectura = 0, minhumedad = 0, maxhumedad = 0, maxluz = 0, minluz = 0,cantagu=0;
int pinBuzzer = 6, bomba = 7;
 
 
/*------FILTRO KALMAN-------*/
SimpleKalmanFilter kalmanSensor1(2, 2, 0.01);  // Filtro Kalman para el Sensor 1
SimpleKalmanFilter kalmanSensor2(2, 2, 0.01);  // Filtro Kalman para el Sensor 2
int sensor1Value = digitalRead(SENSOR_1), sensor2Value = digitalRead(SENSOR_2);
float sensor1Filtered = kalmanSensor1.updateEstimate(sensor1Value), sensor2Filtered = kalmanSensor2.updateEstimate(sensor2Value);  // Aplicar filtro Kalman
 
/*-----Cara feliz-----*/
void carafeliz() {
    byte ojos[8] = { B00000, B00000, B11111, B11111, B11111, B11111, B11111, B11111 };
    byte bocacentro[8] = { B11111, B11111, B11111, B00000, B00000, B00000, B00000, B00000 };
    byte boca2[8] = { B11111, B11111, B00000, B00000, B00000, B00000, B00000, B00000 };
    byte boca3[8] = { B00000, B00000, B00000, B00000, B00000, B00000, B00000, B11111 };
    byte boca4[8] = { B00000, B00000, B00000, B00000, B00000, B11111, B11111, B11111 };
 
    lcd.createChar(0, ojos);
    lcd.setCursor(6, 0);
    lcd.write(byte(0));
    lcd.setCursor(9, 0);
    lcd.write(byte(0));
 
    lcd.createChar(1, bocacentro);
    lcd.setCursor(7, 3);
    lcd.write(byte(1));
    lcd.setCursor(8, 3);
    lcd.write(byte(1));
 
    lcd.createChar(2, boca2);
    lcd.setCursor(6, 3);
    lcd.write(byte(2));
    lcd.setCursor(9, 3);
    lcd.write(byte(2));
 
    lcd.createChar(3, boca3);
    lcd.setCursor(6, 2);
    lcd.write(byte(3));
    lcd.setCursor(9, 2);
    lcd.write(byte(3));
 
    lcd.createChar(4, boca4);
    lcd.setCursor(5, 2);
    lcd.write(byte(4));
    lcd.setCursor(10, 2);
    lcd.write(byte(4));
}
 
/*------DFPlayer-------*/
/*void printDetail(uint8_t type, int value) {
    switch (type) {
        case TimeOut:
            Serial.println(F("Time Out!"));
            break;
        case WrongStack:
            Serial.println(F("Stack Wrong!"));
            break;
        case DFPlayerCardInserted:
            Serial.println(F("Card Inserted!"));
            break;
        case DFPlayerCardRemoved:
            Serial.println(F("Card Removed!"));
            break;
        case DFPlayerCardOnline:
            Serial.println(F("Card Online!"));
            break;
        case DFPlayerUSBInserted:
            Serial.println("USB Inserted!");
            break;
        case DFPlayerUSBRemoved:
            Serial.println("USB Removed!");
            break;
        case DFPlayerPlayFinished:
            Serial.print(F("Number:"));
            Serial.print(value);
            Serial.println(F(" Play Finished!"));
            break;
        case DFPlayerError:
            Serial.print(F("DFPlayerError:"));
            switch (value) {
                case Busy:
                    Serial.println(F("Card not found"));
                    break;
                case Sleeping:
                    Serial.println(F("Sleeping"));
                    break;
                case SerialWrongStack:
                    Serial.println(F("Get Wrong Stack"));
                    break;
                case CheckSumNotMatch:
                    Serial.println(F("Check Sum Not Match"));
                    break;
                case FileIndexOut:
                    Serial.println(F("File Index Out of Bound"));
                    break;
                case FileMismatch:
                    Serial.println(F("Cannot Find File"));
                    break;
                case Advertise:
                    Serial.println(F("In Advertise"));
                    break;
                default:
                    break;
            }
            break;
        default:
            break;
    }
}
*/
void planta(int lectura, int luz, int minhumedad, int maxhumedad, int minluz, int maxluz, int cantagu) {
    if (lectura < minhumedad) {
        digitalWrite(bomba, HIGH);
        // Cambiar cara
        //audio tengo sed
    }
    delay(cantagu);  // Funciona por diez segundos
    digitalWrite(bomba, LOW);  // Apagar bomba
    // Cambiar cara
 
    if (luz < minluz) {
        // Cambiar cara
        // Poner sonido quiero luz
       
     
    }
 
    if (sensor1Filtered < 0.5 || sensor2Filtered > 0.5) {  // Sensibilidad ajustada
       
   // Poner sonido
       
    }
}
 
 
/*-------------------------------------------------------------------------------------------*/
void setup() {
    pinMode(SENSOR_1, INPUT);  // Sensor táctil
    pinMode(SENSOR_2, INPUT);  // Sensor táctil
    pinMode(bomba, OUTPUT);    // Bomba
    lcd.init();                // LCD        
    lcd.backlight();
    carafeliz();
 
    FPSerial.begin(9600);
    Serial.begin(115200);
    Serial.println(F("DFRobot DFPlayer Mini Demo"));
    Serial.println(F("Initializing DFPlayer ... (May take 3~5 seconds)"));
 
    if (!myDFPlayer.begin(FPSerial, true, true)) {
        Serial.println(F("Unable to begin:"));
        Serial.println(F("1. Please recheck the connection!"));
        Serial.println(F("2. Please insert the SD card!"));
        while (true) {
            delay(0); // Para ESP8266 watch dog.
        }
    }
 
    Serial.println(F("DFPlayer Mini online."));
    myDFPlayer.volume(10);  // Set volume value. From 0 to 30
    myDFPlayer.play(1);     // Play the first mp3
 
    Serial.begin(9600);  // Inicializar el Monitor Serial para el sensor de luz
 
 
}
 
/*-------------------------------------------------------------------------------------------*/
void loop() {
    static unsigned long timer = millis();
    if (millis() - timer > 3000) {
        timer = millis();
        myDFPlayer.next();  // Reproducir siguiente mp3 cada 3 segundos.
    }
    if (myDFPlayer.available()) {
      //  printDetail(myDFPlayer.readType(), myDFPlayer.read());  // Manejar diferentes errores y estados del DFPlayer.
    }
 
    // Sensor de humedad
    lectura = analogRead(A0);
 
    // Sensor de luz
    int luz = analogRead(sensorLuz);
 
    // Recibir datos por Bluetooth
    if (Serial.available()) {
        datos = Serial.readString();
        Serial.println(datos);
     
        if(datos=="A1"||datos=="A2"||datos=="A3"||datos=="A4"){//interior
        minhumedad=921;
        maxhumedad=716;
         minluz=5.115 ;
        maxluz=102.3;
}
if(datos=="B1"||datos=="B2"||datos=="B3"||datos=="B4"){//suculentas y cactus
  maxhumedad=716;
  minhumedad=921;
  minluz=204.6  ;
  maxluz= 306.9;
 
}
if(datos=="C1"||datos=="C2"||datos=="C3"||datos=="C4"){//jardin
  maxhumedad=409;
  minhumedad=614;
  minluz=204.6   ;
  maxluz= 511.5;
}
if(datos=="D1"||datos=="D2"||datos=="D3"||datos=="D4"){//Tropicales
digitalWrite(bomba, HIGH);
  maxhumedad=205;
  minhumedad=409;
  minluz=  102.3 ;
  maxluz=  204.6;
}
    }
planta(lectura,luz, minhumedad,  maxhumedad,  minluz,  maxluz,  cantagu) ;
  // Liberar la memoria del arreglo dinámico
    }
 
 
 
 
