Depending on the task at hand, it may be necessary to debug XQEMU.

## Debugging XQEMU code

#### Windows
The [Visual Studio Code](https://code.visualstudio.com/) IDE can be used to launch and debug XQEMU. A sample launch.vs.json file which can be used to launch XQEMU from code [can be found here](https://raw.githubusercontent.com/xqemu/xqemu.com/master/samples/launch.vs.json).

#### macOS
##### Using Xcode
Create a project, edit the "Scheme" to run the xqemu binary, then click the run
button. Xcode has a nice GUI for analyzing the stack frame and looking at local
variables to quickly track down bugs. You can also attach to running processes.

#### Linux
GDB works of course. [Eclipse](https://www.eclipse.org/cdt/) can also be used
for those wanting a graphical source-level debugging solution.

## Graphics debugging

Most code in XQEMU belongs to the graphics emulation.
There are a handful of tools which we recommend for certain debugging tasks:

* [apitrace](https://apitrace.github.io/)
* [Nsight Graphics](https://developer.nvidia.com/nsight-graphics)
* [RenderDoc](https://renderdoc.org/)

Which tool you choose depends on the task, your environment, the version of XQEMU and your personal preferences.
apitrace is generally the most compatible, but also weakest tool.

To make most of these tools, you have to change compile time settings in XQEMU code.
To enable use of OpenGL debug extensions, define `DEBUG_NV2A` and `DEBUG_NV2A_GL`.
You can do this by modifying the code in `hw/xbox/nv2a/nv2a_debug.h`.
The OpenGL debug extensions will then be used to annotate OpenGL objects and calls with information about the Xbox guest.

## Comparing against real hardware

You can write unit tests using [nxdk](https://github.com/XboxDev/nxdk) (C/C++), [xboxpy](https://github.com/xboxdev/xboxpy) (Python).
If you have a debug bios, you can also use [ViridiX](https://github.com/XboxDev/ViridiX) (C#).

For graphics debugging, you can compare against physical hardware using [nv2a-trace](https://github.com/XboxDev/nv2a-trace).

For assembling programs for the APU DSPs you can use [a56](https://github.com/xboxdev/a56).
