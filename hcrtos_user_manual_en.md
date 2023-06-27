Hichip HCRTOS SDK User Manual
==========================

>Notice: this is a work in progress and much needs to be added to this to make it complete

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

----
 
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

```
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
```

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

```
    $ make O=bl kernel-rebuild all
    $ make kernel-rebuild all
```

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

```
    $ cat hcrtos/board/hichip/hc1xxx/post-build.sh
    if [ -f ${IMAGES_DIR}/hcboot.out ] && [ -f ${IMAGES_DIR}/hcboot.bin ] && [ -f
    ${BR2_EXTERNAL_BOARD_DDRINIT_FILE} ]; then

    message "Generating bootloader.bin ....."
    ...
    ...
    message "Generating bootloader.bin done!"
    fi
```

6.2.3 Compression and decompression
---

When making uImage, you can specify different compression methods or no compression. refer to:

```
  $ cat hcrtos/board/hc15xx/post-build.sh
  if [ -f ${IMAGES_DIR}/${app}.bin ] ; then
    message "Generating ${app}.uImage ....."
    gzip -kf9 ${IMAGES_DIR}/${app}.bin > ${IMAGES_DIR}/${app}.bin.gz
    ${HOST_DIR}/bin/mkimage -A mips -O u-boot -T standalone -C gzip -n ${app}-e ${app_ep} -a ${app_load} \
      -d ${IMAGES_DIR}/${app}.bin.gz ${IMAGES_DIR}/${app}.uImage        
    message "Generating ${app}.uImage done!"
  fi
```

From 2022/9/*, hcrtos supports three compression/decompression methods (lzma/lzo/gzip), which can be configured according to needs. Specific reference: SDK-compression and self-extraction setting method.pdf

6.2.4 hcrtos driver
----

Both hcboot and the main firmware program are compiled based on hcrtos configuration. Its drivers are shared. The difference is that the underlying audio/video drivers are provided as precompiled static library files. Since hcboot requires the code size to be as small as possible, the underlying audio/video precompiled library is different. Configure by specifying a different prebuilt library path

The precompiled library of the main program:

```
  $ cd hcrtos
  $ make menuconfig
  > Components
  ()  Prebuilt subdir    /* If the disposition is empty, it means to link the library in `hcrtos/components/prebuilts/sysroot` */
  [*] prebuilts  --->
    [*]   ffplayer
    -*-   audio driver
    -*-   video driver
    [*]   audio decoder plugins  --->
      [*]   mp3 dec
      [*]   aac dec
      [*]   ac3 dec
      [ ]   eac3 dec
      [*]   adpcm dec
      [*]   pcm dec
      [*]   flac dec
      [*]   vorbis dec
      [*]   wma dec
      [*]   opus dec
      [*]   amr dec
```

The precompiled library of hcboot only supports boot show logo:

```
  $ cd hcrtos
  $ make O=output-bl menuconfig
  > Components
  (boot) Prebuilt subdir  /* The precompiled library path representing the link is `hcrtos/components/prebuilts/boot/sysroot` */
  [*] prebuilts  --->
    [ ]   ffplayer
    [*]   audio driver
    -*-   video driver
    [ ]   audio decoder plugins
```

6.3 Local and network multimedia playback
----

The local multimedia player (ffplayer) of Haiqi hcrtos SDK is developed and designed based on the open source software ffmpeg. After using the protocol analysis, container analysis and demux functions in ffmpeg to obtain the audio/video packat, the audio and video decoding output and synchronization are realized based on the audio/video driver interface provided by AVP.

The development of multimedia playback function can be realized based on the interface of Haiqi ffplayer, or based on the underlying audio/video driver interface provided by Haiqi AVP.

Multimedia audio and video driver framework

```
+--------------------------------------------------------------------------------+
|                                                                                |
|                                                                                |
|                                                                                |
|                                                                                |
|                                                 +--------------------------+   |
|          ffplayer                               |                          |   |
|                                                 |                          |   |
|                                                 |        ffmpeg            |   |
|                                                 |                          |   |
|                                                 |                          |   |
|                                                 |                          |   |
|                                                 +--------------------------+   |
|                                                                                |
|                                                                                |
+------+---------------+----------------------------+------------------+---------+
       |               |                            |                  |
       |               |                            |                  |
       |               |                            |                  |
       |               v                            |                  v
       |     +---------------------+                |       +--------------------+
       |     |                     |                |       |                    |
       |     |                     |                |       |                    |
       |     |     audio driver    |                |       |     video driver   |
       |     |                     |                |       |                    |
       |     |                     |                |       |                    |
       |     |                     |                |       |                    |
       |     +---------+-----------+                |       +----------+---------+
       |               |                            |                  |
       |               |                            |                  |
       |               |                            |                  |
       |               |                            |                  |
       |               v                            v                  v
 +-----v---------------------------+          +----------------------------------+
 |                                 |          |                                  |
 |                                 |          |                                  |
 |   audio sink                    |          |       video sink                 |
 |                                 |          |                                  |
 |                                 |          |                                  |
 |                                 |          |                                  |
 +---------------+-----------------+          +----------------+-----------------+
                 |                                             |
                 |                                             |
                 |                                             |
                 |                                             |
                 |                                             v
  +--------------v-------------------+         +----------------------------------+
  |                                  |         |                                  |
  |                                  |         |                                  |
  |   sound driver                   |         |    display engine                |
  |                                  |         |                                  |
  |                                  |         |                                  |
  |                                  |         |                                  |
  +----------------------------------+         +----------------------------------+
