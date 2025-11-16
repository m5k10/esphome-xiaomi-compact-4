## About

This project uses standard ESPHome components to replace original Xiaomi firmware.

## How to use

### Compilation
 See ESPHome documentation about [packages](https://esphome.io/components/packages/), if you don't know how to use it.
 
Minimal working config:
```yaml
packages:
  main: 
    url: https://github.com/m5k10/esphome-xiaomi-compact-4
    files: 
      - xiaomi-compact-4.yaml

esphome:
  name: xiaomi-compact-4
  friendly_name: Xiaomi Compact 4

wifi:
  id: wifiClient
  ssid: !secret wifi_ssid
  password: !secret wifi_password
```

### Flashing
The device contains an ESP32-WROOM-32D module on the main PCB.
1. To replace original firmware, you need to disassemble the device and connect serial adapter to the pins next to ESP32:

![Serial port](resources/serial.jpg)

To activate the bootloader mode, you need to short GPIO0 to the GND while powering up the device.
2. Backup your original firmware in case you need to restore it:
```shell
esptool --port /dev/ttyUSB0 --baud 115200 read_flash 0x00000 0x400000 backup_xiaomi_compact_4.bin
```
3. Write ESPHome firmware to the flash. You can use ESPHome dashboard for that. The other option is to use `esptool`:
```shell
esptool --port /dev/ttyUSB0 --baud 115200 write-flash 0x00000  my_firmware.bin
```
4. At this stage, ESPHome OTA updates should be functional. 
