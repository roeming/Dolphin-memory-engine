# Dolphin Memory Engine

![Screenshot](https://raw.githubusercontent.com/aldelaro5/Dolphin-memory-engine/master/Docs/screenshot.png)

A RAM search program designed to search, track, and edit the emulated memory of [the Dolphin emulator](https://github.com/dolphin-emu/dolphin) during runtime. The primary goal is to make research, tool-assisted speedruns, and reverse engineering of GameCube & Wii games more convenient and easier than with the alternative solution, Cheat Engine. The program's name is derived from Cheat Engine as a symbol for its goal.

The GUI is aimed for convenience, without disrupting the performance of the emulation. Qt 5 is used to help accomplish this.

For binary releases of this program, refer to [the "releases" page](https://github.com/aldelaro5/Dolphin-memory-engine/releases) on [the Github repository](https://github.com/aldelaro5/Dolphin-memory-engine).

## Build Status
### Linux:
[![Build](https://github.com/aldelaro5/Dolphin-memory-engine/actions/workflows/build.yml/badge.svg)](https://github.com/aldelaro5/Dolphin-memory-engine/actions/workflows/build.yml)

### MacOS:
Not Supported (yet)

### Windows:
[![Build](https://github.com/aldelaro5/Dolphin-memory-engine/actions/workflows/build.yml/badge.svg)](https://github.com/aldelaro5/Dolphin-memory-engine/actions/workflows/build.yml)


## System requirements
Any x86_64 based system should work, however, Mac OS is _not_ supported. Additionally, 32-bit x86 based systems are unsupported since Dolphin dropped their support.

You need to have Dolphin running ***and*** _have the emulation started_ for this program to be useful. As such, the system must meet Dolphin's [system requirements](https://github.com/dolphin-emu/dolphin#system-requirements). Additionally, at least 250 MB of free memory is required.

On Linux, installation of the Qt 5 package(s) is required.


## How to Build
### Microsoft Windows
This repository provides a solution file for Visual Studio 2019 and later. The Windows SDK Version 10.0.22621.0 is required*.

Before proceeding, ensure the Qt submodule is initialized by running `git submodule update --init` at the repository's root. The files should appear at the `Externals\Qt` directory.

Once complete, open the solution file `Dolphin-memory-engine.sln` located in the `Source` directory with Visual Studio. Select the build configuration and build it.

#### Windows SDK
The Windows SDK version 10.0.22621.0 comes with the C++ Desktop Development Workload of Visual Studio 2022 — other versions may work but are untested. To use a different Windows SDK you'll need to select the it in the project properties window. Please note that this will change the `vcxproj` file, so if you plan to submit a Pull Request, make sure to not stage this change.


### Linux
> _CMake and Qt 5 are required. Please refer to your distribution's documentation for specific instructions on how to install them._

To build, simply run the following commands from the `Source` directory:

	mkdir build && cd build
	cmake ..
	make

The compiled binaries should be appear in the directory named `build`.


## General usage
Open Dolphin and start a game, then run this program. Make sure that this program reports that the Wii-only extra memory is present for Wii games and absent for GameCube games.

>_Exclusive to the Wii, this is an extra region of memory added as part of the enhancements from the earlier GameCube hardware. Consequently, the presence of this extra memory affects what is considered a valid watch address, as well as what areas of memory the scanner will look though._

If the hooking process is successful, the UI should be enabled, otherwise, click the hook button.

Once hooked, scans can be performed just like Cheat Engine as well as management of a watch list. Save and load a watch list to the disk by using the file menu. The watch list files are in JSON and can be edited in any text editor.

If the program unhooks itself from Dolphin, a read/write operation has failed. This is likely caused by the emulation halting. Boot a game again to solve this; the watch list and scan will be retained during the unhooking.

Finally, the program includes a memory viewer which shows a hexadecimal view and an ASCII view of the memory. Click on the corresponding button or right click on a watch to browse the memory using the memory viewer.

## Troubleshouting

On Linux, the program may require additional kernel permissions to be able to read and write memory to external processes (which is required to read and write the memory of Dolphin).  If nothing happens to Dolphin, but the program frequently unhooks itself, the program is missing the required permissions.  Grant these permissions by running the following command as root:

	setcap cap_sys_ptrace=eip DME

Where `DME` is the path of the Dolphin Memory Engine executable.  This should fix the permission problem for future executions.

If it doesn't work, verify that you do not have the `nosuid` mount flag on your `/etc/fstab` as it can cause this command to silently fail.

## License
This program is licensed under the MIT license which grants you the permission to do  anything you wish to with the software, as long as you preserve all copyright notices. (See the file LICENSE for the legal text.)
