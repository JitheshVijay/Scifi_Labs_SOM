# Scifi_Labs SOM

System-on-Module firmware and hardware definition.

- `SoM_Master_V1.0.ioc` — STM32CubeMX project for the STM32 master.
- `esp32c3 wifi+ble/wifible/` — ESP32-C3 companion sketch: WiFi provisioning
  via WiFiManager plus a BLE GATT server, bridged to the STM32 over UART
  (`Serial1`, 115200 8N1, TX 21 / RX 20).

## Building the ESP32-C3 sketch

Board: **ESP32C3 Dev Module** (esp32 core by Espressif).

Library: **WiFiManager** (tzapu). BLE comes with the ESP32 core.

### Partition scheme — required

WiFiManager and the BLE stack together produce a ~1.33 MB binary, which
**overflows the default 4MB partition scheme** (1.2 MB app):

```
Sketch uses 1361701 bytes (103%) of program storage space. Maximum is 1310720 bytes.
Error during build: text section exceeds available space in board
```

Select a larger app partition before building:

- **Minimal SPIFFS (1.9MB APP with OTA/128KB SPIFFS)** — recommended, keeps OTA (69% used)
- Huge APP (3MB No OTA/1MB SPIFFS) — more headroom, no OTA (43% used)

In the Arduino IDE: *Tools → Partition Scheme*. With `arduino-cli`:

```
arduino-cli compile --fqbn esp32:esp32:esp32c3:PartitionScheme=min_spiffs "esp32c3 wifi+ble/wifible"
```

## BLE

- Service UUID `4fafc201-1fb5-459e-8fcc-c5c9c331914b`
- Characteristic UUID `beb5483e-36e1-4688-b7f5-ea07361b26a8`
