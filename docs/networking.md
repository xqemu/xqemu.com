XQEMU emulates the Xbox network controller (nvnet), and being built on top of
QEMU's robust networking support infrastructure, provides a flexible array of
advanced network configuration options including:

- Socket networking, for connecting multiple running instances of XQEMU for
  possible networked game play.
- Tap networking, for bridging a physical network device for play with real
  Xboxes or other applications.
- User networking, for simple network use where only basic port-forwarding is
  required.

This page contains some quick tips for common use cases regarding Xbox
emulation. Additional details about networking configuration information is
available in the [official QEMU
documentation](https://wiki.qemu.org/Documentation/Networking).

# Bridged Networking

## Linux

__Requirements__
- uml-utilities
- bridge-utils

By running this script via `sudo` before start-up of XQEMU you’ll have set-up the bridge and tap adapters that are required for networking. You will however need to modify it slightly to include the main ethernet adaptor and the user account you’ll be running XQEMU on.

```bash
#!/bin/bash
brctl addbr br0
ip addr flush dev <interface or eth0>
brctl addif br0 <interface or eth0>
tunctl -t tap0 -u <youruseracc>
brctl addif br0 tap0
ifconfig <interface or eth0> up
ifconfig tap0 up
ifconfig br0 up
dhclient -v br0
```

Once that runs and you don’t see any errors you should be able to just run XQEMU with the additional command line option of ` -net nic,model=nvnet -net tap,ifname=tap0,script=no`.

## Windows

__Requirements__
- [OpenVPN TAP Driver](https://openvpn.net/index.php/open-source/downloads.html) (Scroll down to “Tap-Windows”)

Compared to Linux bridging, this is a little more involved but once it’s set-up you won’t need to mess with it again. Install the prerequisites then you’ll need to manually bridge your main adapter and the newly created TAP adapter. This is easily done by going to `Network & Sharing Center` then `Change adapter settings`.

The command line options are very similar to Linux however you’ll need to change the `ifname=tap0` to what ever Windows or yourself had set the name of the new TAP adapter to be.

` -net nic,model=nvnet -net tap,ifname=”Ethernet 2”,script=no`

In this example the interface name is `Ethernet 2`.
