.. title: Setting up Chinese Pinyin on my FreeBSD
.. slug: setting-up-chinese-pinyin-on-my-freebsd
.. date: 2020-12-06 10:27:45 UTC+08:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text

A few days earlier, I was wondering if I can set up pinyin input on my FreeBSD machines. After some research, big thanks to outpaddling on his post on the `FreeBSD forum`_ for pointing me in the right direction. 
I was able to get it to work. Here is the guide to set up pinyin on your FreeBSD machine.



First, install the chinese fonts,

.. code-block:: shell

  $ doas pkg install zh-CJKUnifonts zh-font-std zh-opendesktop-fonts zh-CNS11643-font zh-kcfonts



Next, install the input engine, here we are going to use fcitx. As per recommendation from the original poster, it seems that libpinyin and sunpinyin works well.

.. code-block:: shell

  $ doas pkg install fcitx-qt5 zh-fcitx-configtool zh-fcitx-libpinyin zh-fcitx-sunpinyin



After installation is done, add the following lines to your .xinitrc.

.. code-block:: shell

  XMODIFIERS='@im=fcitx'
  GTK_IM_MODULE=fcitx
  GTK3_IM_MODULE=fcitx
  QT4_IM_MODULE=fcitx # Or use qtconfig to change
  export XMODIFIERS GTK_IM_MODULE GTK3_IM_MODULE QT4_IM_MODULE



Lastly for those using a desktop environment, you can add the fcitx to startup by doing the following,

.. code-block:: shell

   $ ln -s /usr/local/share/applications/fcitx.desktop ~/.config/autostart



And now you can restart X and see if fcitx is loaded. On my mate session, there is now an additional keyboard icon (in the middle) on my panel.

.. image:: /images/setting-up-chinese-pinyin-on-my-freebsd_0.png



Right click on it to open the panel and choose "Configure Current Input Method".

.. image:: /images/setting-up-chinese-pinyin-on-my-freebsd_1.png



Make sure "Only Show Current Language" is unchecked and scroll to Chinese (China) and select Pinyin (LibPinyin) or Sunpinyun. To add the input, click on the right arrow and make sure the selected input 
is added to current input.

.. image:: /images/setting-up-chinese-pinyin-on-my-freebsd_2.png


Now to test if the pinyin input works, you can open a text editior, and click on the fcitx icon to toggle between english and chinese pinyin input method. You can start typing and a small suggestion box should appear.

.. image:: /images/setting-up-chinese-pinyin-on-my-freebsd_3.png


.. _FreeBSD forum: https://forums.freebsd.org/threads/chinese-input-method-with-fcitx.67149/ 
