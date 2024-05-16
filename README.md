𝗥𝗼𝗯𝗼𝘁 𝗠𝗼𝘃𝗲𝗺𝗲𝗻𝘁 𝗦𝗼𝘂𝗿𝗰𝗲 𝗖𝗼𝗱𝗲:

#include "Arduino.h"

#include "SoftwareSerial.h"

#include "DFRobotDFPlayerMini.h"

SoftwareSerial mySoftwareSerial(10, 11);

DFRobotDFPlayerMini myDFPlayer;

int timer2_counter, Timer_Second;

void printDetail(uint8_t type, int value);

const int trigPin = 4;

const int echoPin = 3;

long duration;

int distance;

volatile unsigned int distance1;

String data = "";

int flag = 0;

int obstacle_detected=0;

void setup() {

pinMode(5, INPUT); //IR SENSOR

pinMode(trigPin, OUTPUT); // Sets the trigPin as an Output

pinMode(echoPin, INPUT); // Sets the echoPin as an Input

pinMode(6, OUTPUT);

pinMode(7, OUTPUT);

pinMode(8, OUTPUT);

pinMode(9, OUTPUT);

mySoftwareSerial.begin(9600);

Serial.begin(9600);

Serial.println();

Serial.println(F("DFRobot DFPlayer Mini Demo"));

Serial.println(F("Initializing DFPlayer ... (May take 3~5 seconds)"));

if (!myDFPlayer.begin(mySoftwareSerial)) { //Use softwareSerial to

communicate with mp3.

Serial.println(F("Unable to begin:"));

Serial.println(F("1.Please recheck the connection!"));

Serial.println(F("2.Please insert the SD card!"));

while (true) ; }

Serial.println(F("DFPlayer Mini online."));

myDFPlayer.volume(30); //Set volume value. From 0 to 30

myDFPlayer.play(1); //Play the first mp3

noInterrupts();

TCCR2A = 0;

TCCR2B |= (1 << CS22) | (1 << CS21) | (1 << CS20);

TCNT2 = timer2_counter;

TIMSK2 |= (1 << TOIE2);

timer2_counter = 240;

interrupts();}

void loop() {

while (Serial.available()) {

data = Serial.readString();

Serial.println(data);}

if ((data == "hello") && (flag == 1)) {

data = "";

myDFPlayer.play(2);

delay(5000); }

if ((data == "canteen") && (flag == 1)) {

data = "";

myDFPlayer.play(4);

forward();

delay(3000);

myDFPlayer.play(5);

right();

delay(3000);

Stop();

myDFPlayer.play(7);

delay(6000);

myDFPlayer.play(9);

delay(3000); }

if ((data == "admin block") && (flag == 1)) {

data = "";

myDFPlayer.play(4);

forward();

delay(3000);

Stop();

myDFPlayer.play(3);

delay(6000);

myDFPlayer.play(9);

delay(3000); }

if ((data == "ECE department") && (flag == 1)) {

data = "";

myDFPlayer.play(4);

forward();

delay(3000);

myDFPlayer.play(5);

right();

delay(3000);

myDFPlayer.play(4);

forward();

delay(3000);

myDFPlayer.play(5);

right();

delay(3000);

Stop();

myDFPlayer.play(6);

delay(6000);

myDFPlayer.play(9);

delay(3000);}

if (digitalRead(5) == LOW) {

// Serial.println("ok");

flag = 1;

}

if (obstacle_detected==1) {

Stop();

myDFPlayer.play(10);

Serial.println("okkkk");

// digitalWrite(6, HIGH);

delay(2000);

obstacle_detected=0;

//digitalWrite(6, LOW);}}

void forward() {

digitalWrite(6, LOW);

digitalWrite(7, HIGH);

digitalWrite(8, LOW);

digitalWrite(9, HIGH);}

void back() {

digitalWrite(6, HIGH);

digitalWrite(7, LOW);

digitalWrite(8, HIGH);

digitalWrite(9, LOW);

delay(1000);}

void right() {

digitalWrite(6, LOW);

digitalWrite(7, HIGH);

digitalWrite(8, HIGH);

digitalWrite(9, LOW);

delay(1000);}

void left() {

digitalWrite(6, HIGH);

digitalWrite(7, LOW);

digitalWrite(8, LOW);

digitalWrite(9, HIGH);

delay(1000);}

void Stop() {

digitalWrite(6, LOW);

digitalWrite(7, LOW);

digitalWrite(8, LOW);

digitalWrite(9, LOW);}

void US() {

digitalWrite(trigPin, LOW);

delayMicroseconds(2);

digitalWrite(trigPin, HIGH);

delayMicroseconds(10);

digitalWrite(trigPin, LOW);

duration = pulseIn(echoPin, HIGH);

distance = duration * 0.034 / 2;

// Serial.println(distance);

if(distance <15) {

obstacle_detected=1;

distance1=distance; }}

ISR(TIMER2_OVF_vect) {

TCNT2 = timer2_counter;

Timer_Second++;

if (Timer_Second > 2000) {

Timer_Second = 0;

US(); }}


𝗟𝗶𝗗𝗔𝗥 𝗜𝗻𝘁𝗲𝗴𝗿𝗮𝘁𝗶𝗼𝗻 𝗦𝗼𝘂𝗿𝗰𝗲 𝗖𝗼𝗱𝗲:

import pylidar # Assuming you're using pylidar for LiDAR integration

import speech_recognition as sr

# Initialize LiDAR and speech recognition

lidar = pylidar.initialize()

r = sr.Recognizer()

while True:

#Get LiDAR data and process it (replace with your specific processing logic)

scan_data lidar.get_data()

# (environment mapping, obstacle detection, etc.)

#Listen for user commands

with sr.Microphone() as source:

print("Listening...")

audio r.listen(source)

try:

#Recognize speech and extract command

command = r.recognize_google(audio)

print("You said:", command)

#Handle the command (replace with robot-specific control logic)

if command "go forward":

#Call robot control library function to move forward.

elif command "turn left":

#Call robot control library function to turn left

#... (add more commands as needed)

except sr.UnknownValueError:

print("Could not understand audio")

except sr.RequestError as e:

print("Could not request results from Google Speech Recognition")
