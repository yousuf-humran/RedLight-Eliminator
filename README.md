# RedLight Eliminator

A wearable elimination device for the game "Red Light Green Light", built using the D1 Mini (ESP8266), a MAX7219 Dot Matrix Display, Vibration Motor, Buzzer, and a rechargeable battery.  
With a single button on a hosted Wi-Fi interface, the wearable gives a visual (red light), haptic (vibration), and audio (buzz) response, marking a player as eliminated.

---

## Features

- Wi-Fi-based local control (no internet needed)
- Visual feedback via 8x32 LED matrix
- Instant vibration and buzzer on elimination
- Rechargeable, portable and wearable design
- Simple user interface: click a button → device reacts

---

## Components

| Component                  | Quantity |
|----------------------------|----------|
| D1 Mini (ESP8266)          | 1        |
| MAX7219 Dot Matrix (8x32)  | 1        |
| Vibration Motor            | 1        |
| NPN Transistor (e.g., 2N2222) | 1     |
| Flyback Diode (e.g., 1N4007) | 1      |
| 1kΩ Resistor               | 1        |
| Buzzer (Active recommended)| 1        |
| TP4056 Charging Module     | 1        |
| 600mAh Li-ion Battery      | 1        |
| On/Off Switch (optional)   | 1        |
| Misc: Wires, PCB/Perfboard | –        |

---

## Wiring Diagram (Clear Table Format)

This project uses the D1 Mini (ESP8266) as the core controller, wired with a MAX7219 dot matrix, vibration motor via NPN transistor, buzzer, and a 600mAh Li-ion battery via a TP4056 charging module.

### MAX7219 Dot Matrix Display → D1 Mini

| Dot Matrix Pin | D1 Mini Pin     | Signal Type    | Notes              |
|----------------|------------------|----------------|--------------------|
| VCC            | 5V               | Power          | Use 5V output      |
| GND            | GND              | Ground         | Common ground      |
| DIN            | D7 (GPIO13)      | Data           | SPI Data Input     |
| CS             | D6 (GPIO12)      | Chip Select    | SPI Select Line    |
| CLK            | D5 (GPIO14)      | Clock          | SPI Clock Line     |

### Vibration Motor (with NPN Transistor & Flyback Diode)

| Component              | Connects To             | Notes                                     |
|------------------------|--------------------------|-------------------------------------------|
| Vibration Motor (+)    | 5V                      | Direct power supply                       |
| Vibration Motor (–)    | Collector of NPN        | Ground path through transistor switch     |
| Transistor Emitter     | GND                     | Connect to common ground                  |
| Transistor Base        | D2 (GPIO4) via 1kΩ Resistor | Control signal from D1 Mini          |
| Diode (e.g., 1N4007)   | Across motor terminals (Cathode to +) | Flyback protection      |

### Buzzer (Active Recommended)

| Buzzer Pin  | D1 Mini Pin     | Signal Type    | Notes                       |
|-------------|------------------|----------------|-----------------------------|
| +           | D1 (GPIO5)       | Digital Output | Controlled from code        |
| –           | GND              | Ground         | Shared with system ground   |

### TP4056 Charging Module + 600mAh Battery

| TP4056 Pin | Connects To        | Purpose                    |
|------------|--------------------|----------------------------|
| BAT+       | Battery +          | Positive battery terminal  |
| BAT–       | Battery –          | Negative battery terminal  |
| OUT+       | D1 Mini 5V         | Powers entire system       |
| OUT–       | D1 Mini GND        | Ground line for system     |
| Optional   | Switch between OUT+ and 5V | Manual on/off toggle  |

### Wiring Tips

- All grounds (GND) must be interconnected — Matrix, TP4056, Motor, Buzzer, D1 Mini.
- Use consistent wire color coding for clarity:
  - Red = Power
  - Black = Ground
  - Blue/Yellow/Green = Signal lines

---

## Getting Started

### 1. Flash the Code

#### Dependencies

Install these libraries via Arduino IDE Library Manager:

- [ESPAsyncWebServer](https://github.com/me-no-dev/ESPAsyncWebServer)
- [ESPAsyncTCP](https://github.com/me-no-dev/ESPAsyncTCP)
- [MD_MAX72xx](https://github.com/MajicDesigns/MD_MAX72XX)
- [Adafruit GFX](https://github.com/adafruit/Adafruit-GFX-Library)

> ⚠️ Be sure to use ESP8266-compatible versions.

#### Upload the Code

Use Arduino IDE to flash `RedLightWearable.ino` to your D1 Mini.

---

### 2. Power the Device

- Charge the 600mAh Li-ion battery via the TP4056
- Power flows from battery → TP4056 → D1 Mini 5V

---

### 3. Connect & Trigger

- Device creates a Wi-Fi hotspot:
  - SSID: `RedLightDevice`
  - Password: `12345678`

- Connect to this Wi-Fi and open your browser at: http://192.168.4.1
