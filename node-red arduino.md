# Node-red using arduino and ultrasonic
## installing dependencies
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
## Aurdino sketch
```
const int pingPin = D0; // Trigger Pin of Ultrasonic Sensor
const int echoPin = D1; // Echo Pin of Ultrasonic Sensor

void setup() {
   Serial.begin(9600); // Starting Serial Terminal
}

void loop() {
   long duration, inches, cm;
   pinMode(pingPin, OUTPUT);
   digitalWrite(pingPin, LOW);
   delayMicroseconds(2);
   digitalWrite(pingPin, HIGH);
   delayMicroseconds(10);
   digitalWrite(pingPin, LOW);
   pinMode(echoPin, INPUT);
   duration = pulseIn(echoPin, HIGH);
   cm = microsecondsToCentimeters(duration);
   Serial.print(cm);
   Serial.print("cm");
   Serial.println();
   delay(1000);
}

long microsecondsToCentimeters(long microseconds) {
   return microseconds / 29 / 2;
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
