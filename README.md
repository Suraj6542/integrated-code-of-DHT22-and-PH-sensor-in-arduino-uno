# integrated-code-of-DHT22-and-PH-sensor-in-arduino-uno
#include <Wire.h>
#include <Adafruit_Sensor.h>
#include <Adafruit_DHT.h>
#include <Adafruit_ADS1015.h>

// Pin Definitions
#define DHT22_PIN 2
#define pH_PIN A0

// Sensor Definitions
Adafruit_DHT dht(DHT22_PIN, DHT22_AM2302);
Adafruit_ADS1015 ads; 

void setup() {
  Serial.begin(9600);
  ads.begin();
  ads.setGain(GAIN_TWO); // Set the gain to 2 for measuring 0-3.3V
}

void loop() {
  float pH_value = 0;
  float humidity = 0;
  float temperature = 0;

  // Read the pH value
  pH_value = (float)analogRead(pH_PIN) * 3.3 / 1024;
  pH_value = (2.5 - pH_value) / 0.067;
  Serial.print("pH Value: ");
  Serial.println(pH_value);

  // Read the temperature and humidity from the DHT22
  humidity = dht.readHumidity();
  temperature = dht.readTemperature();
  if (isnan(humidity) || isnan(temperature)) {
    Serial.println("Failed to read from DHT22 sensor");
  }
  else {
    Serial.print("Humidity: ");
    Serial.print(humidity);
    Serial.print(" %, Temperature: ");
    Serial.print(temperature);
    Serial.println(" *C");
  }

  delay(2000);
}