```

- At present, multiple independent audio decoding outputs are supported. Finally, the audio sink module will be used for sound mixing output. Currently the audiosink module supports the resample function, so it can support multiple channels of audio with different samplerates.
  - aac, mp3, vorbis, wma: Simultaneous decoding of multiple audio formats of the same type is not supported. Different types of audio formats can be decoded at the same time, such as aac and mp3 at the same time.
  - adpcm, amr, opus, pcm, flac: to support multi-channel simultaneous decoding
- Video does not support multi-channel simultaneous decoding
- The input interface of multimedia playback, such as the file system, is completed by the protocol supported by ffmpeg.

6.3.1 ffplayer interface
----

```
$ tree hcrtos/components/prebuilts/components/prebuilts/
├── boot
│   └── sysroot
│       └── usr
│           ├── include
│           └── lib
│               ├── libauddrv.a
│               └── libviddrv.a
├── Kconfig
└── sysroot
    └── usr
        ├── include
        │   ├── ffplayer.h
        │   ├── glist.h
        │   ├── opencore-amrnb
        │   │   ├── interf_dec.h
        │   │   └── interf_enc.h
        │   └── opencore-amrwb
        │       ├── dec_if.h
        │       └── if_rom.h
        └── lib
            ├── aplugin
            │   ├── libaacdec.a
            │   ├── libac3dec.a
            │   ├── libadpcmdec.a
            │   ├── libamrdec.a
            │   ├── libeac3dec.a
            │   ├── libflacdec.a
            │   ├── libmp3dec.a
            │   ├── libopencore-amrnb.a
            │   ├── libopencore-amrwb.a
            │   ├── libopusdec.a
            │   ├── libpcmdec.a
            │   ├── libvorbisdec.a
            │   └── libwmadec.a
            ├── libauddrv.a
            ├── libffplayer.a
            └── libviddrv.a
```

The example provided by ffplayer is located in `hcrtos/components/hc-examples/source`

6.3.2 audio/video driver interface
----

The usage method and calling process of the underlying audio/video driver interface will provide example demonstration code in the future. The relevant hcuapi interface is defined as follows:

```
  $ ls hcrtos/components/kernel/source/include/uapi/hcuapi/
```

6.3.3 mplayer
----

If the hc-examples software package is selected, it will generate mplayer test command, which is based on an example program written by ffplayer to test local multimedia playback, for reference. Using the mplayer test command, you can demonstrate the local multimedia playback function.

```
  hc1600a@dbA3100# mplayer   /* Start the mplayer test program */
  hc1600a@dbA3100(mp)# play /mnt/usb/test.mkv -s 0 -t 0.1 -d 1 -b 1
  /* -s 2 means no A/V synchronization (Free Run);
     -b 1 means buffering;
     -d mode(enum IMG_DIS_MODE) means image output mode;
     -t time starts playing from a certain time point of the file, time < 1 When is the percentage of the total time, when it is greater than 1, the unit is seconds */
  hc1600a@dbA3100(mp)# scan /mnt/usb/media        /* Indicates to scan the /mnt/usb/media folder, and play the media files in this folder in a loop, and use audio as a synchronization reference */
  hc1600a@dbA3100(mp)# scan /mnt/usb/media 0        /* Indicates to scan the /mnt/usb/media folder and play the media files in this folder in a loop, 0 means not to do A/V synchronization */
  hc1600a@dbA3100(mp)# help                          /* print help */
  hc1600a@dbA3100(mp)# exit                          /* Exit mplayer test command set */
