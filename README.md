# ardiuno
#include <LiquidCrystal.h> 
#include "DHT.h"
#define DHTTYPE DHT11
#define DHTPIN 8
LiquidCrystal lcd(12, 11, 5, 4, 3, 2); 
DHT dht(DHTPIN, DHTTYPE); 
  
int ir=6;
int outputpin=13;
int pir=7;
const int sensorPin = 0; //analog pin
 int lightCal; 
int lightVal; 
int smokeA0 = A5; // Your threshold value 
int sensorThres = 400; 

void setup()
{
    pinMode(9, OUTPUT);//beep sound
    pinMode(outputpin,OUTPUT);//led
pinMode(10,OUTPUT);//beep2
    pinMode(ir,INPUT);//ir
     pinMode(smokeA0, INPUT); //analog
pinMode(pir,INPUT);
  lightCal = analogRead(sensorPin); 
  lcd.begin(16, 2); 
    dht.begin(); // Print a message to the LCD. 
    lcd.print("Temp: Humidity:");
 dht.begin();
 Serial.begin(9600);
} 
void loop()
{
        int state=0;
 int analogSensor = analogRead(smokeA0); Serial.print("Pin A0: "); Serial.println(analogSensor); // Checks if it has reached the threshold value 
lightVal = analogRead(sensorPin); 
    //adjust its value to 4.7k ohms.
int sensorvalue=digitalRead(ir);
int pirsen=digitalRead(pir);
    Serial.print("sesor pin value:");
    Serial.println(sensorvalue);
 Serial.println(pirsen);
      if(Serial.available()>0){
    state=Serial.read();
        }
    
      if(sensorvalue==LOW){
        digitalWrite(outputpin,HIGH);
    }
    else
    {
        digitalWrite(outputpin,LOW);        
    }
 if(pirsen==LOW)
        {
         noTone(9);
    }
    else
    {
           tone(9, 400);
        noTone(9);
        tone(9,400);  
            }  
    if (analogSensor > sensorThres) 
        { 
         noTone(9);
        tone(9, 400);
noTone(9);
tone(9,400);        
         } else
     { 
         noTone(9);
        }     
lcd.setCursor(0, 5); 
    // read humidity 
    float hh = dht.readHumidity(); //read temperature in Fahrenheit 
    float ff = dht.readTemperature(false); 
       
    if( state=='1'){
    if (isnan(hh) || isnan(ff))
     { 
        lcd.print("ERROR"); 
        return;
         }
    lcd.print(ff); 
    lcd.setCursor(7,1); 
    lcd.print(hh); 
         Serial.print(ff); 
   Serial.print(hh);
            }
        else
    {
        if(state==0){
            lcd.print("shutdowning");
             }
        }
}
          


