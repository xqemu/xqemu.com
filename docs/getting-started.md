Introduction
------------
XQEMU is a low-level, full-system emulator which emulates the actual hardware of
the Xbox; this means that in order to actually run XQEMU, you must have a copy
of the stuff that a real Xbox needs when it turns on:

1. The MCPX Boot ROM image
2. The flash ROM image (aka *BIOS*)
3. A properly-formatted hard disk drive image
4. Game disc image(s)

Unfortunately, distributing some of these items would violate copyright laws, so
you'll need to acquire them on your own.

!!! attention
    The XQEMU project does not endorse or promote piracy. We don't link to the
    copyrighted files, or discuss how to acquire them. The best way to acquire
    these files is to dump them from *your real, physical Xbox*. Please don't
    ask us how to get them.

### Tips

#### MCPX Boot ROM Image

    MD5 (mcpx_1.0.bin) = d49c52a4102f6df7bcf8d0617ac475ed

If your MCPX dump has an MD5 of `196a5f59a13382c185636e691d6c323d`, you dumped
it badly and it's a couple of bytes off. It should start with 0x33 0xC0 and end
with 0x02 0xEE.

#### Flash ROM Image (aka BIOS)

Xbox 1.0 compatible bios (cromwell, 3944, 4034, 4036, ...). You can use a retail
bios a debug bios. Just like a real Xbox, running a retail bios will not boot
unofficial software.

##### Debug BIOS

People have reported success with the 'COMPLEX 4627' modified debug bios. It's
convenient to note that this bios does not necessarily require a _populated_
hard disk image to load an application from DVD (though an empty drive still
needs to be attached), so you can skip the next step in some cases.

    v1.0.2 1M dump: MD5 (Complex_4627Debug.bin) = 19b5c6d3d42a707bba620634fe6d4baf

_or sometimes_

    1MB dump: MD5 (complex_4627debug.bin) = e8dd61cc6abdbd06aac185e371312dc1

##### Retail BIOS

    1M dump: MD5 (3944.bin) = e8b39b98cf775496c1c76e4f7756e6ed

_or sometimes_

    256k dump: MD5 (3944.bin) = 542c62cb976a4993c8c5027dff9638ce

#### Hard Disk Drive Image

You have options:

##### Option 1: Use a pre-built Xbox HDD image

You can use a pre-built 8G Xbox HDD image, free of any copyrighted content, and
only containing a dummy dashboard. [You can download this image from
here!](https://github.com/xqemu/xqemu-hdd-image/releases)

!!! note
    By design, this particular drive image does not contain the official Xbox
    dashboard, but instead contains only a dummy dashboard. Because of this, you
    may see an error message when starting XQEMU with an unmodified retail BIOS
    image due to the system failing to find a properly signed dashboard.

    If you would like to change your dashboard (perhaps to the official retail
    dashboard, or any alternative dashboard), or copy additional files over to
    the Xbox HDD, you can start XQEMU, using a modified BIOS image, and a disc
    containing an alternative dashboard. Then you can either install that
    dashboard, or connect to XQEMU using FTP to transfer your desired dashboard
    files to the HDD.

##### Option 2: Image your real Xbox HDD

This is the most authentic way to do it. Unlock your drive, connect it to a
computer, and `dd` the entire contents of the drive straight to a file. This
file can be used as-is with XQEMU.

##### Option 3: Build a new HDD image from scratch

You can also create an Xbox hard-disk image using XboxHDM:

* Create an [xboxhdm](http://sourceforge.net/projects/xboxhdm2/) cd-rom with the dashboard files
  * If xboxhdm doesn't work for you, try to set the included "mkisofs.exe" to run in Windows XP compatibility mode
* Create a blank hard-disk file: ```qemu-img create -f qcow2 xbox_harddisk.qcow2 8G```
* Run xboxhdm with qemu or something: ```i386-softmmu/qemu-system-i386 -hda xbox_harddisk.qcow2 -cdrom linux.iso```

Building XQEMU from Source
--------------------------

### Building on Windows

!!! tip
    If you'd prefer to skip building from source and instead run a pre-built
    version of XQEMU for Windows, build artifacts are now available through
    [Appveyor](https://ci.appveyor.com/project/mborgerson/xqemu-c5j6o/build/artifacts).

Start by installing and setting up [MSYS2](https://www.msys2.org/).

!!! important
    After installing MSYS2, you'll need to open **MSYS2 MinGW 64-bit** to
    perform building. Otherwise, you may see build failures for cc.exe.

Once MSYS2 has been installed, install all of the necessary packages by running:

    pacman -S git python2 make autoconf automake-wrapper \
    mingw-w64-x86_64-libtool mingw-w64-x86_64-gcc mingw-w64-x86_64-pkg-config \
    mingw-w64-x86_64-glib2 mingw-w64-x86_64-glew mingw-w64-x86_64-SDL \
    mingw-w64-x86_64-SDL2 mingw-w64-x86_64-pixman

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

First enable `deb-src` via:

    sudo gedit /etc/apt/sources.list

In this file, uncomment first `deb-src` line. Now refresh packages:

    sudo apt-get update

Install build deps:

    sudo apt-get build-dep qemu
    sudo apt-get install git libsdl2-dev libglew-dev

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
    brew install libffi gettext glib pixman pkg-config autoconf pixman sdl2

Clone the repo:

    git clone https://github.com/xqemu/xqemu.git

Then change directory:

    cd xqemu

And build using the build script:

    ./build.sh

Launch XQEMU
------------

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

Of course, on Windows the executable path will have a `.exe` extension.

Replace the variables `$MCPX`, `$BIOS`, `$HDD`, and `$DISC` with the appropriate
file paths or define them as variables in your shell.

The Xbox boot animation sequence can be bypassed by adding the
`,short-animation` option to the `-machine` switch above.
