.. _introduction:


Introduction
============

The purpose of this document is to provide a practical introduction on
developing software for the AMOS-820 (Bare board: VAB-820) on a Linux
development host only.

Overview
--------

The VIA AMOS-820 platform is an embedded system powered by ARM
processor with Linux kernel 3.0.35 operating system by default. Major
functions of the Linux include all system-requirement shell commands and
drivers ready for AMOS-820 platform. AMOS-820’s Solution package does not
offer a development environment. Users can develop it under an Ubuntu
environment.

There are three major boot components for Linux, the "**u-boot.bin**", “**uImage**"
and "**Root File System**". The "u-boot.bin" is for initial peripheral hardware
parameter. The "uImage" is the Linux kernel image, and the "Root File System"
is for Linux O.S. The system will not boot successfully into a Linux
environment if one of these files does not exist in the boot media (SPI ROM,
SD storage card or onboard eMMC).

This development guide will use VAB-820 as an example instead of AMOS-
820 to describe relational building procedure.

Package Content
---------------

There are three folders in AMOS-820 Solution Pack.

.. _figure-pack-content:
.. figure:: images/pack_content.*
   :align: center
   :alt: HMI Solution Pack content

   VAB-820/AMOS-820 Solution Pack content


BSP Folder Contents
^^^^^^^^^^^^^^^^^^^

* LTIB (Linux Target Image Builder): A tool that can be used to develop and deploy
  BSPs (Board Support Packages) for a number of embedded target platforms including
  PowerPC, ARM.
* PatchFiles: This folder provides u-boot/kernel patch files for VAB-820.

EVK Folder Contents
^^^^^^^^^^^^^^^^^^^

* vab-820_demo_image.tar.bz2: Configure files when user would like to evaluate
  VAB-820 with Ubuntu root file system.

.. note :: If a user needs the supporting files for all software mentioned in
   AMOS-820 HMI Solution Pack document, please contact our regional sales representative
   for assistance.
