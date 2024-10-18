# nodemcu with nodered and ultrasonic
## installing dependencies for node-red
- install nodejs from official website
- install node-red
```
npm install -g --unsafe-perm node-red
```
- run node-red by typing the following in cmd
```
node-red
```
- go to http://127.0.0.1:1880
- click the burger menu -> Manage palette -> install
- search and install node-red-dashboard node-red-node-serialport
- close and shutdown node-red
# Nodemcu sketch
- configure adruino ide for nodemcu [link](https://www.instructables.com/Steps-to-Setup-Arduino-IDE-for-NODEMCU-ESP8266-WiF/)
```
/*********
  Rui Santos
  Complete project details at https://RandomNerdTutorials.com/esp8266-nodemcu-hc-sr04-ultrasonic-arduino/
  
  Permission is hereby granted, free of charge, to any person obtaining a copy
  of this software and associated documentation files.
  
  The above copyright notice and this permission notice shall be included in all
  copies or substantial portions of the Software.
*********/

const int trigPin = 12;
const int echoPin = 14;

//define sound velocity in cm/uS
#define SOUND_VELOCITY 0.034
#define CM_TO_INCH 0.393701

long duration;
float distanceCm;
float distanceInch;

void setup() {
  Serial.begin(115200); // Starts the serial communication
  pinMode(trigPin, OUTPUT); // Sets the trigPin as an Output
  pinMode(echoPin, INPUT); // Sets the echoPin as an Input
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
  
  // Calculate the distance
  distanceCm = duration * SOUND_VELOCITY/2;
  
  // Convert to inches
  distanceInch = distanceCm * CM_TO_INCH;
  
  // Prints the distance on the Serial Monitor
  Serial.print("Distance (cm): ");
  Serial.println(distanceCm);
//  Serial.print("Distance (inch): ");
//  Serial.println(distanceInch);
  
  delay(1000);
}
```
- compile and upload the above sketch to ardiuno uno (nodered must be shutdown while re uploading the sketch)
## creating node-red flow
- go to cmd and run "node-red" to start nodered
- go to http://127.0.0.1:1880
- from the networks drag and drop "serial in"
- doubble click the "serial in" and configure the serial port by clicking the + icon
- after clicking + give a name and select the serialport in which the arduino is connected and set the apropriate baudrate as in sketch
- then click add to complete
- Now select the created port
- then click done
- Now drag and drop a "function" node from function
- connect the serial in and function
- drag and drop a debug node
- connect function node and debug node
- now click deploy and open debug to see the output
