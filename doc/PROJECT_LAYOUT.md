# Project Layout

```text
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
│   └── docs: SAROO_Technical_Notes.txt, saroocfg.txt
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
```
