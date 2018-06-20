!!! note "Note"
    These instructions are for the upcoming 2.x rebase branch. For instructions
    on the legacy 1.x base of XQEMU, please see the GitHub wiki.

XQEMU currently supports three options for connecting one or more virtual
gamepads:

1. Using an SDL2-supported input device to emulate an Xbox controller
1. Using your PC's keyboard to emulate an Xbox controller
1. Using a real Xbox controller with USB pass-thru (advanced)

And like a real Xbox, you can connect multiple controllers!

In all cases, start by making sure you have the `-usb` option specified on the
XQEMU command line.

## Option 1: Use an SDL2-supported input device

This method is known to work well with Xbox 360 and DualShock 4 controllers,
with little to no setup required (with the exception of installing any required
platform drivers).

When starting XQEMU, simply pass in the following option:

	-device usb-xbox-gamepad-sdl,index=0

If you have multiple gamepads connected to your system, you can change the index
of the connected device by changing `index=X` accordingly.

Multiple gamepads can be connected by specifying the line above multiple times.

## Option 2: Use your PC keyboard

If you do not have access to a real gamepad, you can use your PC's keyboard to
emulate an Xbox gamepad. This works well in a pinch, and for for navigating
through menus.

When starting XQEMU, simply pass in the following option:

	-device usb-xbox-gamepad

If you'd like, you can combine this device with the `usb-xbox-gamepad-sdl`
device to emulate connecting two controllers. The input can't be configured at
the moment but the following buttons are mapped:

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

## Option 3: USB-passthru (advanced)

XQEMU has the option to forward USB Devices from the host to the guest. The
input might be delayed, but it will support all features you'd expect. In theory
even memory units or the communicator should work! You have 2 options to forward
the xbox gamepad.

You can either forward the hub or just the gamepad.

To be able to forward any of the host devices you must take the following steps:

1. Have an [adapter cable (this one has not been tested yet!)](http://www.amazon.com/XBOX-USB-Controller-Converter-Gamepad-Adapter/dp/B00CD0KFU0) or [build one yourself*](http://www.ocmodshop.com/how-to-use-an-xbox-controller-as-a-usb-pc-gamepad/3/)
2. Have libusb installed
3. Find the VID:PID (Vendor and Product ID) of the XID-Hub and/or the internal Gamepad device
4. Make sure that libusb has the necessary permissions

!!! important

	Please do not destroy original controllers. Instead buy an adapter cable, or
	a cheap break-away or extension cable. By cutting it in half you can create
	2 USB adapters: 1. USB to Xbox + 2. Xbox to USB. You can still use your
	adapters as an extension cable for most XIDs (not working with lightguns).

On Linux you can use `lsusb` for step 2. Step 3 involves adding a udev rule on
most linux distributions. The udev rule (/etc/udev/rules.d/999-xbox-
gamepad.rules) for a Controller-S could look like this:

	SUBSYSTEMS=="usb", ATTRS{idVendor}=="045e", ATTRS{idProduct}=="0288", GROUP="users", MODE="660" # Hub
	SUBSYSTEMS=="usb", ATTRS{idVendor}=="045e", ATTRS{idProduct}=="0289", GROUP="users", MODE="660" # Gamepad

#### Hub-Forwarding

To forward the entire hub of the controller we simply have to forward the hub to the emulated xbox.

**Example:**
```
-usb -device usb-host,bus=usb-bus.0,port=3,vendorid=0x45e,productid=0x288
```

#### Gamepad-Forwarding

For Gamepad forwarding we create a virtual hub using QEMU and connect the XID gamepad device to port 2 of the emulated hub.

**Example:**
```
-usb -device usb-hub,bus=usb-bus.0,port=3 -device usb-host,vendorid=0x45e,productid=0x289,bus=usb-bus.0,port=3.2
```

## Advanced Info

### About XID and QEMU USB

The Xbox uses so called [Xbox Input Devices (XID)](http://xboxdevwiki.net/Xbox_Input_Devices).

To connect a device to the virtual Xbox you must specify the driver for the
emulated USB device and the port the device should connect to.

The ports which can be used in XQEMU are:

| Xbox Port | XQEMU USB-Port         |
| :-------: | :--------------------- |
| Player 1  | `bus=usb-bus.0,port=3` |
| Player 2  | `bus=usb-bus.0,port=4` |
| Player 3  | `bus=usb-bus.0,port=1` |
| Player 4  | `bus=usb-bus.0,port=2` |

The XID is usually connected to Port 2 of the XID-hub. So if you have a hub for
Player 1 at `bus=usb-bus.0,port=3`, your gamepad-device would connect to `bus
=usb-bus.0,port=3.2`.

To connect multiple gamepads you can simply specify multiple `-device`.

To find out more about QEMU USB emulation you can read [the QEMU User
Documentation](http://qemu.weilnetz.de/qemu-doc.html#pcsys_005fusb).

### Emulated XID

There is XID emulation in XQEMU which emulates a very basic Duke Xbox Controller
(VID: 0x045e, PID: 0x0202).

To recreate the internal XID hub we use the existing QEMU "usb-hub" device.
The actual XID emulation is provided by the "xbox-gamepad" device.

Example:

	-usb -device usb-hub,bus=usb-bus.0,port=3 -device usb-xbox-gamepad,bus=usb-bus.0,port=3.2

