Hichip HCRTOS SDK User Manual
==========================

>Notice: this is a work in progress and much needs to be added to this to make it complete

>TODO: 

> [ ] Figure out how to make sub-items in TOC actually show as sub-items

> [ ] Figure out how to make the structure diagram render properly 

Table of Contents
-------------------
1. Development environment settings

2. Set the local download path of the third-party software

3. Software related design

  3.1 Haiqi hcrtos sdk software framework:

  3.2 SDK directory introduction

4. Compile and burn hcrtos
  
  4.1 Compile the SDK
  
  4.2 Burning-with-GDB

5. hcrtos sdk configuration design

  5.1 General configuration

  5.2 Other related different configurations will be specified in the general configuration

    5.2.1 Configuration of DTS

6. Supported functions

  6.1 hcrtos kernel driver

  6.2 hcboot

    6.2.1 From bootrom to hcboot

    6.2.3 Compression and decompression

    6.2.4 hcrtos driver

  6.3 Local and network multimedia playback

    6.3.1 ffplayer interface

    6.3.2 audio/video driver interface

    6.3.3 mplayer

  6.4 Boot show-logo/show-av

7. About the configuration of different boards

  7.1 DDR configuration

  7.2 Debug serial port configuration

8. Driver header file

9. GDB debugging tool

10. About 4K decoding and output

11. LCD display configuration instructions

  11.1 Screen verification configuration summary

  11.2 Quick Configuration Instructions for LCD Display

  11.3 Configuration instructions of RGB and LVDS

  11.4 MIPI configuration

12. Quick Development Guide

  12.1 SDK configuration, compilation and packaging

    12.1.1 SDK compilation and packaging

  12.2 Introduction to wireless projection

    12.2.1 Introduction of wireless projection code

    12.2.2 dlna interface function

    12.2.3 miracast interface functions

    12.2.4 aircast interface function

    12.2.5 wifi manager interface function

    12.2.6 Wired same screen interface function

  12.3 How to enable & disable & replace boot show logo

    12.3.1 How to enable boot show logo

      12.3.1.1 If it is an uncompiled project,

      12.3.1.2 If it is a compiled project

    12.3.2 How to close the boot show logo

    12.3.3 How to change the boot show logo

    12.3.4 How to replace the .h264 logo

  12.4 How to rotate the screen.

    12.4.1 Rotation of OSD

    12.4.2 Rotation of video:

    12.4.3 One key rotation function

    12.4.4 How to rotate and set the resolution when mirroring the same screen

  12.5 SD Card Driver

  12.6 LVGL maximum supported frame number configuration

  12.7 How to add customized board-level configuration in addition to the demo configuration

    12.7.1 Add a board-level configuration

  12.8 Distribution of hcRTOS Nor Flash interval

    12.8.1 Process Introduction

    12.8.2 How to add a client-readable and writable partition?

    12.8.3 How to temporarily add an unsupported flash

  12.9 Configuration of uart bluetooth

    12.9.1 Serial Bluetooth

  12.10 wifi support

  12.11 Integration and testing of TP drivers

  12.12 hdmi in debugging

  12.13 OSD layer introduction and resolution modification

  12.14 Introduction, use and test of VIDEO layer

  12.15 Key support

    12.15.1 How to add ir remote control

    12.15.2 How to add adc key support

  12.16 How to pull up or pull down gpio during boot startup

  12.17 Firmware Upgrade

    12.17.1 Introduction of hcfota

    12.17.2 hcfota Implementation Principle

    12.17.3 Schemes supported by hcfota

    12.17.4 hcfota related code introduction

    12.17.5 hcfota configuration

    12.17.6 hcfota debugging: How does the app call the hcfota interface

  12.8 How to open cjc8988?

    12.8.1 Hardware requirements:

    12.8.2 Software requirements:

    12.8.3 Test method

    12.8.4 Test results

    12.8.5 Other similar chips

