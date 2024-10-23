[![License: GPL v2](https://img.shields.io/badge/License-GPL%20v2-blue.svg)](https://www.gnu.org/licenses/old-licenses/gpl-2.0.en.html)

> This is a temporary setup managed for personal use. It is a copy of the 
`pkgs/stable` and `pkgs/unstable` branches of the `awanga/alt-f-next_binaries` 
repository. It has no offical value. **Do not rely on it.**

Alt-F-next
==========

Alt-F-next provides a free and open source alternative firmware for the 
DLINK DNS-320/320L/321/323/325/327L and DNR-322L based on Alt-F.

Alt-F-next has Samba and NFS; supports ext2/3/4, VFAT, NTFS, BTRFS; 
RAID 0, 1, 5 (with external USB disk) and JBOD; supports up to 8TB disks; 
rsync, ftp, sftp, ftps, ssh, lpd, DNS and DHCP servers, DDNS, fan and leds 
control, clean power up and down... and more.

Alt-F-next also has a set of comprehensive administering web pages, you don't 
need to use the command line to configure it.

Besides the built-in software, Alt-F-next also supports additional packages on 
disk, including ffp and Entware packages, that you can install, update and 
uninstall using the administering web pages

Finding the box IP
------------------

When booting, and to assign itself an IP, Alt-F tries to first use a IP supplied
through the kernel command line; if it is not supplied, it then tries
to use its own flashed settings; if unsuccessful it then tries to use the vendors
firmware flash defaults; if unsuccessful it then tries to requests a IP
from a DHCP server and finally tries to find an unassigned IP in the
192.168.1.254 to 192.168.1.240 range.

In detail, when Alt-F boots, it search:
0-For a kernel command line supplied name and IP (the default when using the
	 supplied fun_plug script)
1-If not found, it will search for its own settings in flash, and use them;
	you should do a "Save Settings" from the web page when you made any
	change that you want to preserve across reboots. Alt-F and the vendors
	defaults can coexist in flash while there is space available;
2-If not found, it will use one of the vendor's file in flash, and if the box
  was *not* using DHCP, use the IP specified there;
3-If not found, it will try to use DHCP, you can consult your router web page
	to find what IP was assigned;
4-As a last resort, if no DHCP server is available, Alt-F will use the first
	free IP in the 192.168.1.254 until 192.168.1.240 range.

So, when booting using the fun_plug script, your box it will have the same IP
and name as before.

If booting after flashing Alt-F, and if your box was setup with a fixed IP
in the vendors firmware, it will also have the same IP and name as before.
If your box was using DHCP in the vendors firmware, then it might have a different
IP under Alt-F, and you must go to the DHCP server web page (usually your router)
and find the assigned client IP. Do this before trying Alt-F, to be sure that you
know how to do it.
In the "worst" case scenario, the box IP will be 192.168.1.240, but this is
very unlikely.

The easiest and recommended way for a server is for you to setup first the box
with a fixed IP using the vendor's firmware. This will be the IP used by Alt-F.

First time setup
----------------

After getting the box IP and accessing it using a browser, you will see
Alt-F status page. To go to any other page you have to setup and confirm
an administrative password. The 'root' user (the linux Administrator)
password will be the same.

You can then login using telnet or ssh and change the root password
using the "passwd" command. Note that if you later change the password
using the web page, than the root password will also be changed, so the
recommend procedure is to first set up a web password and then login
using telnet or ssh and change the root password.

The first time that you use the web pages or change the password, you
will be guided through a small set of pages, being prompted for a few
configuration details. If you miss or skip some of these pages, you must
manually follow the following sequence:

Setup->Host: you must setup your host details, such as name, description and
workgroup, and the networking details.

Setup->Time: setup your timezone, i.e., the country and city where
you live, and adjust the clock.

Disk-Wizard: You can format your disks, or leave them as they are, Alt-F will
recognize most partitioning schemes. Be aware that some partitioning schemes,
filesystems or RAID layouts are not recognized be the vendor's firmware.

Setup->Users: you must create at least one user. You will be asked for a filesystem
where a directory called Users will be created, and where all users home directory
will be created.
To create a new user give the user name, such as 'John Smith', a nick name, such
as 'jsmith' (no spaces), and a password and password confirmation.
You can leave the User and Group Id with the default values.
The user name and password will be used to authenticate under MS-Windows, while the
nick name and password will be used to authenticate under linux.

Services->Network: You will have to start the network service of your choice, or
several simultaneously. Most MS-Windows users want smb/samba. By default shares named
'Users', 'Public RW' and 'Public RO' will be available (*after* you create at least
one user)

System->Settings: In the end, you should save the settings in flash memory.
The vendor's flashed settings are only lost if you deliberately clear settings.
However, they use almost all available flash space, so when trying to
save Alt-F settings you can see an error message.

If you don't save settings, on each reboot a new RSA/DSA key pair is generated
for dropbear/ssh usage, and your ssh client will complain.

After you save the settings in flash, Alt-F will use them on the next reboot,
so if you have setup a new IP in the host page, be sure that you write it down.

Alt-F Packages
--------------

You can install additional software packages on disk using the Package->Alt-F menu.
You have to specify a filesystem where a folder named "Alt-F" will be created,
and all installed package files will written.
DON'T directly add, remove or change any file in this folder, as the system might hang.

Some of the installed packages have administering web pages, accessed through the
menus. New entries will appear under Services->User, Services->Network or
Services->System. In some cases new menu entries might also appear, e.g.
Disk->LVM or Disk->Encryption.

Disk Details
------------

When a new disk is detected, either internal or USB connected, either at
boot or by hot-plugging,  Alt-F will first check and if necessary fix all
its filesystems, either if they are "dirty" or if "it's time" to do it.
A filesystem is dirty if the box was not properly shutdown or rebooted,
that is what happens with the vendor's firmware and what motivated Alt-F birth.

Thus, on its first boot, Alt-F will find all filesystems dirty and will
do a lengthy check/fix -- this can tens of minutes on very large disks
(12 minutes on an 88% full 230GB disk). During the check/fix, the led of
the disk which is being checked will flash, and the power led will flash
with an "heartbeat" rhythm. On poweroff or reboot, Alt-F will properly
unmount the disks, so the check/fix step will not happen at the second
and subsequent reboots. However, a check/fix is done every M mounts or
after D days since the last check. In the web status page, you are
warned that is time for you to start a filesystem check, in order to
avoid the lengthy check at the next boot.

After checking/fixing, Alt-F will mount all the disks filesystems in
the directory /mnt/<name>, where <name> can be either the disk partition
name, as in /mnt/sda3, or a filesystem label, such as /mnt/Backups.

If during the check/fix step the partition was dirty and could not be fixed,
it will be mounted read-only, in order to not further damage it.

You can set a filesystem label using the Disk->Filesystem web page
(not for NTFS, at least until you install the ntfsprogs package).
Ext2/3/4/VFAT/NTFS/ISO9660 filesystems are recognized.

RAID
----

RAID-0, 1 and 5 (using an external USB disk) and linear RAID (JBD) are recognized,
assembled and mounted when a disk is inserted or detected.

The leds will be steady orange when a RAID device is degraded, and will
flash in  orange during resync or rebuild. In the status web page you can see
the expected time for completion.

Please note that RAID hot-plugging is not yet fully functional; when
devices are inserted, it works fine, they are incrementally assembled
and filesystems are mounted, but if some device is removed and then
reinserted, hot-plugging might not work properly.

Also notice that because the arrays are incrementally added and mounted
as soon as possible, they must have the "intent bitmap" active,
otherwise a rsync happens when the second (on raid-1) or third (on
raid-5) device is inserted. This is work in progress.

Special Folders
---------------

When mounting filesystems, Alt-F will look for a directory called "ffp"
in the filesystem root, and if found a link is created to it at the
root, /. Using the "ffp Package Manager" web page you can install, update or
remove ffp packages.

If a directory name "Users" is found, will be linked to /home.

Alt-F also looks for a directory called "Alt-F". If found it will be
aufs-mounted on top of root, "/", shadowing it. This means that any files
(including executable) existing there will be used instead.
Mostly DON'T change or add nothing in that directory, or the box
might crash.

Printers
--------
 
USB attached printers will also be recognized, and a rudimentary lpd
spooler used for them. They can also be used with samba.

Disk Spindown
-------------
Disks can be set to sleep after a predetermine time of inactivity using the
Disk->Utilities web page. When a disk spins down the corresponding orange
led will slowly flash. If both disks are in standby, the box power led will
be turned off. The network led can't be easily controlled, and so it will
not be turned off.

Front Button
------------

If you keep the box front button pressed for more than 3 seconds, the
right led will start flashing, and if you release the button while it is
flashing an ordered reboot will be done; if keep pressed by three more
seconds, the left led will start flashing, and if the button is then
released, the box will do an ordered shutdown.
Read the AboutButtonsAndLeds wiki.

The reboot and power-down procedures cleanly stop all processes, unmount
filesystems, and stop raid devices.
Internal or external disks can be hot-removed after being "ejected"
using the "eject" command or the "Disk utils" web page.

Back Button
-----------

The box back button has three functionalities:
-If pressed for only a couple of seconds, a script that you have to write will be executed
-If pressed for more than 10 seconds, you can telnet the box on port 26
  as the 'root' user without supplying a password
-If pressed for more then 20 seconds, all flash memory saved settings will be erased and
 the box will do a controlled reboot.

Read the AboutButtonsAndLeds wiki.

