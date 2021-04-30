.. title: Using webcam on Firefox in FreeBSD
.. slug: using-webcam-on-firefox-in-freebsd
.. date: 2020-11-17 20:20:04 UTC+08:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text

Using webcam is a common thing to do on a laptop and it is not hard to set up in FreeBSD as well. 

First go ahead and install webcamd,

.. code-block:: sh

   $ doas pkg install webcamd


According to the `man page`_, webcamd is a daemon that provides access to webcam devices and such. According to the man pages, we need to load
the cuse kernel module as well as start the webcamd daemon. 

.. code-block:: sh

   # Adding cuse to bootloader and webcamd to start up services
   $ doas echo 'cuse_load="YES"' >> /boot/loader.conf
   $ doas sysrc webcamd_enable="YES"

To load the kernel modules and the webcamd service without reboot, you can do the following,

.. code-block:: sh

   $ doas kldload cuse
   $ doas service webcamd start


After which, you can go online with your Firefox and do a webcam test online. I am relatively surprised as how easy it is to set up webcam support
on my FreeBSD. A shoutout to this `David Schlachter's page`_ for providing me the initial steps to setting up webcam on FreeBSD.


.. _man page: https://www.freebsd.org/cgi/man.cgi?query=webcamd&sektion=8&manpath=freebsd-release-ports
.. _David Schlachter's page: https://www.davidschlachter.com/misc/freebsd-webcam-browser