13. FAQ Q&A

  13.1 Checkout to a new branch, such as updating from 2022.07.y to 2022.09.y, can't compile?

  13.2 The serial port RX of different versions of B200 is different, such as:

  13.3 After the screen is configured, the displayed picture is abnormal

  13.4 Prompt that the tool chain is not installed?

  13.5 Wget parameter error when compiling?

  13.6 The compiler prompts that the hdmirx and usbmirror libraries cannot be found?

  13.7 After compiling, the board has no sound?

  13.8 When encountering hcrtos SDK problems, how to report to the original factory?

  13.9 Does the 1st generation chip support nand flash & spi nand?

 
1. Development environment settings
----
HCRTOS builds a host-side development environment based on Ubuntu 18.04.5 LTS. Both the desktop version and the server version of Ubuntu can be used as the development environment. Since the desktop version comes with rich tools, it is recommended to use the desktop version of Ubuntu.

The system language of Ubuntu needs to be set to English. If the system language is not English, you can modify it in the following way

    1. Edit the file /etc/default/locale
    2. Change to English configuration: LANG=”en_US.UTF-8′′LANGUAGE=”en_US:en”
    3. Then restart Ubuntu to restore the English language environment.

It is recommended to use Huawei's source when installing software on the Ubuntu host

    $ sudo cp /etc/apt/sources.list /etc/apt/source.list.bak

Create a new /etc/apt/source.list file with the following contents:

    # deb cdrom:[Ubuntu 18.04.5 LTS _Bionic Beaver_ - Release amd64 (20200806.1)]/bionic main restricted

    # See http://help.ubuntu.com/community/UpgradeNotes for how to upgrade to
    # newer versions of the distribution.deb http://mirrors.huaweicloud.com/repository/ubuntu/ bionic main restricted
    # deb-src http://cn.archive.ubuntu.com/ubuntu/ bionic main restricted

    ## Major bug fix updates produced after the final release of the
    ## distribution.
    deb http://mirrors.huaweicloud.com/repository/ubuntu/ bionic-updates main restricted
    # deb-src http://cn.archive.ubuntu.com/ubuntu/ bionic-updates main restricted

    ## N.B. software from this repository is ENTIRELY UNSUPPORTED by the Ubuntu
    ## team. Also, please note that software in universe WILL NOT receive any
    ## review or updates from the Ubuntu security team.
    deb http://mirrors.huaweicloud.com/repository/ubuntu/ bionic universe
    # deb-src http://cn.archive.ubuntu.com/ubuntu/ bionic universe
    deb http://mirrors.huaweicloud.com/repository/ubuntu/ bionic-updates universe
    # deb-src http://cn.archive.ubuntu.com/ubuntu/ bionic-updates universe

    ## N.B. software from this repository is ENTIRELY UNSUPPORTED by the Ubuntu
    ## team, and may not be under a free licence. Please satisfy yourself as to
    ## your rights to use the software. Also, please note that software in
    ## multiverse WILL NOT receive any review or updates from the Ubuntu
    ## security team.
    deb http://mirrors.huaweicloud.com/repository/ubuntu/ bionic multiverse
    # deb-src http://cn.archive.ubuntu.com/ubuntu/ bionic multiverse
    deb http://mirrors.huaweicloud.com/repository/ubuntu/ bionic-updates multiverse
    # deb-src http://cn.archive.ubuntu.com/ubuntu/ bionic-updates multiverse

    ## N.B. software from this repository may not have been tested as
    ## extensively as that contained in the main release, although it includes
    ## newer versions of some applications which may provide useful features.
    ## Also, please note that software in backports WILL NOT receive any review
    ## or updates from the Ubuntu security team.
    deb http://mirrors.huaweicloud.com/repository/ubuntu/ bionic-backports main restricted universe multiverse
    # deb-src http://cn.archive.ubuntu.com/ubuntu/ bionic-backports main restricted universe multiverse

    ## Uncomment the following two lines to add software from Canonical's
    # 'partner' repository.
    ## This software is not part of Ubuntu, but is offered by Canonical and the
    ## respective vendors as a service to Ubuntu users.
    # deb http://archive.canonical.com/ubuntu bionic partner
    # deb-src http://archive.canonical.com/ubuntu bionic partner

    deb http://mirrors.huaweicloud.com/repository/ubuntu/ bionic-security main restricted
    # deb-src http://security.ubuntu.com/ubuntu bionic-security main restricted
    deb http://mirrors.huaweicloud.com/repository/ubuntu/ bionic-security universe
    # deb-src http://security.ubuntu.com/ubuntu bionic-security universe
    deb http://mirrors.huaweicloud.com/repository/ubuntu/ bionic-security multiverse
    # deb-src http://security.ubuntu.com/ubuntu bionic-security multiverse

