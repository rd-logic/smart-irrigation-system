# smart-irrigation-system
// Automatic Irrigation System Using Soil Moisture Sensor

#include <LiquidCrystal.h>  // Include LCD library (optional)

// Pin Definitions
const int sensorPin = A0;   // Soil Moisture Sensor connected to Analog Pin A0
const int relayPin = 7;     // Relay module connected to Digital Pin 7
const int threshold = 400;  // Moisture level threshold (adjust as needed)

// LCD Configuration (Optional)
LiquidCrystal lcd(12, 11, 5, 4, 3, 2); // RS, E, D4, D5, D6, D7

void setup() {
    pinMode(relayPin, OUTPUT);
    digitalWrite(relayPin, HIGH); // Ensure pump is OFF initially
    Serial.begin(9600); // Start Serial Communication
    lcd.begin(16, 2);   // Initialize LCD (Optional)
    lcd.print("Irrigation System");
}

void loop() {
    int moistureLevel = analogRead(sensorPin); // Read soil moisture value
    Serial.print("Soil Moisture: ");
    Serial.println(moistureLevel);
    
    lcd.setCursor(0, 1);
    lcd.print("Moisture: ");
    lcd.print(moistureLevel);

    if (moistureLevel < threshold) { // If soil is dry
        digitalWrite(relayPin, LOW);  // Turn ON pump
        Serial.println("Pump ON");
        lcd.print(" PUMP ON");
    } else { // If soil is wet
        digitalWrite(relayPin, HIGH); // Turn OFF pump
        Serial.println("Pump OFF");
        lcd.print(" PUMP OFF");
    }
    delay(2000); // Wait for 2 seconds before next reading
}
