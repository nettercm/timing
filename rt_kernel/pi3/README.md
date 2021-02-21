# Real-Time Linux kernel binaries for the Raspberry Pi 3 Model B V1.2

Instructions:

1. login as root
2. preverably do apt update and apt upgrade
3. make a backup of /boot/kernel7.img ( e.g. cp /boot/kernel7.img /boot/kernel7.bak.img
4. copy the kernel7.img from this repository in to /boot
5. extract the .tar.gz files in /lib/modules  (result is /lib/modules/5.10.13-rt26-v7+)
6. add the following to /boot/cmdline.txt:  " dwc_otg.fiq_fsm_enable=0 dwc_otg.fiq_enable=0 dwc_otg.nak_holdoff=0 "
7. add the following to /boot/cmdline.txt:  " isolcpus=3 "
8. reboot

After the reboot, "uname -a" should report the following:
Linux raspberrypi 5.10.13-rt26-v7+ #4 SMP PREEMPT_RT Sat Feb 13 19:53:52 STD 2021 armv7l GNU/Linux

Also, the "lsmod" command should report lots of modules loaded which confirms that modules/drivers were installed correctly under /lib/modules

What to do if it doesn't boot / something doesn't work right:

Boot w/ HDMI connected and preverably w/ serial console enabled and check the kernel boot output if it fails in some obvious fashion.
If it seems to boot (as evident via HDMI and/or serial console) but ssh is not possible, the modules/drivers may not have been installed correctly

How to uninstall:

To undo the changes even if it doesn't boot: remove the uSD card, insert into another device, copy kernel7.bak.img to kernel7.img, modify cmdline.txt, sync/eject safely, put uSD back into Pi3, etc. etc.  If it DOES boot but you want to undo the changes anyway, simply manipulate the files in /boot to undo the changes while the Pi3 is running

Important notes:

Subsequent "apt upgrade" operations will likely overwrite the custom kernel7.img file (but not the new /lib/modules folder), so you may have to re-install the custom kernel7.img file after an "apt upgrade" operation.  

Step 6 above tells the USB driver to operate in a manner that is compatible with the real-time linux patches.  This results in increased overhead and thus CPU usage by the USB driver.  Unless you are pusshing all cores of the Pi3 to the limit this should not be an issue. 
