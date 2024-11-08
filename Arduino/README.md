# Arduino
##### Proyecto LAB I 
### Por Bianconi Clara y Ogas Avril:
En esta carpeta encontraremos el codigo que realizamos para el funcionamiento del Arduino de nuestro proyecto "Tu amiga Titi":

## CODIGO SIN TERMINAR 08/11/2024

#include <LCDI2C_Viet.h>  // Biblioteca para display
#include <ROM_Standard_JP.h>  
#include <SoftwareSerial.h>
#include "DFRobotDFPlayerMini.h"//bibloteca dfplayer
 
 //dfplayer
#if (defined(ARDUINO_AVR_UNO) || defined(ESP8266))   // Using a soft serial port
SoftwareSerial softSerial(/rx =/4, /tx =/5);
#define FPSerial softSerial
/*
#else
#define FPSerial Serial1*/
#endif
 
 
LiquidCrystal_I2C_Viet lcd(0x27, 20, 4);//pantalla
#define SENSOR_1 8  // Sensor táctil 1
#define SENSOR_2 9  // Sensor táctil 2
#define sensorLuz A1 // Sensor luz
 
DFRobotDFPlayerMini myDFPlayer;
void printDetail(uint8_t type, int value);//mostrar los detalles del dfplayer
 
/------VARIABLES-------/
//const int sensorLuz = A1;
String datos = "";
int lectura = 0, minhumedad = 0, maxhumedad = 0, maxluz = 0, minluz = 0,cantagu=0, pinBuzzer = 6, bomba = 7, sensorHumedad = A0,sensorPin = 8,sensorPin2 = 9, estado,estado2;
 
 
//-------BYTES PARA LAS CARAS--------
 
//Caracteres ojos
byte ojos[8] = { B00000, B00000, B11111, B11111, B11111, B11111, B11111, B11111};
 
//Caracteres boca
byte boca1[8] = { B00000, B00000, B00000, B00000, B00000, B11111, B11111, B11111 };
byte boca2[8] = { B00000, B00000, B00000, B00000, B11111, B11111, B11111, B00000  };
byte boca3[8] = { B00000, B00000, B11111, B11111, B11111, B00000, B00000, B00000 };
 
//Caracteres carita sed
 
byte gota[8] = { B00000, B00000, B00100, B01110, B01110, B11111, B11111, B01110 };
 
//Caracteres carita sol
 
byte sol1[8] = { B00001, B01000, B00101, B00011, B00011, B01001, B00010, B00100 };
byte sol2[8] = { B00010, B00100, B11000, B11101, B11100, B11000, B00100, B10010  };
 
//FUNCIONES PARA DIBUJAR EN EL LCD
/-----Dibujo Cara feliz-----/
void carafeliz() {
     
    lcd.createChar(0, ojos);
    lcd.setCursor(6, 0);
    lcd.write(byte(0));
    lcd.setCursor(9, 0);
    lcd.write(byte(0));
 
    lcd.createChar(1, boca1);
    lcd.setCursor(7, 2);
    lcd.write(byte(1));
    lcd.setCursor(8, 2);
    lcd.write(byte(1));
 
    lcd.createChar(2, boca2);
    lcd.setCursor(6, 2);
    lcd.write(byte(2));
    lcd.setCursor(9, 2);
    lcd.write(byte(2));
   
    lcd.createChar(4, boca3);
    lcd.setCursor(5, 2);
    lcd.write(byte(4));
    lcd.setCursor(10, 2);
    lcd.write(byte(4));
}
 
/-----Dibujo Cara con sed-----/
void caraSed(){
    lcd.createChar(0, ojos);
    lcd.setCursor(6, 0);
    lcd.write(byte(0));
    lcd.setCursor(9, 0);
    lcd.write(byte(0));
 
    lcd.createChar(1, boca1);
    lcd.setCursor(5, 2);
    lcd.write(byte(1));
    lcd.setCursor(10, 2);
    lcd.write(byte(1));
 
    lcd.createChar(2, boca2);
    lcd.setCursor(6, 2);
    lcd.write(byte(2));
    lcd.setCursor(9, 2);
    lcd.write(byte(2));
   
    lcd.createChar(4, boca3);
    lcd.setCursor(7, 2);
    lcd.write(byte(4));
    lcd.setCursor(8, 2);
    lcd.write(byte(4));
 
    lcd.createChar(5, gota);
    lcd.setCursor(12, 1);
    lcd.write(byte(5));
   
}
 
