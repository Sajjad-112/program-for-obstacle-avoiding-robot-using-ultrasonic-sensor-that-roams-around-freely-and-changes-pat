# program-for-obstacle-avoiding-robot-using-ultrasonic-sensor-that-roams-around-freely-and-changes-path
int left_wheelA = 27; int left_wheelB = 26;
int right_wheelA = 25; int right_wheelB = 33;
int EN_L = 14; int EN_R = 32;
const int freq = 5000;
const int Channel_1 =0;
const int Channel_2 = 1;
const int resolution=8;
int noObstacle = HIGH;
int trigPin = 22;
int echoPin = 23;
// defines variables
long duration;
int distance;
void setup(){
pinMode(left_wheelA, OUTPUT); pinMode(left_wheelB, OUTPUT);
pinMode(right_wheelA, OUTPUT); pinMode(right_wheelB, OUTPUT);
// configure PWM functionalitites
ledcSetup(Channel_1, freq, resolution);
ledcSetup(Channel_2, freq, resolution);
// attach the channel to the GPIO to be controlled
ledcAttachPin(EN_L, Channel_1);
ledcAttachPin(EN_R, Channel_2);
delay(2000);
pinMode(Input_Pin, INPUT);
//Ultrasonic sensor
pinMode(trigPin, OUTPUT); // Sets the trigPin as an Output
pinMode(echoPin, INPUT); // Sets the echoPin as an Input
Serial.begin(9600); // Starts the serial communication
}
void loop() {
ObstacleAvoid_USS(Ultra_Sonic_Sensor());
//ObstacleAvoid_USS();}
void Drive_Far(){
digitalWrite(left_wheelA, HIGH);
digitalWrite(left_wheelB, LOW);
digitalWrite(right_wheelA, HIGH);
digitalWrite(right_wheelB, LOW);
ledcWrite(Channel_1, 130);
ledcWrite(Channel_2, 150);
}
void Drive_Back(){
digitalWrite(left_wheelA, LOW);
digitalWrite(left_wheelB, HIGH);
digitalWrite(right_wheelA, LOW);
digitalWrite(right_wheelB, HIGH);
ledcWrite(Channel_1, 140); //130 on EN_L where as 150 on EN_R makes the speed of both
motors equal
ledcWrite(Channel_2, 160);
}
void Stop(){
digitalWrite(left_wheelA, LOW);
digitalWrite(left_wheelB, LOW);
digitalWrite(right_wheelA, LOW);
digitalWrite(right_wheelB, LOW);
delay(500);
}
void Turn_Right(){
ledcWrite(Channel_1, 180); //to turn right speed of left wheel is increased
ledcWrite(Channel_2, 150);
digitalWrite(left_wheelA, HIGH);
digitalWrite(left_wheelB, LOW);
digitalWrite(right_wheelA, HIGH);digitalWrite(right_wheelB, LOW);
delay(200);
}
int Ultra_Sonic_Sensor(){
// Clears the trigPin
digitalWrite(trigPin, LOW);
delayMicroseconds(2);
// Sets the trigPin on HIGH state for 10 micro seconds
digitalWrite(trigPin, HIGH);
delayMicroseconds(10);
digitalWrite(trigPin, LOW);
// Reads the echoPin, returns the sound wave travel time in microseconds
duration = pulseIn(echoPin, HIGH);
Serial.print("Duration in ms: ");
Serial.println(duration);
// Calculating the distance
distance= duration*0.034/2;
// Prints the distance on the Serial Monitor
Serial.print("Distance in cm: ");
Serial.println(distance);
return distance;
}
void ObstacleAvoid_USS(int DISTANCE){
if(DISTANCE <10){
Serial.println("OBSTACLE !! : ");
Stop();
Drive_Back();
delay(500);
Stop();
Turn_Right();
Stop();
} //IF ends Here
else if (DISTANCE >11){
Serial.print("Clear !! ");
Drive_Far(); }
}//USS function ends h
