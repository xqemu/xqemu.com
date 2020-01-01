Welcome, XQEMU Developers!
==========================

Community
---------
Potential XQEMU developers are encouraged to join the
[XboxDev](https://github.com/XboxDev/XboxDev) community, which focuses on
research & development for the original Xbox. General discussion happens on
the [XboxDev Discord Server](https://discord.gg/WxJPPyz).

**XQEMU Development Chat:** The project has a dedicated development-centric
IRC channel at `#xqemu` on irc.freenode.net. The XboxDev Discord server has a
`#xqemu` channel as well, and these channels are bridged together. Developers
are free to chat from both IRC and Discord.

**XQEMU User Chat:** XQEMU user-centric discussion (general, help,
multiplayer, showcase, etc.) takes place on the [XQEMU Discord 
Server](https://discord.gg/4aZYj74).

Repository Info
---------------
This project is hosted on GitHub at [github.com/xqemu/xqemu](https://github.com/xqemu/xqemu).

Build Status
------------
| Platform | Build Status |
|----------|--------------|
| Windows | [![Build status](https://github.com/xqemu/xqemu/workflows/Build%20(Windows)/badge.svg?branch=master)](https://github.com/xqemu/xqemu/actions?query=branch%3Amaster) |
| macOS | [![Build status](https://github.com/xqemu/xqemu/workflows/Build%20(macOS)/badge.svg?branch=master)](https://github.com/xqemu/xqemu/actions?query=branch%3Amaster) |
| Ubuntu | [![Build status](https://github.com/xqemu/xqemu/workflows/Build%20(Ubuntu)/badge.svg?branch=master)](https://github.com/xqemu/xqemu/actions?query=branch%3Amaster) |

Building From Source Code
--------------------
For directions on how to build XQEMU from source, please refer to [this page](building.md).

Debugging Guest Code
--------------------
* QEMU can host a gdb stub! Launch with ```-s -S```, and with gdb run `target remote localhost:1234`
    * Protip: You can also attach to it with [IDA](https://www.hex-rays.com/products/ida/) if you're so inclined. You can then load in a database if you export it as a IDC script!
* XQEMU can emulate a XDK serial port (which with a debug bios hosts KD, as in [this](http://msdn.microsoft.com/en-us/library/hh406279.aspx) and [this](http://www.reactos.org/wiki/Techwiki:Kd))! Launch with something like ```-device lpc47m157 -serial unix:/tmp/xserial,server```. With some effort you can wrestle the unix socket into a vm for with WinDbg. There's also a very barebones perl KD client in scripts/windpl
* [apitrace](https://apitrace.github.io/) is useful for tracking down rendering bugs.

Debugging XQEMU Itself
----------------------
Depending on the task at hand, it may be necessary to debug XQEMU itself.

### Windows
The [Visual Studio Code](https://code.visualstudio.com/) IDE can be used to launch and debug XQEMU. A sample launch.vs.json file which can be used to launch XQEMU from code [can be found here](https://raw.githubusercontent.com/xqemu/xqemu.com/master/samples/launch.vs.json).

### macOS
#### Using Xcode
Create a project, edit the "Scheme" to run the xqemu binary, then click the run
button. Xcode has a nice GUI for analyzing the stack frame and looking at local
variables to quickly track down bugs. You can also attach to running processes.

### Linux
GDB works of course. [Eclipse](https://www.eclipse.org/cdt/) can also be used
for those wanting a graphical source-level debugging solution.

Debug BIOS
----------
People have reported success with the 'COMPLEX 4627' modified debug bios. It's
convenient to note that this bios does not necessarily require a _populated_
hard disk image to load an application from DVD (though an empty drive still
needs to be attached), so you can skip the next step in some cases.

    v1.0.2 1M dump: MD5 (Complex_4627Debug.bin) = 19b5c6d3d42a707bba620634fe6d4baf

_or sometimes_

    1MB dump: MD5 (complex_4627debug.bin) = e8dd61cc6abdbd06aac185e371312dc1
