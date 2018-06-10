!!! note "Note"
    These instructions are for the upcoming 2.x rebase branch. For instructions
    on the legacy 1.x base of XQEMU, please see the GitHub wiki.

macOS Build
-----------
Clone the repo
    
    git clone -b xbox-2.x-rebase https://github.com/xqemu/xqemu.git

Then change directory

    cd xqemu

Use the build script to build:

	./build_macos.sh

Windows Build
-------------
Start by installing and setting up [MSYS2](https://www.msys2.org/). Please
follow the instructions on this website for updating the environment.

!!! attention    
    After installing MSYS2, you'll need to open **MSYS2 MinGW 64-bit** to
    perform building. Otherwise, you may see failures for cc.exe.

Once that's done you should be able to install all of the needed packages by running...

    pacman -S git python2 make autoconf automake-wrapper \
    mingw-w64-x86_64-libtool mingw-w64-x86_64-gcc mingw-w64-x86_64-pkg-config \
    mingw-w64-x86_64-glib2 mingw-w64-x86_64-glew mingw-w64-x86_64-SDL \
    mingw-w64-x86_64-SDL2 mingw-w64-x86_64-pixman

Clone the repo
    
    git clone -b xbox-2.x-rebase https://github.com/xqemu/xqemu.git

Then change directory

    cd xqemu

And run

    sh ./build_windows.sh

Linux Build
-----------
First enable `deb-src` via:

    sudo gedit /etc/apt/sources.list

In this file, uncomment first `deb-src` line. Now refresh packages:

    sudo apt-get update

Install build deps:

    sudo apt-get build-dep qemu 
    sudo apt-get install git libsdl2-dev libglew-dev

Then clone the repo
    
    git clone -b xbox-2.x-rebase https://github.com/xqemu/xqemu.git

Then change directory

    cd xqemu

Use the build script:

    ./build_linux.sh

How to Run
----------
XQEMU is launchable via the command-line interface (though a GUI launcher is in
development!) You can launch with the following command:
    
    ./i386-softmmu/qemu-system-i386 \
        -cpu pentium3 \
        -machine xbox,bootrom=$MCPX \
        -m 64 \
        -bios $BIOS \
        -drive index=0,media=disk,file=$HDD,locked \
        -drive index=1,media=cdrom,file=$DISC \
        -usb -device usb-xbox-gamepad

The Xbox boot animation sequence can be bypassed by adding the
`,short-animation` option to the `-machine` switch above.