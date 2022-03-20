# FDGH Converter 4.0

A script that converts FDGH files (found in several Kirby games) to and from XML.
Copyright (C) 2016-2022 RoadrunnerWMC

## FDGH Files

FDGH is a file format used in various Kirby games that lists the files (models, animations, etc) the game should load in advance of each level. If a level calls for enemies not named by the FDGH file, older Kirby games (such as Return to Dreamland) will tend to lag as the files are loaded during gameplay. Recent games can handle it better, but it's still best to set up the FDGH correctly.

In most (probably all?) Kirby games that use this format, there's a single FDGH file, located at `<disk_root>/fdg/Archive.dat`. The FDGH data is embedded in a very thin wrapper called an XBIN. While most XBIN files have a .bin extension, this particular one has a .dat extension for unknown reasons.

All FDGH files to date have identical semantics, but the binary representations have varied from game to game. FDGH Converter auto-detects the details when loading a file, notes them in attributes of the root XML node, and reproduces them when saving. The attributes are specifically:

* `endian` ("big" or "little"; default "big" for backward compatibility) -- the endianness of the file, which always matches the native endianness of the console the game was released for.
* `xbin_version` ("2" or "4"; default "2" for backward compatibility) -- the version number for the XBIN wrapper. Version 4 XBINs (found in Kirby Battle Royale and newer games) have a small amount of extra, constant data.
* `num_string_null_terminators` (any number; default "4" for backward compatibility) -- the number of null terminators to use at the ends of strings. Kirby and the Forgotten Land uses 1; all prior games use 4.
* `asset_name_hashes` (either not present, or "fnv1a_64") -- if present and set to "fnv1a_64", indicates that the asset names table should include hashes of every string, using the 64-bit FNV-1a algorithm. Kirby and the Forgotten Land is the first game to do this.

## Usage

If using a release build, you can simply drag-and-drop `Archive.dat` or `Archive.xml` onto the executable.

If running from source, run fdgh_converter.py with a recent version of Python 3. (Tested with 3.8.10 on Ubuntu.)

Run with `-h` or `--help` for additional help with CLI options.

An "xbin_cli" script is also provided, which lets you manually add/remove XBIN headers to/from files.

## License

Licensed under the GNU General Public License version 3 or (at your option) any later version. (See the "COPYING" file for more information.)
