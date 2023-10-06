# Elegoo Neptune Thumbnails for PrusaSlicer

[![License: AGPL v3](https://img.shields.io/badge/License-AGPL%20v3-blue.svg)](https://www.gnu.org/licenses/agpl-3.0)

This package converts the thumbnail that PrusaSlicer bakes into the g-code file into the format that is read by the Neptune printers.

<img src="images/main.jpg" width="275" />


## Why the mod?

I was not happy with previous implementations so tried to make it better. But ended up re-writing it :-)

Main changes:
- Faster: it does not read all file content into memory that is important especially for big g-code files;
- Easier installation: image size should be specified in one place only & the script accepts PNG or JPG;
- Better texts quality: texts added after resizing image;
- Better UI: print duration & used filament weight texts modified (weight rounded, print duration can be in shorter format) and their forn & positions adjusted;
- Smaller output file: original thumbnail removed from g-code file;
- Some bugs were fixed (do not worry, I think newer bugs were added).


## Installation

- Download the [latest release](https://github.com/fifonik/ElegooNeptuneThumbnailPrusaMod/releases)
- Extract the zip to location where the exe can be executed


## How to Setup PrusaSlicer for Post-Process Scripts

- 'Printer Settings' / 'G-code thumbnails' -- put something like `200x200` (do not go too high as image will be resized to 200x200 anyway);
- 'Printer Settings' / 'Format of G-code thumbnails' -- select PNG or JPG (it may also work with QOI but I have not checked it);
- 'Print Settings' / 'Post-processing scripts' -- specify path exeutable: `"C:\ElegooNeptuneThumbnailPrusaMod\thumbnail.exe";`

<img src="images/printer_settings.png" width="800" />
<img src="images/printer_settings.png" width="800" />

PrusaSlicer should now run the exe when you export the g-code.

If you do not specify any options, the first thumbnail from g-code file will be used: decoded, resized to 200x200 + 160x160, encoded into new format and baked back into g-code file.
If PrusaSlicer configured to add more than one thumbnail into g-code file you can specify what thumbnail should be used with option `--image_size WxH`


## Running from the script

If you do not want to run the exe, you can always run the Python script directly:
- Install Python;
- Clone the repo;
- Change the settings for 'Post-processing scripts' to:
  `"C:\Program Files\Python311\python.exe" "C:\ElegooNeptuneThumbnailPrusaMod\thumbnail.py";`
  Or, if you do not want to see the terminal window during script execution, use `pythonw.exe` instead:
  `"C:\Program Files\Python311\pythonw.exe" "C:\ElegooNeptuneThumbnailPrusaMod\thumbnail.py";`


## Compatibility

Works with these printers:

- NEPTUNE 3 PRO
- NEPTUNE 3 PLUS
- NEPTUNE 3 MAX
- NEPTUNE 4
- NEPTUNE 4 PRO
- NEPTUNE 4 PLUS
- NEPTUNE 4 MAX

Use the `--old_printer` argument for these printers:

- NEPTUNE 2
- NEPTUNE 2D
- NEPTUNE 2S
- NEPTUNE X


Tested with 2.6.1 and Neptune 4

Apple silicone will not work on the release. In order to run, you must run the script through an x86 python or else the dlls wil not work. You can does this by installing the x86 Homebrew and Rosetta 2.


## Arguments

- `--old_printer`
  Generate thumbnails for Neptune 2 series printers and older
- `--short_duration_format`
  Use short format for print duration: Dd HH:MM (who need these seconds?)
- `--image_size 200x200`
  Image size to look for in the g-code file. If specified it must match to what is in the 'G-code thumbnails' field
- `--debug`
  Put debug additional information into log file (`thumbnail.log`) and save resized images in program folder

To add arguments to the script, make sure to wrap them in double quotes:
`"C:\ElegooNeptuneThumbnailPrusaMod\thumbnail.exe" "--image_size" "300x300";`


## Contribution

This repository is based on [TheJMaster28/ElegooNeptuneThumbnailPrusa](https://github.com/TheJMaster28/ElegooNeptuneThumbnailPrusa), [Molodos/ElegooNeptuneThumbnails](https://github.com/Molodos/ElegooNeptuneThumbnails) and [sigathi/ElegooN3Thumbnail](https://github.com/sigathi/ElegooN3Thumbnail), therefore
released under the **AGPL v3** license.
