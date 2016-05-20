# Raspberry Pi Development Tools
This repository contains files to setup a common development environment for Raspberry Pi.

For now, contains pre-built U-Boot bootloader and boot script to TFTP boot from 192.168.4.1.
Here is the recipe to network boot the Raspberry Pi with a FreeRTOS kernel image.  You won’t
need a console or keyboard attached to the Pi.  U-Boot boot loader for Pi has all the bells
and whistles to do this.

along with a custom script.  Stick SD card in Pi and leave it.

## Steps
0. Assumes starting with a SD card with Raspbian already on it
1. Copy u-boot2.bin and boot.scr.uimg from dir prebuilt to SD card
2. Edit config.txt and add line `kernel=u-boot2.bin`
3. Stick SD card in Pi and leave it
4. Follow steps elsewhere to add a TFTP server on your Ubuntu build machine.
   Your build machine ethernet needs a static IP address of 192.168.4.1
5. Connect an ethernet cable between you build machine and the Pi

Edit, Compile, Test Cycle is now:
- Make FreeRTOS and copy resulting kernel.img to /tftpboot
- Power on Pi.  U-Boot starts, reads custom script which copies kernel.img
  from tftp://192.168.4.1 and jumps to address 0x8000.

That’s it.  To compile and run again, my command line looks like this now:

$ make && cp kernel.img /tftpboot

Then cycle Pi power, and the new kernel.img is running in 10 sec.

P.S. Even though you don’t need a monitor or keyboard connected to Pi, you can hook these up and interact with U-Boot.  It has a command line interface.

