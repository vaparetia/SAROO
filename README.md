
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
Some development notes: [SAROO Technical Notes](doc/SAROO_Technical_Notes.txt)

### Project Layout
<pre>
.
├── FPGA
│   ├── SSMaster.qpf / SSMaster.qsf / SSMaster.sdc
│   ├── Verilog: SSMaster.v, cacheblk.v, cachebus.v, memhub.v, tsdram.v
│   └── IP: cdcfifo.*, mainpll.*
├── Firm_MCU
│   ├── Main: main.c, shell.c, spi1_fpga.c, sdio_h7.c, version.c
│   ├── Saturn: saturn_main.c, saturn_cdc.c, cdimg.c
│   ├── FatFS: ff.* / diskio.*
│   ├── Startup: startup_stm32h750xx.s, system_init.c
│   └── inc: board_config.h, teeny_usb*, tusb*
├── Firm_Saturn
│   ├── main.c, game_load.c, game_save.c, sci_shell.c, version.c
│   ├── Sega BIOS blobs in `sega/`
│   └── linker + startup: ldscript, crt0.S, sysid.S
├── HW
│   ├── Altium schematics: *.SchDoc, *.PcbDoc, *.PrjPCB
│   └── PDFs: ssmaster_top.pdf, ssmaster_bot.pdf, ssmaster_v1.pdf, ssmaster_v2.PDF
├── doc
│   ├── images: saroo_dev1.png, saroo_scr1.png, saroo_v12_top.jpg
│   └── docs: SAROO_Technical_Notes.txt, saroocfg.txt, PROJECT_LAYOUT.md
├── old
│   ├── FPGA_old / FPGA_v11
│   ├── Firm_v11_STM32
│   └── HW_v11
├── tools
│   ├── bdfont
│   ├── cdgtools
│   ├── covertool
│   ├── cpktools
│   └── savetool
└── README.md
</pre>
