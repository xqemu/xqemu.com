As XQEMU is based on QEMU, you can follow [the QEMU User Documentation](http://qemu.weilnetz.de/qemu-doc.html).
However, there are some quirks and new device types introduced by XQEMU which are described here.


## Running XQEMU

!!! important

	The recommended way of launching XQEMU is through XQEMU-Manager.
	See the [user documentation](getting-started.md) for more information.
	
	Developers or advanced users can also retrieve the command line that XQEMU-Manager uses internally.
	This is available in the CLI tab in the settings.
	It also allows you to add additional arguments.

XQEMU emulates a virtual Xbox.
This implies a Pentium 3 based system with a specific set of ROMs and certain peripherals.
It is therefore advised to use the following base-command line:

```
./i386-softmmu/qemu-system-i386 \
    -cpu pentium3 \
    -machine xbox,bootrom=$MCPX \
    -m 64 \
    -bios $BIOS \
    -drive index=0,media=disk,file=$HDD,locked \
    -drive index=1,media=cdrom,file=$DISC \
    -usb -device usb-xbox-gamepad
```

Of course, on Windows the executable path will have a `.exe` extension. If launching
a pre-built binary from AppVeyor, replace `./i386-softmmu/qemu-system-i386` with
`xqemu.exe`.

Replace the variables `$MCPX`, `$BIOS`, `$HDD`, and `$DISC` with the appropriate
file paths or define them as variables in your shell.

The Xbox boot animation sequence can be bypassed by adding the
`,short-animation` option to the `-machine` switch above.


### Development Kits

Development kits are similar to retail units. The base command line can be tweaked to emulate them.

Development kits use an MCPX X2 which does not contain the MCPX ROM.
You can omit the `bootrom` option to skip emulation of the MCPX ROM.

Development kits also use 128MiB of RAM, so you should be using `-m 128` instead of `-m 64`.

Some development kits have had a serial port. This can be emulated in XQEMU.
XQEMU can emulate a XDK serial port (which with a debug bios hosts KD, as in [this](http://msdn.microsoft.com/en-us/library/hh406279.aspx) and [this](http://www.reactos.org/wiki/Techwiki:Kd))!
Launch with something like:
```
-device lpc47m157 -serial unix:/tmp/xserial,server
```
With some effort you can wrestle the unix socket into a vm for with WinDbg.
<!-- There's also a very barebones perl KD client in scripts/windpl --><!-- FIXME: This script is missing since XQEMU2 -->


#### Debug bios

People have reported success with the 'COMPLEX 4627' modified debug bios. It's
convenient to note that this bios does not necessarily require a _populated_
hard disk image to load an application from DVD (though an empty drive still
needs to be attached), so you can skip the next step in some cases.

```
v1.0.2 1M dump: MD5 (Complex_4627Debug.bin) = 19b5c6d3d42a707bba620634fe6d4baf
```

_or sometimes_

```
1MB dump: MD5 (complex_4627debug.bin) = e8dd61cc6abdbd06aac185e371312dc1
```

### Chihiro

Chihiro support in XQEMU is currently broken. It will be reintroduced in the future.


## Useful QEMU options

### Debugging the guest code

QEMU can host a gdb stub!

Launch XQEMU with `-s -S` to start the QEMU gdb stub.

In gdb, connect using `target remote localhost:1234`.

You can also attach to it with [IDA](https://www.hex-rays.com/products/ida/) if you're so inclined.
You can then load in a database if you export it as a IDC script!
