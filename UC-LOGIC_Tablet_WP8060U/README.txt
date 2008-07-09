
UC-LOGIC Tablet WP8060U README

Products

Besides original product produced by UC-LOGIC, the same hardware is used in
the Genius WizardPen 8x6, and possibly some other tablets.


Description

The tablet has 8x6 inches surface with resolution of 16000x12000 points,
which translates to 200 dpi. The pen has pressure sensitivity of 1024 levels
and two buttons on the side. There is also a cordless mouse with a "wheel"
which could be pressed as the third button, but doesn't rotate and instead
registers clicks in corresponding directions. Mouse resolution is unknown
but seems to be pretty low.


Bugs

USB HID input report descriptor has three inputs: one for the mouse and two
for the tablet.

One of the tablet inputs is quite correct, while the other is seemingly out
of date and is left there accidentally, but happen to have the correct
resolution information and more relevant button description. However, in the
actual reports, outdated tablet input is referenced. Mouse input seems to be
correct, but doesn't have resolution information. Additionally it has an
extra button which produces strange input of unknown purpose.


Fixing

Outdated tablet input is cut off and correct tablet input ID is set to that
of actually used, removed input. This is triggered by special HID_QUIRK.
HID_QUIRK_MULTI_INPUT is added to the quirks in order to split mouse and
tablet devices and avoid axis and buttons kernel mapping skew.


TODO

Instead of simply removing the outdated input from the report descriptor we
should replace it with better one. The new descriptor should contain
resolution information for tablet, probably more appropriate pen button
description and shouldn't contain unknown extra mouse button.

