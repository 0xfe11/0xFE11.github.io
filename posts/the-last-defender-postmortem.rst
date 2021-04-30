.. title: The Last Defender PostMortem
.. slug: the-last-defender-postmortem
.. date: 2017-08-12 07:52:25 UTC+08:00
.. tags: 
.. category: Postmortem
.. link: 
.. description: 
.. type: text

This is actually my second 3D game that I have helped to develop. Now that the game is done, I wanted to quickly write down my thoughts
on what went wrong and areas of improvement as well as the things that went smoothly for the project. Following that I would also like
to share my future plans.

What went wrong?
================

1. Lack of communications and vision.
-------------------------------------

The main issue that I had when developing for The Last Defender (TLD) is that we do not have a clear vision of what we want to achieve. Most
of the time, we do not know what we want and we merely implement the stuff based on what we feel is needed for the game. This in turn made the
game went in all sorts of directions. 

2. Poor engine architecture.
----------------------------

A lot of our development time were spent on fixing bugs. I believed that were having memory bugs because it would caused random crash from time to time.
I think that this late into the development, we should be spending more time churning out newer contents and rather than that, most of our time were spent 
fixing engine bugs which caused our gameplay logic to be very fragile. It crashed really easily and sometimes we had no idea where was causing to crash.

One such area where we believed were having issues were the tween system. The tween system initially keeps reference to objects, meaning that when the items
dies, it will still be holding the reference to these items and potentially modiyfing memory. The fix was to recreate the tween front end to take in handles
instead of the reference to the objects. These took a little while to get used to and eventually, all of the system was using it. This fixed some of our
issues that we were having.

3. Losing motivations.
----------------------

Several times during the span of the project, I was not really motivated to continue the project. I would rather focus on my other assignments than work
on the game. Reason because that I feel that this was not a game that I would enjoy playing much. This made me realized that it is tough to work as a game
developer if I cannot enjoy the game. I think I still want to make games, but perhaps not for a big studio, I want to make something that I enjoy making.

Apart from me, I can tell some of my team mates have like really low morale when it comes to the game. We have weekly meetings and standups but I feel that
rather than serving its purpose of informing what others are doing, I feel that it is more of a chore. I think it might be because of the way it is organized.
I think team meetings is where team mates come together and show each other what they have done. Rather than talking about it, it's better to show others 
what you have been working on.

That being said, I am going to try my hands on being an app developer for a while. Did I mention, I currently found an internship position at an NUS 
lab/startup. They are making apps that will benefit people and I want to try my hands on working on apps rather than games for a little while.

What went right?
================

1. Able to make assets even without artists.
--------------------------------------------

Most important thing in games is always about creating assets. Unfortunately for us, our artists left during the start of the semester and therefore we
had to rely on our own to create our own 2D/3D art assets. Additionally, we also had an additional external member help with the sound design. 

2. Juicy particles and feedbacks.
---------------------------------

Our particle system is quite powerful, thanks to our graphics engineer. Thanks to the tool, our gameplay programmer was able to add lots of feedbacks and
particles to the game. To the point where it's too much. I think that is good in any game. Feedback and adding juice to the gameplay is equally important
to the gameplay experience. And here I would like to thank our gameplay programmers for making the game so juicy.

Conclusion
==========

This is my first 3D game (without commercial engine) that I can proudly say that I completed. Though the game could have been better, I am glad that we 
did what we can and finished up the game. The whole journey is not wasted, we can always learn something from this current projects. I hope that with the
current experience and knowledge, we would not make the same kind of mistakes again, in regards to engineering to game design decisions. With that, I hope
that everybody will enjoy the game. You can check out the game at the DigiPen games gallery and I will be posting a trailer and pictures soon. Stay tuned.

