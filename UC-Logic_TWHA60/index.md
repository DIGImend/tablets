---
VID: "5543"
PID: "0781"
vendor: UC-Logic
product: Tablet TWHA60
image: Monoprice_MP1060-HA60
working_area:
    width: 10
    height: 6.25
    resolution: 4000
report_rate: 200
pen:
    pressure_levels: 1024
frame_controls: 4 buttons ("desktop", "flip 3D", "next page", "previous page") or 8 buttons ("copy", "cut", "paste", "group", "zoom in", "zoom out", "save", "close window")
sold_as:
    - Genius EasyPen M610
    - Monoprice MP1060-HA60
support:
    kernel: ">= 3.7"
---
There are two versions of this tablet, one represented by Genius EasyPen M610, another by Monoprice MP1060-HA60. The Genius version has 4 frame buttons and lower power requirement - 160mA. The Monoprice version has 8 frame buttons and requires 300mA.

A [patch](http://thread.gmane.org/gmane.linux.kernel.input/26544) adding support for this tablet was accepted into the kernel and should be included into 3.7 release.

As this tablet has several variations with different number and different assignments of frame buttons, they are simply mapped to F1-F24 range and are left for users to remap in userspace.

