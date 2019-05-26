Frequently Asked Questions
==========================

#### Q: Are there system requirements?

There are not strict requirements, except that your graphics card should have
a driver that supports OpenGL 3.3 core profile (or better). Most relatively
modern laptops and desktops should be able to run XQEMU. XQEMU runs on all
major operating systems.

Please be aware that XQEMU has significant performance issues right now that
are actively being addressed. Even with the best hardware, the master build
runs very poorly. This will be fixed soon.

#### Q: Is there a GUI?

Yes, please use [XQEMU-Manager](https://github.com/xqemu/xqemu-manager).

#### Q: How can I get the [xxx] build?

Developers may have experimental builds including exciting new features or bug
fixes in their own repositories. While your testing is welcome, these are
considired unofficial, experimental builds. For this reason, we do not link to
them directly from the site, to avoid confusion.

#### Q: What BIOS do I need to use?

Your MCPX and BIOS dump should be for a 1.0 Xbox. It's suggested that
your MCPX dump be 1.0 and that a compatible BIOS image be used (users have
reported success with "COMPLEX 4627").

#### Q: Why am I getting "The guest has not initialized the display"?

This is likely due to a mismatch of MCPX and BIOS images.

#### Q: Is there a game compatibility list?

There is a compatibility list
[here](https://docs.google.com/spreadsheets/d/1sVtQ9SNPathKAMCqfYtvJQP0bs0UeLzP9otPHvZDMwE/edit#gid=70987934),
but this may not be up to date. We encourage users to test their games and help us discover issues.

#### Q: Does XQEMU run my game's default.xbe?

No, not directly. XQEMU emulates the hardware of the system, so you'll need to
have a disc image of your game backup. You can use
[extract-xiso](https://github.com/xboxdev/extract-xiso) or qwix to create an
image.

#### Q: Does XQEMU support "redump" style ISOs?

No, not yet. Please use extract-xiso to repack your ISO.
