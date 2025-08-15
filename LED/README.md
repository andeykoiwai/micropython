# MicroPython LED Control Exercises

This repository contains MicroPython example scripts for controlling LEDs on a microcontroller (e.g., ESP32, ESP8266, or Raspberry Pi Pico). The project includes two main exercises:
1. **Individual LED Control**: Turn on/off 10 LEDs individually using serial commands like `pin 1 on` or `pin 1 off`.
2. **LED Timer Mode**: Automatically turn LEDs on and off based on a timer with configurable durations.

These exercises are designed for beginners and intermediate learners to understand GPIO control and timing in MicroPython.

## Features
- Control 10 LEDs connected to GPIO pins via serial console commands.
- Supports commands in the format `pin <number> <on/off>` (e.g., `pin 1 on`).
- Timer mode to cycle LEDs on and off with user-defined durations.
- Error handling for invalid commands or pin numbers.
- Modular and well-commented code for easy learning and modification.

## Prerequisites
- **Hardware**:
  - Microcontroller with MicroPython support (e.g., ESP32, ESP8266, Raspberry Pi Pico).
  - 10 LEDs with appropriate current-limiting resistors (e.g., 220-330Ω).
  - Jumper wires and a breadboard.
  - Power supply (USB or external, depending on the board).
- **Software**:
  - MicroPython firmware installed on the microcontroller.
  - A serial terminal tool (e.g., Thonny, uPyCraft, or PuTTY) with a baud rate of 115200.

## Hardware Setup
1. Connect 10 LEDs to PWM-capable GPIO pins (e.g., GPIO 0 to GPIO 9):
   - **Anode (longer leg)**: Connect to a GPIO pin via a 220-330Ω resistor.
   - **Cathode (shorter leg)**: Connect to a ground (GND) pin.
2. Ensure the microcontroller and LEDs share a common ground.
3. Verify that the selected GPIO pins are available and not reserved by your board.

## Installation
1. Install MicroPython firmware on your microcontroller (see [MicroPython's official guide](https://micropython.org/download/)).
2. Clone or download this repository:
   ```bash
   git clone https://github.com/your-username/micropython-led-exercises.git
   ```
3. Copy the `main.py` script (below) to your microcontroller using Thonny or uPyCraft.

## Example Code
The following script implements both the individual LED control and timer mode:

```python
from machine import Pin
import time

# Initialize 10 LEDs on GPIO pins 0 to 9
leds = [Pin(i, Pin.OUT) for i in range(10)]

def parse_command(command):
    # Parse command like "pin 1 on" or "pin 1 off"
    parts = command.lower().split()
    if len(parts) != 3 or parts[0] != 'pin':
        return None, None
    try:
        pin_num = int(parts[1])
        state = parts[2]
        if pin_num < 1 or pin_num > 10 or state not in ['on', 'off']:
            return None, None
        return pin_num - 1, state == 'on'
    except ValueError:
        return None, None

def timer_mode(duration_on, duration_off, cycles):
    # Timer mode: cycle all LEDs on/off for specified cycles
    print(f"Starting timer mode: {cycles} cycles, {duration_on}s on, {duration_off}s off")
    for _ in range(cycles):
        for led in leds:
            led.on()
        time.sleep(duration_on)
        for led in leds:
            led.off()
        time.sleep(duration_off)
    print("Timer mode finished")

# Main loop
try:
    while True:
        command = input("Enter command (e.g., 'pin 1 on', 'timer 1 1 5', or 'exit'): ")
        if command.lower() == 'exit':
            print("Exiting program...")
            break
        
        # Check for timer mode command (e.g., "timer 1 1 5")
        if command.lower().startswith('timer'):
            parts = command.split()
            if len(parts) == 4:
                try:
                    duration_on = float(parts[1])
                    duration_off = float(parts[2])
                    cycles = int(parts[3])
                    timer_mode(duration_on, duration_off, cycles)
                except ValueError:
                    print("Invalid timer command. Use: timer <on_duration> <off_duration> <cycles>")
            else:
                print("Invalid timer command. Use: timer <on_duration> <off_duration> <cycles>")
            continue
        
        # Parse pin control command
        pin_num, state = parse_command(command)
        if pin_num is not None and state is not None:
            leds[pin_num].value(state)
            print(f"Pin {pin_num + 1} set to {'on' if state else 'off'}")
        else:
            print("Invalid command. Use: pin <1-10> <on/off> or timer <on_duration> <off_duration> <cycles>")

except KeyboardInterrupt:
    print("\nProgram terminated by user")
finally:
    # Turn off all LEDs on exit
    for led in leds:
        led.off()
    print("All LEDs turned off")
```

## Usage
1. Upload the `main.py` script to your microcontroller.
2. Open the serial console (baud rate: 115200).
3. Use the following commands:
   - **Individual LED Control**: Enter `pin <number> <on/off>` (e.g., `pin 1 on` to turn on LED on GPIO 0, or `pin 1 off` to turn it off).
   - **Timer Mode**: Enter `timer <on_duration> <off_duration> <cycles>` (e.g., `timer 1 1 5` to turn all LEDs on for 1 second, off for 1 second, for 5 cycles).
   - **Exit**: Type `exit` to stop the program.
4. Invalid commands will prompt an error message.

## Code Explanation
- **LED Initialization**:
  - `leds = [Pin(i, Pin.OUT) for i in range(10)]`: Configures GPIO pins 0-9 as outputs for 10 LEDs.
- **Command Parser (`parse_command`)**:
  - Parses serial input like `pin 1 on` to extract pin number and state.
  - Validates pin number (1-10) and state (`on` or `off`).
- **Timer Mode (`timer_mode`)**:
  - Cycles all LEDs on for `duration_on` seconds and off for `duration_off` seconds, repeating for `cycles` times.
- **Main Loop**:
  - Reads user input via `input()`.
  - Handles `pin` commands to control individual LEDs or `timer` commands for automated cycling.
  - Exits on `exit` command or `Ctrl+C`.
- **Cleanup**:
  - Turns off all LEDs when the program ends using a `finally` block.

## Notes
- **GPIO Pins**: Ensure GPIO 0-9 are available and PWM-capable on your board. Adjust pin numbers in the code if needed.
- **Resistors**: Always use current-limiting resistors (220-330Ω) with LEDs to prevent damage.
- **Power Supply**: If LEDs flicker, consider an external power source for the LEDs, ensuring a common ground.
- **Customization**: Modify the script to change pin assignments, add more LEDs, or adjust timer behavior.

## Contributing
Contributions are welcome! To contribute:
1. Fork this repository.
2. Add new features (e.g., PWM dimming, LED patterns) or improve documentation.
3. Submit a pull request with a clear description of your changes.

## License
This project is licensed under the MIT License.