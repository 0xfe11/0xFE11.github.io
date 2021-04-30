.. title: Memory bugs in The Last Defender
.. slug: memory-bugs-in-the-last-defender
.. date: 2017-04-15 13:00:29 UTC+08:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text

In this post, I will be talking about the different types of bugs that we faced when we were developing
for our new game The Last Defender. The Last Defender is a first person/tower defense hybrid game where
player controls a drone and build towers around a base to prevent enemies from destroying the points of interest.

Usually in games, there are bugs and depending on the severity of the bugs, we will decide to fix it or not. Currently
the game is not completed yet. We are only at the engine building stage of the game. I will talk about the kind of bugs that
we encountered throughout the development cycle. Before we talk about the kinds of bugs that we faced, I would like to share
a little about our engine architecture (this would be useful as to why we encountered the types of bugs that we did).

Our engine is structured in a component based architecture, at the bottom of the layer we have a system layer which have the
platform dependent code and our engine state management build on top of that. The bugs that we faced here are usually logic error
where we had to deal with window losing/gaining focus and pausing properly when we do that. On top of that, our engine is running a
state machine internally. The different states are scene changing state, playing, paused state and loading. Our engine will listen
for messages related to this different state change and do the corresponding update accordingly. This engine portion is actually
what we often refer to as the game state manager. On top of the state managers, we have all the managers(systems) for the different
components. For our engine, we have 2 types of components. The system level components and scripts. System level components
comprises of transform, graphics, text, sound and so on. While scripts are game logic components and there all reside under the
logic system.

Our system level managers are responsible for managing the components within them. When we update the systems, we are updating
all the components at once. Initially, our systems all held a vector of components inside and we will update through that vector.
This could work for games where items are cleared and recreated every scene and objects are not removed at any point throughout the
gameplay. We had thought that if we were to be using just vectors to manage and store our items, we will eventually run into some
issue when our game becomes bigger. Therefore one of my classmate went ahead and created the handles for our objects. Our biggest
issue with using pointers is that if the item is removed but still pointing to the vector, the pointer will still be valid.
Therefore we tried to eliminate that kind of issue.

Recently we were having weird issues with our game, it will randomly crash at places where we have no idea why. We suspect it was a
memory bug and memory bugs are hard to fix. Our game were running in release mode in visual studio and that enabled our game to run
at 60fps however we could not debug the underlying symbols. It turns out that, it was the problem with our handles, don't get me wrong, our handles were working properly up until now that was. For the past few days, we kept adding content to our game that something broke our game and we had to figure out what. It turns out to be the amount of memory allocated for our handles was less than the amount of objects in our scene. That took us about two hours or so to fix. Memory bugs are easier to debug when using an memory allocator.

Another memory bug that we ran into was with the physics manager. The physics manager was written by me with reference to Randy
Gaul's `Impulse Engine`_. The physics engine was experiencing some weird behavior, some of which was when we were spawning bullets,
the physics seems to have issues with the collision and movement. When we shoot bullets, some of the bullets seems to move in the
wrong direction. Initially I thought it was because of the way I had architect the manager because I was trying a new approach of
laying out my data. More on that in a separate post. Turns out, the collision info was corrupted because I was using a pointer for
the colliders and I had tried to have my own pooling using a simple array and a swap-with-back strategy. Turn out, it was reusing
the colliders for some of the physics components and thus causing the physics computation to be wrong. Since then I had fixed that
to use a proper object pool implementation.

In the next post, I will talk about other kinds of bugs that we had to fix in game.

.. _Impulse Engine: https://github.com/RandyGaul/ImpulseEngine

