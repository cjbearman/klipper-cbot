# klipper-cbot
Klipper config for experimental under-development core-xy printer.

# Custom GCODE (superslicer)
START_PRINT=[first_layer_bed_temperature] EXTRUDER_TEMP=[first_layer_temperature]

END_PRINT

# MCU Config
Enable low level config (YES)
STM32 / STM32F446
32KiB bootloader
12Mhz crystal
Comms USB on PA11/PA12

USB:
0x1d50 VID
0x614e DID
USB serial number from CHIPID