update source:

    $ sudo apt update

The Ubuntu host environment needs to install the following tools:

    shell
    $ sudo apt install git
    $ sudo apt install gcc
    $ sudo apt install flex
    $ sudo apt install bison
    $ sudo apt install gperf
    $ sudo apt install make
    $ sudo apt install python
    $ sudo apt install unzip
    $ sudo apt install rar
    $ sudo apt install dos2unix
    $ sudo apt install swig
    $ sudo apt install python-dev
    $ sudo apt install python3-dev
    $ sudo apt install python3-pip
    $ sudo apt install clang-format
    $ sudo apt install python3
    $ sudo apt install python3-pip
    $ sudo apt install python3-setuptools
    $ sudo apt install python3-wheel
    $ sudo apt install ninja-build
    $ sudo apt install rename
    $ sudo apt install gdb
    $ sudo apt install apache2
    $ sudo apt install re2c
    $ sudo apt install ctags
    $ sudo apt install lzip
    $ sudo apt install libncurses-dev
    $ sudo apt install tree
    $ sudo apt install pkg-config
    $ sudo apt install cmake
    $ sudo apt install python-pip
    $ sudo apt install automake
    $ sudo apt install lzop
    $ sudo apt install doxygen
    $ sudo apt install graphviz
    $ sudo apt install libssl-dev

Haiqi's hcrtos SDK needs to use a mips32R1 toolchain, after downloading, install it to /opt/mips32-mti-elf

The download path is as follows:
  ><https://gitlab.hichiptech.com:62443/sw/dl/-/blob/main/Codescape.GNU.Tools.Package.2019.09-03-2.for.MIPS32.MTI.Bare.Metal.Ubuntu-18.04.5.x86_64.tar.gz>

Notice! ! ! : hichip freeRTOS SDK version 2022.09.y and earlier versions use:
  ><https://gitlab.hichiptech.com:62443/sw/dl/-/blob/main/Codescape.GNU.Tools.Package.2019.09-03.for.MIPS32.MTI.Bare.Metal.Ubuntu-18.04.5.x86_64.tar.gz>

