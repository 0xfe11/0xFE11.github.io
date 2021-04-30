.. title: Mono Embedding Crashes on Windows
.. slug: mono-embedding-crashes-on-windows
.. date: 2016-06-12 15:38:22 UTC+08:00
.. tags: 
.. category: unknown
.. link: 
.. description: 
.. type: text

When I was looking at the documentation on how to integrate mono into our c++ application, I was running into an issue where the application crashed every time I try to initialize mono. A simple search on the internet did not return any immediate results and I went to look for tutorials on how to setup a project in visual studio 2015 and mono. I thought it was my vs settings.

After looking at some of the sites, I came across `this embed sample`_ which have a simple visual studio project. After some inspection, I realized that the problem with the tutorial on mono-projects did not mention about setting the directory of your mono binary/library.

Finally I just did what the embed sample did and make a copy of mono into my project directory and call **mono_set_dirs**.

TL;DR if **mono_jit_init** crashes, be sure to call **mono_set_dirs** before and have mono in your project directory.

.. _this embed sample: https://github.com/PoppermostProductions/Embedded-Mono-Sample/
