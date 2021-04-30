.. title: Setting default startup path in Cmder
.. slug: setting-default-startup-path-in-cmder
.. date: 2019-05-01 11:21:43 UTC+08:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text

Recently I have been migrating more of my applications from being portable installations to
being managed by scoop_. I find that using scoop saves me a lot of time when I want to
update the versions of my applications. So I went and remove my portable cmder installation
and setup cmder again using scoop. 

As usual, first thing I usually run 

.. code-block:: batch

   cmder.exe /registerall

to add cmder to my right click context menu. After which, I will pin cmder to the task bar.
Now I wanted to make cmder start in a specified path everytime I open cmder from the task 
bar and I wanted cmder to open in the correct path when I click open cmder from my right
click context menu.

What I did was add the following to the target of the taskbar icon.

.. code-block:: batch

   /start C:\

which will make cmder start in C:\ by default. Hope that is useful for reference.


.. _scoop: https://scoop.sh/
