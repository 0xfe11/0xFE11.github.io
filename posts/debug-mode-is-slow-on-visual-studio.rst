.. title: Debug mode is slow on Visual Studio
.. slug: debug-mode-is-slow-on-visual-studio
.. date: 2017-04-15 15:12:59 UTC+08:00
.. tags: 
.. category: unknown
.. link: 
.. description: 
.. type: text

Previously I had talked about fixing memory bugs with Visual Studio. Running our game in Debug mode in Visual Studio 2015 is utterly slow. Our game runs about ~10 FPS in debug mode. Why is it so slow, you may ask, well there are numerous reason for the that kind of speed and what kind of operations that you are doing. For one, Debug mode will run assert code and it will check iterators which makes it slow especially when using the STL. I am still looking for a viable alternative for STL for game dev. I think that the STL might be good for general purpose programming but it definitely is not suitable for game dev.

This post is not to discuss the STL, maybe I will leave that to a future post. I was experimenting and getting EASTL to work but I had no such luck. Today I will mention something that I came across a few days ago, [Microsoft]

Thankfully there was this article by Microsoft_ that describes how to debug a release build which is very useful for us. Apart from that there is the checked iterator business that I mentioned which might make it faster. After I followed the guide, my FastDebug(what I named my new mode) build ran about the same speed as my release build but with the added functionality of being able to debug it.

Hopefully someone might find this useful for their Visual Studio debugging.

.. _Microsoft: https://msdn.microsoft.com/en-us/library/fsk896zz.aspx
