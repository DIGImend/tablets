---
VID: "172f"
PID: "0038"
vendor: Waltop
product: PID 0038
image: Genius_G-Pen_F509
working_area:
    width: 8.75
    height: 5.25
    resolution: 2048
pen:
    pressure_levels: 1024
sold_as:
    - Genius G-Pen F509
maybe_sold_as:
    - Manhattan 177405
supported: true
supported_in:
    kernel: ">= 3.4"
---
This tablet is not listed on Waltop website, so no name is assigned, the product ID is used to identify it instead. Curiously, in default mode, this model reports full pressure whenever a side pen button is pressed. This is being worked around in the kernel driver. No other Waltop tablet has shown such behavior so far.

A [patch](http://thread.gmane.org/gmane.linux.kernel.input/23858/focus=23859) supporting this tablet was accepted into the kernel 3.4 release.

