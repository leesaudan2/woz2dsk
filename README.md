# woz2dsk #

*woz2dsk* is a utility for converting .woz files to .dsk, .po and .nib
files.  These formats are widely used by Apple II emulators.


## Synopsis ##

    woz2dsk [options...] in.woz out.dsk

Options:

* `-f STR`, `--format STR`:
  output format: one of `auto`, `DOS`, `ProDOS`, `nibble`
* `--output-40-tracks`:
  output 40 tracks (instead of the standard of 35)
* `-q`, `--quiet`:
  suppress output messages
* `-d`, `--debug`:
  show debug messages
* `-h`, `--help`:
  print usage message and exit


## Description ##

Convert an Apple II disk image from `.woz` format
to one of `.dsk` (a.k.a. `.do`), `.po` or `.nib` format.
Only images for 5.25" single-sided disks
in the standard 16-sector or 13-sector formats
(ProDOS, DOS 3.3, DOS 3.2)
without copy protection
are supported.

Not every Apple II emulator supports the recent `.woz` format.
Most emulators, however, support the well established
`.dsk`, `.po` and `.nib` formats.
This tool thus provides a means for such emulators to
use disk images available only in `.woz` format.


### Input format ###

This program only accepts
[WOZ](https://applesaucefdc.com/woz/) image files,
both [version 2](https://applesaucefdc.com/woz/reference2/)
and [version 1](https://applesaucefdc.com/woz/reference1/).
Images without copy protection and in the following standard formats
are supported.

* 16-sector format: used by DOS 3.3 and ProDOS.  
  35 tracks * 16 sectors * 256 bytes = 140KB raw capacity  
  (with `--output-40-tracks`: 40 tracks, 160KB)

* 13-sector format: used by DOS 3.2.  Must be used with `--format=nibble`.  
  35 tracks * 13 sectors * 256 bytes = <114KB raw capacity  
  (with `--output-40-tracks`: 40 tracks, 130KB)

Images for disks with non-standard formats and simplistic copy-protection
techniques may work with `--format=nibble`.
This would include disks that can be copied with nibble copiers.
Disks employing advanced bit-based tricks involving precise timing
would most likely **not** work, as the required information is not preserved
in the `--format=nibble` format.

### Output format ###
If the output format is `auto` (default), then it is determined from
the extension of the output file name.
If the extension begins with `p`, then the output format is `ProDOS`.
If the extension begins with `nib`, then the output format is `nibble`.
Otherwise the output format is `DOS`.
Output images contain 35 tracks of data, which is the standard.
However, if `--output-40-tracks` is specified, 40 tracks are written out.
These 40-track images may be incompatible with some emulators or utilities.

* `DOS`: This format is very common and is usually found in image files
  with the extensions `.dsk` and `.do`.
  The file contains the contents of the formatted disk,
  track after track.
  Within each track, sectors are ordered by logical sector numbers,
  mapped to physical sector numbers in the DOS 3.3 way.[[1]][r1]
  These images have a fixed size of 144360 bytes (= 140 KB).
  With `--output-40-tracks`: 40 track images are generated,
  with a size of 163840 bytes (= 160 KB)

* `ProDOS`: This format is usually found in files with the `.po` extension.
  It is like the `DOS` format in most aspects, except that the
  mapping between logical and physical sector numbers follows
  the ProDOS convention.[[1]][r1]
  
* `nibble`: This is usually associated with filename extension `.nib`.
  These files contain the raw nibbles in each track, one track after another.
  Each track has exactly 6656 (hex 1A00) bytes per track.
  The total file size is 232960 bytes (= 227.5 KB).
  With `--output-40-tracks`: 40 track images are generated,
  with a size of 266240 bytes (= 260 KB)

[r1]: http://www.applelogic.org/TheAppleIIEGettingStarted.html


## Examples ##

    woz2dsk 'DOS 3.3 System Master.woz' DOS_3.3_System_Master.dsk
    woz2dsk 'DOS 3.3 System Master.woz' DOS_3.3_System_Master.nib
    woz2dsk 'The Apple at Play.woz' The_Apple_at_Play.po
    woz2dsk 'The Apple at Play.woz' The_Apple_at_Play.nib
    woz2dsk 'DOS 3.2 System Master.woz' DOS_3.2_System_Master.nib

These `.woz` files come from
[WOZ test images](http://evolutioninteractive.com/applesauce/woz_images.zip).


## Installation ##

There is no need to install this program.
Just download the `woz2dsk` file and run it with a
Perl interpreter.

### Pre-requisites ###

A working [Perl](https://www.perl.org/) installation, at least version 5.14.
The following [CPAN](https://www.cpan.org/) modules are needed.

* [File::Slurp](https://metacpan.org/pod/File::Slurp) module
* [Digest::CRC](https://metacpan.org/pod/Digest::CRC) module
* [Getopt::Long::Descriptive](https://metacpan.org/pod/Getopt::Long::Descriptive) module


## GitHub ##

<https://github.com/leesaudan2/woz2dsk>


## Copyright ##

Copyright (C) 2021--2022 LEE Sau Dan <<leesaudan@gmail.com>>


*woz2dsk* is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

*woz2dsk* is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see <https://www.gnu.org/licenses/>.

