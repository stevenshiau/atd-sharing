#   Image Rockpro64 SD with Clonezilla

##  Clonezilla live testing release for ARM64 arch.

The [ROCKPro64][] is a powerful Single Board Computer released by Pine64. It is powered by a Rockchip RK3399 Hexa-Core (dual ARM Cortex A72 and quad ARM Cortex A53) 64-Bit Processor with a MALI T-860 Quad-Core GPU.

[Clonezilla][] is free and open source software for disk imaging and cloning.

## How to use Clonezilla to backup and restore the OS on the SD card of Rockpro64:

 1. First, follow [this](https://github.com/sigmaris/u-boot/wiki/Flashing-U-Boot-to-SPI) to flash U-Boot to SPI:
    
    We have tested the release rockpro64 u-boot v2020.01-ci work for booting [Clonezilla live][ClonezillaLive] from USB flash drive.

 2. Download [Clonezilla live][ClonezillaLive]

    The Ubuntu-based release, e.g., `clonezilla-live-20200414-focal-arm64.zip` which we have tested OK on Rockpro64.

 3. Create a FAT32 partition on your USB flash drive, then mount it, e.g.,

        pmount /dev/sde1 /media/disk
        unzip clonezilla-live-20200414-focal-arm64.zip -d /media/disk
        pumount /media/disk

 4. Insert the USB flash drive on your Rockpro64, then boot it. Remember to set the it to boot from USB device.

    i.e., when you see:

        Hit any key to stop autoboot:  0

    Press any key, then type:

        setenv boot_targets usb0
        boot

    If all goes well, you should be able to see the [Clonezilla live][ClonezillaLive] boot menu.

 5. In the 1^st^ item of grub boot menu, edit the boot parameter by pressing `e`, append `ocs_live_run_tty=/dev/ttyS2`, so that the Clonezilla main menu will be shown in the serial console.

    e.g.,

        linux /live/vmlinuz boot=live union=overlay quiet \
                components noswap edd=on nomodeset enforcing=0 \
                noeject locales=en_US.UTF-8 keyboard-layouts=en \
                ocs_live_run="ocs-live-general" ocs_live_extra_param="" \
                ocs_live_batch="no" vga=788 ip= net.ifnames=0 \
                splash i915.blacklist=yes radeonhd.blacklist=yes \
                nouveau.blacklist=yes vmwgfx.enable_fbdev=1 \
                ocs_live_run_tty=/dev/ttyS2

    Of course, the serial console device name in `ocs_live_run_tty` has to match your environment. Then press `Ctrl-X` to boot it.

 6. Wait a while, and you should be able to see the main menu of Clonezilla live, then you can save your disk as an image, restore the image, or clone the disks. More docs are available on Clonezilla website: https://clonezilla.org/clonezilla-live-doc.php

---

[ROCKPro64]: https://wiki.pine64.org/index.php/ROCKPro64
[Clonezilla]: https://clonezilla.org
[ClonezillaLive]: http://free.nchc.org.tw/clonezilla-live/experimental/arm/

-   The photo about the test environment:
    ![](https://github.com/stevenshiau/atd-sharing/raw/master/20200421-Rockpro64-Clonezilla/0-test-env.jpg)

-   The rate about saving a SSD disk using `-z9p` (zstd) of Clonezilla option:
    ![](https://github.com/stevenshiau/atd-sharing/raw/master/20200421-Rockpro64-Clonezilla/2-rockpro-save-ssd.png)

-   The rate about restoring an image to SSD:
    ![](https://github.com/stevenshiau/atd-sharing/raw/master/20200421-Rockpro64-Clonezilla/3-rockpro-restore-ssd.png)

-   The plain text file for boot progress:
    https://github.com/stevenshiau/atd-sharing/blob/master/20200421-Rockpro64-Clonezilla/0-boot-process.txt

-   The plain text file for the progress about saving the micro SD which has Armbian 20.02.1 Buster:
    https://github.com/stevenshiau/atd-sharing/blob/master/20200421-Rockpro64-Clonezilla/4-save-sd.txt

-   The plain text file for tThe progress about wiping the micro SD:
    https://github.com/stevenshiau/atd-sharing/blob/master/20200421-Rockpro64-Clonezilla/5-wipe-sd.txt

-   The plain text file for the progress about restoring the image to micro SD, then boot the restored Armbian:
    https://github.com/stevenshiau/atd-sharing/blob/master/20200421-Rockpro64-Clonezilla/6-restore-sd.txt

Bugs report, comments and suggestions are welcome.
Thank you very much.
