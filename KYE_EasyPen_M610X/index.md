---
VID: "0458"
PID: "5013"
vendor: KYE
product: EasyPen M610X
image: Genius_EasyPen_M610X
report_rate: 200
working_area:
    width: 10
    height: 6
    resolution: 4096
pen:
    pressure_levels: 1024
frame_controls: 4 buttons ("undo", "eraser", "zoom in", "zoom out")
sold_as:
    - Genius EasyPen M610X
supported: true
supported_in:
    kernel: ">= 3.4 (\"eraser\" button as \"redo\" - to be fixed)"
    digimend: ">= 6"
---
A [patch](http://thread.gmane.org/gmane.linux.kernel.input/23744/focus=23799) supporting this tablet was accepted into the kernel 3.4 release.

