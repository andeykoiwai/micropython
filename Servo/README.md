# MicroPython Servo SG90 Controller

This project provides a MicroPython script to control an SG90 servo motor, allowing it to rotate to a specified angle (0-180 degrees) based on user input. Users can enter an angle via the serial console, and the servo will move to that angle. Typing "keluar" (exit) terminates the program.

## Features
- Control an SG90 servo motor using PWM signals.
- Accepts angle inputs (0-180 degrees) via serial console.
- Exits gracefully with the "keluar" command.
- Includes error handling for invalid inputs.
- Cleans up PWM resources when the program ends.

## Requirements
- A microcontroller board with MicroPython support (e.g., ESP32, ESP8266, Raspberry Pi Pico).
- SG90 servo motor.
- Jumper wires and a suitable power source (5V or 3.3V, depending on the board).
- MicroPython firmware installed on the board.
- A serial terminal tool (e.g., Thonny, uPyCraft, or PuTTY) with a baud rate of 115200.

## Hardware Setup
1. Connect the SG90 servo to your microcontroller:
   - **Red wire (VCC)**: Connect to 5V (or 3.3V, depending on your board).
   - **Brown wire (GND)**: Connect to a ground (GND) pin.
   - **Orange wire (signal)**: Connect to a PWM-capable GPIO pin (e.g., GPIO 15).
2. Ensure the microcontroller and servo share a common ground.
3. If the servo movement is unstable, use an external power source for the servo's VCC, ensuring the grounds are connected.

## Installation
1. Install MicroPython firmware on your microcontroller (refer to [MicroPython's official guide](https://micropython.org/download/)).
2. Copy the following code into a file named `main.py`:

```python
from machine import Pin, PWM
import time

# Initialize PWM for servo on GPIO 15 (change pin as needed)
servo = PWM(Pin(15))

# Set PWM frequency to 50 Hz for SG90
servo.freq(50)

def set_servo_angle(angle):
    # Ensure angle is within 0-180 degrees
    if angle < 0:
        angle = 0
    elif angle > 180:
        angle = 180
    
    # Calculate duty cycle for SG90 (0.5ms = duty 25, 2.5ms = duty 125)
    duty = int(25 + (100 * angle) / 180)
    servo.duty(duty)

# Main loop
try:
    while True:
        # Prompt user for angle input
        angle_input = input("Enter angle (0-180) or 'keluar' to exit: ")
        
        # Check for exit command
        if angle_input.lower() == 'keluar':
            print("Exiting program...")
            break
        
        # Convert input to integer and move servo
        try:
            angle = int(angle_input)
            set_servo_angle(angle)
            print(f"Servo moved to {angle} degrees")
            time.sleep(0.5)  # Delay to allow servo movement
        except ValueError:
            print("Enter a valid number between 0-180 or 'keluar'!")

except KeyboardInterrupt:
    # Handle Ctrl+C
    print("\nProgram terminated by user")
finally:
    # Clean up PWM
    servo.deinit()
    print("Servo turned off")
```

3. Upload `main.py` to your microcontroller using a tool like Thonny or uPyCraft.
4. Open the serial console (baud rate: 115200) to interact with the program.

## Usage
1. Run the script on your microcontroller.
2. In the serial console, enter an angle (e.g., `90`) and press Enter to move the servo to that angle.
3. Type `keluar` (case-insensitive) to exit the program.
4. Invalid inputs (e.g., letters or numbers outside 0-180) will prompt an error message.
5. Press `Ctrl+C` to forcefully terminate the program, which will also clean up PWM resources.

## Code Explanation
- **Imports**:
  - `machine.Pin, PWM`: Configures GPIO pins and PWM for servo control.
  - `time`: Adds delays for stable servo movement.
- **Servo Initialization**:
  - `PWM(Pin(15))`: Sets GPIO 15 as the PWM pin (adjust as needed).
  - `servo.freq(50)`: Configures PWM frequency to 50 Hz (standard for SG90).
- **Function `set_servo_angle`**:
  - Limits angle to 0-180 degrees.
  - Maps angle to PWM duty cycle (25 for 0°, 125 for 180°).
  - Sets the PWM duty cycle to move the servo.
- **Main Loop**:
  - Prompts for user input via `input()`.
  - Exits on "keluar" command.
  - Converts input to an integer and calls `set_servo_angle`.
  - Handles invalid inputs with `try`/`except`.
- **Cleanup**:
  - Handles `KeyboardInterrupt` (Ctrl+C) gracefully.
  - Uses `finally` to disable PWM with `servo.deinit()`.

## Notes
- **GPIO Pin**: Ensure the chosen pin (e.g., GPIO 15) supports PWM. Check your board's documentation.
- **Power Supply**: If the servo jitters, use an external 5V power source for the servo, ensuring a common ground with the microcontroller.
- **Calibration**: The duty cycle range (25-125) may need slight adjustments for precise angles depending on your servo.
- **Error Handling**: The script handles invalid inputs and ensures clean shutdown.

## Contributing
Feel free to fork this repository, make improvements, and submit pull requests. Suggestions for additional features (e.g., multiple servo control or angle presets) are welcome!

## License
This project is licensed under the MIT License.