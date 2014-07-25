.. _ltib:

Building through LTIB
=====================

Once the building environment has been configured as described in Chapter 2,
the environment and host are now ready to run the LTIB. This chapter will
guide through building the BSP via LTIB.

Getting iMX6x Based Board Packages
----------------------------------

To get iMX6 based board packages through LTIB when you first use the LTIB.
Those packages include iMX6 hardware definitions, drivers and many more::

  user@ubuntu:~/imx6/ltib$ ./ltib –c

Depending on the performance of user’s computer, this process will take time
to run. It may take several minutes to an hour. Once the LTIB environment is
configured, a menu will be available for selecting configurations.

The first menu is shown in :num:`Fuigure #figure-ltib-platform`. Use the arrow keys to move the cursor
between **<Select>** and **<Exit>**.

Choose **<Select>** then press enter key to open the selected item.

.. _figure-ltib-platform:
.. figure:: images/ltib_platform.*
   :align: center
   :alt: Target Image Builder Platform Selection

   Target Image Builder Platform Selection

In :num:`Figure #figure-ltib-selection`, it shows that **<Yes>** has been selected. Press enter key to save.

.. _figure-ltib-selection:
.. figure:: images/ltib_selection.*
   :align: center
   :alt: Save Platform Image Selection

   Save Platform Image Selection

The next menu is shown in :num:`Figure #figure-imx-platforms` and provides the target platform to be
selected. Using the cursor arrow down key, move the cursor over the
**Selection (imx25_3stack)** and press enter. The submenu of the selected
platform is shown in :num:`Figure #figure-imx6q-platform`.

.. _figure-imx-platforms:
.. figure:: images/imx_platforms.*
   :align: center
   :alt: i.MX Development Platforms

   i.MX Development Platforms

In submenu selection, select the **imx6q** platform by moving the cursor over
the imx6q as shown in :num:`Figure #figure-imx6q-platform`, then press enter key.

.. _figure-imx6q-platform:
.. figure:: images/imx6q_platform.*
   :align: center
   :alt: imx6q Platform Selection

   imx6q Platform Selection

Move the cursor to **<Exit>** then press enter key. The save screen is presented
as shown in :num:`Figure #figure-platform-save`. Select **<Yes>** and press enter key to go to the option.

.. _figure-platform-save:
.. figure:: images/platform_save.*
   :align: center
   :alt: Platform Save

   Platform Save


The imx6q Based Boards menu is now presented as shown in :num:`Figure #figure-boards-selection`. The u-
Boot board selection must be changed to mx6q_sabrelite. Move the cursor
over **board (mx6q_arm2)** and press enter key.

.. _figure-boards-selection:
.. figure:: images/boards_selection.*
   :align: center
   :alt: iMX6q Based Boards

   iMX6q Based Boards

Then choose the **mx6q_sabrelite** as shown in :num:`Figure #figure-sabrelite`.

.. _figure-sabrelite:
.. figure:: images/sabrelite.*
   :align: center
   :alt: iMX6q sabrelite

   iMX6q_sabrelite

Move the cursor over **<Exit>** and press enter key.
In :num:`Figure #figure-save-config`, it shows that **<Yes>** has been selected. Press enter key to save
the configuration.

.. _figure-save-config:
.. figure:: images/save_config.*
   :align: center
   :alt: Save the configuration

   Save the configuration

The sudoer’s password is asked for the current user. Enter the password to
begin the building process. The building process will take 1.5 hours to
complete.
