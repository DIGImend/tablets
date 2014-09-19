---
VID: "172f"
PID: "0502"
vendor: Waltop
product: Sirius Battery Free Tablet
image: Princeton_PTB-S1BK
working_area:
    width: 10
    height: 6
    resolution: 4000
report_rate: 200
pen:
    pressure_levels: 1024
    tilt_detection: true
sold_as:
    - VisTablet Muse
    - PENTAGRAM Designer P 2700
maybe_sold_as:
    - Princeton PTB-S1BK
supported: true
supported_in:
    kernel: ">= 3.5 (2000 LPI, no horizontal scrolling)"
    wacom: ">= 0.18.0"
---
A [patch](http://thread.gmane.org/gmane.linux.kernel.input/24153) adding support for this tablet was [accepted](http://thread.gmane.org/gmane.linux.kernel.input/24153/focus=24241) into the kernel.

Reports
-------

As probably all the other Waltop tablets, this tablet has other modes than the default. One such mode is enabled by sending 0x10, 0x01 as feature report with ID 0x02.

After that, the tablet starts reporting pen coordinates in full resolution of 4000 LPI and all the frame button pressures.

The structure of the reports becomes the following.

### 0x02 ###

This is the pen report.

|Byte|Bit|Meaning|
|----|---|-------|
|0|0-7|Report ID|
|1-2|8-23|X axis (0-40000)|
|3-4|24-39|Y axis (0-24000)|
|5|40-41|Proximity|
||42|Tip button|
||43|Lower side button|
||44|Upper side button|
||45-47|Padding (always zero)|
|6-7|48-63|Tip pressure (0-1023)|
|8|64-71|X tilt|
|9|72-79|Y tilt|

The tilt measurement is quite imprecise. The zero position is tilted about 15 degrees towards the bottom edge of the tablet. The negative/positive tilting angle values are not equal. The change speed looks more like the sine of the angle, than the angle. Maximum seen angles so far are [-69, 69] and [-71, 73] for X and Y tilt correspondingly.

### 0x05 ###

This is the pen report for working area border locations. Probably intended for detecting the "virtual" button clicks. Probably, indescribable with a HID report descriptor.

|Byte|Bit|Meaning|
|----|---|-------|
|0|0-7|Report ID|
|1|8-9|Proximity|
||10|Tip button|
||11|Lower side button|
||12|Upper side button|
||13-15|Padding (always zero)|
|2|16-23|Border: 1 - bottom, 2 - top, 4 - left, 8 - right|
|3|24-31|Border coordinate: vertical borders - 0x01-0x18, top-bottom; horizontal borders - 0x01-0x28, left-right|
|4|32-39|The higher byte of the Y axis value last reported with report 0x02. Basically, garbage.|
|5-7|40-63|Padding (always zero)|

### 0x0A ###

This report is used for all the frame buttons and most of the dials' functions. The button reports are 10 bytes long, the dials' report are 8. The second byte signifies the "subreport" ID.

The overloaded usage probably makes this report impossible to describe fully with a HID report descriptor, which is a pity.

Subreport descriptions follow.

#### 0x0E ####

This is the frame button subreport.

|Byte|Bit|Meaning|
|----|---|-------|
|0|0-7|Report ID|
|1|8-15|Subreport ID|
|2|16|Left "eraser" button|
||17|Left Shift|
||18|Left Tab|
||19|Left Alt|
||20|Left Ctrl|
||21|Right "eraser" button|
||22|Right Shift|
||23|Right Tab|
|3|24|Right Alt|
||25|Right Ctrl|
||26|"Vertical/horizontal scroll" dials' function selection button|
||27|"Zoom/volume" dials' function selection button|
||28|"Keyboard direction" dials' function selection button|
||29|Dials' subfunction switch button|
||30-31|Padding (always zero)|
|4-9|32-79|Padding (always zero)|

#### 0x02 ####

This is the dials subreport. Note: it is only used for vertical/horizontal scroll and zoom/volume functions. For cursor movement function 0x0D report is used instead.

|Byte|Bit|Meaning|
|----|---|-------|
|0|0-7|Report ID|
|1|8-15|Subreport ID|
|2|16-23|Left/right wheel position change: -1/0/1 for counterclockwise/none/clockwise correspondingly.|
|3-5|24-47|Padding (always zero)|
|6|48-55|Right wheel position: 0 for no finger, 1-8 for position starting from 12 hours, clockwise.|
|7|56-63|Left wheel position: 0 for no finger, 1-8 for position starting from 12 hours, clockwise.|

The finger sensing is quite imprecise to the point of reporting wrong direction or not reporting change when moving slowly.

### 0x0D ###

This is keyboard-imitating report for the dials' "cursor movement" function. The report is 8 bytes long. Only byte with offset 3 is used. The values are as follows.

|Value|Meaning|
|-----|-------|
|0x50|Left|
|0x51|Down|
|0x52|Up|
|0x4F|Right|
|0x4A|Home|
|0x4B|Page Up|
|0x4D|End|
|0x4E|Page Down|


