# TITTLE: IOT_BASED_ACCIDENT_ALERTER

## ABSTRACT:

This abstract encapsulates the essence of an IoT-based mini project designed to address the critical need for real-time accident alert systems in India's daily life. The project leverages Arduino in Proteus to create an innovative solution for timely accident detection and notification, aiming to enhance safety and response mechanisms on roads and in public spaces.In India, where the intensity of road traffic and the prevalence of accidents remain concerning, there exists a pronounced necessity for proactive safety measures. This project endeavors to fill this gap by employing IoT technology and Arduino integration to develop an Accident Alerter System. The system functions by utilizing various sensors such as accelerometers, GPS modules, and GSM modules to detect sudden impacts or collisions in vehicles or public areas. Upon detecting an accident, the system rapidly transmits alerts to predefined emergency contacts and relevant authorities, ensuring swift and effective response times.

## INTRODUCTION:

Introducing the innovative Accident Detection System using Arduino and various sensors, this project aims to revolutionize safety measures in vehicles and public spaces. By integrating vibration sensors, ultrasonic modules, and GPS technology, the system swiftly identifies potential accidents, promptly alerting authorities via GSM communication. Its multifaceted approach not only detects collisions but also provides precise location details, enhancing emergency response efficiency. This project embodies a proactive solution, promising to significantly mitigate the impact of accidents and bolster safety across diverse environments in real-time.

