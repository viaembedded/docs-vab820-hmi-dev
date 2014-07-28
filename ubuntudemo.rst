.. _ubuntudemo:

Appendix A. Making Ubuntu Demo Image
====================================

There is a Canonical trademark policy when using Ubuntu in commercial usage
or redistribution. The AMOS-820 HMI Solution Pack does not provide Ubuntu
DEMO image for evaluation actively. User can follow Freescale’s policy and
get the demo image from Freescale official web site, if user would like to
evaluate Ubuntu on VAB-820.

This section will guide you through making an Ubuntu demo image, then copy
it into Micro SD storage card or eMMC.

**Required files**

**Ubuntu file system**: You can download Ubuntu file system from `Freescale
official web site`_. The file name for this example is ``oneiric.tgz``.

.. _figure-freescale-ubuntu:
.. figure:: images/freescale-ubuntu.*
   :align: center
   :alt: Freescale website screenshot

   Freescale website screenshot

.. _Freescale official web site: http://www.freescale.com/webapp/sps/site/prod_summary.jsp?code=i.MX6Q&fpsp=1&tab=Design_Tools_Tab

**EVK/vab820_demo_image.tar.gz**: We provide some scripts for you to install
demo images on Micro SD card and eMMC.

Making demo image into Micro SD
-------------------------------

**Step 1**

Prepare a Micro SD storage card (at least 4GB size and Class 4), and insert it
into your Linux developing PC (Ubuntu 10.04.x x86 at least).

**Step 2**

Copy the demo image ``EVK/vab-820_demo_image.tar.bz2`` to your developing PC.

**Step 3**

Open Terminal utility.

**Step 4**

Untar ``vab-820_demo_image.tar.bz2``::

  user@ubuntu:~/$ tar jxvf vab-820_demo_image.tar.bz2

**Step 5**

Put the downloaded file system ``oneiric.tgz`` under ``vab820_demo_image/``::

  user@ubuntu:~/$ cp oneiric.tgz vab-820_demo_image/

**Step 6**

Change directory to ``vab-820_demo_image/``::

  user@ubuntu:~/$ cd vab-820_demo_image/
  user@ubuntu:~/vab-820_demo_image$

**Step 7**

Run ``820_create_sd_fs.sh`` script::

  user@ubuntu:~/vab-820_demo_image$ ./820_create_sd_fs.sh /dev/sdb

**Step 8**

Remove the Micro SD card from your developing PC and insert it into VAB-820.
Switch the jumper to boot from Micro SD.

**Step 9**

Modify the u-boot parameter to load kernel from Micro SD card::

  setenv bootcmd 'run bootcmd_sd'

**Step 10**

After booting to ubuntu, open Terminal utility and run the script on Desktop
to update X11 acceleration files::

  linaro@linaro:~/$ cd Desktop/820_x11_hw_accel/
  linaro@linaro:~/Desktop/820_x11_hw_accel$ ./X11-acceleration-setup.sh

Making demo image into eMMC
---------------------------

**Step 1**

Copy the demo image ``EVK/vab-820_demo_image.tar.bz2`` to your bootable
Micro SD card::

  user@ubuntu:~/$ cp vab-820_demo_image.tar.bz2 /media/sd_820/home/linaro/

**Step 2**

Open Terminal utility and untar ``vab-820_demo_image.tar.bz2``::

  user@ubuntu:~/$ cd /media/sd_820/home/linaro
  user@ubuntu:/media/sdcard/home/linaro$ tar jxvf vab-820_demo_image.tar.bz2

**Step 3**

Put the downloaded file system oneiric.tgz under ``vab820_demo_image/``::

  user@ubuntu:~/$ cp oneiric.tgz /media/sd_820/home/linaro/vab-820_demo_image

**Step 4**

Insert the Micro SD card into VAB-820 and switch the jumper to boot VAB-820
from Micro SD card.

**Step 5**

Open Terminal utility.

**Step 6**
Change directory to ``vab-820_demo_image/``::

  linaro@linaro:~/$ cd vab-820_demo_image/
  linaro@linaro:~/vab-820_demo_image$

**Step 7**

Run ``820_create_emmc_fs.sh`` script::

  linaro@linaro:~/vab-820_demo_image$ ./820_create_emmc_fs.sh

**Step 8**

Remove the Micro SD card from VAB-820. Switch the jumper to boot from SPI ROM
and reboot VAB-820.

**Step 9**

Modify the u-boot parameter to load kernel from eMMC::

  setenv bootcmd ‘run bootcmd_mmc’

**Step 10**

After booting to ubuntu, open Terminal utility and run the script on Desktop
to update some X11 acceleration related files::

  linaro@linaro:~/$ cd Desktop/820_x11_hw_accel/
  linaro@linaro:~/Desktop/820_x11_hw_accel$ ./X11-acceleration-setup.sh

Setting u-boot parameters
-------------------------

**Step 1**

Setting the display devices.

**[HDMI]**

To set HDMI as display output::

  setenv bootargs_base ‘setenv bootargs console=ttymxc1,115200 ${hdmi}’

To set HDMI resolution::

  setenv hdmi ‘video=mxcfb0:dev=hdmi,1920x1080M@60,if=RGB24’

**[LVDS]**

To set LVDS as display output::

  setenv bootargs_base ‘setenv bootargs console=ttymxc1,115200 ${lvds}’

To set LVDS resolution::

  setenv lvds ‘video=mxcfb0:dev= ldb,LDB-WSXGA+,if=RGB24 ldb=spl0’

.. note:: The LVDS type for this example is TP220C01 V0(AUO G220SVN01.0)

**Step 2**

Setting storage devices

**[eMMC]**

Enabling booting from eMMC::

  setenv bootargs_mmc 'set bootargs ${bootargs} root=/dev/mmcblk0p1 rw rootwait'
  setenv bootcmd_mmc 'run bootargs_base bootargs_sd; mmc dev 1; ext2load mmc 1:1 $loadaddr $vkernel && bootm'
  setenv bootcmd 'run bootcmd_mc'

**[Micro SD storage card]**

Enabling booting from SD card::

  setenv bootargs_sd 'set bootargs ${bootargs} root=/dev/mmcblk1p1 rw rootwait'
  setenv bootcmd_sd 'run bootargs_base bootargs_sd; mmc dev 0; ext2load mmc 0:1 $loadaddr $vkernel && bootm'
  setenv bootcmd 'run bootcmd_sd'

**Step 3**

Setting MAC address. Two ways to set MAC address:

**[Way 1]**

Pass MAC address from u-boot parameter; please ensure that the "ethaddr" has a
valid MAC address as parameter (replace ``xx:xx:xx:xx:xx:xx`` with that MAC
address below). User can set a real MAC address according to sticker on
Ethernet PHY::

  setenv ethaddr 'xx:xx:xx:xx:xx:xx'
  setenv bootargs_base 'setenv bootargs console=ttymxc1,115200 fec_mac=${ethaddr} ${hdmi}'

**[Way 2]**

Here, user can set the MAC address in eFuse. The address is on the Ethernet
physical port.

The MAC Address for this example is ``11:22:33:44:55:66``.
User can write the MAC address::

  imxotp blow --force 22 0x33445566
  imxotp blow --force 23 0x1122

User can check the MAC address::

  imxotp read 22
  0x33445566 (Shows the address that the user wrote)
  imxotp read 23
  0x1122 (Shows the address that the user wrote)

It is a must to reset after you set MAC address and save::

  reset

.. warning:: Be careful to write MAC address into eFuse, because the eFuse can only be written once.
