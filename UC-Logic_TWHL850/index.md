---
VID: "5543"
PID: "0522"
vendor: UC-Logic
product: Wireless Tablet TWHL850
image: Genius_MousePen_M508W
working_area:
    width: 8
    height: 5
    resolution: 4000
report_rate: 200
pen:
    pressure_levels: 1024
mouse:
    absolute: true
frame_controls: 4 buttons ("desktop", "flip 3D", "next page", "previous page")
sold_as:
    - Genius MousePen M508W
support:
    kernel: ">= 3.5 (frame buttons mixed up)"
---
A [patch](http://thread.gmane.org/gmane.linux.kernel.input/25190/focus=25202) adding support for this tablet was accepted into 3.5 kernel release.

This tablet has a bug in the default (compatibility) operating mode which is used in the current driver: frame button functions are mixed up. This is to be fixed with a driver using proprietary protocol, which is yet to be fully reverse-engineered.

