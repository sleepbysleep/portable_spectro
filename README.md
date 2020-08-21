# Introduction
I just want to make new kind of portable spectrometer, which would be equipped with more-efficient LED illumination ranging from 400nm to 1000nm than Tungsten-Hallogen lamp. The design intends to be switchable between two kinds of miniatured spectrometer(i.e. C12880MA, and ADS8860IDGS) for flexiblity. Indeed, the main controller should handle BlueTooth communication with intermediate device like smart phone or personal computer in order to gather the spectrum data into remote server.

In terms of Vis/NIR radiation, the useful choices are so-called braodband IR LED like SFH4736, SFH4776, and L1IG-0750100000000. Unfortunately, these things doesn't emit visible light except for violet band, which intends to make a phosper excited. So, I have to make the lighting in the combination of Vis LED and such a broadband LEDs. To accomplish proper illumination, many things have to be solved in aspects of blending ratio of Vis, and NIR, and how many LEDs are mounted on PCB for sufficient lighting. Therefore, the below have to be done;

1. Analyze the comparision of spectral intensity among SFH4736, SFH4776, and L1IG-0750100000000.
2. Determine blending ratio of Vis, and NIR LEDs for flatness of spectral intensity from Vis to NIR.
3. Design the hardware that can equip with nRF52840, Vis/NIR LEDs (corresponding driver as well), switchable spectrometer (corresponding periphertal indeed), and power management unit including Li-ion or Li-po battery, charging part, and boost regulator.
4. Develop the firmware that can process the BlueTooth communication with smart phone (iOS, and Android), interfacing the spectrometer, and interactive operation between LED-based illumination, and spectrometer.
5. Develop the application for iOS, and Android that can be portal to pass, and signal process the spectrum into the remote server.
6. Develop the remote server program, that can manage huge spectra with extra infomation (i.e. object, date, dependant like sugar content), and do machine learning, initially being PLS, and SVM-regression hopefully I want to goes into Deap Learning (CNN or RNN).

Definitly, all the staffs are very challenging to me in terms of funding, and workloads. I will do as much as possible!

# Hardware
The hardware consists of Li-ion or Li-po battery, charging unit, boost regulator, C12880MA spectrometer including analog amplifier, and ADC unit, AS7260X spectrometer, and nRF52840 BlueTooth CPU.

TODO: Block diagram

The schematic of ADS8860IDGS is referred from https://www.ti.com/lit/df/tidrau9a/tidrau9a.pdf
TODO: add image with highlight of the copied.

The schematic of C12880MA is referred from https://github.com/Alexey-Danilchenko/Spectron
TODO: add image with highlight of the copied

The schematic of AS7260x is referred from


The schematic of MCP73871 is referred from 


# Mechanical Drawing

# Spectral intesity comparison
TODO: add the graph among them.

# Firmware Design
openocd, my budget is the stlink v2
TODO: add Flow chart
TODO: add block diagram

# Integration time calculation

# App Design

# Server Design

# BOM
Spectrometer: C12880MA, or AS7260x be switchable
CPU: nrf52840, mdbt501-v10
Candidators for Light source: SFH4736, or SFH4776, or L1IG-0750100000000

# References
https://github.com/Alexey-Danilchenko/Spectron

# Appendix

## nRF52840 development environment

Install Toolchain on debian testing(currently bulleseye)
$ apt install libnewlib-dev libstdc++-arm-none-eabi-newlib libnewlib-arm-none-eabi gcc-arm-none-eabi binutils-arm-none-eabi

Verify the version of installed gcc
$ arm-none-eabi-gcc --version
arm-none-eabi-gcc (15:7-2018-q2-6+b1) 7.3.1 20180622 (release) [ARM/embedded-7-branch revision 261907]
Copyright (C) 2017 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

Download, and install the nRF5 SDK
Go to https://www.nordicsemi.com/Software-and-Tools/Software/nRF5-SDK/, and
download the latest verion of SDK at https://www.nordicsemi.com/-/media/Software-and-other-downloads/SDKs/nRF5/Binaries/nRF5_SDK_16.0.0_98a08e2.zip
Unzip nRF5_SDK_16.0.0_98a08e2.zip like as:
nRF5_SDK_16.0.0_98a08e2
+- components
+- config
+- documentation
+- examples
+- external
+- external_tools
+- intergation
+- modules
+- license.txt
+- nRF5x_MDK_8_27_0_IAR_NordicLicense.msi
+- nRF5x_MDK_8_27_0_Keil4_NordicLicense.msi

At "$(SDK_ROOT)/components/toolchain/gcc" under SDK, there are several kinds of Makefile for Windows/Linux/MacOs.
Now, we should edit "Makefile.posix" in the case of Linux or MacOs like the below.

GNU_INSTALL_ROOT ?= /usr/local/gcc-arm-none-eabi-7-2018-q2-update/bin/
GNU_VERSION ?= 7.3.1
GNU_PREFIX ?= arm-none-eabi

into

GNU_INSTALL_ROOT ?= /usr/bin/
GNU_VERSION ?= 7.3.1
GNU_PREFIX ?= arm-none-eabi