Versions after 2022.09.y use:
  ><https://gitlab.hichiptech.com:62443/sw/dl/-/blob/main/Codescape.GNU.Tools.Package.2019.09-03-2.for.MIPS32.MTI.Bare.Metal.Ubuntu-18.04.5.x86_64.tar.gz>

    $ sudo tar -xzf Codescape.GNU.Tools.Package.2019.09-03-2.for.MIPS32.MTI.Bare.Metal.Ubuntu-18.04.5.x86_64.tar.gz -C /opt
    $ ls /opt/mips32-mti-elf/2019.09-03-2/bin/ -l
    total 126760
    -rwxr-xr-x 1 ps ps 5612736 6月  17 09:11 mips-mti-elf-addr2line
    -rwxr-xr-x 2 ps ps 5791072 6月  17 09:11 mips-mti-elf-ar
    -rwxr-xr-x 2 ps ps 8706848 6月  17 09:11 mips-mti-elf-as
    -rwxr-xr-x 2 ps ps 5157136 6月  17 09:38 mips-mti-elf-c++
    -rwxr-xr-x 1 ps ps 5562648 6月  17 09:11 mips-mti-elf-c++filt
    -rwxr-xr-x 1 ps ps 5150200 6月  17 09:38 mips-mti-elf-cpp
    -rwxr-xr-x 1 ps ps  108776 6月  17 09:11 mips-mti-elf-elfedit
    -rwxr-xr-x 2 ps ps 5157136 6月  17 09:38 mips-mti-elf-g++
    -rwxr-xr-x 2 ps ps 5136928 6月  17 09:38 mips-mti-elf-gcc
    -rwxr-xr-x 2 ps ps 5136928 6月  17 09:38 mips-mti-elf-gcc-7.4.0
    -rwxr-xr-x 1 ps ps  163320 6月  17 09:38 mips-mti-elf-gcc-ar
    -rwxr-xr-x 1 ps ps  163168 6月  17 09:38 mips-mti-elf-gcc-nm
    -rwxr-xr-x 1 ps ps  163176 6月  17 09:38 mips-mti-elf-gcc-ranlib
    -rwxr-xr-x 1 ps ps 3875864 6月  17 09:38 mips-mti-elf-gcov
    -rwxr-xr-x 1 ps ps 3283736 6月  17 09:38 mips-mti-elf-gcov-dump
    -rwxr-xr-x 1 ps ps 3535840 6月  17 09:38 mips-mti-elf-gcov-tool
    -rwxr-xr-x 1 ps ps 6165440 6月  17 09:11 mips-mti-elf-gprof
    -rwxr-xr-x 4 ps ps 7740896 6月  17 09:11 mips-mti-elf-ld
    -rwxr-xr-x 4 ps ps 7740896 6月  17 09:11 mips-mti-elf-ld.bfd
    -rwxr-xr-x 2 ps ps 5655368 6月  17 09:11 mips-mti-elf-nm
    -rwxr-xr-x 2 ps ps 6370296 6月  17 09:11 mips-mti-elf-objcopy
    -rwxr-xr-x 2 ps ps 7942520 6月  17 09:11 mips-mti-elf-objdump
    -rwxr-xr-x 2 ps ps 5791096 6月  17 09:11 mips-mti-elf-ranlib
    -rwxr-xr-x 2 ps ps 2062496 6月  17 09:11 mips-mti-elf-readelf
    -rwxr-xr-x 1 ps ps 5600912 6月  17 09:11 mips-mti-elf-size
    -rwxr-xr-x 1 ps ps 5597816 6月  17 09:11 mips-mti-elf-strings
    -rwxr-xr-x 2 ps ps 6370288 6月  17 09:11 mips-mti-elf-strip

As shown above, it means that the mips32R1 tool chain is installed.

Compile the hcrtos SDK for the first time, because the SDK will download some open source software and tools from the official website to ensure that the host development environment is connected to the network.

    $ cd hcrtos
    $ make O=bl hichip_hc15xx_db_b100_v12_hcscreen_bl_defconfig
    $ make O=bl all
    $ make hichip_hc15xx_db_b100_v12_hcscreen_defconfig
    $ make all

After the compilation is completed, a file that can be used for GDB download debug or flash burning will be generated in the output/images directory.

    $ tree output/images/
    output/images/
    ├── aux_code_ddr2_64M_1066MHz.abs
    ├── bootloader.bin
    ├── dtb.bin
    ├── flashwr_unify.abs
    ├── fs-partition1-root
    │   └── logo.hc
    ├── fs-partition2-root
    ├── fs-partition3-root
    ├── hc15xx-db-a110.dtb
    ├── hc15xx_ddr2_64M_1066MHz.abs
    ├── hcboot.bin
    ├── hcboot.out
    ├── hcboot.out.map
    ├── hcboot.uncompress.bin
    ├── hcboot.uncompress.map
    ├── hcboot.uncompress.out
    ├── hcdemo.bin
    ├── hcdemo.bin.gz
    ├── hcdemo.out
    ├── hcdemo.out.map
    ├── hcdemo.uImage
    ├── HCFota_Generator.exe
    ├── hcprog.ini
    ├── HCProgram_bridge.exe
    ├── persistentmem.bin
    ├── romfs.img
    ├── sfburn.bin
    ├── sfburn.bin.d82acd59b3e10ed73c7c79fcc9af185e
    ├── sfburn.ini
    └── updater.bin

Subsequent compilation needs to be carried out according to the changed content!

2. Set the local download path of the third-party software
---

The hcrtos SDK will download some third-party software packages during the compilation process, usually from the public network. Sometimes the download is slow or impossible due to network problems, you can use other methods to download the third-party software package in advance. Then you can choose to download the third-party software from the downloaded path through the configuration of hcrtos.

For example, download a third-party software package to a local path:

    /media/data/dl

