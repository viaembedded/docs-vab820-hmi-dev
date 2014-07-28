.. _touchpanel:

Appendix B. Touch Panel Calibration
===================================
The Touch panel type for this example is TP220C01 V0(AUO G220SVN01.0)

**Step 1**

Download Linux Driver package from the `EETI official web site`_.

.. _EETI official web site: http://home.eeti.com.tw/drivers.html

Click "Yes, go to Touch Driver Download page" to get driver package.

.. _figure-touch-download:
.. figure:: images/touch_download.*
   :align: center
   :alt: Touchscreen driver start

   Touchscreen driver start

**Step 2**

Click "Linux" then down **ARM/MIPS “eGTouch_v2.5.3120.L-ma**. The
``eGTouch_v2.5.3120.L-ma.zip`` will be downloaded to user’s local storage.

.. _figure-touch-download2:
.. figure:: images/touch_download2.*
   :align: center
   :alt: Touchscreen driver download

.. _figure-touch-download3:
.. figure:: images/touch_download3.*
   :align: center
   :alt: Touchscreen driver download

**Step 3**

Unzip ``eGTouch_v2.5.3120.L-ma.zip``. A folder ``eGTouch_v2.5.3120.L-ma`` will
be created::

  $unzip eGTouch_v2.5.3120.L-ma.zip

**Step 4**

Before running install setup script, please plug-in the controller first. Then you
could execute.

Execute script file ``setup.sh`` to install driver automatically::

  $sudo sh setup.sh 

If you want to remove remove the eGTouch driver later::

  $sudo sh setup.sh uninstall

For more detailed information, user can refer to the guide
``EETI_eGTouch_Linux_Programming_Guide_v2.5f.pdf`` under
``eGTouch_v2.5.3120.L-ma\Guide\``

**Step 5**

Execute eCalib to process calibration procedure.

Please execute tools under root permissions::

  $sudo eCalib

eCalib: The tool eCalib is a calibration tool with command line. Please type
``eCalib -h`` to see the usage content.

User can select 4 or 9 point to calibrate.
