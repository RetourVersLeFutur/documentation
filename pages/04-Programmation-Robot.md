<h3>Code Source Arduino commenté en Français</h3>

```c
#include <Servo.h>
#include <SoftwareSerial.h>
// Définition des broches RX et TX pour la communication avec le module Bluetooth HC-06
#define rxpin 2 // Broche 2 en tant que Rxpin(réception), à raccorder sur Transmission du HC-06
#define txpin 4 // Broche 4 en tant que Transmission, à raccorder sur Reception du HC-06

// Déclaration des objets Servo pour chaque servo moteur
Servo myServo0;
Servo myServo1;
Servo myServo2;

// Création d'un objet SoftwareSerial pour communiquer avec le module Bluetooth
SoftwareSerial mySerial (rxpin, txpin);

void setup()
{ 
    // Définition des modes de broches pour les broches RX et TX
    pinMode(rxpin, INPUT);
    pinMode(txpin, OUTPUT);
    // Initialisation de la communication série avec le moniteur série et le module Bluetooth
    mySerial.begin (9600); // Set the debit
    Serial.begin (9600); 
    // Attachez chaque servo à une broche PWM
    myServo0.attach(9);// Broche du servo0
    myServo1.attach(10);// Broche du servo1
    myServo2.attach(11);// Broche du servo2
    myServo0.write(0);
    myServo1.write(0);
    myServo2.write(0);
}

void loop()
{
    char receivedChar[2] = {0};
    byte i = 0;
    // Réception de données depuis le module Bluetooth
    while (mySerial.available())
    {
      receivedChar[i] = mySerial.read();
      i++;
      delay(5);  
    }

    /*
    Serial.print("Données reçues depuis le module Bluetooth: ");
    Serial.println((char)receivedChar);
    */

    if(i == 2)
    {
        switch(receivedChar[0])
        {
          case '0':
            myServo0.write(receivedChar[1]);
            break;

          case '1':
            myServo1.write(receivedChar[2]);
            break;

          case '2':
            myServo2.write(receivedChar[3]);
            break;
        }
    }

    int analogValue = analogRead(A0); // Initialise le potentiomètre A0
    int angle = map(analogValue, 0, 1023, 0, 180); // Initialise le "manager" pour Servo0

    myServo0.write(angle); // Initialise la position de  Servo0

    analogValue = analogRead(A1); // Initialise le potentiomètre A1
    angle = map(analogValue, 0, 1023, 0, 180); // Initialise le "manager" pour Servo1

    myServo1.write(angle); // Initialise la position de Servo1

    analogValue = analogRead(A2); // Initialise le potentiomètre A2
    angle = map(analogValue, 0, 1023, 0, 180); // Initialise le "manager" pour Servo2

    myServo2.write(angle); // Initialise la position de Servo2
  
    delay(15);
}  
```
