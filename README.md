# IOT-based-wheather-station-
An IoT-based Weather Station built with ESP32 and DHT22 sensor.  The project measures temperature and humidity, sends live data to the Blynk IoT App,  and supports Wokwi simulation for testing without hardware.
// ----------------- Blynk Template Config -----------------
#define BLYNK_TEMPLATE_ID "TMPL3KdMAJfyp"
#define BLYNK_TEMPLATE_NAME "Weather station"
#define BLYNK_AUTH_TOKEN "-RV6V2TzQyXs-4whE3f1374HnYA4m8XP"

// ----------------- Libraries -----------------
#include <WiFi.h>
#include <BlynkSimpleEsp32.h>
#include "DHT.h"

// ----------------- WiFi Configuration -----------------
char ssid[] = "YourWiFiName";       // Change this
char pass[] = "YourWiFiPassword";   // Change this

// ----------------- DHT22 Configuration -----------------
#define DHTPIN 15       // GPIO where the DHT22 data pin is connected
#define DHTTYPE DHT22
DHT dht(DHTPIN, DHTTYPE);

// ----------------- Virtual Pins -----------------
#define TEMP_VPIN V0
#define HUMID_VPIN V1

// ----------------- Setup -----------------
void setup() {
  Serial.begin(115200);

  // Initialize DHT sensor
  dht.begin();

  // Connect to Blynk
  Blynk.begin(BLYNK_AUTH_TOKEN, ssid, pass);
}

// ----------------- Loop -----------------
void loop() {
  Blynk.run();   // Run Blynk

  // Read temperature and humidity
  float temp = dht.readTemperature();
  float humid = dht.readHumidity();

  // Check if any reads failed
  if (isnan(temp) || isnan(humid)) {
    Serial.println("Failed to read from DHT sensor!");
    return;
  }

  // Print to Serial Monitor
  Serial.print("Temperature: ");
  Serial.print(temp);
  Serial.print("°C, Humidity: ");
  Serial.print(humid);
  Serial.println("%");

  // Send to Blynk virtual pins
  Blynk.virtualWrite(TEMP_VPIN, temp);
  Blynk.virtualWrite(HUMID_VPIN, humid);

  delay(2000); // Wait 2 seconds between readings
}
