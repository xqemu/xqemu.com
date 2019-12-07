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

[apitrace](https://apitrace.github.io/) is useful for tracking down rendering bugs.