```

For other test commands inside mplayer, please refer to the help command.

6.4 Boot show-logo/show-av
----

The default precompiled underlying a/v driver used by hcboot only has mpeg2 decoder, no audio decoder. Therefore, hcboot supports logo files made from video files in mepg2 format.

The recognized logo file is in:

```
  $ ls hcrtos/board/hc16xx/logo.m2v
  board/hc16xx/logo.m2v
  $ cat hcrtos/board/hc1xxx/post-build.sh
  message "Generating logo.hc ....."
  ${TOPDIR}/build/scripts/genbootmedia -i
  ${current_dir}/logo.m2v -o${IMAGES_DIR}/fs-partition1-root/logo.hc
  message "Generating logo.hc done!"
  message "Generating romfs.bin ....."
  genromfs -f ${IMAGES_DIR}/romfs.img -d
  ${IMAGES_DIR}/fs-partition1-root/ -v
  "romfs"
  message "Generating romfs.bin done!"
```

7. About the configuration of different boards
====
7.1 DDR configuration
----

The ddr configuration file is placed in `hcrtos/board/hichip/hc16xx/ddrinit/`, select different ddr parameter files through `menuconfig` configuration, and this file will be used by `hcrtos/board/hichip/hc16xx/post-build.sh` And used to generate the final `bootloader.bin` when `make all`. The bootrom will use this configuration to initialize DDR during cold boot.

```
  $ cd hcrtos
  $ tree board/hc16xx/common/ddrinitboard/hc16xx/ddrinit/
  ├── aux_code_ddr2_128M_1066MHZ.abs
  ├── aux_code_ddr2_128M_800MHZ.abs
  ├── aux_code_ddr2_64M_800MHZ.abs
  ├── aux_code_ddr3_128M_1066MHz_176Pin.abs
  ├── aux_code_ddr3_128M_1066MHz.abs
  ├── aux_code_ddr3_128M_1333MHz.abs
  ├── aux_code_ddr3_128M_1464MHz.abs
  ├── aux_code_ddr3_128M_1560MHz.abs
  ├── aux_code_ddr3_128M_1600MHz.abs
  ├── aux_code_ddr3_128M_800MHz_176Pin.abs
  ├── aux_code_ddr3_128M_800MHz.abs
  ├── aux_code_ddr3_256M_1066MHz.abs
  ├── aux_code_ddr3_256M_1333MHz.abs
  ├── aux_code_ddr3_256M_1600MHz.abs
  ├── aux_code_ddr3_256M_1612MHz.abs
  
  $ cd hcrtos
  $ make menuconfig
  > System configuration
  (${TOPDIR}/board/hc16xx/common/ddrinit/aux_code_ddr3_128M_1333MHz.abs) DDRinit
  file
```

When debugging with GDB, you need to configure the GDB tool to set memory initialization parameters. The DDR parameter configuration file used by the GDB tool is in the following path, and different parameter files are selected according to different boards.

```
  $ cd hcrtos
  $ tree board/hc16xx/common/ddrinit/gdb/
  tree board/hc16xx/common/ddrinit/gdb/
  board/hc16xx/common/ddrinit/gdb/
  ├── gdb_hc16xx_ddr2_128M_1066MHz.abs
  ├── gdb_hc16xx_ddr2_128M_400MHz.abs
  ├── gdb_hc16xx_ddr2_128M_600MHz.abs
  ├── gdb_hc16xx_ddr2_128M_600MHz_ODToff.abs
  ├── gdb_hc16xx_ddr2_128M_800MHz.abs
  ├── gdb_hc16xx_ddr2_128M_900MHz.abs
  ├── gdb_hc16xx_ddr3_128M_1066MHz_176pin.abs
  ├── gdb_hc16xx_ddr3_128M_1066MHz.abs
  ├── gdb_hc16xx_ddr3_128M_1200MHz.abs
  ├── gdb_hc16xx_ddr3_128M_1333MHz.abs
  ├── gdb_hc16xx_ddr3_128M_1600MHz.abs
  ├── gdb_hc16xx_ddr3_128M_800MHz.abs
  ├── gdb_hc16xx_ddr3_256M_1066MHz_176pin.abs
  ├── gdb_hc16xx_ddr3_256M_1066MHz.abs
  ├── gdb_hc16xx_ddr3_256M_1200MHz.abs
  ├── gdb_hc16xx_ddr3_256M_1333MHz.abs
  ├── gdb_hc16xx_ddr3_256M_1600MHz.abs
  ├── gdb_hc16xx_ddr3_256M_800MHz.abs
  ├── gdb_hc16xx_sip_ddr3_128M_1333MHz.abs
  ├── gdb_hc16xx_sip_ddr3_128M_1600MHz.abs
  ├── gdb_hc16xx_sip_ddr3_256M_1333MHz.abs
  ├── gdb_hc16xx_sip_ddr3_256M_1600MHz.abs
