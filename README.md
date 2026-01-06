# Yet another PrusaSlicer/OrcaSlicer thumbnail script for Elegoo Neptune

[![License: AGPL v3](https://img.shields.io/badge/License-AGPL%20v3-blue.svg)](https://www.gnu.org/licenses/agpl-3.0)

This package converts the thumbnail that PrusaSlicer/OrcaSlicer bakes into the g-code file into the format that is read by the Neptune printers.

Modified image example:

<img src="images/screenshot.png" />

Photos of Neptune 4 screen (left -- from PrusaSlicer, right -- from OrcaSlicer):
<p float="left">
  <img src="images/ScreenshotPrusaSlicer1.jpg" width="300" />
  <img src="images/ScreenshotOrcaSlicer1.jpg" width="300" />
</p>

Example of modified image in Klipper UI (light theme):

<img src="images/klipper.png" />


## Why another mod?

I was not happy with previous implementations so tried to make it better. But ended up re-writing it :-)

Notable changes:
- Faster: it does not read all file content into memory, which is vital for big g-code files;
- Easier installation: image size should only be specified in slicer's printer settings and the script accepts PNG and JPG;
- More information on thumbnail: print duration, used filament weight, used filament length, model height and filament cost can be added to image and you can specify what should be specified in each corner;
- Better texts quality: texts added after image resizing, font size and texts positions adjusted automatically;
- Adjust g-code so completed percentage and remaining time are displayed correctly under the thumbnail (you need to tick "Support remaining times" checkbox in Printer Settings);
- Adjust original image that is displayed in Klipper UI (light and dark themes both supported);
- Supports PrusaSlicer and OrcaSlicer.


## Installation

- Download the [latest release](https://github.com/fifonik/ElegooNeptuneThumbnailPrusaMod/releases);
- Unzip the executable and put it to desired location.


## How to configure PrusaSlicer:

1. `Printer Settings` / `General` / `G-code thumbnails` -- put something like `300x300`;
2. `Printer Settings` / `General` / `Format of G-code thumbnails` select `PNG` or `JPG`:
3. `Print Settings` / `Output options` / `Post-processing scripts` - specify path to executable: `"C:\Path\Where\You\Put\thumbnail.exe";`:
<img src="images/printer_settings.png" width="720" />
<img src="images/print_settings.png" width="720" />


## How to configure OrcaSlicer:

- `Printer Settings` / `Basic information` / `G-code thumbnails` -- put something like `300x300`;
- `Printer Settings` / `Basic information` / `Format of G-code thumbnails` select `PNG` or `JPG`;
- `Print Settings` / `Others` / `Post-processing Scripts` - specify path to executable: `"C:\Path\Where\You\Put\thumbnail.exe";`:
<img src="images/orca_setup.png" width="720" />


PrusaSlicer/OrcaSlicer should now run the thumbnail.exe when you export your g-code.
In case of issues - check `thumbnail.log`.

If you do not specify any options, the first thumbnail from g-code file that is bigger than 100x100 will be used: decoded, resized, encoded into new format and injected back into g-code file.

If PrusaSlicer/OrcaSlicer is configured to add more than one thumbnail into g-code file, you can specify what thumbnail the script should use with option:
`--image_size WIDTHxHEIGHT`



## Running from the Python script

If you do not want to run the supplied executable (as myself), you can always run the Python script directly:
- Install [Python](https://www.python.org) (remember directory where you installed it);
- Install [PyQT6](https://pypi.org/project/PyQt6/: `pip install PyQt6` from console;
- Clone the repo (or download `thumbnail.py` + `lib_col_pic.py` and put them into the same folder);
- In `Post-processing scripts` put `"C:\Path\Where\You\Installed\python.exe" "C:\Path\Where\You\Put\thumbnail.py";`
- Or, to hide the terminal window: `"C:\Path\Where\You\Installed\pythonw.exe" "C:\Path\Where\You\Put\thumbnail.py";`


## Building your own executable from the Python script

- Install [Python](https://www.python.org) (remember directory where you installed it);
- Install [PyQT6](https://pypi.org/project/PyQt6/): `pip install PyQt6` from console;
- Install [Pyinstaller](pyinstaller.org): `pip install pyinstaller` from console;
- Clone the repo;
- Open console, navigate to the repo folder and run `pyinstaller build.spec` or just run supplied `build.bat` => `thumbnail.exe` will be created in `dist` folder.


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


Tested with PrusaSlicer 2.6.1/2.7.0, OrcaSlicer 1.8.0/1.8.1/1.9.0/1.9.1/2.0.0/2.1.0/2.2.0 on Neptune 4.



## Supported command line parameters

- `--info info-abbr-top-left,info-abbr-top-right,info-abbr-bottom-left,info-abbr-bottom-right`
  What information to add to image in each corner. Supported info-abbr values: `print_duration`, `model_height`, `filament_weight`, `filament_length`, `filament_cost`
  Default: `print_duration, model_height, filament_weight, filament_length`
- `--short_duration_format`
  Use short format for `print_duration` (`1d 23:45` instead of `1d 23h 45m 56s`. Who need these seconds, really?)
- `--currency $`
  Currency character for `filament_cost`
  Default: `$`
- `--currency_suffix`
  Currency character is added as a suffix (example: `10$`). Without the option (default), it is added as a prefix (example: `$10`)
- `--update_original_image`
  Original image (that is used by Klipper) is also modified with text info. So you will see the modified image in fluidd.
- `--original_image_light_theme`
  Original image will be adjusted for light Klipper's theme. Use this option if you use light fluidd theme (without the option it is adjusted for dark fluidd theme -- default Klipper theme)
- `--debug`
  Write debug information into `thumbnail.log` and save resized images in program folder. I will ask you to run with the option and supply log in case you face any issues.
- `--old_printer`
  Generate thumbnails for Neptune 2 series printers & older
- `--image_size 200x200`
  Without this option the first thumbnail that is bigger than 100x100px from g-code file will be used.
  If specified, the script will try to find in g-code thumbnail with the specified size. It will report error if such thumbnail is not found. So I'd recommend not to use the option at all and only specify size 300x300 in 'Printer Settings'.
- `--images WxH/gimage,WxH/simage`
  Images to be generated in specified order. The option does not work with option `--old_printer`.
  Default: `200x200/gimage,160x160/simage`
- `--no-modify_slicer_header`
  Original slicer header will not be modified. This option should only be used if your printer firmware displays thumbnail without Cura mentioned in gcode.

To add script's command line option in PrusaSlicer/OrcaSlicer, make sure you wrap them in double quotes:
`"C:\ElegooNeptuneThumbnailPrusaMod\thumbnail.exe" "--image_size" "300x300";`


## Author

fifonik

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/fifonik)


## Contribution

This repository is based on:
- [TheJMaster28/ElegooNeptuneThumbnailPrusa](https://github.com/TheJMaster28/ElegooNeptuneThumbnailPrusa)
- [Molodos/ElegooNeptuneThumbnails-Prusa](https://github.com/Molodos/ElegooNeptuneThumbnails-Prusa)
- [sigathi/ElegooN3Thumbnail](https://github.com/sigathi/ElegooN3Thumbnail)

Therefore it is released under the **AGPL v3** license.

