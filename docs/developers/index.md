Repository Info
---------------
This project is hosted on GitHub at [github.com/xqemu/xqemu](https://github.com/xqemu/xqemu).

Build Status
------------
| Windows | Linux | macOS |
| ------- | ----- | ----- |
| [![Build status](https://ci.appveyor.com/api/projects/status/i1m0keigjabxg170/branch/master?svg=true)](https://ci.appveyor.com/project/xqemu-bot/xqemu) | [![Travis-CI Status](https://travis-ci.org/xqemu/xqemu.svg?branch=master)](https://travis-ci.org/xqemu/xqemu) | [![Travis-CI Status](https://travis-ci.org/xqemu/xqemu.svg?branch=master)](https://travis-ci.org/xqemu/xqemu) |

Building From Source Code
--------------------

See [building](/building)

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
