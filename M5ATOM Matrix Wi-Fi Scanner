// M5ATOM Matrix Wi-Fi Scanner
// Developer: Julio Della Flora
// Instagram: @juliodellaflora
// Description: This code enables the M5ATOM Matrix to display the number
// of available Wi-Fi networks using its LED matrix with a color-changing effect.

#include <M5Atom.h>  // Includes the M5Atom library to control the M5ATOM Matrix
#include <WiFi.h>    // Includes the WiFi library for network functions

// Function to convert a hue value to an RGB color
uint32_t hue2rgb(uint16_t hue) {
  uint8_t r, g, b;

  hue %= 360; // Hue is an angle between 0 and 359
  int sector = hue / 60 % 6;
  int offset = hue % 60; // Offset within the sector
  int p = 0;
  int q = (255 * offset) / 60;
  int t = (255 * (60 - offset)) / 60;

  switch (sector) {
    case 0: r = 255; g = q; b = p; break;
    case 1: r = t; g = 255; b = p; break;
    case 2: r = p; g = 255; b = q; break;
    case 3: r = p; g = t; b = 255; break;
    case 4: r = q; g = p; b = 255; break;
    case 5: r = 255; g = p; b = t; break;
  }
  return (r << 16) | (g << 8) | b;
}

void setup() {
  M5.begin(true, false, true); // Initializes the M5Atom. Parameters enable serial, disable I2C, and enable the display.
  WiFi.mode(WIFI_STA);         // Sets up the ESP32 to operate as a station (STA), necessary for scanning Wi-Fi networks
  WiFi.disconnect();           // Disconnects from any Wi-Fi network, ensuring the device is in scanning mode
  delay(100);                  // A short delay to ensure the settings are applied
}

void loop() {
  int n = WiFi.scanNetworks(); // Starts scanning Wi-Fi networks and returns the number of networks found

  // Loop to light up LEDs according to the number of networks found with a color-changing effect
  for (int i = 0; i < n; i++) {
    for (int hue = 0; hue < 360; hue += 10) {
      M5.dis.drawpix(i, hue2rgb(hue)); // Lights up the LED at position 'i' with the color corresponding to the hue value
      delay(10); // Delay to make the color change visible
    }
  }

  delay(3000);                  // 3-second delay after displaying the number of networks
  M5.dis.clear();               // Clears the screen by turning off all LEDs
  delay(100);                  // 5-second delay before starting a new scan
}