By default, hcrtos sdk will try to download from http://hichip01/dl, which is the http server established by Hichip’s internal network, and the external network cannot be connected, so the sdk will download from the public network after the attempt fails. After the third-party software package has been downloaded, you can modify the preferred download path of the third-party software as follows:

    $ cd hcrtos
    $ make menuconfig
    > Mirrors and Download locations
      (file:///media/data/dl) Primary download site

Save time for downloading third-party software by modifying the preferred download path of third-party software to file:///media/data/dl.

3. Software related design
3.1 Haiqi hcrtos sdk software framework:

The system startup of Hatch hcRTOS adopts hcboot.
Hcboot is developed based on the hcRTOS development platform, and is used to start the hcRTOS system firmware after deep cuts.
hcRTOS is developed based on FreeRTOS v10.4.4 kernel and provides Posix Compatible API

> Hcboot (bootloader)  --> hcRTOS

Its simplified structure diagram is as follows:

  +----------------------------------------------------+    
  |                                                    |    
  |                                                    |   
  |             Application                            |    
  |                                                    |    
  |                                                    |    
  +----------------------------------------------------+
-------------------------VFS--------------------------------------
  +----------------------------------------------------+    
  |                                                    |    
  |                                                    |    
  |                  Driver                            |    
  |                                                    |    
  |                                                    |    
  +----------------------------------------------------+    

  +----------------------------------------------------+    
  |                                                    |    
  |                                                    |    
  |                   Hardware                         |    
  |                                                    |    
  |                                                    |    
  +----------------------------------------------------+

The detailed architecture diagram is as follows:

[ image needs to go here from pg. 8]

- The driver provides posix standard `open()`/`close()`/`read()`/`write()`/`poll()`/`ioctl()` interfaces to the application through VFS.

3.2 SDK directory introduction

    $ tree -L 2.
    ├── board                                       // Board config
    │   ├── hc15xx                                  // 1st generation HC chip board level config
    │   ├── hc16xx                                  // 2nd generation HC chip board level config
    │   └── hc1xxx
    ├── build                                       // Compile organization makefile and script related.
    │   ├── app-deps.mk
    │   ├── download
    │   ├── gnuconfig
    │   ├── kconfig
    │   ├── Kconfig.dummy
    │   ├── Makefile.build
    │   ├── Makefile.clean
    │   ├── Makefile.entry
    │   ├── Makefile.in
    │   ├── Makefile.link
    │   ├── Makefile.rules
    │   ├── misc
    │   ├── pkg-autotools.mk
    │   ├── pkg-cmake.mk
    │   ├── pkg-download.mk
    │   ├── pkg-generic.mk
    │   ├── pkg-release.mk
    │   ├── pkg-utils.mk
    │   ├── scripts
    │   └── tools
    ├── components                                  // Code path
    │   ├── applications                            // Upper application.
    │   ├── bluetooth                               // Bluetooth driver
    │   ├── cjson                                                            
    │   ├── cmds                                    // Serial command line test code
    │   ├── dtc                                                                
    │   ├── ffmpeg                                  // Media playback middleware
    │   ├── hccast                                  // HC screen sharer component
    │   ├── hc-examples                             // HC app single-function test case
    │   ├── hcfota                                  // HC upgrade components
    │   ├── hclib                                   // Underlying library file, unopened code
    │   ├── Kconfig
    │   ├── kernel                                  // Kernel driver component
    │   ├── ld                                      // Link script
    │   ├── libcurl                                 // libcurl related
    │   ├── liblvgl                                 // lvgl library code
    │   ├── liblzo                                  // Compression algorithm code
    │   ├── libogg                                  // ogg library
    │   ├── libopenssl                              // openssl related library code
    │   ├── libusb                                  // usb library related
    │   ├── libvorbis                               // vorbis lirary
    │   ├── lzo1x                                   // lzo compression algorithm
    │   ├── newlib                                  // standard library
    │   ├── opencore-amr                            // amr library
    │   ├── prebuilts                               // HC precompiled library files
    │   ├── pthread                              
    │   ├── quicklz
    │   ├── uboot-tools
    │   └── zlib                                    // zlib compression algorithm
    ├── configs                                     // Chip configuration
    │   ├── hichip_hc15xx_db_a210_hcscreen_bl_defconfig                     // bootloader configuration of a210
    │   ├── hichip_hc15xx_db_a210_hcscreen_defconfig                        // app configuration of a210
    │   ├── hichip_hc16xx_cb_d3101_v20_projector_c1_cj01_bl_defconfig       // bootloader configuration of d3101
    │   ├── hichip_hc16xx_cb_d3101_v20_projector_c1_cj01_defconfig          // app configuration of d3101
    ├── dl
    │   ├── cjson
    │   ├── dtc
    │   ├── libopenssl
    │   ├── uboot-tools
    │   └── zlib
    ├── document                                    // HC development documents related
    │   ├── 1600 hcRTOS PQ工具使用说明.pdf
    │   ├── HCProgram_i2c_master_userguide.pdf
    │   ├── hcrtos-keyadc使用说明.pdf                // hcrtos-keyadc Instructions for use.pdf
    │   ├── hcrtos-pinmux配置说明.pdf                // hcrtos-pinmux configuration instructions.pdf
    │   ├── hcrtos_uart_upgrade_user_guide.pdf
    │   ├── hcrtos-uart使用说明.pdf                  // hcrtos-uart instruction.pdf
    │   ├── hcrtos_user_manual.pdf
    │   ├── hcrtos_wifi_user_guide.pdf
    │   ├── hichipgdb_userguide.pdf
    │   └── stack_probe_tool_for_debug.pdf
    ├── hrepo
    ├── Kconfig                                     // Total configuration file
    ├── Makefile                                    // Total Makefile
    ├── output                                      // The final app compiles intermediate files and compiled output files, the default final output folder is not output
    │   ├── build
    │   ├── host
    │   ├── images
    │   ├── include
    │   └── staging
    ├── output_bl                                   // bootloader output file. The folder name comes from O=xxx at compile time
    │   ├── build
    │   ├── host
    │   ├── include
    │   └── staging
    └── target    
    ├── chip    
    └── Kconfig

4. Compile and burn hcrtos
----
4.1 Compile the SDK

    make O=bl hichip_hc15xx_db_a210_hcscreen_bl_defconfig
    make O=bl all
    make hichip_hc15xx_db_a210_hcscreen_defconfig
    make all

Related common compilation commands:

    make O=bl kernel-rebuild all
    make O=bl menuconfig
    make kernel-rebuild all
    make menuconfig
    make apps-hcscreen-rebuild
    make hccast-rebuild
    ...

4.2 Burning-with-GDB
----

After the compilation is complete, the following file will be generated under hcrtos/output/images for GDB to burn norflash.

    $ tree output/images
    output/images/
    ├── flashwr_unify.abs                                 // flash initialization file.
    ├── sfburn.bin                                        // abs file, consistent with the abs file used in ini
    ├── sfburn.bin.65501d4f2d86f9dda9abc173271a5773       // abs file used in ini
    ├── sfburn.ini                                        // The configuration file used when burning gdb

Use GDB tool to download hcrtos/output/images/sfburn.ini to memory, and press F5 to run. At this point, you can see the process of burning norflash in the **Mst Output** sub-tab of the **output** window of the GDB tool, as shown below, until the burning is completed. After the burning is completed, restart the development board to start the system.

    Flash size is 1000000
    Flash Writer: source=0x80060000, target(Offset)=0x00000000, length=0x00280000
    Flash size large than 4M, don't do CRC check!
    00000000 OK!
    00010000 OK!
    00020000 OK!
    00030000 OK!
    00040000 OK!
    00050000 OK!
    00060000 OK!
    00070000 OK!
    00080000 OK!
    00090000 OK!
    000a0000 OK!
    000b0000 OK!
    000c0000 OK!
    000d0000 OK!
    000e0000 OK!
    000f0000 OK!
    00100000 OK!
    00110000 OK!
    00120000 OK!
    00130000 OK!
    00140000 OK!
    00150000 OK!
    00160000 OK!
    00170000 OK!
    00180000 OK!
    00190000 OK!
    001a0000 OK!
    001b0000 OK!
    001c0000 OK!
    001d0000 OK!
    001e0000 OK!
    001f0000 OK!
    00200000 OK!
    00210000 OK!
    00220000 OK!
    00230000 OK!
    00240000 OK!
    00250000 OK!
    00260000 OK!
    00270000 OK!
    NOR FLASH Burnning Done!

**For more detailed programming steps, please refer to Chapter 9: GDB Debugging Tool**

5. hcrtos sdk configuration design
====
5.1 General configuration
----
The top-level total configuration is in the `hcrtos/configs` directory, which can be viewed with `ls configs`. Then select the corresponding defconfig according to the corresponding board model

    | Configuration file        | Description                        |
    | :------------------------------------------------------------- | ----------------------------------------:|
    | hcrtos/configs/hichip_hc15xx_db_b100_v12_hcdemo_bl_defconfig   | Configuration file for generating hcboot |
    | hcrtos/configs/hichip_hc15xx_db_b100_v12_hcdemo_defconfig      | for generating firmware                  |

5.2 Other related different configurations will be specified in the general configuration
----
5.2.1 Configuration of DTS
----
HC hcRTOS SDK uses device tree to describe some device drivers. In order to facilitate unified management, Hichip DTS files are placed in the (`hcrtos/board/hichip/hc1xxx/dts`) directory

    CONFIG_CUSTOM_DTS_PATH="$(TOPDIR)/board/hc15xx/common/dts/hc15xx-db-b100-hcscreen.dts"

The DTS of hcboot and main firmware are usually specified as the same set of configurations, specified in `hichip_hc15xx_db_b100_v12_hcdemo_bl_defconfig` and `hichip_hc15xx_db_b100_v12_hcdemo_defconfig` respectively.

DTS is embedded in the firmware by default. If DTS is modified, recompile with the following command, both must be compiled, because DTS is shared by boot and firmware:

    $ make O=bl kernel-rebuild all
    $ make kernel-rebuild all


6. Supported functions
====
6.1 hcrtos kernel driver
----
6.2 hcboot
----
6.2.1 From bootrom to hcboot

The `hcrtos/output/images/bootloader.bin` file is formed by merging the hcboot.bin and ddr initialization files.

The ddr initialization files are located in the `board/hichip/hc1xxx/ddrinit` directory, and each file is 4KB. The mapping method between ddr initialization file and hcboot on norflash is as follows.

```
                                                         On-DRAM

                                                                   memory-size
                                                     +------------+<---------
                                                     |            |
                                                     |            |
                       On-Flash                      |            |
                                                     |            |
    up_align32(4k+uboot_len)                         |            |
     ----------->+------------------------+          |            |
                 |                        |          |            |
                 |                        |          |            |
                 |                        |          |            |
                 |                        |          |            |
                 |                        |          |            |
                 |                        |          |            |
                 |                        |          |            |
                 |         hcboot         |          |            |
                 |                        |          |            |
                 |                        |          +------------+
                 |                        |          |            |
                 |                        |          |   hcboot   |
     4k          |                        |          |            |
    HCRTOS_BOOTMEM_OFFSET
    ----------->+------------------------+          +------------+<---------
                |                        |          |            |
                |   ddr-init-parameters  |          |            |
    0           |                        |          |            |         0
    ----------->+------------------------+          +------------+<---------
```

The bootrom program inside the chip will obtain ddr initialization data from the front 4KB of norflash and initialize the memory. Then load hcboot to the `HCRTOS_BOOTMEM_OFFSET` (this variable is set by macro definition in the DTS file) location of the memory and run it. In addition to the ddr parameter, there are other parameters in the ddr initialization file, including the location of hcboot on norflash (4KB by default), the size of hcboot, and the address where hcboot will be loaded into the memory to run. Since the size of hcboot will change frequently, when the `bootloader.bin` is generated in the `post-build.sh` script of hcrtos SDK, the ddr initialization file will be dynamically modified and the real size of hcboot (aligned up to 32 bytes) will be updated. Reference script:

    $ cat hcrtos/board/hichip/hc1xxx/post-build.sh
    if [ -f ${IMAGES_DIR}/hcboot.out ] && [ -f ${IMAGES_DIR}/hcboot.bin ] && [ -f
    ${BR2_EXTERNAL_BOARD_DDRINIT_FILE} ]; then

    message "Generating bootloader.bin ....."
    ...
    ...
    message "Generating bootloader.bin done!"
    fi

