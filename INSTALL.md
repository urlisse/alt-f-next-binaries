How to install:
===============

To use Alt-F you have to flash it using the same method used to upgrade the vendor’s firmware.
This method replaces the D-Link firmware and probably will void the box warranty.

1. Reboot the box before start performing any upgrade.

2. Start uninstalling ffp if you have it installed. That's enough to rename the 'ffp' folder to 'ffp-orig'.
 You can latter install ffp using Alt-F "ffp Package Manager" and recover configuration
 files from the previous 'ffp-orig' install.
 When Alt-F starts, if a ffp directory is found in the root directory of any filesystem,
 its init script will be executed, so all non needed ffp init scripts should be disable,
 and only services that Alt-F don't have should be kept.

3. Look at the box bottom attached label to see its hardware revision level, then select the appropriate
 Alt-F-0.1RC5 bin file for it and use the normal firmware upgrade method to flash it.
 For example, the Alt-F-0.1RC5-DNS-320-rev-Ax.bin file is to be used on the DNS-320 rev-A1 or rev-A2,
 but not on the DNS-320-rev-B1.
 Don't try to flash firmware if your box is not supported
 
 Alt-F firmware upgrade web page can flash the vendor's supplied firmware,
 so you can always do a "downgrade".

  Alt-F and several D-Link firmware versions have been tested on a DNS-323-REV-A1/B1,
  a DNS-320L-rev-A1, a DNS-325-rev-A1, and a DNS-327L-rev-A1 boxes.

  Reports say that it also work on other box model and boards: the
  DNS-325-rev-A2, DNS-320L-rev-A3, DNS-320-rev-A1/A2/B1/B2, DNS-321-rev-A1/A2/A3,
  DNS-323-rev-C1, and on the Conceptronic CH3SNAS and Fujitsu-Siemens DUO 35-LR
  which are just rebranded DNS-323-rev-B1.

  OF COURSE THIS IS DANGEROUS, AND CAN TURN YOUR BOX INTO AN EXPENSIVE BLACK
  BRICK IN YOUR DESKTOP. YOU HAVE BEEN WARNED.

4. After the reboot clear the cache of the browser, as this often causes problems.
 Also, try using Chrome or Firefox, as IE might also cause problems (and was not tested at all).

Diagnosing Installation Problems
================================

0-The blue power led keeps blinking with a slow *heart beat* rate,
  the blue disk leds blinks randomly.
  This means that fsck is running and checking your disk filesystems.
  This happens when the box is not cleanly shutdown, what happens
  with the vendors firmware.
  During the disk checking your data is not available, and you have to wait until
  the disk checking finishes, at what time the blue power leds stops blinking
  with the heartbeat pace and stays on steadily.
  It can take a very long time for fsck to finish, depending on the disks
  capacity and usage. As an example, checking a 88% full 230GB disk
  takes about 12 minutes.
  On power-off or reboot, Alt-F will properly unmount the disks, so the check/fix
  step will not happens at the second and subsequent reboots.

1-If the power led is not blinking with the heartbeat pace and you can't access
  Alt-F web home page, before pulling the power cord you can try to see if Alt-F
  is running by pressing and *keep* pressing the power button.

  If Alt-F is running, after three seconds the right orange led starts flashing,
  and three seconds later the orange left led; three seconds latter the
  the leds stop blinking and you can release the power button.
  This means that Alt-F is running, but you can't communicate with it --
  there are network problems, see 3) below.

  You can reboot the box by releasing the power button while the right led
  is flashing, or power it off releasing the power button when the left
  led is flashing.

  If the above does not work, you have to restart the box by pulling the
  power cord and reboot the box, which returns to the vendor's software.


2-The most common problem here are network problems.

  If your box is using DHCP, assign it a static IP instead, using the
  vendor's web page. Alt-F will use that IP. If using DHCP the current IP might
  be in use by another computer, or the router might ignore communications with a
  device using a IP not assigned by it.

  You can also try to ping, telnet, ssh or ftp the box, post the results.

How to upgrade:
===============

1. If you are still using the "on top"/"reloaded" method, which is now discontinued,
 you will have to flash Alt-F and after rebooting you can delete the 'alt-f' folder
 (NOT the 'Alt-F' folder, notice the case) and the fun_plug script.

2. Reboot the box before start performing any upgrade.

3. Save current settings to a desktop computer (System->Settings, in the "Computer Disk" section,
 "Save current settings to file", hit the Download button)
 
4. Look at the box bottom attached label to see its hardware revision level, then select the appropriate
 Alt-F-0.1RC5 bin file for it and use the normal firmware upgrade method to flash it.
 For example, the Alt-F-0.1RC5-DNS-320-rev-Ax.bin file is to be used on the DNS-320 rev-A1 or rev-A2,
 but not the DNS-320-rev-B1.
 Don't try to flash firmware if your box is not supported

5. All Disk-installable packages must be updated (Packages->Alt-F, UpdatePackagList, then UpdateAll)
 Stop all services before doing that (System->Utilities, Services section, hit the StopAll button)