```

7.2 Debug serial port configuration
----

Taking `hc16xx-db-a3100` as an example, the serial0 device description is specified in the `hcrtos/board/hichip/hc16xx/dts/hc16xx-db-a3100.dts` file, here it is stated that the serial port 3 (four serial port labels for 0/1/2/3)

```
  $ cat board/hc16xx/dts/hc16xx-db-a3100.dts
  
  uart@3 {
          pinmux = <12 4 13 4>;
          devpath = "/dev/uart3";
          strapin-ctrl-clear = <0x00000000>;
          strapin-ctrl-set = <0x00000000>;
          status = "okay";
  };
  
  stdio {
          serial0 = "/hcrtos/uart@3";
  };
```

8. Driver header file
====

The drivers of the hcrtos SDK are placed in `hcrtos/components/kernel/source/include/uapi/hcuapi`. The header files of the drivers can be used for application reference development.

```
  .
  ├── amprpc.h
  ├── auddec.h
  ├── audsink.h
  ├── avevent.h
  ├── avsync.h
  ├── codec_id.h
  ├── common.h
  ├── dis.h
  ├── dsc.h
  ├── dumpstack.h
  ├── dvpdevice.h
  ├── efuse.h
  ├── fb.h
  ├── ge.h
  ├── gpio.h
  ├── hdmi_rx.h
  ├── hdmi_tx.h
  ├── i2c-master.h
  ├── i2c-slave.h
  ├── input-event-codes.h
  ├── input.h
  ├── iocbase.h
  ├── kshm.h
  ├── kumsgq.h
  ├── lvds.h
  ├── mipi.h
  ├── mmz.h
  ├── notifier.h
  ├── persistentmem.h
  ├── pinmux
  │   ├── hc15xx_pinmux.h
  │   └── hc16xx_pinmux.h
  ├── pinmux.h
  ├── pinpad.h
  ├── pixfmt.h
  ├── pq.h
  ├── pwm.h
  ├── ramdisk.h
  ├── rc-proto.h
  ├── sci.h
  ├── snd.h
  ├── spidev.h
  ├── standby.h
  ├── sys-blocking-notify.h
  ├── sysdata.h
  ├── tvdec.h
  ├── tvtype.h
  ├── viddec.h
  ├── vidmp.h
  ├── vidsink.h
  ├── vindvp.h
  ├── virtuart.h
  └── watchdog.h
```

9. GDB debugging tool
====

Download address: https://gitlab.hichiptech.com:62443/sw/tools.git

Among them: \tools\GDB\HiChipGDB-H15\HiChipGDB@H1512 is the gdb used for H15xx series chips, and A210/A110/B200/B100 are all H15xx series.
\tools\GDB\HiChipGDB-H16\HiChipGDB-0.1.0 is the gdb used for H16xx series chips, among which A3100/B3100/C3100/D5000 are all H16xx series.
\tools\GDB\Driver is the driver of the jtag debugging tool. After installation, it is recommended to restart the computer to use.

Reference document: hichipgdb_15xx_userguide.pdf & LINUX_HiChipGDB user manual.pdf (H16 series chips)

10. About 4K decoding and output
====

Currently, 4K decoding requires 256MB of memory, and the default DDR parameter configured in configs/hichip_hc16xx_db_a3100_xxx_4k_defconfig is a 256MB development board.

Currently, the default output for all configurations is 2k output. Regardless of 128MB or 256MB, it can support 2k output or 4k output. After the system is running, there are several ways to switch the output format

After entering the command line, enter the command directly

```
  4k output:
  hc1600a@dbA3100#dis
  hc1600a@dbA3100(dis)# tvsys -c 0 -l 1 -d 1 -t 24 -p 1
```

If you want to switch back to 2k format output, enter the command

```
  hc1600a@dbA3100(dis)# tvsys -c 0 -l 1 -d 1 -t 1 -p 1
```

Or by calling the driver API:

Call the interface of hcuapi in the application program developed by yourself, and set the output of different formats. The interface of hcuapi is defined in

```
  hcrtos/components/kernel/source/include/uapi/hcuapi/dis.h
