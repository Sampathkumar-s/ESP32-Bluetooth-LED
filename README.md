# ESP32-Bluetooth-LED
Control ESP32 onboard LED using Bluetooth

#include "BluetoothSerial.h"

BluetoothSerial SerialBT;

#define BLUE_LED 2  // Onboard blue LED pin on most ESP32 boards

void setup() {
  Serial.begin(115200);
  SerialBT.begin("ESP32-BULB");  // Set your Bluetooth name
  pinMode(BLUE_LED, OUTPUT);     // Set LED pin as output
  digitalWrite(BLUE_LED, LOW);   // Start with LED off
  Serial.println("Bluetooth is ready. Waiting for message...");
}

void loop() {
  if (SerialBT.available()) {
    char incomingChar = SerialBT.read(); // Read message
    Serial.print("Received: ");
    Serial.println(incomingChar);

    if (incomingChar == '1') {
      digitalWrite(BLUE_LED, HIGH);      // Turn LED ON
      SerialBT.println("LED is ON");
    } else if (incomingChar == '0') {
      digitalWrite(BLUE_LED, LOW);       // Turn LED OFF
      SerialBT.println("LED is OFF");
    } else {
      SerialBT.println("Unknown command");
    }
  }
}
