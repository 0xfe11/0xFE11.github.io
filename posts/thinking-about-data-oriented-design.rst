.. title: Thinking about data oriented design
.. slug: thinking-about-data-oriented-design
.. date: 2016-01-16 14:01:00 UTC+08:00
.. tags: 
.. category: data oriented
.. link: 
.. description: 
.. type: text

I have been programming with the object oriented for a long time and I came across data oriented design and so I thought I would give it a try. This is my attempt at trying to create a system that is data oriented.

I would start with a simple example of how I would apply this to my game engine's transform system. Entity have position, scale, rotation information and instead of creating an array of the Entity structure with all the properties which is not cache friendly, we also cannot write an algorithm that makes use of the cache efficiently. Why is this so?

Imagine we have an array of Entity structure, in our memory layout all our information that we want to access are all surrounded by unused data due to other data that are present in the structure but not needed for the algorithm.

Therefore the proposed method of doing things is separating the data into an array of positions, an array of rotations and an array of scales. We then write our code to access all the positions/scale/rotation at once, this saves the number of cache miss since we need to access the data that are only needed by the algorithm.

There are lots of good website with good explanation for data oriented design. This is my first attempt to try to implement it into my game engine. I will try to post more when I learn more.

