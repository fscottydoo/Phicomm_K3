# Phicomm_K3
Phicomm K3 (not K3c) Data Dump\
Google Drive Link
[Phicomm K3](https://drive.google.com/file/d/1Ne0cfeJaO40-oQRr71ObIBD7xsGqn1Fn)


https://www.snbforums.com/threads/phicomm-k3-not-k3c-discussion.49109/
The link above leads to the original information I used to flash OpenWRT onto my Phicomm K3 router.
Extreme thanks to https://www.snbforums.com/members/jyxent.31686/

---------------------------------------------------------------------------------------------------------------------------
2019-06-23 jyxent:

I got this recently when it was on sale, and have successfully flashed OpenWrt on it. The process is a little involved, but I'm satisfied with the results.

There are two pages that I found with the needed instructions (They're in Chinese. I just used Google Translate.).

https://tbvv.net/posts/0101-k3.html
https://www.right.com.cn/forum/thread-262218-1-1.html

The first link has a file called us.dat, which you can restore settings from. It will change the password to tbvv.net and then you can go set a user name and password in the usb storage menu. After you do that, you can telnet into the router (Possibly ssh too, through I didn't try it). It may have been unnecessary, but I ended up finding the K3_V21.6.8.46_tb.bin image and flashing it, which does have ssh.

With ssh access, you can copy /dev/mtdblock0 to a file. This is the CFE partition. The tool in the second link will downgrade the CFE, which will allow you to run the flash command which doesn't work in the up to date version. I downgraded to 212, which produced a file called new212.bin. After overwriting /dev/mtdblock0 with that file, you can reboot into the downgraded CFE (hold reset button down while turning on). With static networking setup (I used 192.168.2.2 for an IP), you can access the CFE in a browser at 192.168.2.1.

To flash the image, you must have a tftp server running on your machine. You must have the phicomm k3 image in the shared directory (openwrt has snapshot images for it).

If you used 192.168.2.2 as your static ip, the following link will run a command to flash the image.

http://192.168.2.1/do.htm?cmd=flash...rt-bcm53xx-phicomm-k3-squashfs.trx+flash0.trx

I performed a nvram erase after the command finished and rebooted into openwrt.

For some reason, the firmware for the 4366 wireless modules that is included in the openwrt image performs terribly (it's the firmware from the linux-firmware repo). I got very bad signal strength and constant disconnects, even for devices in the same room. I replaced /lib/firmware/brcm/brcmfmac4366c-pcie.bin with the ac88u firmware from here:

https://github.com/Hill-98/phicommk3-firmware

With that change, it is working well now.

--------------------------------------------------------------------------------------------------------------------------
