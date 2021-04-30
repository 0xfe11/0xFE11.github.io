.. title: Setting up powerline on FreeBSD
.. slug: setting-up-powerline-on-freebsd
.. date: 2020-07-25 15:53:04 UTC+08:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text

Earlier in the week, I got my Windows machine with the Powerline prompt working and I thought it
would be a fun exercise to get the same on my FreeBSD machine. After messing around for a few hours,
I finally got it to work. Here are the steps that I took to set it up.



1. Installation of packages
---------------------------

Install the following packages as root,

.. code-block:: sh

   # pkg install powerline-fonts
   # pkg install py37-powerline-status

The powerline-fonts package contains patched fonts for special symbols that are not found in the
regular fonts.



2. Start powerline at start
---------------------------

Add the following line to your shell's rc file, for my case, I am using zsh and so I add the 
following line to my zshrc file.

.. code-block:: sh

   # rc file
   . /usr/local/lib/python3.7/site-packages/powerline/bindings/zsh/powerline.zsh

This will invoke powerline when you open the terminal.



3. Getting patched font to use
------------------------------

You might need to consult your manual related to the specific terminal you are using. 
Firstly, the get the name of the patched fonts to use, you can do the following command,

.. code-block:: sh

   # Get all fonts registered that are in powerline patched folders
   $ fc-list | grep powerline

I like the Hack-Regular patched font so I am going to go with that.



4. Set up your terminal to use patched fonts
--------------------------------------------

Personally, I am using xterm so this is what I have added to my .Xresources. 

.. code-block:: sh

   # .Xresources file
   XTerm*faceName: Hack-Regular

5. Use Unicode for your FreeBSD
-------------------------------

Before I came upon this step, I was having issue with the fonts in my terminal. As it turned out,
after this step, my terminal is showing the powerline fonts properly.

If this file .login_conf do not exist on your home directory, have it created and add the
following based on your locale.

To check all of your available locale, you can do the following.

.. code-block:: sh

   locale -a | more

Make sure to pick something that have UTF-8 support, for my case I will be going with en_US.UTF-8.
Set the lang accordingly to what you have picked.

.. code-block:: 

   me:\
    :charset=UTF-8:\
    :lang=en_US.UTF-8:



Done
----

Now save the file. You are ready to try the powerline. Make sure to logout and login back to try
if the .login_conf is correctly loaded as well as your shell's rc file is loaded properly. Here is
a screenshot of my powerline in my FreeBSD's xterm.

.. image:: /images/setting-up-powerline-on-freebsd_0.png