/-----Dibujo Cara que necesita sol-----/
void caraSol(){
    lcd.createChar(0, ojos);
    lcd.setCursor(6, 0);
    lcd.write(byte(0));
    lcd.setCursor(9, 0);
    lcd.write(byte(0));
 
    lcd.createChar(1, boca1);
    lcd.setCursor(6, 2);
    lcd.write(byte(1));
    lcd.setCursor(7, 2);
    lcd.write(byte(1));
    lcd.setCursor(8, 2);
    lcd.write(byte(1));
    lcd.setCursor(9, 2);
    lcd.write(byte(1));
 
    lcd.createChar(6, sol1);
    lcd.setCursor(3, 1);
    lcd.write(byte(6));
 
    lcd.createChar(7, sol2);
    lcd.setCursor(4, 1);
    lcd.write(byte(7));
}
 
/------DFPlayer-------/
void printDetail(uint8_t type, int value) {
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
 
void planta(int lectura, int luz, int minhumedad, int maxhumedad, int minluz, int maxluz, int cantagu) {
    if (lectura <= minhumedad) {
        digitalWrite(bomba, HIGH);
        Serial.println("Bomba prendida, humedad menos a la minimo");
        // Cambiar cara
        //audio tengo sed
    }
    delay(cantagu);  // Funciona por diez segundos
    digitalWrite(bomba, LOW);  // Apagar bomba
    // Cambiar cara
 
    if (luz < minluz) {
        // Cambiar cara
        // Poner sonido quiero luz
                Serial.println(" luz menos a la minimo");
     
    }
 
    if (estado==0 || estado2==1) {  
               Serial.println("Sensor anda");
   // Poner sonido
       
    }
}
 
 
/-------------------------------------------------------------------------------------------/
void setup() {
    pinMode(sensorPin, INPUT);
    pinMode(sensorPin2, INPUT);
    pinMode(bomba, OUTPUT);    // Bomba
    lcd.init();                // LCD        
    lcd.backlight();
    carafeliz();
 
    FPSerial.begin(9600);
    Serial.begin(9600);
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
 
/-------------------------------------------------------------------------------------------/
void loop() {
    static unsigned long timer = millis();
    
    if (millis() - timer > 3000) {
        timer = millis();
        myDFPlayer.next();  // Reproduce siguiente mp3 cada 3 segundos.
    }
    
    if (myDFPlayer.available()) {
        printDetail(myDFPlayer.readType(), myDFPlayer.read());  // Maneja errores del DFPlayer.
    }
    
    // Lectura de sensores
    lectura = analogRead(sensorHumedad);
    int luz = analogRead(sensorLuz);
    
    // Bluetooth / datos
    if (Serial.available()) {
        datos = Serial.readString();
        datos.trim();
        Serial.print("Datos recibidos: ");
        Serial.println(datos);
        
        if (datos == "A1" || datos == "A2" || datos == "A3" || datos == "A4") {
            Serial.println("ENTRO en Interior");
            minhumedad = 921;
            maxhumedad = 716;
            minluz = 5;
            maxluz = 102;
        } else if (datos == "B1" || datos == "B2" || datos == "B3" || datos == "B4") {
            Serial.println("ENTRO en Suculentas y cactus");
            minhumedad = 921;
            maxhumedad = 716;
            minluz = 204;
            maxluz = 306;
        } else if (datos == "C1" || datos == "C2" || datos == "C3" || datos == "C4") {
            Serial.println("ENTRO en Jardín");
            minhumedad = 614;
            maxhumedad = 409;
            minluz = 204;
            maxluz = 511;
        } else if (datos == "D1" || datos == "D2" || datos == "D3" || datos == "D4") {
            Serial.println("ENTRO en Tropicales");
            minhumedad = 409;
            maxhumedad = 205;
            minluz = 102;
            maxluz = 204;
        } else {
            Serial.println("No se reconoció el comando recibido.");
        }
        
        datos = "";  // Limpiar datos
    }
    
    // Llama a la función planta
    planta(lectura, luz, minhumedad, maxhumedad, minluz, maxluz, cantagu);
}

 
 
 
