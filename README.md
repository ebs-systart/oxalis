# SYSTART - Oxalis


This guide will take you through all the steps from unboxing your kit to
getting to a working U-Boot console on the Oxalis.

## What you need

Your kit should include the following:

* Oxalis Board
* LS1012A SOM mounted onto the Oxalis
* A plug fitting the 12V power jack on the board, in case you need to fashion a power supply cable

![Image of Oxalis with connectors](img/OxalisV200_Connectors.svg.png?raw=true "Oxalis")


To complete this guide you'll also need:

* 12V 2A DC  power supply
* Micro-USB cable
* A Windows, Linux or Mac computer loaded with your favorite terminal software for your system
  * See Appendix A for information on terminal programs for your system
* 5 minutes

## Mix and stirr...

1. Connect your computer to the Micro-USB port (X1) of the Oxalis via the Micro-USB cable.
2. Fire up that terminal software on your computer and select the new port that Oxalis just has shown up as
  * Baud rate: 115200
  * 8-N-1:
    * 8 data bits
    * no parity
    * 1 stop bit
3. Without connecting the Oxalis, power up your 12V power source.

## Apply heat...

1. Connect the Oxalis to 12V power and watch the Oxalis Status LEDs turn on
2. You should see the Oxalis going through it's boot process, which finishes with a "=>"

The full boot log looks something like this:

```
U-Boot 2016.092.0+ga06b209 (Jun 29 2017 - 16:33:07 +0200)

SoC:  LS1012AE Rev1.0 (0x87040010)
Clock Configuration:
       CPU0(A53):800  MHz  
       Bus:      250  MHz  DDR:      1000 MT/s
Reset Configuration Word (RCW):
       00000000: 08000008 00000000 00000000 00000000
       00000010: 35080000 c000000c 40000000 00001800
       00000020: 00000000 00000000 00000000 00014551
       00000030: 00000000 18c2a120 00000096 00000000
I2C:   ready
DRAM:  1022 MiB
SEC0: RNG instantiated
SEC Firmware: Bad firmware image (not a FIT image)
PSCI: PSCI does not exist.
Did not wake secondary cores
Using SERDES1 Protocol: 13576 (0x3508)
MMC:   FSL_SDHC: 0, FSL_SDHC: 1
SF: Detected S25FS512S with page size 256 Bytes, erase size 256 KiB, total 64 MiB
In:    serial
Out:   serial
Err:   serial
Model: LS1012A RDB Board
Board: LS1012ARDB Error reading i2c boot information!
SATA link 0 timeout.
AHCI 0001.0301 32 slots 1 ports 6 Gbps 0x1 impl SATA mode
flags: 64bit ncq pm clo only pmp fbss pio slum part ccc apst 
Found 0 device(s).
SCSI:  Net:   cbus_baseaddr: 0000000004000000, ddr_baseaddr: 0000000083800000, ddr_phys_baseaddr: 03800000
class init complete
tmu init complete
bmu1 init: done
bmu2 init: done
GPI1 init complete
GPI2 init complete
HGPI init complete
hif_tx_desc_init: Tx desc_base: 0000000083e40400, base_pa: 03e40400, desc_count: 64
hif_rx_desc_init: Rx desc base: 0000000083e40000, base_pa: 03e40000, desc_count: 64
HIF tx desc: base_va: 0000000083e40400, base_pa: 03e40400
HIF init complete
bmu1 enabled
bmu2 enabled
pfe_hw_init: done
pfe_firmware_init
pfe_load_elf: no of sections: 13
pfe_firmware_init: class firmware loaded
pfe_load_elf: no of sections: 10
pfe_firmware_init: tmu firmware loaded
ls1012a_configure_serdes 0
PCIe0: pcie@3400000 Root Complex: no link
pfe_eth0 [PRIME], pfe_eth1
Hit any key to stop autoboot:  0 
=> 
```

Congratulations! You are running U-Boot now.

[Read on here to find out how to boot linux](https://github.com/ebs-systart/oxalis/oxalis_linux_gettingstarted.md).

## Appendix A: Terminal software

SparkFun has a rather comprehensive [tutorial on terminal software for Windows, Mac and Linux](https://learn.sparkfun.com/tutorials/terminal-basics/all#command-line-windows-mac-linux).

It is long though, if you want our short and sweet information, read on.

### Recommendations in short:

* Windows: putty (for example: `putty.exe -serial com# -sercfg 19200,8,n,1,N` )
* MacOS: screen (for example: `screen /dev/cu.usbserial-XXXXX 115200 -L` )
* Linux: screen  (for example: `screen /dev/ttyACM0 115200` )

#### Windows: putty

#### Linux: screen

#### MacOs: screen




