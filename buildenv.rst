.. _buildenv:

Setup Building Environment
==========================

.. highlight:: bash

This chapter will guide you through setting up your developing environment.
All instructions in this guide are for Ubuntu 10.04 (32Bit). Please install the
Ubuntu to your PC/NB in advance.

Configure Ubuntu
----------------

Change Default Editor (optional)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The default editor is ``nano``. To set ``vi``, or another as the default editor use::

  user@ubuntu:~$ sudo update-alternatives --config editor
  There are 3 choices for the alternative editor (providing /usr/bin/editor).

    Selection    Path               Priority   Status
  ------------------------------------------------------------
  * 0            /bin/nano           40        auto mode
    1            /bin/ed            -100       manual mode
    2            /bin/nano           40        manual mode
    3            /usr/bin/vim.tiny   10        manual mode

  Press enter to keep the current choice[*], or type selection number: 3
  update-alternatives: using /usr/bin/vim.tiny to provide /usr/bin/editor
  (editor) in manual mode.

Sudoers
^^^^^^^

The sudoers file must be updated to allow the login account to run the rpm
commands as root. The login for this example is ``user``. Edit the sudoer’s file
using the ``$ sudo visudo`` and add the line after the comment ``# User alias
specification`` as shown below. The first word ``%user`` will be the login name
you are using. Save the changes when it is done::

  user@ubuntu:~$ sudo visudo
  ...
  ...
  # User alias specification
  %user ALL = NOPASSWD: /usr/bin/rpm, /opt/freescale/ltib/usr/bin/rpm

Install the Host Packages
^^^^^^^^^^^^^^^^^^^^^^^^^

The following packages are installed to support a LTIB development
environment and presented in a bash script that can be cut and pasted into
your environment and execute::

  sudo apt-get install gettext libgtk2.0-dev rpm bison m4 libfreetype6-dev \
    libdbus-glib-1-dev liborbit2-dev intltool \
    ccache ncurses-dev zlib1g zlib1g-dev gcc g++ libtool \
    uuid-dev liblzo2-dev \
    tcl dpkg \
    texinfo texlive

  # The following recommended for Linux development.
  # They are not required by LTIB.

  sudo apt-get install gparted openssh-server \
    nfs-common nfs-kernel-server lintian \
    git-core git-doc git-email git-gui gitk \
    diffstat indent tofrodos fakeroot doxygen ubootmkimage \
    sendmail mailutils meld atftpd sharutils \
    manpages-dev manpages-posix manpages-posix-dev linuxdoc \
    vnc4server xvnc4viewer

Change the Default Shell
^^^^^^^^^^^^^^^^^^^^^^^^

The Ubuntu default shell is dash. To change the default shell to bash, select
``<No>`` and exit. This will remove the dash and use bash::

  user@ubuntu: $ sudo dpkg-reconfigure dash

Here are the results before and after the reconfiguration::

  user@ubuntu:~$ ls -l /bin/sh
  lrwxrwxrwx 1 root root 4 2013-05-16 10:32 /bin/sh -> dash
  user@ubuntu:~$ sudo dpkg-reconfigure dash
  [sudo] password for user:
  Removing 'diversion of /bin/sh to /bin/sh.distrib by dash'
  Adding 'diversion of /bin/sh to /bin/sh.distrib by bash'
  Removing 'diversion of /usr/share/man/man1/sh.1.gz to
  /usr/share/man/man1/sh.distrib.1.gz by dash'
  Adding 'diversion of /usr/share/man/man1/sh.1.gz to
  /usr/share/man/man1/sh.distrib.1.gz by bash'
  user@ubuntu:~$ ls -l /bin/sh
  lrwxrwxrwx 1 root root 4 2013-05-16 10:32 /bin/sh -> bash

Configure Ccache (optional)
^^^^^^^^^^^^^^^^^^^^^^^^^^^

LTIB uses ccache to speed up the compilation. The cache that LTIB uses exists
as a .ccache directory in each user's home directory. The path for this example
is "/home/user/.ccache". This directory can grow to be quite large if no upper
limit is set.

