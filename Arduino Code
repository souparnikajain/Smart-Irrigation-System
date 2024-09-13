# Smart-Irrigation-System-With-Leaf-Disease-Detection
Designed and implemented a smart irrigation system integrated with utilizing IoT sensors to optimize water usage based on the crops selected  with advance leaf disease detection system with machine learning algorithms and MATLAB to ensure early identification and treatment of plant  diseases, resulting in improved crop health productivity
#include "thingProperties.h"
#include <Adafruit_GFX.h>
#include <Adafruit_GrayOLED.h>
#include <Adafruit_SPITFT.h>
#include <Adafruit_SPITFT_Macros.h>
#include <gfxfont.h>

// Adafruit SSD1306 - Version: Latest
#include <Adafruit_SSD1306.h>
#include <splash.h>

// DHT sensor library - Version: Latest
#include <DHT.h>
#include <DHT_U.h>

// Wire Library for I2C
#include <Wire.h>

// Set OLED size in pixels
#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64

// Set OLED parameters
#define OLED_RESET -1
#define SCREEN_ADDRESS 0x3C
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, OLED_RESET);

// Define DHT22 Parameters
#define DHTPIN 8
#define DHTTYPE DHT22
DHT dht(DHTPIN, DHTTYPE);

// Define variables for Temperature
float temp;
int control=0;
int moisture_limit;

// Sensor constants - replace with values from calibration sketch

// Constant for dry sensor
const int DryValue = 2650;

// Constant for wet sensor
const int WetValue = 1800;

// Variables for soil moisture
int soilMoistureValue;
int soilMoisturePercent;

// Analog input port
#define SENSOR_IN 0

// Relay Port
#define RELAY_OUT 3

// Pump Status Text
String pump_status_text = "OFF";

// Variable for pump trigger

\

void wheat();
void paddy();
void cotton();
void maize();

void setup() {
  // Initialize serial and wait for port to open:
  Serial.begin(9600);
  // This delay gives the chance to wait for a Serial Monitor without blocking if none is found
  delay(1500); 

  // Defined in thingProperties.h
  initProperties();

  // Connect to Arduino IoT Cloud
  ArduinoCloud.begin(ArduinoIoTPreferredConnection);
   display.begin(SSD1306_SWITCHCAPVCC, SCREEN_ADDRESS);
  display.clearDisplay();

  // Initialize DHT22
  dht.begin();

  // Set ADC to use 12 bits
  analogReadResolution(12);

  // Set Relay as Output
  pinMode(RELAY_OUT, OUTPUT);

  // Turn off relay
  digitalWrite(RELAY_OUT, LOW);

  // Set Pump Status to Off
  pump_status = false;
  setDebugMessageLevel(2);
  ArduinoCloud.printDebugInfo();
}

void loop() {
  ArduinoCloud.update();
  ArduinoCloud.update();

  // Get temperature 
  temp = dht.readTemperature();

  

  // Pass temperature values to cloud variables
  current_temperature = temp;
  

  // Get soil mositure value
  soilMoistureValue = analogRead(SENSOR_IN);

  // Determine soil moisture percentage value
  soilMoisturePercent = map(soilMoistureValue, DryValue, WetValue, 0, 100);

  // Keep values between 0 and 100
  soilMoisturePercent = constrain(soilMoisturePercent, 0, 100);

  // Print raw value to serial monitor for sensor calibration
  Serial.println(soilMoistureValue);

  // Pass soil moisture to cloud variable
  current_moisture = soilMoisturePercent;

 
  if(my_control==control){
    
  switch(option)
  {
  case 0: wheat();
          break;
  case 1: paddy();
          break;
  case 2: cotton();
          break;
  case 3: maize();
          break;
  case 4: pumpOn();
          break;
  case 5: pumpOff();
          break;
  default: Serial.println("Error in finding the crop");
          break;
  }

  if(option !=4 && option!=5)
  {
  if (soilMoisturePercent<moisture_limit) {
    // Turn pump on
    pumpOn();

  } else {
    // Turn pump off
    pumpOff();
  }
  }
  }

  else
  {
  if (soilMoisturePercent<my_control) {
    // Turn pump on
    pumpOn();

  } else {
    // Turn pump off
    pumpOff();
  }
  }
  
  // Cycle values on OLED Display
  printOLED(35, "PUMP", 40, pump_status_text, 2000);
  printOLED(35, "TEMP", 10, String(temp) + "C", 2000);
  printOLED(35, "MOIST", 30, String(soilMoisturePercent) + "%", 2000);
  
}

  void wheat()
  {
    moisture_limit=25;
    
  }
  void paddy()
  {
    moisture_limit=60;
    
  }
  void cotton()
  {
    moisture_limit=50;
    
  }
  void maize()
  {
    moisture_limit=65;
    
  }
  
void pumpOn() {
  // Turn pump on
  digitalWrite(RELAY_OUT, HIGH);
  pump_status_text = "ON";
  pump_status = true;

}

void pumpOff() {
  // Turn pump off
  digitalWrite(RELAY_OUT, LOW);
  pump_status_text = "OFF";
  pump_status = false;

}

void printOLED(int top_cursor, String top_text, int main_cursor, String main_text, int delay_time){
  // Prints to OLED and holds display for delay_time
  display.setCursor(top_cursor, 0);
  display.setTextSize(2);
  display.setTextColor(WHITE);
  display.println(top_text);

  display.setCursor(main_cursor, 40);
  display.setTextSize(3);
  display.setTextColor(WHITE);
  display.print(main_text);
  display.display();

  delay(delay_time);
  display.clearDisplay();
 
}
void onOptionChange() {

}
  

  
void onMyControlChange()  {
  // Add your code here to act upon MyControl change

}


/*
  Since PumpStatus is READ_WRITE variable, onPumpStatusChange() is
  executed every time a new value is received from IoT Cloud.
*/
void onPumpStatusChange()  {
  
}
