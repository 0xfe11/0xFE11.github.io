.. title: Setting up a single user XFCE environment for FreeBSD
.. slug: setting-up-a-single-user-xfce-environment-for-freebsd
.. date: 2020-09-15 22:30:59 UTC+08:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text

Recently I wanted to install XFCE on an old laptop of mine for someone who was not very familiar with the command line interface. I had
previously installed a tiling manager but I think having a desktop environment will be more user friendly. In addition, there will most likely
be the only person using the machine so I decided to set it up that way.

This are my notes on how to set up the xfce desktop environment, feel free to install your own desktop environment of choice. This guide will
assume that you have just installed FreeBSD with a user which is (at least) added to the operator group. I am also using tcsh as my login 
shell.

Steps
=====

1. Install xfce
2. Enable shutdown option in xfce
3. Enable autologin to xfce

1. Install xfce
---------------

Proceed to install xfce or your desktop environment of choice. Before that, ensure that Xorg is also installed.

.. code-block:: shell

  $ doas pkg update
  $ doas pkg install xorg xfce

2. Enable shutdown option in xfce
---------------------------------

By default, after you install xfce and start the xfce, one thing you will notice is that when you click on your name, there is only an option
for logout. Shutdown/restart/hibernate seems to be grayed out. To enable the buttons, you will have to do the following.

* Make sure myuser is in group operator
* Enable dbus in rc.conf
* Adding rules in polkit

Make sure myuser is in group operator
_____________________________________

Make sure myuser using the desktop environment is in group operator. This can be checked with the following command,

.. code-block:: shell

  $ id myuser
  uid=1001(myuser) gid=1001(myuser) groups=1001(myuser),5(operator)

Otherwise if myuser is not in the group, use the following to add it to the group,

.. code-block:: shell

  $ doas pw groupmod operator -m myuser

Verify it again using the id command.

Enable dbus in rc.conf
______________________

D-Bus is installed as part of the xfce package according to the FreeBSD handbook_ and can be
enabled in your rc.conf by adding the following line to your rc.conf.

.. code-block:: shell

  $ doas sysrc dbus_enable="YES"

Adding rules in polkit
______________________

Add a new rule to polkit, assuming shutdown.rules does not exist,

.. code-block:: shell

  $ cd /usr/local/etc/polkit-1/rules.d
  $ doas touch shutdown.rules

Paste the contents below to the file shutdown.rules

.. code-block:: shell

  # shutdown.rules

  polkit.addRule(function (action, subject) {
    if ((action.id == "org.freedesktop.consolekit.system.restart" ||
    action.id == "org.freedesktop.consolekit.system.stop")
    && subject.isInGroup("operator")) {
      return polkit.Result.YES;
    }
  });

  polkit.addRule(function (action, subject) {
    if (action.id == "org.freedesktop.consolekit.system.suspend"
    && subject.isInGroup("operator")) {
      return polkit.Result.YES;
    }
  });

Reboot your FreeBSD.

.. code-block:: shell

  $ doas shutdown -r now

Log into myuser and start xfce.

.. code-block:: shell

  $ startxfce4

When clicking log out, the shutdown/restart should be enabled now instead of greyed out.

.. image:: /images/setting-up-xfce-environment-for-single-user_0.PNG

Next we are going to set up auto login for myuser.

3. Enable autologin to xfce
---------------------------

Firstly we copy startxfce4 script to the user's home .xinitrc.

.. code-block:: shell

  $ cp /usr/local/bin/startxfce4 ~/.xinitrc

Next we will edit /etc/gettytab

.. code-block:: shell
   
  $ doas vi /etc/gettytab

And add the following into /etc/gettytab

.. code-block:: shell
   
  Al|Autologin myuser:\
     :al=myuser:tc=Pc:

Next we will edit /etc/ttys

.. code-block:: shell
   
  $ doas vi /etc/ttys

And change the following lines into /etc/ttys, from

.. code-block:: shell

  ttyv0    "/usr/libexec/getty Pc"   xterm   onifexists  secure

to the following

.. code-block:: shell

  ttyv0    "/usr/libexec/getty Al"   xterm   onifexists  secure

When you have completed the above, your FreeBSD should be able to login automatically to myuser after you reboot. You can verify by rebooting,

.. code-block:: shell

  $ doas shutdown -r now

and when you see myuser logged in automatically, the configuration is correct. Now the final step is to add a .login script to execute
automatically whenever we are logged in.

.. code-block:: shell

  $ vi .login

and add the following lines, (note: I am using tcsh as my shell for myuser, change the script accordingly to suit your shell)

.. code-block:: shell

  if ($tty == "ttyv0") then
    echo Starting xfce...
    startx
  endif

Afterwards, you can test if the .login script works by logging out.

.. code-block:: shell

  $ exit

This will let it logout and because of the autologin behavior, it should log back into myuser and run the .login script. If the all else
succeed, you will be able to see a xfce environment with shutdown/restart/suspend buttons working.

Here are a list of resources that I got my scripts and configurations from

FreeBSD Handbook

* x11-wm_

FreeBSD Forums

* `Display Manager`_
* `Trouble calling 'startx' in Start-Up script`_
* `xfce4 no abiltiy to shutdown and reboot that went away`_

.. _handbook: https://www.freebsd.org/doc/handbook/x11-wm.html
.. _x11-wm: handbook_
.. _Display Manager: https://forums.freebsd.org/threads/display-manager.47305/#post-264818
.. _Trouble calling 'startx' in Start-Up script: https://forums.freebsd.org/threads/trouble-calling-startx-in-start-up-script.22304/#post-125787
.. _xfce4 no abiltiy to shutdown and reboot that went away: https://forums.freebsd.org/threads/xfce4-no-abiltiy-to-shutdown-and-reboot-that-went-away.72671/
