# AirCube Firmware

## Dev Setup

Requires ESP-IDF >= 5.0.0 and the ESP32-H2 toolchain.

### macOS (via Homebrew)

```bash
brew bundle          # from repo root — installs cmake, dfu-util, python, eim, esptool
eim install          # installs ESP-IDF; note the printed path to export.sh
```

### Linux / Windows

Follow the official ESP-IDF installation guide:
https://docs.espressif.com/projects/esp-idf/en/stable/esp32h2/get-started/

### All platforms — after IDF is installed

```bash
source <IDF_PATH>/export.sh   # or export.ps1 on Windows

cd firmware
idf.py set-target esp32h2
idf.py build
```

## Flash & Monitor

```bash
idf.py -p <PORT> flash monitor
```

Press `Ctrl+]` to exit the monitor.

## Zigbee TX Power

AirCube exposes a build-time Zigbee TX power setting:

- `AirCube Configuration` -> `Zigbee TX power (dBm)` (`CONFIG_AIRCUBE_ZB_TX_POWER_DBM`)
- Default: `10 dBm`
- Supported menuconfig range: `-24` to `20 dBm`

Set it with:

```bash
idf.py menuconfig
```

Then build and flash as usual. At boot, the firmware logs requested and applied TX power.

Higher values can improve edge-link reliability, but actual power may be limited by hardware,
SDK behavior, and regional regulatory limits.

## Troubleshooting

* Program upload failure

    * Hardware connection is not correct: run `idf.py -p PORT monitor`, and reboot your board to see if there are any output logs.
    * The baud rate for downloading is too high: lower your baud rate in the `menuconfig` menu, and try again.
