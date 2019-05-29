Building XQEMU from Source
--------------------------

### Building on Windows

Start by installing and setting up [MSYS2](https://www.msys2.org/).

!!! important

    After installing MSYS2, you'll need to open **MSYS2 MinGW 64-bit** to
    perform building. Otherwise, you may see build failures for cc.exe.

Once MSYS2 has been installed, install all of the necessary packages by running:

    pacman -S git python3 make autoconf automake-wrapper \
    mingw-w64-x86_64-libtool mingw-w64-x86_64-gcc mingw-w64-x86_64-pkg-config \
    mingw-w64-x86_64-glib2 mingw-w64-x86_64-libepoxy \
    mingw-w64-x86_64-SDL2 mingw-w64-x86_64-pixman

!!! important

    Build failures have been reported when the path to the XQEMU root directory
    contains spaces. Please make sure to not have any whitespace in your build path.
    E.g: `C:\Users\User Name\xqemu\build.sh` will not work.

Clone the repo:

    git clone https://github.com/xqemu/xqemu.git

Then change directory:

    cd xqemu

And build using the build script:

    sh ./build.sh

### Building on GNU/Linux

!!! note

    These instructions were tested with Ubuntu 18.04. Depending on the
    Linux distribution being used, these instructions may vary.

Install build deps:

    sudo apt-get update
    sudo apt-get install git build-essential pkg-config libsdl2-dev \
    libepoxy-dev zlib1g-dev libpixman-1-dev

Then clone the repo:

    git clone https://github.com/xqemu/xqemu.git

Then change directory:

    cd xqemu

And build using the build script:

    ./build.sh

### Building on macOS

First make sure you've installed the [Homebrew](https://brew.sh/) package
manager, then update and install necessary packages:

    brew update
    brew install libffi gettext glib pixman pkg-config autoconf pixman sdl2 libepoxy

Clone the repo:

    git clone https://github.com/xqemu/xqemu.git

Then change directory:

    cd xqemu

And build using the build script:

    ./build.sh
