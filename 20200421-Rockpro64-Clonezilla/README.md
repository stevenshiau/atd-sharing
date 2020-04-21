---
# Image Rockpro64 SD with Clonezilla
---
## Clonezilla live testing release for ARM64 arch.

- The [ROCKPro64][1] is a powerful Single Board Computer released by Pine64. It is powered by a Rockchip RK3399 Hexa-Core (dual ARM Cortex A72 and quad ARM Cortex A53) 64-Bit Processor with a MALI T-860 Quad-Core GPU.

- [Clonezilla][2] is free and open source software for disk imaging and cloning.

### How to use Clonezilla to backup and restore the OS on the SD card of Rockpro64:
1. First, follow this to flash U-Boot to SPI:
   - https://github.com/sigmaris/u-boot/wiki/Flashing-U-Boot-to-SPI
   - We have tested the release rockpro64 u-boot v2020.01-ci work for booting Clonezilla live from USB flash drive.

2. Download Clonezilla live from:
   http://free.nchc.org.tw/clonezilla-live/experimental/arm/
   - The Ubuntu-based release, e.g., clonezilla-live-20200414-focal-arm64.zip which we have tested OK on Rockpro64.

3. Create a FAT32 partition on your USB flash drive, then mount it, e.g.,
   - pmount /dev/sde1 /media/disk
   - unzip clonezilla-live-20200414-focal-arm64.zip -d /media/disk
   - pumount /media/disk

4. Insert the USB flash drive on your Rockpro64, then boot it. Remember to set the it to boot from USB device.
   i.e., when you see:
   ```
   Hit any key to stop autoboot:  0
   ```
   - Press any key, then type:
   ```
   => setenv boot_targets usb0
   => boot
   ```
   - If all goes well, you should be able to see the Clonezilla live boot menu.

5. In the 1st item of grub boot menu, edit the boot parameter by pressing "e",
   append "ocs_live_run_tty=/dev/ttyS2", so that the Clonezilla main menu will be shown in the serial console.
   e.g.,

   `linux /live/vmlinuz boot=live union=overlay quiet components noswap edd=on nomodeset enforcing=0 noeject locales=en_US.UTF-8 keyboard-layouts=en ocs_live_run="ocs-live-general" ocs_live_extra_param="" ocs_live_batch="no" vga=788 ip= net.ifnames=0  splash i915.blacklist=yes radeonhd.blacklist=yes nouveau.blacklist=yes vmwgfx.enable_fbdev=1 ocs_live_run_tty=/dev/ttyS2`
   - Of course, the serial console device name in ocs_live_run_tty has to match your environment. Then press Ctrl-x to boot it.

6. Wait a while, and you should be able to see the main menu of Clonezilla live, then you can save your disk as an image, restore the image, or clone the disks. More docs are available on Clonezilla website:
   - https://clonezilla.org/clonezilla-live-doc.php

The photo about the test environment:
![](0-test-env.jpg)

The rate about saving a SSD disk using -z9p (zstd) of Clonezilla option:
![](2-rockpro-save-ssd.png)

Rate about restoring an image to SSD:
![](3-rockpro-restore-ssd.png)

Boot progress:
![download plain text file](0-boot-process.txt)

The progress about saving the micro SD which has Armbian 20.02.1 Buster:
![download plain text file](4-save-sd.txt)

The progress about wiping the micro SD:
![download plain text file](5-wipe-sd.txt)

The progress about restoring the image to micro SD, then boot the restored Armbian:
![download plain text file](6-restore-sd.txt)

Bugs report, comments and suggestions are welcome.
Thank you very much.

[1]: https://wiki.pine64.org/index.php/ROCKPro64
[2]: https://clonezilla.org