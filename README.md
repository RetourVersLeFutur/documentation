<h1 align="left">Documentation Hackathon Retour vers le Futur</h1>

<!-- DEBUT - MENU SOMMAIRE -->
<!-- INTRODUCTION -->
<h3>Introduction</h3>
<ul>
  <li><a href="./pages/01-Introduction.md#objectif-du-projet">Objectif du Projet.</a></li>
  <li><a href="./pages/01-Introduction.md#contexte-du-hackathon">Contexte du Hackathon.</a></li>
</ul>

<!-- PRESENTATION DU ROBOT -->
<h3>Présentation du Robot</h3>
<ul>
  <li><a href="./pages/02-Présentation-Robot.md#composants-matériels-utilisés">Composants Matériels Utilisés.</a></li>
  <li><a href="./pages/02-Présentation-Robot.md#schémas-ou-images-de-la-disposition-des-composants">Schémas ou Images de la Disposition des Composants.</a></li>
</ul>

<!-- PROGRAMMATION DU ROBOT -->
<h3>Programmation du Robot</h3>
<ul>
  <li><a href="./pages/04-Programmation-Robot.md#">Code Source Arduino Commenté.</a></li>
</ul>

<!-- RESSOURCES SUPPLEMENTAIRES -->
<h3>Ressources Supplémentaires</h3>
<ul>
  <li><a href="./pages/07-Ressources-Supplémentaires.md">Liens vers des Tutoriels, Articles ou Bibliothèques Utilisées.</a></li>
</ul>
<!-- FIN  - MENU SOMMAIRE -->

<hr>

<!-- DEBUT - CONTENU INTRODUCTION -->
<h3>Objectif du Projet</h3>
<p>Le projet vise à concevoir et à créer un robot contrôlé par un Arduino Uno, trois servo-moteurs et trois potentiomètres.<br>
L'objectif principal est de permettre aux participants de concevoir et de développer un robot capable d'exécuter des mouvements précis et contrôlés à l'aide des potentiomètres pour ajuster les positions des servo-moteurs.</p>

<h3>Contexte du Hackathon</h3>
<ul>
  <li>Explorer les capacités de l'Arduino Uno en matière de contrôle robotique.</li>
  <li>Encourager l'innovation et la créativité dans la conception et la fonctionnalité du robot.</li>
  <li>Expérimenter avec le contrôle de mouvements en utilisant des servo-moteurs et des potentiomètres.</li>
</ul>
<!-- FIN - CONTENU INTRODUCTION -->

<hr>

<!-- DEBUT - PRESENTATION DU ROBOT -->
<h3>Composants Matériels Utilisés</h3>
<ul>
  <li>1 Arduino UNO.</li>
  <li>3 Servo Moteurs.</li>
  <li>3 Potentiomètres.</li>
  <li>1 PowerBank (Batterie Externe).</li>
  <li>1 BreadBord (Platine d'expérimentation).</li>
</ul>

<h3>Schémas ou Images de la Disposition des Composants</h3>
<p>Les schémas ou images de la disposition des composants sont les suivants,</p>
<img height="455px" width="940px" src="https://github.com/RetourVersLeFutur/documentation/assets/85597175/04783eca-ce7a-4364-b0eb-6ef43f73986a"/>
<!-- FIN - PRESENTATION DU ROBOT -->

<hr>

<!-- DEBUT - CODE SOURCE ARDUINO COMMENTE -->
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
<!-- FIN - CODE SOURCE ARDUINO COMMENTE -->

<hr>