```

Example: the command of the above method 1 is the implementation of the following example.

```
  static int tvsys_test(int argc, char *argv[])
  {
    struct dis_tvsys tvsys = { 0 };
    int fd = -1;
    long cmd = -1;
    int opt;
    
    opterr = 0;
    optind = 0;
    
    while ((opt = getopt(argc, argv, "c:l:d:t:p:")) != EOF) {
      switch (opt) {
        case 'c':
          cmd = atoi(optarg);
          break;
        case 'l':
          tvsys.layer = atoi(optarg);
          break;
        case 't':
          tvsys.tvtype = atoi(optarg);
          break;
        case 'd':
          tvsys.distype = atoi(optarg);
          break;
        case 'p':
          tvsys.progressive = atoi(optarg);
          break;
        default:
          break;
      }
    }
    
    fd = open("/dev/dis", O_WRONLY);
    if (fd < 0) {
      return -1;
    }
    
    if (tvsys.layer != DIS_LAYER_MAIN && tvsys.layer != DIS_LAYER_AUXP) {
      printf("error layer %d\n", tvsys.layer);
      return -1;
    }
    
    if (tvsys.distype != DIS_TYPE_SD && tvsys.distype != DIS_TYPE_HD &&    tvsys.distype != DIS_TYPE_UHD) {
      printf("error display type %d\n", tvsys.distype);
      return -1;
    }
    
    switch (cmd) {
      case 0:
        cmd = DIS_SET_TVSYS;
        break;
      case 1:
        cmd = DIS_SET_ZOOM;
        break;
      default:
        break;
    }
    
    ioctl(fd, cmd, &tvsys);
    
    close(fd);
    
    return 0;
  }
```

11. LCD display configuration instructions
====
11.1 Screen verification configuration summary
----

```
  +----------------------------------------------+------------------------------+----------------------------------------+------------------------------+
  | CONFIGURATION                                | RGB                          | LVDS                                   | MIPI                         |
  +----------------------------------------------+------------------------------+----------------------------------------+------------------------------+
  | hichip_hc16xx_db_b300_v10_hcdemo_defconfig   | *                            | 1024×600(VESA)/1920×1080(VESA Default) | *                            |
  | hichip_hc16xx_db_b3100_v20_hcdemo_defconfig  | 800×480(RGB565 Default)      | 1024×600(VESA)/1920×1080(VESA)         | *                            |
  | hichip_hc16xx_db_b3120_v20_hcdemo_defconfig  | 800×480(RGB565 Default)      | 1024×600(VESA)/1920×1080(VESA)         | *                            |
  | hichip_hc16xx_db_c3000_v10_hcdemo_defconfig  | *                            | 1024×600(VESA Default)/1920×1080(VESA) | *                            |
  | hichip_hc16xx_db_c300_v10_hcdemo_defconfig   |                              | 1024×600(VESA)/1920×1080(VESA)         | 1080×1920(Default)/1920×1200 |
  | hichip_hc16xx_db_c300_v20_hcdemo_defconfig   | 800×480(RGB666 Default)      | *                                      | 1920×1200                    |
  | hichip_hc16xx_db_c3100_v20_hcdemo_defconfig  | *                            | *                                      | 1920×1200(Default)           |
  | hichip_hc16xx_db_c5200_v10_hcdemo_defconfig  | *                            | 1024×600(VESA Default)/1920×1080(VESA) | *                            |
  | hichip_hc16xx_db_d3000_v10_hcdemo_defconfig  | *                            | 1024×600(VESA Default)/1920×1080(VESA) | *                            |
  | hichip_hc16xx_db_d3100_v10_hcdemo_defconfig  | 800×480(RGB565 Default)      | 1024×600(VESA)/1920×1080(VESA)         | *                            |
  | hichip_hc16xx_db_d3100_v20_hcdemo_defconfig  | *                            | 1024×600(VESA Default)                 | *                            |
  | hichip_hc16xx_db_d5200_v11_hcdemo_defconfig  | 800×480(RGB888 VESA Default) | 1024×600/1920×1080(VESA)               | 1920×1200                    |
  +----------------------------------------------+------------------------------+----------------------------------------+------------------------------+
```

11.2 Quick Configuration Instructions for LCD Display
----

```
  1. Introduction
  In order to facilitate and quickly switch the lcd display, all the lcd displays that have been lit are currently placed
  In the directory of hcrtos\board\hc16xx\common\dts\lcd\, you need to configure lcd for reference here;
  
  2. Modify the LCD display under the default configuration
  For example, board\hc16xx\common\dts\hc16xx-db-d3100-v10.dts is configured with an 800×480 RGB display by default. If you need to change it to a 1024×600 LVDS display, you can make the following modifications;
  
  // #include "lcd_rgb_800_480_rgb565.dtsi" /* Comment out the default configuration */
  #include "lcd_lvds_1024_600_vesa.dtsi" /* Add new LCD display configuration */
  
  3. Compile
  cd hcrtos
  make O=bl kernel-rebuild all
  make kernel-rebuild all
  
  Note: Only versions after November 2022 are supported
