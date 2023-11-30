// BLE Scanner for M5StickC Plus
// Written by @juliodellaflora

#include <M5StickCPlus.h>        // Include the M5StickC Plus library
#include <BLEDevice.h>           // Include the BLE device library
#include <BLEScan.h>             // Include the BLE scan library
#include <BLEAdvertisedDevice.h> // Include the library for advertised BLE devices

const int maxDevices = 10;       // Maximum number of devices to store in history
int scanTime = 5;                // Scan time in seconds
int deviceCount = 0;             // Counter for the number of devices found

// Structure to store device information
struct DeviceInfo {
  String name;
  String address;
} devices[maxDevices];

// Function to add a device to the list
void addDeviceToList(BLEAdvertisedDevice advertisedDevice) {
  if (deviceCount < maxDevices) {
    devices[deviceCount].name = advertisedDevice.getName().c_str();
    devices[deviceCount].address = advertisedDevice.getAddress().toString().c_str();
    deviceCount++;
  } else {
    for (int i = 0; i < maxDevices - 1; i++) {
      devices[i] = devices[i + 1];
    }
    devices[maxDevices - 1].name = advertisedDevice.getName().c_str();
    devices[maxDevices - 1].address = advertisedDevice.getAddress().toString().c_str();
  }
}

// Function to display devices on the screen
void displayDevices() {
  M5.Lcd.fillScreen(BLACK);          // Clear the screen
  M5.Lcd.setCursor(0, 0);            // Set cursor to top left
  M5.Lcd.setTextSize(2);             // Set text size
  M5.Lcd.setTextColor(TFT_ORANGE);   // Set text color to orange
  for (int i = 0; i < deviceCount; i++) {
    M5.Lcd.println(devices[i].name + "\n" + devices[i].address + "\n");
  }
}

// Callback class for BLE advertised devices
class MyAdvertisedDeviceCallbacks: public BLEAdvertisedDeviceCallbacks {
  void onResult(BLEAdvertisedDevice advertisedDevice) {
    addDeviceToList(advertisedDevice);
    displayDevices();
  }
};

// Setup function
void setup() {
  M5.begin();                                   // Initialize the M5StickC Plus
  M5.Lcd.setRotation(3);                        // Set display rotation
  M5.Lcd.fillScreen(BLACK);                     // Clear the screen
  M5.Lcd.setTextSize(2);                        // Set the initial text size
  M5.Lcd.setTextColor(TFT_ORANGE);              // Set the initial text color
  M5.Lcd.println("Escaneando BLE...");         // Print initial message

  BLEDevice::init("");                          // Initialize the BLE device
  BLEScan* pBLEScan = BLEDevice::getScan();     // Create a new BLE scan
  pBLEScan->setAdvertisedDeviceCallbacks(new MyAdvertisedDeviceCallbacks());
  pBLEScan->setActiveScan(true);                // Set the scan to active mode
  pBLEScan->setInterval(100);
  pBLEScan->setWindow(99);
}

// Loop function
void loop() {
  BLEDevice::getScan()->start(scanTime, false); // Start a BLE device scan
  delay(2000);                                  // Wait a bit before starting the next scan
}
