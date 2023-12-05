// M5Stack Standalone UART Communication Viewer
// This project creates a standalone device with M5Stack to visualize UART communication
// on pins 16 and 17. It allows for real-time monitoring and interaction with UART-enabled hardware.

#include <M5Stack.h>

// Define UART pins and number
#define UART_NUM 2
const int RX_PIN = 16; // RX pin for M5Stack
const int TX_PIN = 17; // TX pin for M5Stack

// Baud rate definitions
const int BAUD_RATES[] = {9600, 14400, 19200, 38400, 57600, 115200};
int baudRateIndex = 5;  // Starting with 115200 baud rate

// Character height and maximum display lines configuration
const int CHAR_HEIGHT = 16; // Approximate character height with textSize(2)
const int MAX_LINES = 240 / CHAR_HEIGHT; // Maximum number of lines on the display
int currentLine = 0;

void setup() {
    M5.begin(); // Initialize M5Stack
    M5.Lcd.setTextSize(2); // Set text size
    M5.Lcd.setTextColor(WHITE, BLACK); // Set text and background color
    M5.Lcd.clear(); // Clear the screen
    M5.Lcd.println("M5Stack UART Communication Viewer"); // Initial message

    // Initialize serial communication with starting baud rate
    Serial2.begin(BAUD_RATES[baudRateIndex], SERIAL_8N1, RX_PIN, TX_PIN);
}

void loop() {
    M5.update(); // Update button states

    // Change baud rate if button A is pressed
    if(M5.BtnA.wasPressed()) {
        baudRateIndex = (baudRateIndex + 1) % (sizeof(BAUD_RATES)/sizeof(BAUD_RATES[0]));
        Serial2.end();
        Serial2.begin(BAUD_RATES[baudRateIndex], SERIAL_8N1, RX_PIN, TX_PIN);

        scrollScreen(); // Scroll screen to display new baud rate
        M5.Lcd.setCursor(0, currentLine * CHAR_HEIGHT);
        M5.Lcd.print("Baud Rate: ");
        M5.Lcd.println(BAUD_RATES[baudRateIndex]);
    }

    // Send 'help' command if button B is pressed
    if(M5.BtnB.wasPressed()) {
        Serial2.print("help\r\n");
        scrollScreen(); // Scroll screen to display sent command
        M5.Lcd.setCursor(0, currentLine * CHAR_HEIGHT);
        M5.Lcd.println("> help");
    }

    // Read UART communication and display it
    while(Serial2.available()) {
        char c = Serial2.read();
        if (c == '\n') {
            scrollScreen(); // Scroll screen for new line
        }
        M5.Lcd.write(c); // Write character to display
    }
}

// Function to scroll screen
void scrollScreen() {
    if (currentLine < MAX_LINES - 1) {
        currentLine++;
    } else {
        // Fast scroll: capture and redraw shifted screen
        uint16_t *buffer = (uint16_t *)malloc(320 * CHAR_HEIGHT * 2);
        M5.Lcd.readRect(0, CHAR_HEIGHT, 320, 240 - CHAR_HEIGHT, buffer);
        M5.Lcd.pushRect(0, 0, 320, 240 - CHAR_HEIGHT, buffer);
        free(buffer);
        M5.Lcd.fillRect(0, 240 - CHAR_HEIGHT, 320, CHAR_HEIGHT, BLACK);
    }
    M5.Lcd.setCursor(0, currentLine * CHAR_HEIGHT); // Position cursor for new line
}
