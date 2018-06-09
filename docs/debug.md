Debugging on macOS
------------------
If your build of Qemu is crashing, I've found it easiest to use the Xcode
debugger to look at stack traces. Fire up Xcode, create a project, edit the
"Scheme" to run the xqemu binary, then click the run button. Xcode has a nice
GUI for analyzing the stack frame and looking at local variables to quickly
track down bugs. You can also attach to running processes.
