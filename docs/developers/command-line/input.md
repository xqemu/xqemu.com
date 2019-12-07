The Xbox uses so called [Xbox Input Devices (XID)](http://xboxdevwiki.net/Xbox_Input_Devices).

In all cases, start by making sure you have the `-usb` option specified on the
XQEMU command line.
To connect multiple gamepads you can simply specify multiple `-device`.

To find out more about QEMU USB emulation you can read [the QEMU User Documentation](http://qemu.weilnetz.de/qemu-doc.html#pcsys_005fusb).

## Connecting a XID

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

To recreate the internal XID hub we use the existing QEMU "usb-hub" device.
The actual XID emulation is provided by the "xbox-gamepad" device.

**Example:**
```
-usb -device usb-hub,bus=usb-bus.0,port=3 -device usb-xbox-gamepad,bus=usb-bus.0,port=3.2
```

## XID emulation

There is XID emulation in XQEMU which emulates a very basic Duke Xbox Controller (VID: 0x045e, PID: 0x0202).
Alternatively, you can forward a physical XID device from the host into the guest.


### Option 1: Use an SDL2-supported input device

When starting XQEMU, simply pass in the following option:

```
-device usb-xbox-gamepad-sdl,index=0
```

If you have multiple gamepads connected to your system, you can change the index
of the connected device by changing `index=X` accordingly.

Multiple gamepads can be connected by specifying the line above multiple times.


### Option 2: Use your PC keyboard

When starting XQEMU, simply pass in the following option:

```
-device usb-xbox-gamepad
```

The keyboard button mapping is described in the [user section](input.md).

## XID USB-passthru (advanced)

XQEMU has the option to forward USB Devices from the host to the guest.
The input might be delayed, but it will support all features you'd expect.
In theory even memory units or the communicator should work!

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
most linux distributions. The udev rule (/etc/udev/rules.d/999-xbox-gamepad.rules) for a Controller-S could look like this:

```
## Hub
SUBSYSTEMS=="usb", ATTRS{idVendor}=="045e", ATTRS{idProduct}=="0288", GROUP="users", MODE="660"
## Gamepad
SUBSYSTEMS=="usb", ATTRS{idVendor}=="045e", ATTRS{idProduct}=="0289", GROUP="users", MODE="660"
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
