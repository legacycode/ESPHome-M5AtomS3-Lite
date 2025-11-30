# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This repository contains an ESPHome configuration for the M5Stack ATOM S3 Lite, a compact ESP32-S3 development board with built-in RGB LEDs, button, and IR transmitter. The configuration is designed to be community-ready and easily customizable for various IoT projects.

**Project URL**: https://github.com/legacycode/ESPHome-M5AtomS3-Lite.git

## Configuration Files

- **m5stack-atoms3-lite.yaml**: Main device configuration
  - Target platform: ESP32-S3 (esp32-s3-devkitc-1 board)
  - Hardware: M5Stack ATOM S3 Lite
  - Features: RGB LEDs (4x WS2812), button, IR transmitter, web server, OTA updates
  - Integration: Home Assistant API with encryption

- **secrets.yaml.example**: Template for user credentials (not tracked in git)

## Key Architecture Concepts

### Device Configuration Structure
The YAML file follows ESPHome's declarative configuration pattern:

1. **Substitutions**: Define reusable variables at the top
   - `devicename`: Internal name (lowercase, hyphens allowed)
   - `friendly_name`: Display name in Home Assistant
   - `device_comment`: Device description

2. **Platform Setup**:
   - `esphome`: Core configuration (name, friendly_name, comment)
   - `esp32`: Hardware platform (board, framework)

3. **Connectivity**:
   - WiFi with fallback AP mode
   - Home Assistant API with encryption
   - OTA updates for wireless firmware updates
   - Web server on port 80

4. **Hardware Components**:
   - **Light (LED)**: 4x WS2812 RGB LEDs on GPIO35 using FastLED platform
   - **Binary Sensor (Button)**: GPIO41 with pullup, debouncing, triggers IR on press
   - **Remote Transmitter (IR)**: GPIO4 for IR transmission (Samsung TV example)
   - **Status Sensor**: Device online/offline status

### LED Platform
Originally used `esp32_rmt_led_strip`, but switched to `fastled_clockless` to avoid RMT channel conflicts with the IR transmitter. FastLED doesn't use RMT hardware, making it compatible with other components.

### GPIO Pinout
**Used Pins**:
- GPIO35: RGB LED strip (4x WS2812)
- GPIO41: Button (active low, with pullup)
- GPIO4: IR transmitter LED

**Available for Expansion**:
- GPIO1, GPIO2: Grove port (I2C, UART, or GPIO)
- GPIO5, GPIO6, GPIO7, GPIO8: General purpose I/O
- GPIO38, GPIO39: General purpose I/O

## Working with Configurations

### Secrets Management
Configuration uses `!secret` directive for sensitive data:
- `encryption_key`: Home Assistant API encryption (32-byte base64)
- `ota_password`: OTA update password
- `wifi_ssid` / `wifi_password`: WiFi credentials
- `ap_password`: Fallback hotspot password

Copy `secrets.yaml.example` to `secrets.yaml` and fill in your values. The `secrets.yaml` file is gitignored.

### Customizing for Your Project
To adapt this configuration:

1. Edit substitutions at the top:
   ```yaml
   substitutions:
     devicename: "your-device-name"
     friendly_name: "Your Device Name"
     device_comment: "Your device description"
   ```

2. Modify hardware features as needed:
   - Add sensors via Grove port (GPIO1/GPIO2)
   - Change IR codes for different devices
   - Customize LED effects
   - Add automation logic

3. Keep YAML indentation consistent (2 spaces)

### ESPHome CLI Usage
Common commands for this project:
- `esphome compile m5stack-atoms3-lite.yaml`: Validate and compile
- `esphome run m5stack-atoms3-lite.yaml`: Compile, upload, and monitor
- `esphome logs m5stack-atoms3-lite.yaml`: View device logs
- `esphome clean m5stack-atoms3-lite.yaml`: Clean build cache

First-time flashing requires USB connection. Subsequent updates can use OTA.

## Device-Specific Notes

### Hardware Specifications
- **Microcontroller**: ESP32-S3-FN8 (8MB Flash, 512KB SRAM)
- **Connectivity**: Wi-Fi 802.11 b/g/n, USB Type-C (USB-CDC)
- **Dimensions**: 24 x 24 x 14 mm
- **LEDs**: 4x WS2812 RGB (beneath button)
- **Documentation**: https://docs.m5stack.com/en/core/AtomS3%20Lite

### Logger Configuration
**Important**: ESP32-S3 uses USB-CDC, not traditional UART:
```yaml
logger:
  baud_rate: 0  # Required for ESP32-S3
```

### Home Assistant Integration
After flashing:
1. Device auto-discovered in ESPHome integration
2. Configure with encryption key from secrets.yaml
3. Available entities:
   - `light.{devicename}_led`: RGB LED control
   - `binary_sensor.{devicename}_button`: Button state
   - `binary_sensor.{devicename}_status`: Connection status

### Common Issues

**LED not working after flash**:
- Check RMT errors in logs
- Hard reset device (unplug/replug)
- Verify GPIO35 not conflicting with other components

**Device not discovered after Home Assistant update**:
- Remove and re-add device in ESPHome integration
- Ensure device name matches configuration
- Check encryption key matches secrets.yaml

**IR transmitter range issues**:
- Ensure line-of-sight (1-3 meters max)
- Test with phone camera (IR appears purple)
- Adjust `carrier_duty_percent` if needed

## Documentation Resources

### Project Documentation
- README.md: Complete user guide with examples
- secrets.yaml.example: Template for credentials

### Hardware Documentation (in resources/)
- m5stack-atoms3-lite-datasheet.pdf: Device specifications
- m5stack-atoms3-lite-pinout.jpg: GPIO pinout diagram
- m5stack-atoms3-lite-schematic.pdf: Circuit schematic
- esp32-s3-datasheet.pdf: Microcontroller datasheet

### ESPHome Resources
- ESPHome Documentation: https://esphome.io/
- FastLED Component: https://esphome.io/components/light/fastled.html
- Remote Transmitter: https://esphome.io/components/remote_transmitter.html
- ESP32 Platform: https://esphome.io/components/esp32.html