```

11.3 Configuration instructions of RGB and LVDS
----

- Step 1: Configure the demo board

To configure RGB compilation, please refer to the configuration

```
  cd hcrtos
  rm -rf output*
  rm -rf bl
  make O=bl hichip_hc16xx_db_d5200_v11_hcdemo_bl_defconfig
  make O=bl all
  make hichip_hc16xx_db_d5200_v11_hcdemo_defconfig
  make all
```

To configure lvds, please refer to the configuration

```
  cd hcrtos
  rm -rf output*
  rm -rf bl
  make O=bl hichip_hc16xx_db_d3100_v20_hcdemo_bl_defconfig
  make O=bl all
  make hichip_hc16xx_db_d3100_v20_hcdemo_defconfig
  make all
```

- Step 2: Check whether the menuconfig has opened the lvds driver (open by default)

```
Both lvds and rgb are configured to open CONFIG_DRV_LVDS
1. Check whether the bootloader is configured with lvds
make O=bl menuconfig
cd hcrtos
x There is no help available for this option.
  x Symbol: CONFIG_DRV_LVDS [=y]
  x Type: bool
  x Prompt: lvds
  x   Location:
  x     -> Components
  x -> kernel (BR2_PACKAGE_KERNEL [=y])
  x -> Drivers
  x   Defined at drivers: 165
  x   Depends on: BR2_PACKAGE_KERNEL [=y]

2. Check whether the kernel is configured with lvds
make menuconfig
x There is no help available for this option.
  x Symbol: CONFIG_DRV_LVDS [=y]
  x Type: bool
  x Prompt: lvds
  x   Location:
  x     -> Components
  x -> kernel (BR2_PACKAGE_KERNEL [=y])
  x -> Drivers
  x   Defined at drivers: 165
  x   Depends on: BR2_PACKAGE_KERNEL [=y]
```

- Step 3: Configure DE

```
  For example board\hc16xx\common\dts\lcd\lcd_rgb_800_480_rgb888.dtsi
  
  &DE {
    tvtype = <15>;//1. The general configuration is 15
    VPInitInfo {
      rgb-cfg {
        b-rgb-enable = <1>;//2, enable RGB is set to 1
        /*
        * 0: MODE_PRGB
          * 1: MODE_SRGB
          */
        rgb-mode = <0>;//3, set to parallel mode
        /*
          * 0: MODE_PRGB_888_10bit
          * 1: MODE_PRGB_666
          */
        prgb-mode = <0>; //4, set to RGB888 mode
        lcd-width = <800>;//5, lcd-width and h-active-len need to be consistent
        b-disable-ejtag = <1>;
        b-dlpc-enale = <0>;
        timing-para {
          /* bool type, 0 or 1*/
          b-enable = <1>;//6, enable, the following parameters can only be configured after enabling
          /*
            * VPO_RGB_CLOCK_27M = 1,
            * VPO_RGB_CLOCK_33M = 2,
            * VPO_RGB_CLOCK_49M = 3,
            * VPO_RGB_CLOCK_66M = 4,
            * VPO_RGB_CLOCK_74M = 5,
            * VPO_RGB_CLOCK_85M = 6,
            * VPO_RGB_CLOCK_108M = 7,
            * VPO_RGB_CLOCK_6_6M = 8,
            * VPO_RGB_CLOCK_9M = 9,
            * VPO_RGB_CLOCK_39_6M = 10,
            * VPO_RGB_CLOCK_74_25M = 11, //H1600
            * VPO_RGB_CLOCK_148_5M = 12, //H1600
          */
          output-clock = <1>;//7, output-clock = h-total-len * v-total-len* frequency (50~60H)
          
          8. Fill in the relevant screen parameters according to the parameters provided by the screen factory
          h-total-len = <1026>;//h-total-len = h-active-len + h-front-len+ h-sync-len+h-back-len
          v-total-len = <525>;//v-total-len = v-active-len + v-front-len +v-sync-len+v-back-len
          h-active-len = <800>;
          v-active-len = <480>;
          h-front-len = <210>;
          h-sync-len = <6>;
          h-back-len = <10>;
          v-front-len = <22>;
          v-sync-len = <3>;
          v-back-len = <20>;
          /* bool type, 0 or 1*/
          h-sync-level = <0>;
          /* bool type, 0 or 1*/
          v-sync-level = <0>;
          frame-rate = <500000>;//9, set the frame rate to 50HZ
        };
      };
    };
  };
