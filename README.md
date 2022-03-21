#include<Wire.h>
#include<Servo.h>
#include <SPI.h>
#include <SD.h>

const int chipSelect = 4;


File myFile;

Servo servo;
Servo servo2;

int redLED = 7;
int greenLED = 6;
int blueLED = 5;

int angle1;
int angle2;


int xangle = 0;
int yangle = 0;
int zangle = 0;

const int ledPin = 2;

const int MPU_addr=0x68;
int16_t AcX,AcY,AcZ,Tmp,GyX,GyY,GyZ;

void setup() {
  // put your setup code here, to run once:
  //Serial.begin(9600);
 Serial.begin(9600);
  while (!Serial) {
    ; // wait for serial port to connect. Needed for native USB port only
  }
  
  Serial.print("Initializing SD card...");

  // see if the card is present and can be initialized:
  if (!SD.begin(chipSelect)) {
    Serial.println("Card failed, or not present");
    // don't do anything more:
    while (1);
  }
  Serial.println("card initialized.");
  


  Wire.begin();
  Wire.beginTransmission(MPU_addr);
  Wire.write(0x6B);
  Wire.write(0);
  Wire.endTransmission(true);
  servo.attach(9);
  servo2.attach(10);
  
   pinMode(ledPin, OUTPUT);
  pinMode(redLED, OUTPUT);
  pinMode(greenLED, OUTPUT);
  pinMode(blueLED, OUTPUT);


 // Open serial communications and wait for port to open:
  Serial.begin(9600);
  while (!Serial) {
    ; // wait for serial port to connect. Needed for native USB port only
  }
  Serial.print("Initializing SD card...");

  if (!SD.begin(4)) {
    Serial.println("initialization failed!");
    while (1);
  }
  Serial.println("initialization done.");

  // open the file. note that only one file can be open at a time,
  // so you have to close this one before opening another.
  myFile = SD.open("test.txt", FILE_WRITE);

  // if the file opened okay, write to it:
  if (myFile) {
    Serial.print("Writing to test.txt...");
  
  //----------------------------------------------------------------------------------------------------------------------------------------------------------------//

 //                                                                         START-UP

  //----------------------------------------------------------------------------------------------------------------------------------------------------------------//
  myFile.println("Start System");
  myFile.println("Pad Idle Mode");
  
  delay(100);
  digitalWrite(7, HIGH);
  digitalWrite(6, HIGH);
  delay(3000);
  servo.write(0);
  servo.write(5);
  servo.write(0);
  servo.write(-5);
  delay(2000);
  servo2.write(0);
  servo2.write(0);
  servo2.write(0);
  servo2.write(0);
  delay(2000);
   digitalWrite(7, LOW);
  digitalWrite(6, LOW);
  delay(1000);
   digitalWrite(7, HIGH);
  digitalWrite(6, HIGH);
  delay(1000);
  digitalWrite(7, LOW);
  digitalWrite(6, LOW);
  delay(1000);
  
 
  
}
}

void loop() {
  // put your main code here, to run repeatedly:
      
   //----------------------------------------------------------------------------------------------------------------------------------------------------------------//

 //                                                                         TVC + SOUND SENSOR

  //----------------------------------------------------------------------------------------------------------------------------------------------------------------//

 //AcZ = Wire.read()<<8|Wire.read();
 //zangle = AcZ/182.04;
 int sensorValue = analogRead(A7);
 if (sensorValue > 400) {
 myFile.println("Lift Off!!!!");
 myFile.println(sensorValue);  
  
  digitalWrite(7, HIGH);
  digitalWrite(5, HIGH);

  digitalWrite(ledPin, HIGH);

//  
  //x ax
  Wire.beginTransmission(MPU_addr);
  Wire.write(0x3B);
  Wire.endTransmission(false);
  Wire.requestFrom(MPU_addr, 14, true);

  AcX = Wire.read()<<8|Wire.read();
  xangle = AcX/182.4;

  // y ax
  Wire.beginTransmission(MPU_addr);
  Wire.write(0x3D);
  Wire.endTransmission(false);
  Wire.requestFrom(MPU_addr, 14, true);

  AcY = Wire.read()<<8|Wire.read();
  yangle = AcY/-182.4;


  servo.write(xangle + 35);
  servo2.write(35 + yangle);

  delay(100);
  digitalWrite(ledPin, HIGH);
  delay(3000);
  digitalWrite(ledPin, LOW);
  digitalWrite(7, HIGH);
  digitalWrite(6, HIGH);

 }
 //----------------------------------------------------------------------------------------------------------------------------------------------------------------//

 //                                                                         SD CARD

  //----------------------------------------------------------------------------------------------------------------------------------------------------------------//


  
  

  
    
    
    
myFile.close();
}
 




 
  
