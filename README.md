# Air-Quality-Monitor
Smart Indoor Air Quality Monitor is an IoT-based system using ESP8266, MQ135, and DHT11 sensors to track air quality, temperature, and humidity in real time. It displays AQI-like status locally and on Blynk, while generating alerts when harmful gas or pollution levels exceed safe limits, promoting healthier indoor environments.
mkdir Air-Quality-Monitor
cd Air-Quality-Monitor

git init

echo "# Air Quality Monitor" > README.md

git add README.md
git commit -m "Initial commit"
git remote add origin https://github.com/USERNAME/Air-Quality-Monitor.git
git branch -M main
git push -u origin main
# Air Quality Monitoring System

## Overview
This project monitors air quality using NodeMCU, MQ135, DHT11, LCD Display, and Blynk IoT.

## Code
#include <GDBStub.h>

#define BLYNK_TEMPLATE_ID "TMPL3_sSnxrb6"
#define BLYNK_TEMPLATE_NAME "Air Quality Monitoring"
#define BLYNK_AUTH_TOKEN "7sEmZgn_sFLB1QabgnvEyJrXWjzEC2wO"

#define BLYNK_PRINT Serial

#include <ESP8266WiFi.h>
#include <BlynkSimpleEsp8266.h>
#include <DHT.h>
#include <Wire.h>
#include <LiquidCrystal_I2C.h>

// WiFi Credentials
char auth[] = BLYNK_AUTH_TOKEN;
char ssid[] = "Myhomewifi_2";
char pass[] = "snpsu123";

// LCD Setup
LiquidCrystal_I2C lcd(0x27, 16, 2);

// DHT Sensor Setup
#define DHTPIN D4
#define DHTTYPE DHT11

DHT dht(DHTPIN, DHTTYPE);

// MQ135 Sensor
#define MQ135 A0

byte degree_symbol[8] =
{
  0b00111,
  0b00101,
  0b00111,
  0b00000,
  0b00000,
  0b00000,
  0b00000,
  0b00000
};

void setup()
{
  Serial.begin(115200);

  lcd.init();
  lcd.backlight();

  lcd.createChar(1, degree_symbol);

  dht.begin();

  Blynk.begin(auth, ssid, pass);

  lcd.setCursor(0,0);
  lcd.print("Air Quality");
  lcd.setCursor(0,1);
  lcd.print("Monitoring");
  
  delay(2000);

  lcd.clear();
}

void loop()
{
  Blynk.run();

  float temperature = dht.readTemperature();
  float humidity = dht.readHumidity();

  int airQuality = analogRead(MQ135);

  // Serial Monitor
  Serial.print("Temperature: ");
  Serial.println(temperature);

  Serial.print("Humidity: ");
  Serial.println(humidity);

  Serial.print("Air Quality: ");
  Serial.println(airQuality);

  // LCD Display
  lcd.setCursor(0,0);
  lcd.print("T:");
  lcd.print(temperature);
  lcd.write(1);
  lcd.print("C ");

  lcd.print("H:");
  lcd.print(humidity);
  lcd.print("%");

  lcd.setCursor(0,1);
  lcd.print("Air:");
  lcd.print(airQuality);
  lcd.print("   ");

  // Air Quality Status
  if(airQuality > 350)
  {
    lcd.setCursor(10,1);
    lcd.print("BAD ");
  }
  else
  {
    lcd.setCursor(10,1);
    lcd.print("GOOD");
  }

  // Send Data to Blynk
  Blynk.virtualWrite(V0, temperature);
  Blynk.virtualWrite(V1, humidity);
  Blynk.virtualWrite(V2, airQuality);

  delay(2000);
}

## Components Used
- NodeMCU ESP8266
- MQ135 Gas Sensor
- DHT11 Temperature & Humidity Sensor
- 16x2 I2C LCD
- Jumper Wires
- Breadboard

## Features
- Real-time air quality monitoring
- Temperature and humidity display
- Blynk cloud integration
- LCD output display

## Author
Jaya Kumar
