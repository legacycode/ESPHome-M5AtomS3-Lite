---
title: M5Stack ATOM S3 Lite
date-published: 2025-11-29 14:22:59 +0100
type: misc
standard: global
board: esp32
difficulty: 2
made-for-esphome: false
project-url: https://github.com/legacycode/ESPHome-M5AtomS3-Lite.git
---

# M5Stack ATOM S3 Lite - ESPHome Configuration

A comprehensive ESPHome configuration for the M5Stack ATOM S3 Lite, a compact ESP32-S3 development board with built-in RGB LED, button, and IR transmitter.

## 📑 Table of Contents

- [Overview](#-overview)
- [Hardware Specifications](#-hardware-specifications)
- [Features](#-features)
- [Quick Start](#-quick-start)
- [Configuration](#-configuration)
- [Use Cases & Examples](#-use-cases--examples)
- [GPIO Pinout](#-gpio-pinout)
- [Troubleshooting](#-troubleshooting)
- [Documentation](#-documentation)
- [Contributing](#-contributing)
- [License](#-license)
- [Acknowledgments](#-acknowledgments)

## 📋 Overview

This project provides a ready-to-use ESPHome configuration for the M5Stack ATOM S3 Lite, making it easy to integrate this versatile device into your smart home setup. The configuration exposes all hardware features and can be easily customized for various use cases.

<img src="resources/m5stack-atoms3-lite-overview.webp" alt="M5Stack ATOM S3 Lite" width="400px">

## 🔧 Hardware Specifications

**M5Stack ATOM S3 Lite**

- **Microcontroller**: ESP32-S3-FN8 (8MB Flash, 512KB SRAM)
- **Connectivity**: Wi-Fi 802.11 b/g/n, USB Type-C (USB-CDC)
- **Built-in Features**:
  - WS2812 RGB LED (GPIO35) - Located beneath the button, fully RGB controllable
  - Programmable Button (GPIO41)
  - IR Transmitter LED (GPIO4)
  - Grove Port for external sensors (GPIO1, GPIO2)
- **Dimensions**: 24 x 24 x 14 mm
- **Documentation**: [M5Stack ATOM S3 Lite Official Docs](https://docs.m5stack.com/en/core/AtomS3%20Lite)

## ✨ Features

### Integrated Components

- ✅ **RGB LED Control** - One addressable WS2812 LED located beneath the button, fully RGB controllable with effects (pulse, strobe, random)
- ✅ **Button Input** - Built-in button with debouncing and IR trigger example
- ✅ **IR Transmitter** - Control IR devices (TVs, ACs, etc.)
- ✅ **Status Reporting** - Device online/offline status

### Home Assistant Integration

- ✅ Native API with encryption
- ✅ OTA (Over-The-Air) updates
- ✅ Web server for standalone control
- ✅ Captive portal for easy WiFi setup
- ✅ Fallback hotspot mode

### Available Entities

- `LED` - RGB LED control (WS2812 addressable LED)
- `Button` - Button press sensor with IR transmitter example
- `Status` - Device availability sensor (online/offline)

## 🚀 Quick Start

You can copy the configuration code directly from the [`m5stack-atoms3-lite.yaml`](m5stack-atoms3-lite.yaml) file and configure your secrets in ESPHome (top right corner: Secrets icon).

### Prerequisites

1. **ESPHome** (v2025.11.2 or newer)

   ```bash
   pip install --upgrade esphome
   ```

2. **Hardware**
   - M5Stack ATOM S3 Lite
   - USB-C cable
   - (Optional) Home Assistant for integration

### Installation

1. **Clone this repository**

   ```bash
   git clone https://github.com/legacycode/ESPHome-M5AtomS3-Lite.git
   cd ESPHome-M5AtomS3-Lite
   ```

2. **Configure secrets**

   ```bash
   cp secrets.yaml.example secrets.yaml
   # Edit secrets.yaml with your WiFi credentials and keys
   ```

3. **Customize the configuration** (optional)

   Edit `m5stack-atoms3-lite.yaml` and change the substitutions:

   ```yaml
   substitutions:
     devicename: "your-device-name"
     friendly_name: "Your Device Name"
   ```

4. **Flash the device**

   First time (via USB):

   ```bash
   esphome run m5stack-atoms3-lite.yaml
   ```

   Updates (via OTA):

   ```bash
   esphome run m5stack-atoms3-lite.yaml
   ```

5. **Add to Home Assistant**

   The device should be auto-discovered. Go to:
   - Settings → Devices & Services → ESPHome
   - Click "Configure" on the discovered device
   - Enter your encryption key from `secrets.yaml`

   Once configured, you'll see the following controls in Home Assistant:

   <img src="resources/m5stack-atoms3-lite-homeassistant-control.jpg" alt="M5Stack ATOM S3 Lite Home Assistant Controls" width="400px">

   **Available Controls:**
   - **LED** - Control LED color, brightness, and effects
   - **Button** - View button press state
   - **Status** - Connection status (Connected/Disconnected)

## 📝 Configuration

Here is the complete `m5stack-atoms3-lite.yaml` configuration file:

```yaml
# ============================================================================
# M5Stack ATOM S3 Lite - ESPHome Configuration
# ============================================================================
# This is a complete configuration for the M5Stack ATOM S3 Lite board.
# The ATOM S3 Lite is a tiny ESP32-S3 board with a RGB LED, a button, and IR.

# Substitutions: Variables that can be reused throughout this configuration
# Think of these as "find and replace" - everywhere you see ${devicename},
# it will be replaced with the value defined here
substitutions:
  # The internal name used by ESPHome (use lowercase, no spaces, hyphens OK)
  devicename: "m5stack-atoms3-lite"

  # The friendly name shown in Home Assistant (can have spaces and capitals)
  friendly_name: "M5Stack ATOM S3 Lite"

  # Description of what this device does
  device_comment: "M5Stack ATOM S3 Lite - Multi-purpose ESPHome Device"

  # Tip: Change these to match your use case, for example:
  #   devicename: "living-room-sensor"
  #   friendly_name: "Living Room Sensor"
  #   device_comment: "Temperature and humidity sensor for living room"

# ============================================================================
# Core ESPHome Configuration
# ============================================================================
esphome:
  # Uses the devicename variable defined above (m5stack-atoms3-lite)
  name: ${devicename}

  # Uses the friendly_name variable defined above
  friendly_name: ${friendly_name}

  # Uses the device_comment variable defined above
  comment: ${device_comment}

  # Don't add MAC address to the device name (set to true if you have multiple identical devices)
  name_add_mac_suffix: false

# ============================================================================
# ESP32 Hardware Configuration
# ============================================================================
esp32:
  # Tells ESPHome which board you're using (ATOM S3 Lite uses ESP32-S3)
  board: esp32-s3-devkitc-1

  # Which framework to use for compiling (Arduino is easier for beginners)
  framework:
    type: arduino

# ============================================================================
# Logger - Serial Output for Debugging
# ============================================================================
# The logger lets you see what's happening on your device in real-time
logger:
  # baud_rate: 0 is REQUIRED for ESP32-S3 (it uses USB-CDC, not traditional UART)
  baud_rate: 0

  # How much detail to show in logs (DEBUG shows everything, INFO shows less)
  level: DEBUG

# ============================================================================
# Home Assistant API - Communication with Home Assistant
# ============================================================================
# This enables your device to talk to Home Assistant
api:
  # Encryption protects the communication between device and Home Assistant
  encryption:
    # The encryption key is stored in secrets.yaml for security
    key: !secret encryption_key

# ============================================================================
# OTA Updates - Update Firmware Over WiFi
# ============================================================================
# OTA (Over-The-Air) lets you update the device wirelessly without USB cable
ota:
  # Use ESPHome's built-in OTA system
  platform: esphome

  # Password protects OTA updates (stored in secrets.yaml)
  password: !secret ota_password

# ============================================================================
# WiFi Configuration
# ============================================================================
# Configure WiFi connection to your home network
wifi:
  # Your WiFi network name (SSID) - stored in secrets.yaml
  ssid: !secret wifi_ssid

  # Your WiFi password - stored in secrets.yaml
  password: !secret wifi_password

  # Fallback Access Point - If WiFi fails, device creates its own hotspot
  ap:
    # The name of the fallback hotspot that will appear (max 32 characters)
    ssid: "${friendly_name} Fallback"

    # Password for the fallback hotspot - stored in secrets.yaml
    password: !secret ap_password

    # How long to wait before activating fallback mode (15 seconds)
    ap_timeout: 15s

# ============================================================================
# Web Server - Control Device from Web Browser
# ============================================================================
# This creates a web interface at http://[device-ip-address]/
web_server:
  # The web server runs on port 80 (default HTTP port)
  port: 80

# ============================================================================
# Time - Synchronize Clock with Home Assistant
# ============================================================================
# Gets the current time from Home Assistant (needed for time-based automations)
time:
  # Use Home Assistant as the time source
  - platform: homeassistant

    # Internal ID to reference this time component
    id: homeassistant_time

# ============================================================================
# Captive Portal - Easy WiFi Setup
# ============================================================================
# When in fallback mode, this shows a configuration page automatically
# (like when you connect to hotel WiFi and get a popup)
captive_portal:

# ============================================================================
# Colorful RGB LED
# ============================================================================
# The ATOM S3 Lite has a RGB LED built-in on GPIO35
# Documentation: https://esphome.io/components/light/fastled
light:
  # Use FastLED library for addressable LED control
  - platform: fastled_clockless

    # WS2812B is the type of LED chip used
    chipset: WS2812B

    # The LED is connected to GPIO35 on the board
    pin: GPIO35

    # There is a single LED on the board
    num_leds: 1

    # GRB is the color order for this specific LED (Green-Red-Blue)
    rgb_order: GRB

    # Internal ID to reference this LED in automations
    id: led

    # The name that appears in Home Assistant
    name: "${friendly_name} LED"

    # When device restarts, LED starts OFF
    restore_mode: RESTORE_DEFAULT_OFF

    # Built-in light effects you can activate from Home Assistant
    effects:
      # Pulse effect: LED fades in and out smoothly
      - pulse:
          name: "Pulse"
          # How long each fade in/out takes (1 second)
          transition_length: 1s
          # How often to update the effect (1 second)
          update_interval: 1s

      # Strobe effect: LED flashes on and off rapidly
      - strobe:
          name: "Strobe"

      # Random effect: LED changes to random colors
      - random:
          name: "Random"
          # How long to transition between colors (5 seconds)
          transition_length: 5s
          # How often to pick a new random color (3 seconds)
          update_interval: 3s

# ============================================================================
# IR Remote Transmitter - Infrared LED for Remote Control
# ============================================================================
# The ATOM S3 Lite has a built-in IR LED on GPIO4 for controlling TVs, etc.
# Documentation: https://esphome.io/components/remote_transmitter/
remote_transmitter:
  # The IR LED is connected to GPIO4
  pin: GPIO4

  # Carrier duty cycle controls IR signal strength (50% is standard)
  carrier_duty_percent: 50%

  # Internal ID to reference the IR transmitter in automations
  id: ir_transmitter

  # Non-blocking allows other code to run while IR signal is being sent
  non_blocking: true

# ============================================================================
# Binary Sensors - On/Off Sensors
# ============================================================================
binary_sensor:
  # Status Sensor - Shows if device is online or offline in Home Assistant
  - platform: status
    # The name that appears in Home Assistant
    name: "${friendly_name} Status"

  # Physical Button - The built-in button on GPIO41
  - platform: gpio
    # The name that appears in Home Assistant
    name: "${friendly_name} Button"

    # Button configuration
    pin:
      # The button is connected to GPIO41
      number: GPIO41

      # Inverted means pressed=LOW, released=HIGH (common for buttons with pullup)
      inverted: true

      # Pin mode configuration
      mode:
        # Configure as input (reads signal)
        input: true

        # Enable internal pullup resistor (keeps pin HIGH when button not pressed)
        pullup: true

    # Filters clean up the button signal
    filters:
      # Debouncing: Ignore button bouncing for 10 milliseconds
      - delayed_off: 10ms

    # Action to perform when button is pressed
    on_press:
      then:
        # Send Samsung TV power signal via IR LED
        - remote_transmitter.transmit_samsung:
            # Samsung TV power code (change this for other devices)
            data: 0xE0E040BF

        # Write a message to the logs for debugging
        - logger.log: "Samsung TV Power Signal sent"
```

## 🎯 Use Cases & Examples

### Example 1: IR Remote Control

Use as a universal remote control for TVs, ACs, and other IR devices.

```yaml
# The button is already configured to send Samsung TV power command
# Customize the IR code in the configuration:
on_press:
  then:
    - remote_transmitter.transmit_samsung:
        data: 0xE0E040BF  # Change this to your device code
```

Common Samsung TV codes:

- Power: `0xE0E040BF` or `0xE0E09966`
- Volume Up: `0xE0E0E01F`
- Volume Down: `0xE0E0D02F`
- Mute: `0xE0E0F00F`

### Example 2: Status Indicator

Use the RGB LED to show system status, notifications, or alerts.

```yaml
# Add to Home Assistant automations
service: light.turn_on
target:
  entity_id: light.m5stack_atoms3_lite_led
data:
  brightness: 255
  rgb_color: [255, 0, 0]  # Red for alerts
  effect: "Pulse"
```

## 🔌 GPIO Pinout

<img src="resources/m5stack-atoms3-lite-pinout.jpg" alt="M5Stack ATOM S3 Lite Pinout" width="400px">

### Used Pins in Default Configuration

| GPIO | Function | Description |
|------|----------|-------------|
| GPIO35 | RGB LED | WS2812 addressable LED |
| GPIO41 | Button | Built-in button (active low) |
| GPIO4 | IR TX | Infrared transmitter LED |

### Available Pins for Expansion

| GPIO | Use Case | Notes |
|------|----------|-------|
| GPIO1, GPIO2 | Grove Port | I2C, UART, or GPIO |
| GPIO5, GPIO6, GPIO7, GPIO8 | Expansion | General purpose I/O |
| GPIO38, GPIO39 | Expansion | General purpose I/O |

## 🐛 Troubleshooting

### LED Not Working

**After flashing, LED doesn't respond:**

1. Check logs: `esphome logs m5stack-atoms3-lite.yaml`
2. Look for RMT errors or GPIO conflicts
3. Hard reset the device (unplug and reconnect)
4. Reflash: `esphome run m5stack-atoms3-lite.yaml`

### IR Transmitter Range Issues

**IR commands not reaching target:**

- Ensure direct line-of-sight (1-3 meters max)
- Test with phone camera (IR LED visible as purple light)
- Consider external IR LED for longer range
- Adjust carrier duty cycle: `carrier_duty_percent: 30%`

## 📖 Documentation

### Hardware Resources

- [M5Stack ATOM S3 Lite Docs](https://docs.m5stack.com/en/core/AtomS3%20Lite)
- [M5Stack ATOM S3 Lite Datasheet](resources/m5stack-atoms3-lite-datasheet.pdf)
- [ESP32-S3 Datasheet](resources/esp32-s3-datasheet.pdf)
- [Schematic](resources/m5stack-atoms3-lite-schematic.pdf)
- [Pinout Diagram](resources/m5stack-atoms3-lite-pinout.jpg)
- [SY8089 Power IC Datasheet](resources/sy8089-power-ic-datasheet.pdf)

### ESPHome Resources

- [ESPHome Documentation](https://esphome.io/)
- [ESP32 Platform](https://esphome.io/components/esp32.html)
- [FastLED Light Component](https://esphome.io/components/light/fastled.html)
- [Remote Transmitter](https://esphome.io/components/remote_transmitter.html)

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- Based on [wildekek/ESPHome-M5Stack-Atom-S3-Lite](https://github.com/wildekek/ESPHome-M5Stack-Atom-S3-Lite)
- M5Stack for creating the ATOM S3 Lite hardware
- ESPHome community for the excellent framework

---

**Note**: This is a community project and not officially affiliated with M5Stack or ESPHome.
