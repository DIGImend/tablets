---
VID: "28bd"
PID: "0905"
vendor: XP-Pen
product: Deco 01 v2 revb
image: XP-Pen_Deco_01_v2_revb
report_rate: 233
working_area:
    width: 10
    height: 6.25
    resolution: 5080
pen:
    pressure_levels: 8192
frame_controls: 8 buttons
sold_as:
supported: false
supported_in:
---
Xsetwacom reads the dimensions of the tablet resolution at 32767 x 32767, this doesn't make sense as it's not square drawing area.
uclogic-decode rightly reports 25400 x 15875, which is sensible for a 10 x 6,25 inch Tablet (the proportions match).
Also: the top stylus seemingly buttons reports nothing in usb-hid dump, the tablet frame buttons actually do what they
should when they are driven with xf86-input-evdev instead of xf86-input-wacom (if that is the driver that actually handles it when
nothing else is specified in a manual Xorg config). I also see a UGTABLET "kbd" device show up under xinput --list with this driver.
Unfortunately the upper stylus button still does nothing with this driver from cursory testing with xev and xinput test.
The Bus ID stays the same while the device ID keeps climbing upon replugging the tablet.
I also want to note; that in testing, at times the usb vid:pid went from 28bd:0905 to like 28bd:1227 after running uclogic-probe.