## LITERATURE REVIEW:)
The papers collectively propose IoT-based accident detection systems that aim to improve response time and save lives. Y 2023 presents a real-time accident detection system using IoT components like a microcontroller, GPS, gyroscope, and vibration sensor. The system detects accidents, collects data, and sends it to emergency services through cloud servers. [Puthur 2019](https://api.semanticscholar.org/CorpusID:213998098) focuses on accident detection and prevention, providing real-time tracking and immediate notification to closed ones and authorities. [Bergonda 2017](https://api.semanticscholar.org/CorpusID:202725943) introduces an accident detection system that sends location information via Wi-Fi to a mobile number. [Reddy 2023](https://ieeexplore.ieee.org/document/10127125) proposes an IoT-based accidental detection system that automatically informs the nearest hospital about accidents.

## PROPOSED METHODLOGY:
The main principle of the project is the detection and rescue management. The system
is on and initialization. If vehicle is normal, no messages has been sent to rescue team.
And the location of the driver is monitored in all the time, if it reaches the
threshold level then the action has been taken automatically. Whenever accident
occurred, the  Vibration sensor and UltraSonic sensor detects the accident happened
with vehicle. The controller get the input from sensors and send the accident alert
information to road side unit and then message is send to the rescue team and also
WIFI and GPS finds location of the vehicle and that also send to the rescue team. It
will facilitate connectivity to the nearest hospital and provide medical help through
IOT technology

## CIRCUIT DIAGRAM:

![Initial status](https://github.com/naveenkumar12624/Simulation-project/assets/93427235/7813dfee-2100-4912-87c9-2b27dcb80a3d)

## PROGRAM:
```c
#include <LiquidCrystal.h>
#include <TinyGPS.h>
LiquidCrystal lcd(4, 5, 6, 7, 8, 9);
const int relay_Pin = 2;
int triggerPin =10;
int echoPin = 11;
const int vibration_Sensor = 12;
TinyGPS gps;
long lat,lon;
bool vibration_Status = LOW;
long duration, distance;
void setup() {
  pinMode(relay_Pin, OUTPUT);
  pinMode(vibration_Sensor, INPUT);
  pinMode(triggerPin, OUTPUT);
  pinMode(echoPin, INPUT);
  Serial.begin(9600);
  lcd.begin(16, 2);
  lcd.clear();
  lcd.print("ACCIDENT DETECTION");
  lcd.setCursor(3,2);
  lcd.print("SYSTEM");
  delay(500);  
}
void loop() {   
       digitalWrite(relay_Pin, HIGH);   
        delay(200);  
        lcd.clear();
        lcd.print("Vehicle Started");
        delay(500);     
        while(1)
        {   
          vibration_Status = digitalRead(vibration_Sensor);
          delay(100);   
          ultrasonic();
          delay(100); 
          if((vibration_Status == HIGH )  && (distance < 5))
          {
           delay(100);
           digitalWrite(relay_Pin, LOW);  
           delay(100);
           lcd.clear();
           lcd.print("Accident Detected");
           lcd.setCursor(3,2);
           lcd.print("Sending Msg");
           delay(500);    
           Serial.println("AT+CMGF=1");    //Sets the GSM Module in Text Mode
           delay(100);  // Delay of 1000 milli seconds or 1 second
           Serial.println("AT+CMGS=\"+919922512017\"\r"); // Replace x with mobile number
           delay(100);
           Serial.println("Accident Detected ");// The SMS text you want to send
           Serial.println("please check location");// The SMS text you want to send
           while(1)
           {
            gps_read();

           }
          }
}
}

void gps_read()
{ 
 byte a;
  
  if(Serial.available())  
  {
    a=Serial.read();
   
   //Serial.write(a);
   
    while(gps.encode(a))   // encode gps data 
    { 
      gps.get_position(&lat,&lon); // get latitude and longitude
    
      Serial.println("Position: ");
      Serial.print("lat:");
      Serial.println((lat*0.000001),8);
      Serial.print("log:");
      Serial.println((lon*0.000001),8);
    }
  }
}

void ultrasonic()
{ // variable to hold the duration and distance value for HC-SR04
  digitalWrite(triggerPin, LOW); // Write trigger pin is as low
  delayMicroseconds(2); // Delay for 2microseconds
  digitalWrite(triggerPin, HIGH); //Write trigger pin is as high
  delayMicroseconds(10);// Delay for 10microseconds
  digitalWrite(triggerPin, LOW); //Write trigger pin is as low
  duration = pulseIn(echoPin, HIGH);//Read the echo pin
  distance = (duration / 2) / 29.1; // calculate the distance
}
```

## RESULTS:

![Screenshot 2023-11-16 093901](https://github.com/naveenkumar12624/Simulation-project/assets/93427235/e3398030-da89-4f05-8089-4cd2a0bed382)

## CONCLUSION:

In conclusion, the development of an Accident Detection System using IoT and Arduino represents a significant leap towards enhancing road safety and emergency response mechanisms. This innovative system, designed to detect accidents and manage rescue operations, operates dynamically based on vehicle status and location monitoring.

By integrating Vibration and Ultrasonic sensors, coupled with GPS and communication technologies, the system efficiently identifies accidents and promptly notifies rescue teams and roadside units. The continuous monitoring of the driver's location ensures proactive measures are taken if predefined thresholds are exceeded, contributing to a proactive safety approach.

The successful amalgamation of IoT technologies facilitates swift connectivity with emergency services, aiding in rapid medical assistance through established links with nearby hospitals. This comprehensive system aims to minimize response time during accidents, potentially saving lives and mitigating the severity of injuries.

## REFERENCE:

[A Comprehensive Study on IoT Based Accident Detection Systems for Smart VehiclesPublisher: IEEE](https://ieeexplore.ieee.org/document/9133106)
[A Comprehensive Study on IoT Based Accident Detection Systems for Smart VehiclesAuthors:Unaiza AlviNational University of Sciences and TechnologyMuazzam A. Khan KhattakQuaid-i-Azam UniversityBalawal ShabirAsal MalikIntelligent Systems Center](https://www.researchgate.net/publication/342685590_A_Comprehensive_Study_on_IoT_Based_Accident_Detection_Systems_for_Smart_Vehicles)
[A Framework and IoT-Based Accident Detection System to Securely Report an Accident and the Driver’s Private Informationby Amal Hussain Alkhaiwani and Badr Soliman Alsamani](https://www.mdpi.com/2071-1050/15/10/8314)

