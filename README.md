
### SAROO is a HDLoader for SEGA Saturn.

SAROO is a Saturn CD-drive emulator. SAROO plugs into the cartridge slot and implements the original mainboard CDBLOCK functionality, loading and running games from an SD card.
SAROO also provides 1MB/4MB RAM expansion functionality.

--------
### Some Pictures

<img src="doc/saroo_v12_top.jpg" width=48%/>  <img src="doc/saroo_v12_bot.jpg" width=48%/>
<img src="doc/saroo_scr1.png" width=48%/>  <img src="doc/saroo_scr2.png" width=48%/>
<img src="doc/saroo_dev1.png"/>
<img src="doc/saroo_devhw.jpg"/>


--------
### Development History

#### V1.0
The earliest SAROO version only added a USB host interface to a common usbdevcart. It required patching each game’s main program to convert CDBLOCK access into USB storage access.
This meant every game needed its own modification, so it was not general. Performance and stability were also poor. Only a few games were made to run in this way.
(V1.0-related files are not included in this project.)


#### V1.1
The new version was a complete redesign: FPGA + MCU. The FPGA (EP4CE6) implements the CDBLOCK hardware interface, and the MCU (STM32F103) runs firmware to handle CDBLOCK commands.
This version largely achieved the goal and some games were nearly playable. But there was a fatal issue: random data errors. During intro videos, mosaics/glitches would appear and eventually the game would hang.
This was very difficult to debug and led to the project stalling for a long time.


#### V1.2
Version 1.2 is a bugfix and performance upgrade of 1.1, using a higher-performance MCU: STM32H750. Its frequency is high enough (400MHz) and it has enough internal SRAM to hold a full CDC cache.
The FPGA was also reworked internally, abandoning Qsys and using a custom SDRAM and bus architecture. This version met expectations and is nearly perfect.
By backporting the FPGA and MCU firmware to V1.1 hardware, V1.1 also essentially reaches V1.2 performance.


--------
### Current Status

Dozens of tested games run perfectly.  
1MB/4MB RAM expansion works normally.  
SD cards with FAT32/ExFAT are supported.  
Supports cue/bin images, single-bin or multi-bin.  
Some games get stuck at loading/intro screens.  
Some games hang during gameplay.  


--------
### Hardware and Firmware

Schematics and PCB were created in Altium Designer 14.  
V1.1 requires fly wires to work correctly. This version should no longer be used.  
V1.2 still needs an extra pull-up resistor to use the FPGA AS configuration mode.  

FPGA development uses Quartus 14.0.  

Firm_Saturn is built with the SH-ELF compiler included in SaturnOrbit.  
Firm_v11 is built with MDK4.  
Firm_V12 is built with MDK5.  


--------
### SD Card File Layout

<pre>
/ramimage.bin      ; Saturn firmware program.
/SAROO/ISO/        ; Game images. One game per directory. The directory name is shown in the menu.
/SAROO/update/     ; Firmware update files.
                   ;  FPGA: SSMaster.rbf
                   ;  MCU : ssmaster.bin
</pre>


--------
Some development notes: [SAROO 技术点滴](doc/SAROO技术点滴.txt)

--------
### Project Layout

`.git/` Git metadata for the repository.
`.git/hooks/` Git hook templates and hooks.
`.git/info/` Repository-specific Git info.
`.git/objects/` Git object database.
`.git/refs/` Git references (branches, tags).
`.git/logs/` Git ref logs.
`FPGA/` FPGA source, build scripts, and Quartus project files for the CDBLOCK implementation.
`Firm_MCU/` STM32 MCU firmware source and project files that handle CDBLOCK commands and SD access.
`Firm_MCU/DebugConfig/` Debug configuration profiles for STM32 tools.
`Firm_MCU/FatFS/` FatFS filesystem sources and configuration.
`Firm_MCU/Main/` MCU firmware entry points and platform drivers.
`Firm_MCU/RTE/` CMSIS/RTX runtime environment configuration and device startup files.
`Firm_MCU/Saturn/` Saturn-side protocol and CDC handling code used by the MCU.
`Firm_MCU/Startup/` STM32 startup and system initialization files.
`Firm_MCU/inc/` MCU firmware headers and USB/FATFS interfaces.
`Firm_Saturn/` Saturn-side firmware and tooling used by the system software on the console.
`Firm_Saturn/sega/` Saturn system area binaries and regional system data.
`HW/` Hardware design files (schematics/PCB) and related outputs.
`HW/__Previews/` Altium preview artifacts for schematics/PCB.
`doc/` Documentation, configuration notes, and images used by the README.
`old/` Archived or legacy versions of hardware/firmware/FPGA sources.
`old/FPGA_old/` Early FPGA experiments and Qsys-based designs.
`old/FPGA_v11/` Legacy FPGA project for V1.1 hardware.
`old/Firm_v11_STM32/` Legacy STM32 firmware for V1.1 hardware.
`old/HW_v11/` Legacy V1.1 hardware design files.
`tools/` Host-side utilities for assets, saves, fonts, and media processing.
`tools/bdfont/` Bitmap font tools and font sources.
`tools/cdgtools/` CDG playback/fix tools.
`tools/covertool/` Cover image tool.
`tools/cpktools/` Cinepak and media tooling.
`tools/cpktools/ffmpeg/` Local ffmpeg config/script used by media tools.
`tools/cpktools/cplayk/` Cinepak player tooling and sources.
`tools/savetool/` Save data utilities.
