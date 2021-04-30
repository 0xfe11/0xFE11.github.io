.. title: State of the Build 1
.. slug: state-of-the-build-1
.. date: 2018-04-14 11:23:45 UTC+08:00
.. tags: 
.. category: unknown
.. link: 
.. description: 
.. type: text

Build system is on the of most important tool for building softwares. In the past, I have always taken these tool for granted. Coming
from a Visual Studio and Windows background, I must say that I have no idea how important a build system is. On Visual Studio, one just set
up the project and it will build as long as the project matches the IDE version. This can be quite useful when you want to setup the 
project and immediately jump into building the project. 

Though all this sounds good, it can be quite a pain especially if using an older version of Visual Studio with a newer version of the project. Another
pain point would be using the same project in a linux environment. This can be quite annoying if your project only supports Windows. Hence my quest to
find a good build system for my projects. Note: Currently I am only using C++ for my projects, other language may have their own fancy build systems.

My requirements for a build system is simple and must fulfill the following,
1. Be cross platform (thinking of building some cross platform fanciful app).
2. Easy to learn, DUH
3. Fast, DUH

Currently I have chosen to test Meson_ build system because it is advertised to be fast and easy to learn. Let's see about that. This is part 1 of my
State of the Build series where I explain my need for a build system. In the next post, I will explore Meson and the good, bad, ugly.

.. _Meson: http://mesonbuild.com/