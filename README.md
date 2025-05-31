# AI-Voice-Controlled-Entry-Exit-System
•	Created an AI-based people counter using ultrasonic sensors.
•	Included real-time occupancy tracking and voice instructions.

Project Overview:
Sensor used: Ultrasonic (HC-SR04) or IR sensor.
Controller: Arduino Uno/Nano.
Actuator: Servo Motor (SG90 or similar).
Function: When someone approaches the door, the sensor detects them and the servo opens the door. After a delay, the door closes automatically.

CODE:
#include <Servo.h>
// Define pins for Ultrasonic Sensor
const int trigPin = 9;
const int echoPin = 10;
// Create Servo object
Servo doorServo;
// Define distance threshold in cm
const int distanceThreshold = 15; // adjust as needed
// Servo positions
const int openPosition = 90;
const int closePosition = 0;
void setup() {
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  doorServo.attach(6); // Connect servo to pin 6
  doorServo.write(closePosition); // Initially closed
  Serial.begin(9600);
}

void loop() {
  long duration;
  int distance;

  // Send ultrasonic pulse
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  // Read echo
  duration = pulseIn(echoPin, HIGH);
  distance = duration * 0.034 / 2; // Convert to cm

  Serial.print("Distance: ");
  Serial.println(distance);

  // Check if someone is near
  if (distance > 0 && distance < distanceThreshold) {
    doorServo.write(openPosition); // Open door
    delay(3000); // Keep open for 3 seconds
    doorServo.write(closePosition); // Close door
  }

  delay(500); // Check every 0.5 second
}
