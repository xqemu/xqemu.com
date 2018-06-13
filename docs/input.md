# About XID and QEMU USB

The Xbox uses so called [Xbox Input Devices (XID)](http://xboxdevwiki.net/Xbox_Input_Devices).

To connect a device to the virtual Xbox you must specify the driver for the emulated USB device and the port the device should connect to.
You also need to specify `-usb` on the command line to add usb functionality.

The ports which can be used in XQEMU are:

| Xbox Port | XQEMU USB-Port         |
| :-------: | :--------------------- |
| Player 1  | `bus=usb-bus.0,port=3` |
| Player 2  | `bus=usb-bus.0,port=4` |
| Player 3  | `bus=usb-bus.0,port=1` |
| Player 4  | `bus=usb-bus.0,port=2` |

The XID is usually connected to Port 2 of the XID-hub.
So if you have a hub for Player 1 at `bus=usb-bus.0,port=3`, your gamepad-device would connect to `bus=usb-bus.0,port=3.2`.

To connect multiple gamepads you can simply specify multiple `-device`.

To find out more about QEMU USB emulation you can read [the QEMU User Documentation](http://qemu.weilnetz.de/qemu-doc.html#pcsys_005fusb) (Note that XQEMU is based on QEMU 1.7 at this time while the Documentation is for the more recent QEMU 2.4.0+)

## Emulated XID

There is XID emulation in XQEMU which emulates a very basic Duke Xbox Controller [VID: 0x045e, PID: 0x0202].
The input can't be configured at the moment but the following buttons are mapped:

| Xbox        | PC Keyboard     |
| ----------: | :-------------- |
| ![A](https://upload.wikimedia.org/wikipedia/commons/d/d2/Xbox_button_A.svg) | <kbd>S</kbd> |
| ![B](https://upload.wikimedia.org/wikipedia/commons/b/b8/Xbox_button_B.svg) | <kbd>D</kbd> |
| ![X](https://upload.wikimedia.org/wikipedia/commons/8/8c/Xbox_button_X.svg) | <kbd>W</kbd> |
| ![Y](https://upload.wikimedia.org/wikipedia/commons/d/df/Xbox_button_Y.svg) | <kbd>E</kbd> |
| White       | <kbd>X</kbd> |
| Black       | <kbd>C</kbd> |
| Start       | <kbd>Return</kbd> |
| Back        | <kbd>Backspace</kbd> |
| DPad-Up     | <kbd>&uarr;</kbd> |
| DPad-Down   | <kbd>&darr;</kbd> |
| DPad-Left   | <kbd>&larr;</kbd> |
| DPad-Right  | <kbd>&rarr;</kbd> |
| Left Trigger | <kbd>Q</kbd> |
| Right Trigger | <kbd>R</kbd> |
| Left-Thumbstick-Up | <kbd>T</kbd> |
| Left-Thumbstick-Down | <kbd>G</kbd> |
| Left-Thumbstick-Left | <kbd>F</kbd> |
| Left-Thumbstick-Right | <kbd>H</kbd> |
| Left-Thumbstick-Press | <kbd>V</kbd> |
| Right-Thumbstick-Up | <kbd>I</kbd> |
| Right-Thumbstick-Down | <kbd>K</kbd> |
| Right-Thumbstick-Left | <kbd>J</kbd> |
| Right-Thumbstick-Right | <kbd>L</kbd> |
| Right-Thumbstick-Press | <kbd>M</kbd> |

There is no force feedback indicator yet.

To recreate the internal XID hub we use the existing QEMU "usb-hub" device.
The actual XID emulation is provided by the "xbox-gamepad" device.

**Example:**
```
-usb -device usb-hub,bus=usb-bus.0,port=3 -device usb-xbox-gamepad,bus=usb-bus.0,port=3.2
```

## USB Forwarding

QEMU has the option to forward USB Devices from the host to the guest.
The input might be delayed but it will support all features you'd expect.
In theory even memory units or the communicator should work!
You have 2 options to forward the xbox gamepad.

You can either forward the hub or just the gamepad.

To be able to forward any of the host devices you must take the following steps:

1. Have an [adapter cable (this one has not been tested yet!)](http://www.amazon.com/XBOX-USB-Controller-Converter-Gamepad-Adapter/dp/B00CD0KFU0) or [build one yourself*](http://www.ocmodshop.com/how-to-use-an-xbox-controller-as-a-usb-pc-gamepad/3/)
2. Have libusb installed
3. Find the VID:PID (Vendor and Product ID) of the XID-Hub and/or the internal Gamepad device
4. Make sure that libusb has the necessary permissions

<sup>\* Please do not destroy original controllers. Instead buy a cheap break-away or extension cable. By cutting it in half you can create 2 USB adapters: 1. USB to Xbox + 2. Xbox to USB. You can still use your adapters as an extension cable for most XIDs (not working with lightguns).</sup>

On Linux you can use "lsusb" for step 2. Step 3 involves adding a udev rule on most linux distributions.
The udev rule (/etc/udev/rules.d/999-xbox-gamepad.rules) for a Controller-S could look like this:
```
SUBSYSTEMS=="usb", ATTRS{idVendor}=="045e", ATTRS{idProduct}=="0288", GROUP="users", MODE="660" # Hub
SUBSYSTEMS=="usb", ATTRS{idVendor}=="045e", ATTRS{idProduct}=="0289", GROUP="users", MODE="660" # Gamepad
```

### Hub-Forwarding

To forward the entire hub of the controller we simply have to forward the hub to the emulated xbox.

**Example:**
```
-usb -device usb-host,bus=usb-bus.0,port=3,vendorid=0x45e,productid=0x288
```

### Gamepad-Forwarding

For Gamepad forwarding we create a virtual hub using QEMU and connect the XID gamepad device to port 2 of the emulated hub.

**Example:**
```
-usb -device usb-hub,bus=usb-bus.0,port=3 -device usb-host,vendorid=0x45e,productid=0x289,bus=usb-bus.0,port=3.2
```

# Contribute

If you are a developer you can also check out the [XID emulation source code](../blob/xbox/hw/xbox/xid.c).
You could write a new driver to turn connected Xbox 360 gamepads into original Xbox XIDs for example.