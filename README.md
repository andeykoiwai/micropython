# MicroPython Sensor and Data Exercises

This repository contains a collection of MicroPython example scripts designed for learning and experimenting with sensors and data processing on microcontroller platforms such as ESP32, ESP8266, or Raspberry Pi Pico. The exercises cover various sensors (e.g., temperature, humidity, light, motion) and demonstrate how to collect, process, and display data in practical applications.

## Project Overview
This project aims to provide beginners and intermediate learners with hands-on examples to understand how to interface sensors with MicroPython-enabled microcontrollers. Each script includes clear comments and explanations to help users learn how to read sensor data, process it, and output meaningful results (e.g., to a serial console, OLED display, or external service).

## Features
- **Sensor Examples**: Includes code for common sensors like DHT11/DHT22 (temperature and humidity), HC-SR04 (ultrasonic distance), LDR (light-dependent resistor), and MPU6050 (accelerometer/gyroscope).
- **Data Processing**: Demonstrates techniques for filtering, averaging, and logging sensor data.
- **Output Options**: Examples for displaying data via serial console, OLED displays, or sending to external platforms (e.g., via MQTT or HTTP, where applicable).
- **Modular Code**: Each script is self-contained and focuses on a single sensor or concept for easy understanding.
- **Error Handling**: Includes robust error handling to manage sensor failures or invalid data.

## Prerequisites
- **Hardware**:
  - Microcontroller with MicroPython support (e.g., ESP32, ESP8266, Raspberry Pi Pico).
  - Sensors (e.g., DHT11/DHT22, HC-SR04, LDR, MPU6050, etc.).
  - Jumper wires, breadboard, and optional OLED display (e.g., SSD1306).
  - Power supply (USB or external, depending on the board).
- **Software**:
  - MicroPython firmware installed on the microcontroller.
  - A serial terminal tool (e.g., Thonny, uPyCraft, or PuTTY) with a baud rate of 115200.
  - Optional: Python libraries for local data visualization (e.g., `pyserial` for reading serial output).

## Installation
1. Install MicroPython firmware on your microcontroller (see [MicroPython's official guide](https://micropython.org/download/)).
2. Clone or download this repository to your computer:
   ```bash
   git clone https://github.com/andeykoiwai/micropython
   ```
3. Navigate to the specific example folder (e.g., `dht11_temperature` or `hc_sr04_distance`).
4. Upload the desired script (e.g., `main.py`) to your microcontroller using Thonny or uPyCraft.
5. Connect the sensor to the microcontroller as described in each example's documentation.

## Usage
1. Open the serial console (baud rate: 115200) to interact with the scripts.
2. Follow the specific instructions in each example's folder (e.g., wiring diagrams and code comments).
3. Run the script to collect and process sensor data. Examples include:
   - Reading temperature and humidity from a DHT11 sensor.
   - Measuring distance with an HC-SR04 ultrasonic sensor.
   - Logging light intensity with an LDR.
   - Visualizing accelerometer data from an MPU6050.
4. Modify the scripts to experiment with different data processing techniques or output methods.

## Example Structure
Each example folder contains:
- `main.py`: The MicroPython script for the specific sensor or exercise.
- `README.md`: Instructions for wiring, running the script, and understanding the output.
- Optional: Utility scripts (e.g., for data logging or visualization on a PC).

## Example Code (DHT11 Temperature and Humidity)
Below is a sample script for reading data from a DHT11 sensor:

```python
from machine import Pin
import dht
import time

# Initialize DHT11 sensor on GPIO 14
sensor = dht.DHT11(Pin(14))

while True:
    try:
        sensor.measure()
        temp = sensor.temperature()
        hum = sensor.humidity()
        print(f"Temperature: {temp}°C, Humidity: {hum}%")
        time.sleep(2)  # DHT11 requires 2-second delay between readings
    except OSError:
        print("Failed to read sensor")
    time.sleep(1)
```

For more examples, check the individual folders in this repository.

## Notes
- **Sensor Compatibility**: Ensure your sensor is compatible with your microcontroller's voltage (3.3V or 5V).
- **Power Supply**: Some sensors (e.g., HC-SR04) may require an external power source for stable operation.
- **Calibration**: Adjust timing or thresholds in scripts if sensor readings are inconsistent.
- **Extending Projects**: Try combining multiple sensors or adding data logging to an SD card or cloud service.

## Contributing
Contributions are welcome! To contribute:
1. Fork this repository.
2. Add new sensor examples, improve existing scripts, or enhance documentation.
3. Submit a pull request with a clear description of your changes.

Please ensure your code follows the repository's structure and includes detailed comments.

## License
This project is licensed under the MIT License.