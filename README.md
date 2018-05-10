DIGImend tablet database
========================

This is a database of tablets the DIGImend project has encountered and
catalogued, and is used for general reference during development, tracking of
driver support, and also for displaying on the [DIGImend website][website].

Tablet names
------------
An important part of cataloguing is finding out which name to use for a
tablet. Tablets are often made by one company, and then rebranded and sold by
another company under another name. They can also use hardware shared by other
tablet models from the same company or tablets from other companies, and be
somewhat different in looks, but the same in functionality.

In the end it's necessary to decide whether to catalogue a tablet as an
entirely new tablet, or as a rebranding, or a version of another one, already
in the database. The tablets can be considered to be the same based on the
following:

* They look the same or very similar.
* They have the same type of pen: battery-powered or battery-free, with
  or without tilt detection, same number of buttons, etc.
* The drawing area specifications are the same: size, resolution, pressure
  detection range, report rate, etc. Note that numbers in specifications can
  sometimes be incorrect, so certain differences can occur.
* They both either support or don't support a special mouse.
* The `lsusb -v` output for the tablet is mostly the same. `idVendor`,
  `idProduct`, `iManufacturer`, `iProduct` and `iSerial` fields can differ.

Unfortunately, manufacturers started reusing USB VID:PID pairs for different
models, even across different companies, and now it is not possible to
identify tablets based on that.

Sometimes tablets are sold under one name, but can report a different name in
their USB device descriptors. For example, a tablet purchased under name
"Genius MousePen 8x6" had the following in the `lsusb -v` output:

    iManufacturer           1 UC-LOGIC
    iProduct                2 Tablet WP8060U

Here the `iManufacturer` field set by the tablet's hardware manufacturer
says that it was made by UC-Logic, and the `iProduct` field says the model
name is `WP8060U`. In this case the tablet should be named "UC-Logic WP8060U".
The general rule of thumb is go as close to the hardware manufacturer's name
as possible.

When two or more new tablets are added to the database, which are the same
but are sold under different names, they should be given a name of the first
one appearing on the market. The other names should be added to the `sold_as`
field. If you're cataloguing a tablet which is very likely to be the same as
another, but cannot verify that, add its name to the `maybe_sold_as` field.

Tablet identifiers
------------------
Each tablet can have an identifier formed from its manufacturer or primary
seller's name, and the model name assigned by them. Tablet identifiers are
used to name tablet directories and photo files.

To produce an identifier take each name, replace every space with an
underscore character (`_`), remove any special characters, or replace them
with a word (e.g. replace `"` with `inch`), and then put the
manufacturer/seller name in front, followed by an underscore, followed by the
model name. Preserve the original character case. Add extra identifying
information (such as version) after another underscore.

E.g. "Waltop Slim Tablet 12.1"" would become `Waltop_Slim_Tablet_12.1_inch`,
"Yiynova MVP10UHD+IPS" would become "Yiynova_MVP10UHD+IPS", and version three
of "UC-Logic TWHA60" would become `UC-Logic_TWHA60_v3`.

Photos
------
Photos should be of sufficient resolution to discern tablet features, such as
number and location of buttons on the frame and pen, symbols or writing near
the buttons, model name, etc. The tablet should preferably be flat towards the
viewer, without distortion.

Store the photo in a file named with a tablet identifier corresponding to the
actual model on the photo. After you set the tablet image name in `index.md`,
you can generate the thumbnail image by executing `./gen-thumbs` in the
repository's top directory. You will need to have `imagemagick` installed for
it to work.

Structure
---------

Each tablet's information is stored in a directory named after tablet
identifier (see above). Each such directory must contain an `index.md` file,
and a photo of one of the tablet versions. The photo should be referred to
from the `index.md` file.

The `index.md` file is a Markdown file with a "YAML front matter" -
[YAML](http://yaml.org) data at the beginning of the file, surrounded by lines
with three dash characters (`---`), and described by a [schema](schema.yml).
The "front matter" can be followed by a free-form tablet description or notes
in Markdown format.

The general directory structure is as follows:

* `index.md` - tablet information, description and notes.
* `descriptors.txt` - `lsusb -v` output for the tablet.
* `probe.txt` - output of `uclogic-probe` (from
  [uclogic-tools][uclogic-tools]).
* `dumps` - directory with USB, evdev, xinput traffic dumps.
* `<PHOTO_ID>.jpg` - photo of one of the tablet versions. Here, `<PHOTO_ID>`
  is a tablet identifier formed from the name of the tablet on the photo.
* `<PHOTO_ID>.thumb.jpg` - smaller version of the photo for use in tablet
  lists, generated by `gen-thumbs` in the top directory.
* `rd` - directory with report descriptors as output by `usbhid-dump -ed`,
  with each interface's descriptor in its own file named `original<N>.txt`,
  where `<N>` is the interface number (last number before `DESCRIPTOR` in
  `usbhid-dump` output).

See [Collecting tablet diagnostics][diagnostics_howto] HOWTO for instructions
on collecting most of the above.

Before submitting tablet information, please run `./validate` in the top
directory to verify the data structure. You will need to have `kwalify`
installed for it to work.

Testing
-------
Before submitting please test if the DIGImend website can display your tablet.
Follow the instructions in the [website repo][website_repo] to start the
server on your local machine, apply your changes to the tablets repo
sub-module, and see if they make sense. It's also easier to make the tablets
changes under the website repo from the start.


[website]: http://digimend.github.io/
[website_repo]: https://github.com/DIGImend/digimend.github.io
[uclogic-tools]: https://github.com/DIGImend/uclogic-tools
[diagnostics_howto]: http://digimend.github.io/support/howto/trbl/diagnostics/
