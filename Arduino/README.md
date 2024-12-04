# Arduino
##### Proyecto LAB I 
### Por Bianconi Clara y Ogas Avril:
En esta carpeta encontraremos el codigo que realizamos para el funcionamiento del Arduino de nuestro proyecto "Tu amiga Titi":

Tambien esta subido en su archivo .ino para ser usado en Arduino IDE.

#include <LCDI2C_Viet.h>  // Biblioteca para display LCD
#include <ROM_Standard_JP.h> //bibloteca para hacer rostros
#include "DFRobotDFPlayerMini.h"//bibloteca dfplayer
 
#define FPSerial Serial1// Configuración del DFPlayer Mini usando Serial1
DFRobotDFPlayerMini myDFPlayer;
 
// Configuración del LCD
LiquidCrystal_I2C_Viet lcd(0x27, 20, 4);  
/*------Variables-------*/
String datos = "";
int humedad = 0, minhumedad = 1500, maxhumedad = 0, maxluz = 0, minluz = 2000, luz = 0;
// Caracteres personalizados para el LCD
const byte ojos[8] = { B00000, B00000, B11111, B11111, B11111, B11111, B11111, B11111 };
const byte boca1[8] = { B00000, B00000, B00000, B00000, B00000, B11111, B11111, B11111 };
const byte boca2[8] = { B00000, B00000, B00000, B00000, B11111, B11111, B11111, B00000 };
const byte boca3[8] = { B00000, B00000, B11111, B11111, B11111, B00000, B00000, B00000 };
const byte gota[8] = { B00000, B00000, B00100, B01110, B01110, B11111, B11111, B01110 };
const byte sol1[8] = { B00001, B01000, B00101, B00011, B00011, B01001, B00010, B00100 };
const byte sol2[8] = { B00010, B00100, B11000, B11101, B11100, B11000, B00100, B10010 };
 
// Funciones para dibujar caras en el LCD
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
 
/*-----Dibujo Cara con sed-----*/
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
 
/*-----Dibujo Cara que necesita sol-----*/
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
/*------Funcion para actualizar las ncecesidades de la planta-------*/
void actualizarConfiguracion(String datos) {
    if (datos == "AA" || datos == "AB" || datos == "AC" || datos == "AD"||datos == "AE"||datos=="AX") {  //si es interior
        minhumedad = 500;
        maxhumedad = 250;
        minluz =1000 ;
        maxluz =80;
    } else if (datos == "BA" || datos == "BB" || datos == "BC" || datos == "BD"||datos == "BE"||datos=="BX") {  //si es exterior
        minhumedad = 1200;
        maxhumedad = 400;
        minluz = 800;
        maxluz = 50;
    }
      carafeliz();
}
 /*------Funcion para audios-------*/
void planta(int humedad, int luz, int minhumedad, int maxhumedad, int minluz, int maxluz, String dato) {
    if(dato=="AE"||dato=="BE"){
      if (humedad <= maxhumedad) {//si la humedad es mayor al maximo
          Serial.println("Demasiada agua");
          myDFPlayer.play(7);///audio demasiada agua
          delay(5000);
           caraSed();//cara con gota de agua
      }
      if (luz <= maxluz) {//si luz mayor al maximo
          Serial.println("Demasiada luz");
            myDFPlayer.play(9);  //audio demasiada luz
            delay(5000);
              caraSol();//cara con sol
      }
      if (luz >= minluz) {
          Serial.println("Luz menor al mínimo");//si luz menor al minimo
          myDFPlayer.play(4);//audio le falta luz
           caraSol();//cara con sol
      }
      if(humedad>=maxhumedad&&humedad<=minhumedad&&luz>=maxluz&&luz<=minluz){//si todos los parametros se encuentran entre el mim y el max
         myDFPlayer.play(6);  //estoy bien
         lcd.init();
    lcd.backlight();
          carafeliz();
      }
      dato="";//limpiamos dato
        carafeliz();
    }
}
   /*------Funcion Setup-------*/
void setup() {
    Serial2.begin(9600);  // Comunicación con el monitor serie 2
     Serial.begin(9600);  // Comunicación con el monitor serie 1
    FPSerial.begin(9600); // Comunicación con el DFPlayer Mini
 
    if (!myDFPlayer.begin(FPSerial)) { //si no se inicializo corectamente el dfplayer
        Serial.println("Error al inicializar el DFPlayer Mini. Verifica conexiones y SD.");
        while (true);
    }
    myDFPlayer.volume(30);  
    myDFPlayer.play(5);//reproduce el audio hola soy titi
   pinMode(8, INPUT);//sensor tactil
    pinMode(7, OUTPUT);//bomba  
    lcd.init();
    lcd.backlight();
    carafeliz();
}
  /*------Funcion Loop-------*/
void loop() {
   int estado=digitalRead(8);
   Serial.println(" SENSOR: ");
  Serial.println(estado);
  if (estado==1) {//si el valor es mayor a 0.04
    Serial.println(" SENSOR activado.");
      myDFPlayer.play(10);  //audio risa
      delay(7000);
  }
    delay(700);
    humedad = analogRead(A0);//sesnor de humedad
    luz = analogRead(A1);//sebsor de luz
 
         Serial.println("Estado planta:");
        Serial.print("Humedad: ");
        Serial.println(humedad);
        Serial.print(F("Luz: "));
        Serial.println(luz);
   
  if (Serial2.available() > 0) {  // Verifica que haya datos
          Serial.println("Datos detectados en Serial2...");
           datos = Serial2.readString();//lee el dato y lo guarda en datos
          datos.trim();//saca espacios
           Serial.print("Datos recibidos: ");
          Serial.println(datos);  // Muestra los datos recibidos
 
              if (datos=="AA"||datos=="BA"){
                myDFPlayer.play(3);//reproduce audio hola
              }
              if (datos=="AB"||datos=="BB"){
              myDFPlayer.play(2);//reproduce audio chau
                }
            if (datos=="AD"||datos=="BD"){
              myDFPlayer.play(1);//reproduce audio buenos dias
              }
               if (datos=="AC"||datos=="BC"){
              myDFPlayer.play(5);//reproduce audio titi
              }
              actualizarConfiguracion(datos);
                planta(humedad, luz,  minhumedad,  maxhumedad,  minluz, maxluz, datos);
    }
 
    if(datos!=""){
    actualizarConfiguracion(datos);
   }
   Serial.println(minhumedad);
     if (humedad >= minhumedad) {//si la humedad es menor al minimo se prende la bomba
        digitalWrite(7, HIGH);
        Serial.println("Bomba prendida, humedad menor al mínimo");
        delay(7000);//se prende durante 7000ms
        digitalWrite(7, LOW);
         carafeliz();
    }
    delay(1000);
      carafeliz();
}