```

- Step 4: Set the mode to RGB&LVDS
- 4.1 Configure RGB

```
  For example board\hc16xx\common\dts\lcd\lcd_rgb_800_480_rgb888.dtsi //configure rgb565 linux node
  
  &lvds {
    reg = <0xB8860000 0x200>, <0xB8800000 0x500>;
    /*
     * "lvds": LVDS screen
     * "rgb888" : RGB screen
     * "rgb666" : RGB screen
     * "rgb565" : RGB screen
     * "i2so": I2S OUT
     * "gpio": only GPIO out
     */
    lvds_ch0-type = "rgb888";
    lvds_ch1-type = "rgb888";
    / * rgb ggb bgb
      * rrb rgb rbb
     * rgr rgg rgb
     */
    rgb_src_sel = "rgb";
    lcd-backlight-gpios-rtos = <PINPAD_L15 GPIO_ACTIVE_HIGH>;//backlight
    /* 0: E_SRC_SEL_FXDE = 0
     * 1: E_SRC_SEL_4KDE
     * 2: E_SRC_SEL_HDMI_RX
     */
    src-sel = <CONFIG_DE4K_OUTPUT_SUPPORT>;
    pinmux-rgb888 = <PINPAD_B00 1 PINPAD_B01 1 PINPAD_B02 1 PINPAD_B03 1 PINPAD_B04 1PINPAD_B05 1 PINPAD_B06 1 PINPAD_B07 1>;
    /* RGB888 other pin set */
    pinmux-rgb666 = <PINPAD_B04 4 PINPAD_B07 4>; 
    /* RGB666 other pin set */
    status = "okay";
  };
```

- 4.2 configure lvds

```
  1. Enable lvds
  board\hc16xx\common\dts\hc16xx-db-d3100-v20.dts
  lvds: lvds@0xb8860000 {
    status = "okay";
  };
  
  2. Configure the content of the lvds node
  board\hc16xx\common\dts\lcd\lcd_lvds_1024_600_vesa.dtsi //Configure the node of lvds vesa
  &lvds {
    /*
    * "lvds": LVDS screen
    * "rgb888" : RGB screen
    * "rgb666" : RGB screen
    * "rgb565" : RGB screen
    * "i2so": I2S OUT
    * "gpio": only GPIO out
    */
    lvds_ch0-type = "lvds";//1, set the output channel LVDS D0~D3
    lvds_ch1-type = "lvds";//LVDS D4~D7
    
    /*
    * 0: E_CHANNEL_MODE_SINGLE_IN_SINGLE_OUT ,
    * 1: E_CHANNEL_MODE_SINGLE_IN_DUAL_OUT
    */
    channel-mode = <0>;//2, output
    /*
    * 0: E_MAP_MODE_VESA_24BIT ,
    * 1: E_MAP_MODE_VESA_18BIT_OR_JEDIA
    */
    map-mode = <0>;//3. Set the signal format of the screen 0: VESA 1: JEDIA
    ch0-src_sel = <0>;
    ch1-src_sel = <0>;
    ch0-invert_clk_sel = <0>;
    ch1-invert_clk_sel = <0>;
    ch0-clk_gate = <0>;
    ch1-clk_gate = <0>;
    hsync-polarity = <1>;//Consistent with de-engine's h-sync-level
    vsync-polarity = <1>;//Consistent with the v-sync-level of de-engine
    /*
     * 0: E_ADJUST_MODE_FRAME_START
     * 1: E_ADJUST_MODE_HSYNC_POS
     * 2:E_ADJUST_MODE_VSYNC_POS
     */
    even-odd-adjust-mode = <0>;
    even-odd-init-value = <0>;
    /*
     * 0: E_SRC_SEL_FXDE = 0
     * 1: E_SRC_SEL_4KDE
     * 2: E_SRC_SEL_HDMI_RX
     */
    src-sel = <CONFIG_DE4K_OUTPUT_SUPPORT>;
    status = "okay";
  };
```

- Step 5: Compile
- 5.1 Recompile

```
  $cd hcrtos
  $make O=bl kernel-rebuild all; make kernel-rebuild all
```

Step 6: Precautions:
6.1 dts note

Suppose `board\hc16xx\common\dts\hc16xx-db-d5200-v11.dts` is configured with mipi: dsi0, please set status="disable" and recompile:

```
  $cd hcrtos
  $make O=bl kernel-rebuild all; make kernel-rebuild all
