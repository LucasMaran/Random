Sure! Below is an example Arduino code that reads data from a Galvanic Skin Response (GSR) sensor and sends the readings to both the Serial Plotter (for visualization) and an OLED display.

I'll assume you are using the following hardware:

1. **GSR sensor** (analog output)
2. **OLED display** (e.g., SSD1306 or SH1106, which are commonly used 128x64 OLED displays)
3. **Arduino board** (e.g., Uno, Nano, or similar)

The code will:

- Read the analog value from the GSR sensor.
- Plot the data in the Serial Plotter for real-time visualization.
- Display the value on the OLED screen.

You'll need the `Adafruit_SSD1306` and `Adafruit_GFX` libraries for the OLED. You can install them through the Library Manager in the Arduino IDE.

Here’s the code:

### Required Libraries
1. **Adafruit_SSD1306**
2. **Adafruit_GFX**

### Wiring
- **GSR sensor** analog pin: `A0`
- **OLED SDA**: `A4` (for most Arduino boards like Uno/Nano)
- **OLED SCL**: `A5` (for most Arduino boards like Uno/Nano)

### Arduino Code:

```cpp
#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>

// OLED display configuration (128x64 pixels)
#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64
#define OLED_RESET    -1
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, OLED_RESET);

// GSR sensor pin
const int gsrPin = A0;

void setup() {
  // Start the serial communication
  Serial.begin(9600);
  
  // Initialize the OLED display
  if (!display.begin(SSD1306_I2C_ADDRESS, OLED_RESET)) {
    Serial.println(F("SSD1306 allocation failed"));
    for(;;);  // Infinite loop if OLED setup fails
  }
  
  display.clearDisplay();
  display.setTextSize(1);
  display.setTextColor(SSD1306_WHITE);
  display.setCursor(0, 0);
  display.println("GSR Sensor Reading");
  display.display();
  
  delay(2000);  // Wait for 2 seconds to display initial message
}

void loop() {
  // Read the GSR sensor value (analog)
  int gsrValue = analogRead(gsrPin);
  
  // Print the value to Serial Plotter (for visualization)
  Serial.println(gsrValue);
  
  // Display the GSR value on the OLED screen
  display.clearDisplay();
  display.setCursor(0, 0);
  display.print("GSR Value: ");
  display.println(gsrValue);
  display.display();
  
  // Wait for a short period before reading again
  delay(100);
}
```

### How it works:
1. **GSR Sensor**: The `analogRead()` function reads the analog voltage from the GSR sensor and maps it to a value between 0 and 1023. This value is sent to the Serial Plotter and displayed on the OLED.
2. **Serial Plotter**: The `Serial.println(gsrValue)` sends the GSR sensor data to the Serial Plotter for real-time graphing.
3. **OLED Display**: The `Adafruit_SSD1306` library is used to update the display every time the sensor value is read. The `display.clearDisplay()` ensures that the screen is updated with the latest value.

### How to use:
1. Upload the code to your Arduino.
2. Open the Serial Monitor (Tools → Serial Monitor) and set it to "Plotter" mode to view the real-time graph.
3. You should see the GSR data plotted in the Serial Plotter and also displayed on the OLED screen.

If you need more features, like adding smoothing or filtering to the sensor data, let me know!
