---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
---

Debug edge is an open standard for exposing debug signals such as [Serial Wire Debug (SWD)](https://developer.arm.com/architectures/system-architectures/system-components/coresight/serial-wire-debug) and [Joint Test Action Group (JTAG)](https://en.wikipedia.org/wiki/JTAG) on the edge of a 1.6mm printed circuit board (PCB).

It uses low cost board-to-board connectors to that no part is needed on the
target PCB. This minimizes cost for everyone but ensures easy debug access when
desired.

# Overview

The debug edge spec support four variants, from a stripped down 4 pin version that has an edge footprint of 4mm x 4.5mm to a full 10 pin version that is 10mm x 4.5mm. The variants use a compatible pinout and simply add functionality with additional pins.

The diagram below demonstrates the pin assignment. Note that the pin numbering does *NOT* match the connector numbering because the connectors are renumbered by the manufacturer.

![Illustration of pinout](/assets/debug-edge.png)

# 4-pin
## SWD, bring your own power

The core 4 pins feature the SWD pins used to control the target:
* SWDIO - Bidirectional data signal.
* SWCLK - Host -> Target clock signal.

It also includes shared signals:
* GND - Reference ground for the signals.
* nReset - Resets the target when set to ground.

SWD will not work with these four signals alone. It also needs a Vcc voltage reference, typically 3.3v or 5v, it is used for the high value in the signal lines. The Debug Edge host connector has a pin header to connect the Vref separately. We assume that the voltage reference is most likely to be broken out separately and is accessible, hence the subtitle, "bring your own power".

The core 4-pin layout is designed to ground nReset when the Host and Target are flipped upside down. It is assumed that the target will high-z the remaining pins when in reset.

Host to host or target to target connections may cause trouble by grounding SWDIO.

# 6-pin
## SWD with power and SWO

The 6-pin layout adds two additional pins:
* Vref - The high signal voltage, typically 3.3v or 5v
* SWO - Serial Wire Output, can be used for printf output

The 6-pin connector is the smallest self-contained SWD connection. It is the best footprint to use on small ARM boards.

# 8-pin
## SWD and JTAG!

The 8-pin layout adds two additional pins, one completes the JTAG pinout and the other begins to add SWD trace support and UART output. It is the best footprint to use on small boards that require JTAG.

The JTAG pinout is:

* TMS
* TCK
* TDO
* TDI

This variant also adds UART TX, which is output from the target to the host. Make sure your host supports it.

# 10-pin
## SWD, SWD Trace and JTAG

The 10-pin layout includes all of the bells and whistles. It is self contained and can do SWD or JTAG with a UART connection or 2-bit SWD trace.

This is the footprint to use on most hosts!

# Links

[GitHub](https://github.com/debug-edge/)
[Website](https://debug-edge.io/)
[Connector Datasheet](http://datasheets.avx.com/OpenEndedCard%20Edge_00-9159.pdf)
[Original Twitter Thread](https://twitter.com/_nitz/status/1299066513132072973)

# Thanks
Thanks to [@tachiniererin](https://twitter.com/tachiniererin) for the inspiration. Thanks to [@GregDavill](https://twitter.com/GregDavill) for suggesting the connector. Thanks to [@_nitz](https://twitter.com/_nitz) and [@tannewt](https://twitter.com/tannewt) for fleshing out the design and associated assets. Thanks to [@k0dSE](https://twitter.com/k0dSE) for fixing the website and suggesting UART.