The followings are the ccache commands to limit the size to 50 MB. The size
may change as you see fit, clean the ccache and show the settings::

  [1] user@ubuntu:~$ ccache -M 50M
  Set cache size limit to 50.0 Mbytes
  [2] user@ubuntu:~$ ccache -c
  Cleaned cache
  [3] user@ubuntu:~$ ccache -s
  cache directory                      /home/user/.ccache
  cache hit (direct)                      0
  cache hit (preprocessed)                0
  cache miss                              0
  files in cache                          0
  cache size                              0 Kbytes
  max cache size                       50.0 Mbytes

Change Permissions on /opt
^^^^^^^^^^^^^^^^^^^^^^^^^^

The LTIB installation process creates the directory “/opt/freescale”. By default
the “/opt” directory has root privileges which are changed to allow a regular
user to access::

  user@ubuntu:~/$ ls -ld /opt
  drwxr-xr-x 2 root root 4096 2013-05-16 10:47 /opt
  user@ubuntu:~/$ sudo chmod 777 /opt
  [sudo] password for user:
  user@ubuntu:~/$ ls -ld /opt
  drwxrwxrwx 2 root root 4096 2013-05-16 10:47 /opt

Install LTIB
------------

The **LTIB** (Linux Target Image Builder) is a tool that can be used to develop
and deploy BSPs (Board Support Packages) for a number of embedded target
platforms including PowerPC, ARM.

The AMOS-820 Solution Pack is developed based on Freescale released
i.MX6x BSP ``L3.0.35_4.1.0_130816_source.tar.gz``. Users can get it from
Freescale official web site.

Extracting Bundle and Installing LTIB
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
This section describes the steps to extract the content from the source bundle
and to install LTIB.

Download i.MX6 Linux Source Bundle
"""""""""""""""""""""""""""""""""""""""""""

The Linux source bundle can be downloaded from the `Freescale software directory`_.

.. _Freescale software directory: http://www.freescale.com/webapp/sps/site/prod_summary.jsp?code=i.MX6Q&fpsp=1&tab=Design_Tools_Tab

The Run-time Software section in Software & Tools tab is shown in :num:`Figure #figure-ltib`.

.. _figure-ltib:
.. figure:: images/ltib.*
   :align: center
   :alt: Source Code download link

   Source Code download link

Using Firefox, the default save location is in the Downloads folder that
contains the file: ``L3.0.35_4.1.0_130816_source.tar.gz``.

User install
""""""""""""

All LTIB installation and execution should be done by a regular user instead of
a root account.

To accommodate running LTIB as a regular user and allow this process to
perform privileged commands requiring root permissions that the sudoer’s file
is modified (see section 2.1.2), and the permissions on the /opt directory are
changed (see section 2.1.6).

Extract content
"""""""""""""""

We assume that the file ``L3.0.35_4.1.0_130816_source.tar.gz`` is already in the
``/home/user/imx6/`` folder::

  user@ubuntu:~/imx6/$ ls -l
  total 1047352
  -rwxr-xr-x 1 user user 896737878 2013-04-09 16:37
  L3.0.35_4.1.0_130816_source.tar.gz
  
  user@ubuntu:~/imx6$ tar -zxf L3.0.35_4.1.0_130816_source.tar.gz
  user@ubuntu:~/imx6$ cd L3.0.35_4.1.0_130816_source/
  user@ubuntu:~/imx6/L3.0.35_4.1.0_130816_source$ ls
  EULA install ltib.tar.gz package_manifest.txt pkgs redboot_201003.zip tftp.zip

  user@ubuntu:~/imx6/L3.0.35_4.1.0_130816_source$ ./install

  You are about to install the LTIB (GNU/Linux Target Image Builder)
  Before installing LTIB, you must read and accept the EULA (End User
  License Agreement) which will be presented next.

  Do you want to continue ? Y|n
  Y
  - - - - - - - - - - - - -
  I have read and accept the EULA (yes|no):
  yes
  The LTIB files are extracted from a tar file which includes the
  prefix ltib. After installation you will find LTIB in:
  /home/user/imx6/L3.0.35_4.1.0_130816_source/ltib
  Where do you want to install LTIB ?
  (/home/user/imx6/L3.0.35_4.1.0_130816_source)
  /home/user/imx6/
  - - - - - - - - - - - - -
  Copying packages to ../ltib/pkgs
  Installation complete, your ltib installation has been placed in
  /home/user/imx6/ltib, to complete the installation:

  cd /home/user/imx6/ltib
  ./ltib

  user@ubuntu:~/imx6/L3.0.35_4.1.0_130816_source$