```

6.2 Description of lvds channel

Correct configuration, channel D0~D3 can be configured as "lvds", "i2so", "gpio", D4~D7 can be configured as "lvds", "i2so", "gpio" For example, the correct configuration:

```
  lvds_ch0-type = "lvds"
  lvds_ch1-type = "gpio"

  lvds_ch0-type = "i2so"
  lvds_ch1-type = "lvds"

  lvds_ch0-type = "i2so"
  lvds_ch1-type = "i2so"
```

wrong configuration:
The simultaneous combination of rgb565 rgb666 rgb888 gpio i2so is not possible, for example

```
  lvds_ch0-type = "rgb565";//rgb666 rgb888
  lvds_ch1-type = "gpio";
  
  lvds_ch0-type = "rgb565";//rgb666 rgb888
  lvds_ch1-type = "i2so";
  
  lvds_ch0-type = "gpio";
  lvds_ch1-type = "i2so";
```

11.4 MIPI configuration
====

- Step 1: Board level reference configuration
MIPI related configuration can only be configured in hcrtos

```
  cd hcrtos
  rm -rf output*
  make O=bl hichip_hc16xx_db_c300_v10_hcdemo_bl_defconfig
  make all
  cd hcrtos
  make hichip_hc16xx_db_c300_v10_hcdemo_defconfig
  make all
```

- Step 2: Check whether mipi is configured in menuconfig

```
  cd hcrtos
  1. Check whether the bootloader is configured with mipi
  make O=bl menuconfig
  x There is no help available for this option.
    x Symbol: CONFIG_DRV_MIPI [=y]
    x Type  : bool
    x Prompt: mipi
    x   Location:
    x     -> Components
    x       -> kernel (BR2_PACKAGE_KERNEL [=y])
    x         -> Drivers
    x   Defined at drivers:177
    x   Depends on: BR2_PACKAGE_KERNEL [=y]
  2. Check whether the kernel is configured with mipi
  make menuconfig
  x There is no help available for this option.
    x Symbol: CONFIG_DRV_MIPI [=y]
    x Type  : bool
    x Prompt: mipi
    x   Location:
    x     -> Components
    x       -> kernel (BR2_PACKAGE_KERNEL [=y])
    x         -> Drivers
    x   Defined at drivers:177
    x   Depends on: BR2_PACKAGE_KERNEL [=y]
```

- Step 3: Configure DE

```
board\hc16xx\common\dts\lcd\lcd_mipi_ILI7807D_1080_1920_rgb888.dtsi

&DE {
  tvtype = <15>;//1. Generally 15
  VPInitInfo {
    rgb-cfg{
      b-rgb-enable = <1>;//2, enable RGB is set to 1
      /*
       * 0: MODE_PRGB
       * 1: MODE_SRGB
       */
      rgb-mode = <0>;//3、设置为并行模式
      /*
       * 0: MODE_PRGB_888_10bit
       * 1: MODE_PRGB_666
       */
      prgb-mode = <0>;//4、设置为RGB888的模式
      lcd-width = <1080>;//5、lcd-width与h-active-len需要一致
      screen_timing: timing-para {
        /* bool type, 0 or 1*/
        b-enable = <1>;//6、使能为，使能之后才能配置以下的参数
        /*
         * VPO_RGB_CLOCK_27M = 1,
         * VPO_RGB_CLOCK_33M = 2,* VPO_RGB_CLOCK_49M = 3,* VPO_RGB_CLOCK_66M = 4,* VPO_RGB_CLOCK_74M = 5,* VPO_RGB_CLOCK_85M = 6,* VPO_RGB_CLOCK_108M = 7,* VPO_RGB_CLOCK_6_6M = 8,* VPO_RGB_CLOCK_9M = 9,* VPO_RGB_CLOCK_39_6M = 10,* VPO_RGB_CLOCK_74_25M = 11,//H1600* VPO_RGB_CLOCK_148_5M = 12,//H1600*/output-clock = <12>;//7、output-clock = h-total-len * v-total-len* 频率 (50~60H）8、按照屏厂提供参数填写相关屏参h-total-len = <1315>;v-total-len = <1980>;h-active-len = <1080>;v-active-len = <1920>;h-front-len = <200>;h-sync-len = <5>;h-back-len = <30>;v-front-len = <30>;v-sync-len = <10>;v-back-len = <20>;/* bool type, 0 or 1*/h-sync-level = <1>;/* bool type, 0 or 1*/v-sync-level = <1>;frame-rate = <600000>;//9、设置帧率 60HZ};};};};

```