Try to compile the example.
Under "$(SDK_ROOT)/examples/peripheral/blinky/PCA10056/blank/armgcc", 
$ make
mkdir _build
cd _build && mkdir nrf52840_xxaa
Assembling file: gcc_startup_nrf52840.S
Compiling file: nrf_log_frontend.c
Compiling file: nrf_log_str_formatter.c
Compiling file: boards.c
Compiling file: app_error.c
Compiling file: app_error_handler_gcc.c
Compiling file: app_error_weak.c
Compiling file: app_util_platform.c
Compiling file: nrf_assert.c
Compiling file: nrf_atomic.c
Compiling file: nrf_balloc.c
Compiling file: nrf_fprintf.c
Compiling file: nrf_fprintf_format.c
Compiling file: nrf_memobj.c
Compiling file: nrf_ringbuf.c
Compiling file: nrf_strerror.c
Compiling file: nrfx_atomic.c
Compiling file: main.c
Compiling file: system_nrf52840.c
Linking target: _build/nrf52840_xxaa.out
   text	   data	    bss	    dec	    hex	filename
   2168	    112	    172	   2452	    994	_build/nrf52840_xxaa.out
Preparing: _build/nrf52840_xxaa.hex
Preparing: _build/nrf52840_xxaa.bin
DONE nrf52840_xxaa

## Install programmer for nRF52, and setup correspondances
Download nRF Command Line Tools via https://www.nordicsemi.com/Software-and-tools/Development-Tools/nRF-Command-Line-Tools/Download#infotabs
In my case of Debian Linux(), the nRF-Command-Line-Tools_10_5_0_Linux-amd64.tar.gz is appropriate.

If you want to generate DFU formatted binary, please see the nRF-Utils for desktop at https://www.nordicsemi.com/Software-and-tools/Development-Tools/nRF-Util. Its actual location is https://github.com/NordicSemiconductor/pc-nrfutil. It isn't dependent from installed OS due to python package.

## Make the new project
Make new directory named as "projects" under the root of SDK.
$ mkdir projects
$ cd projects
$ mkdir template
$ cd ../

Copy files from main.c at examples/peripheral/bliky/, blinky_gcc_nrf52.ld, and Makefile at examples/peripheral/blinky/pca10056/blank/armgcc, and config/sdk_config.h at examples/peripheral/blinky/pca10056/blank into newly created project of projects/template.

$ cp -r ./examples/peripheral/blinky/main.c ./examples/peripheral/blinky/pca10056/blank/armgcc/blinky_gcc_nrf52.ld ./examples/peripheral/blinky/pca10056/blank/armgcc/Makefile ./examples/peripheral/blinky/pca10056/blank/config ./projects/template

Change some definitions in Makefile like;

SDK_ROOT := ../../../../../..
PROJ_DIR := ../../..
...
INC_FOLDERS += \
  $(SDK_ROOT)/components \
  $(SDK_ROOT)/modules/nrfx/mdk \
  $(PROJ_DIR) \
  $(SDK_ROOT)/components/libraries/strerror \
  $(SDK_ROOT)/components/toolchain/cmsis/include \
  $(SDK_ROOT)/components/libraries/util \
  ../config \
...
CFLAGS += -Wall -Werror
as

SDK_ROOT := ../..
PROJ_DIR := .
...
INC_FOLDERS += \
  $(SDK_ROOT)/components \
  $(SDK_ROOT)/modules/nrfx/mdk \
  $(PROJ_DIR) \
  $(SDK_ROOT)/components/libraries/strerror \
  $(SDK_ROOT)/components/toolchain/cmsis/include \
  $(SDK_ROOT)/components/libraries/util \
  ./config \
...
CFLAGS += -Wall #-Werror

Finally, make sure the compilation done.
daysleep@t480s-debian ~/W/p/f/p/template> make
mkdir _build
cd _build && mkdir nrf52840_xxaa
Assembling file: gcc_startup_nrf52840.S
Compiling file: nrf_log_frontend.c
Compiling file: nrf_log_str_formatter.c
Compiling file: boards.c
Compiling file: app_error.c
Compiling file: app_error_handler_gcc.c
Compiling file: app_error_weak.c
Compiling file: app_util_platform.c
Compiling file: nrf_assert.c
Compiling file: nrf_atomic.c
Compiling file: nrf_balloc.c
Compiling file: nrf_fprintf.c
Compiling file: nrf_fprintf_format.c
Compiling file: nrf_memobj.c
Compiling file: nrf_ringbuf.c
Compiling file: nrf_strerror.c
Compiling file: nrfx_atomic.c
Compiling file: main.c
Compiling file: system_nrf52840.c
Linking target: _build/nrf52840_xxaa.out
   text	   data	    bss	    dec	    hex	filename
   2168	    112	    172	   2452	    994	_build/nrf52840_xxaa.out
Preparing: _build/nrf52840_xxaa.hex
Preparing: _build/nrf52840_xxaa.bin
DONE nrf52840_xxaa
