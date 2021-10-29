# Sources
- https://github.com/fidian/bonkers
- https://github.com/Simon-Tang/py_dream_cheeky/blob/master/py_dream_cheeky/button.py
- https://github.com/miguelmota/big-red-button

# Install

## Libraries
```bash
sudo apt install libusb-dev
pip3 install evdev
pip3 install pyusb
```

## Identify the device
```bash
lsusb | grep Button
```

> Bus 003 Device 007: ID 1d34:000d Dream Cheeky Dream Cheeky Big Red Button

Meaning:
- Bus 3 
- Device 007
- living at: /dev/bus/usb/003/007
- BUTTON_VENDOR_ID = 0x1d34
- BUTTON_PRODUCT_ID = 0x000d

## Change udev permissions to use USB device
From: https://github.com/fidian/bonkers/blob/master/doc/99-bonkers.rules

Add the following to a new udev rule:

```bash
ACTION=="add", SUBSYSTEM=="usb", ATTR{idVendor}=="1d34", ATTR{idProduct}=="000d", MODE="0666"
```

```bash
sudo nano /etc/udev/rules.d/99-button.rules
sudo udevadm control --reload-rules
```

Or be lazy:


```bash
sudo sh -c "echo 'SUBSYSTEM==\"usb\", ATTRS{idVendor}==\"1d34\", ATTRS{idProduct}==\"000d\", MODE=\"0666\", GROUP=\"plugdev\"' >> /etc/udev/rules.d/99-dream_cheeky.rules"
```

**THEN REPLUG THE DEVICE!!!**


## Change EVDEV permissions to send keystrokes
```bash
sudo chmod +0666 /dev/uinput
```

# Usage
## Adapt the events in __main__
list of evdev codes should be in 
`/usr/include/linux/input-event-codes.h`

## run

```bash
python3 button.py
```

