# Python API for Bluefruit LE Sniffer 

This repository contains the Python API for Adafruit's Bluefruit LE Sniffer, and our easy to use API wrapper.
It can also be used on any other Nordic board that can be loaded with the Nordic nRF Sniffer firmware.

It has been tested on the following platforms using Python 2.7:

- Linux Mint Nadia
- OSX should work
- Supporting named pipes on Windows should be easy

## Related Links

[Bluefruit LE Sniffer product page](https://www.adafruit.com/product/2269)

[Bluefruit LE Sniffer Learning Guide](https://learn.adafruit.com/introducing-the-adafruit-bluefruit-le-sniffer/introduction)

[Nordic Semiconductor nRF Sniffer](https://www.nordicsemi.com/eng/Products/Bluetooth-Smart-Bluetooth-low-energy/nRF-Sniffer)

[BLE Sniffer in Linux using Wireshark](https://devzone.nordicsemi.com/blogs/750/ble-sniffer-in-linux-using-wireshark)

# Sniffer Python Wrapper

Running sniffer.py in this folder on the Bluefruit LE Friend Sniffer Edition board will cause the device to scan for Bluetooth LE devices in range, and log any data from the selected device to a libpcap named pipe (in `logs/ble.pipe`) that can be opened in Wireshark.

## Using sniffer.py

To use sniffer.py, simply specify the serial port where the sniffer can be found (ex. `COM14` on Windows, `/dev/tty.usbmodem1412311` on OS X, `/dev/ttyACM0` or Linux, etc.):

```
python sniffer.py /dev/ttyACM0
```

**Note:** You will need to run python with `sudo` on Linux to be able to open the serial port, so `sudo python sniffer.py /dev/ttyACM0`, etc..

This will create a new named pipe and start scanning for BLE devices, which should result in the following menu:

```
$ python sniffer.py /dev/tty.usbmodem1412311
Logging data to logs/capture.pcap
Connecting to sniffer on /dev/tty.usbmodem1412311
Scanning for BLE devices (5s) ...
Found 2 BLE devices:

  [1] "" (14:99:E2:05:29:CF, RSSI = -85)
  [2] "" (E7:0C:E1:BE:87:66, RSSI = -49)

Select a device to sniff, or '0' to scan again
> 
```

Simply select the device you wish to sniff then run Wireshark:

```
wireshark -Y btle -k -i logs/capture.pcap
```

Type **CTRL+C** to stop sniffing and quit the application, closing the libpcap named pipe.
Alternatively stop the Wireshark capture.

**NOTE:** You may need to remove the sniffer and re-insert it before starting a new session if you see any unusual error messages running sniffer.py.

## Requirements

This Python script was written and tested on **Python 2.7.6**, and will require that both Python 2.7 and **pySerial** are installed on your system.

To build Wireshark for Linux follow the instructions at [BLE Sniffer in Linux using Wireshark](https://devzone.nordicsemi.com/blogs/750/ble-sniffer-in-linux-using-wireshark).

**NOTE:** to run Wireshark from the build directory run:
```
WIRESHARK_RUN_FROM_BUILD_DIRECTORY=1 ./wireshark -Y btle -k -i /path/to/Adafruit_BLESniffer_Python/logs/ble.pipe
```

**NOTE:** if Wireshark cannot start try moving your `~\.wireshark`
```
mv ~\.wireshark ~\.wireshark.backup
```

