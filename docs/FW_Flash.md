# How to flash the firmware

This is based on the guide for CANbus support on the octopus, included here. Not covering how to flash the CANFlash bootloader using STMCubePrograbber here, see the other doc.

## Build Firwmware for octopus pro
In klipper directory:

```
$ make menuconfig
--------------------------------------------------------------------------------
                         Klipper Firmware Configuration
[*] Enable extra low-level configuration options
    Micro-controller Architecture (STMicroelectronics STM32)  --->
    Processor model (STM32F446)  --->
    Bootloader offset (32KiB bootloader)  --->
    Clock Reference (12 MHz crystal)  --->
    Communication interface (USB to CAN bus bridge (USB on PA11/PA12))  --->
    CAN bus interface (CAN bus (on PD0/PD1))  --->
    USB ids  --->
(500000) CAN bus speed
()  GPIO pins to set at micro-controller startup
--------------------------------------------------------------------------------
$ make clean
$ make
$ cp out/klipper.bin $HOME/CanFW/octopus_klipper.bin
```

## Build Firmware for EBB42
```
--------------------------------------------------------------------------------
                         Klipper Firmware Configuration
[*] Enable extra low-level configuration options
    Micro-controller Architecture (STMicroelectronics STM32)  --->
    Processor model (STM32G0B1)  --->
    Bootloader offset (8KiB bootloader)  --->
    Clock Reference (8 MHz crystal)  --->
    Communication interface (CAN bus (on PB0/PB1))  --->
(500000) CAN bus speed
()  GPIO pins to set at micro-controller startup
--------------------------------------------------------------------------------
$ make clean
$ make
$ cp out/klipper.bin $HOME/CanFW/ebb42_klipper.bin
```

## Flashing the firmwares
So this one's a little icky, but from CanBoot_OctopusPro/scripts directory:
```
$ ./flash_can.py -u ce429b316153 -f ~/CanFW/octopus_klipper.bin
.. This will fail and take down the can0 network, allowing you to flash over serial:
$ ./flash_can.py -d /dev/serial/by-id/usb-CanBoot_stm32f446xx_35000800165053424E363620-if00  -f ~/CanFW/octopus_klipper.bin
```
Once this is complete, the EBB42 should be available on the CAN Bus. If not a double press of the reset button (not the one by USBC connector, the other one), should work to get it available
```
$ ./flash_can.py -u 93da6270593a -f ~/CanFW/ebb42_klipper.bin
```

SHUTDOWN, POWER OFF, AND RESTART

## Misc notes
Use the following to check what is seen on the CAN Bus:
```
./flash_can.py  -q -i can0
```
It will probably show nothing except after flashing stuff.

