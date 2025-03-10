#include <Servo.h>
#define PIR_PIN 5 // PIR sensor connected to digital pin 5
#define BUZZER_PIN 2 // Buzzer connected to digital pin 2
#define SERVO_PIN 6 // Servo motor connected to digital pin 6
Servo servoMotor;
#define NOTE_C4 262
#define NOTE_G4 392
#define NOTE_A4 440
#define NOTE_F4 349
#define NOTE_E4 330
#define NOTE_D4 294
const int melody[] = {
 NOTE_C4, NOTE_C4, NOTE_G4, NOTE_G4, NOTE_A4, NOTE_A4, NOTE_G4,
 NOTE_F4, NOTE_F4, NOTE_E4, NOTE_E4, NOTE_D4, NOTE_D4, NOTE_C4};
const int noteDurations[] = {
 4, 4, 4, 4, 4, 4, 2,
 4, 4, 4, 4, 4, 4, 2
};
const float feverThreshold = 37.5;
int tempPin=0;
int angle=0;
const int moistPin=1;
int led_moisture=12;
int led_fever=13;
void setup() {
 pinMode(PIR_PIN, INPUT);
 pinMode(BUZZER_PIN, OUTPUT);
 pinMode(SERVO_PIN, OUTPUT);
 pinMode(led_moisture,OUTPUT);
 pinMode(led_fever,OUTPUT);
 servoMotor.attach(SERVO_PIN); // Attaching the servo motor to the pin
 Serial.begin(9600);
}
void playSong() {
 for (int i = 0; i < sizeof(melody) / sizeof(melody[0]); i++) {
 if (melody[i] == 0) {
 delay(noteDurations[i] * 100);
 } else {
 tone(BUZZER_PIN, melody[i], noteDurations[i] * 100);
 delay(noteDurations[i] * 110);
 noTone(BUZZER_PIN);
 }
 }}
void loop() {
 int motionDetected=0;
 motionDetected=digitalRead(PIR_PIN);
 int moisture;
 moisture=analogRead(moistPin);
 float moisture_percentage;
 moisture_percentage=(100-((moisture/1023.00)*100));
Smart Cradle System
13
 float temp;
 temp=analogRead(tempPin);
 temp=temp*0.48828125;
 if (motionDetected==HIGH) { // Check if motion is detected
 playSong();
 Serial.println("motion detected");
 for(angle=45;angle<90;angle++)
 {
 servoMotor.write(angle);
 delay(5);
 }
 for(angle=90;angle>45;angle--)
 {
 servoMotor.write(angle);
 delay(5);
 }
 servoMotor.write(180); // Move the servo motor to 180 degrees position
 delay(100); // Delay to avoid continuous triggering
 servoMotor.write(90); // Return the servo motor to 90 degrees position
 delay(100); // Delay to avoid re-triggering immediately
 }
 if (moisture_percentage>10) {
 Serial.println("Moisture detected - Change the diaper!"); // Prompt for diaper change
 digitalWrite(led_moisture,HIGH);
 delay(1000);
 digitalWrite(led_moisture,LOW);
 }
 Serial.print("Temperature: ");
 Serial.print(temp);
 Serial.println("*C");
 if (temp>feverThreshold) {
 Serial.println("Fever detected!");
 digitalWrite(led_fever,HIGH);
 delay(1000);
 digitalWrite(led_fever,LOW);
 }
 delay(1000); // Adjust the delay as needed
}
