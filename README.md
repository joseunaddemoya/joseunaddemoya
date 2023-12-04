//Librerias
#include <SoftwareSerial.h>
#include <LiquidCrystal.h>      //libreria para la LCD
#include <Wire.h>               //libreria para comunicar dispositivos mediante el protocolo I2C/TWI
#include <DHT.h>                //libreria para el sensor de humedad DTH11
#include <DHT_U.h>              //libreria para el sensor de humedad

//Declaracion de las variables
//Variables Humedad y temperatura (DTH11)
DHT dhtExt(9, DHT11);          //Humedad Exterior y temperatura exterior se le asigna el pin 9
DHT dhtInt(8, DHT11);          //Humedad Interior y temperatura interior se le asigna el pin 8
int R_Humedad;                 //Humedad Relativa

//Variables del sensor de humedad del suelo
int valor;
int parametro_humedad=600;

//Variables LCD
const int rs = 12, en = 11, d4 = 5, d5 = 4, d6 = 3, d7 = 2;
LiquidCrystal lcd(rs, en, d4, d5, d6, d7);

void setup() {
//En el void setup se declaran las salidas y las entradas del sistema
dhtExt.begin();                 //Se inicia el sensor de humedad exterior
dhtInt.begin();                 //Se inicia el sensor de humedad Interior
lcd.begin(16, 2);               //Se define el tamaño de la LCD
Serial.begin(9600);             //Frecuencia del puerto serial
Serial.write(12);
}

void loop() {
//En el void loop se escribe lo que se va ejecutar en el ciclo
//Sensor de Humedad y temperatura DTH11
float Humedad_Ext=dhtExt.readHumidity();  //lee el valor del sensor de humedad exterior
float Humedad_Int=dhtInt.readHumidity();  //lee el valor del sensor de humedad interior
R_Humedad=(Humedad_Ext-Humedad_Int);      //formula de relacion de la humedad exterior e interior
float temperature_Ext=dhtExt.readTemperature();
float temperature_Int=dhtInt.readTemperature();
lcd.clear();                             //Imprimir en la LCD los resultados
lcd.setCursor(0,0);                      //Posicion de la LCD donde se imprime el nombre
lcd.print("H_Int:");                     //Nombre a imprimir
lcd.setCursor(8,0);                      //Posicion de la LCD donde se imprime el valor
lcd.print(Humedad_Int);                  //Nombre de la variable a imprimir
delay(500);                              //Pausa
Serial.println("Humedad_Int:");          //Imprimir en el puerto serial los resultados de la temperatura
Serial.println(Humedad_Int);             //Variable a imprimir
lcd.clear();
lcd.setCursor(0,1);
lcd.print("H_Ext:");
lcd.setCursor(8,1);
lcd.print(Humedad_Ext);
delay(500);
Serial.println("Humedad_Ext:");         //Impresion puerto serial    
Serial.println(Humedad_Ext);
lcd.clear();
lcd.setCursor(0,0);
lcd.print("Humedad_Relativa:");
lcd.setCursor(0,4);
lcd.print(R_Humedad);
delay(500);
Serial.println("R_Humedad:");           //Impresion puerto serial      
Serial.println(R_Humedad);
Serial.println("Temperatura_Int:");     //Imprimir en el puerto serial los resultados de la temperatura
Serial.println(temperature_Int);        //Variable a imprimir
Serial.println("Temperatura_Ext:");     //Imprimir en el puerto serial los resultados de la temperatura
Serial.println(temperature_Ext);        //Variable a imprimir
//
//Sensor Humedad del suelo
valor=analogRead(A0);
Serial.println("Humedaddelsuelo:"); //Impresion puerto serial  
Serial.println(valor);
delay(1000);
Serial.write(12);
lcd.clear();              //Imprimir en la LCD los resultados y luego de la pausa borre para mostrar el siguiente valor
lcd.setCursor(0,0);       //Posicion de la LCD donde se imprime el nombre
lcd.print("Hum_Suelo:");  //Nombre a imprimir de la humedad del suelo
lcd.setCursor(3,0);       //Posicion de la LCD donde se imprime el valor
lcd.print(valor);         //Nombre de la variable a imprimir
delay(500);               //Pausa
}
