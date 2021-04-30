.. title: How Roll Engine was made?
.. slug: how-roll-engine-was-made
.. date: 2016-03-25 05:27:00 UTC+08:00
.. tags: 
.. category: unknown
.. link: 
.. description: 
.. type: text

Hey everyone, today I am going to talk about how our game engine aka Roll Engine, (RE for short) is made. and I am going to share about some of the technical problems I have faced when programming a game engine from scratch. Although this is not a commercial engine, I wanted to share some errors that I made and give you tips and how to make a better engine (hopefully) in the future.

I will share about my background experiences in game engine building before I delve into talking about RE.

The first time I learned about engines is from flash, mainly FlashPunk_, then I moved on to learn Haxe_ and (sort of) develop my own engine, ZE2D_ for making simple 2D games. From then on, I learned about main game loops and the different types of main game loops, I recommend reading Koen Witters's blog post about main `game loop`_. It wasn't until when I started to develop my own game engine from scratch (in c++) that I had to find out how to write a proper game loop.

Before I started writing my own game engine from scratch, I had worked on 2 games that require me to code in C++, that was my first exposure to writing games in C++. The first project, I worked on the game play logic mostly and in the second project, we were given a graphics engine and I worked on getting up a working structure for the game engine. Here I learned how a simple engine can be constructed with the handling of game state and entities.

After the first two projects, we had to build our own game engine which I named it RE. Before I started to build RE, I looked around for resources on how to make games with C++ and I managed to stumble onto this wonder video series, `Handmade Hero`_ by `Casey Muratori`_. At that point of time, he just started the series and I tried to follow his series especially the beginning where he demonstrated how to build the initial systems for the game. That is how I got into the world of Win32 programming and I started to do my own side project. The project that I did was having a simple WinMain function and a very simple game loop along with simple input using RawInput, on top of that I tried to do a simple editor like window for the application. The result was a windowed application with a toolbar and basic input and simple DirectX rendering (without sprites yet).

This led me to my second attempt at building a game engine from scratch again, this time round, I tried using the wxWidgets_ because I have seen a senior team using it for their game project and so I wanted to do the same thing as well. This second attempt is mostly about getting wxWidgets to work on top of the base that I had previously built. WxWidgets is quite troublesome to set up because of the large source files and I couldn't find a proper way to build the code from scratch initially, but I managed to dig through the readme files and found a way to get it to build. That roughly marks the start of how RE was born.

So far, the major problems I face was incorporating an editor into the engine, building the core systems for the engine and how to incorporate the logic components into the game engine and which graphics rendering api to adopt.

Note: The decisions that I made here is only valid to me and my team. Whatever I said here might be bias or incorrect but it works for my team, so do take into considerations when making a sound decision.

The decisions I made was to stick with wxWidgets rather than the Win32 api or Qt (or the 101 windows toolkit out there) because I had tried to use wx earlier and I seen how powerful the editor that my seniors have made. The second decision that I made to the team was to use Direct X 11, similarly this was also the decision that my senior had made and some seniors told us that DirectX is easier to use than OpenGL.

A few weeks into development and I had got the editor to work partially with the necessary windows however there is one burning question that I always asked myself, how do I do reflection/serialization with my game objects? I went on to the internet and I tried to follow a few tutorials and pages but I didn't find much useful article except for one by `Randy Gaul`_, his article on metadata reflection and serialization. I tried to use his codes as a base to build my own solutions but I ended up not using it because mine was quite complicated to use. End up, I decided to rewrite my reflection system and use the visitor pattern to implement a simpler version of a `reflection system`_ by `Sean Middleditch`_.

Meanwhile, I am still getting the editor up and running, there are a few teams which are interested in building an external editor like me, they also started Qt/wx. However as I progressed, I realized that building the external editor is going to take up lots of my development time because I have still been struggling to build the basic game engine structure, so the next technical decision that I made was to throw away the external editor and turn towards an in game editor solution. There are two libraries that I know of that can do this, the first one being `dear imgui`_, and the second one was AntTweakBar_. I have briefly looked at the imgui's (Immediate mode GUI) way of displaying UI and I took a liking to how easy it was to display UI, though I think AntTweakBar might do the same, I did not look at the sample code, so I went along with dear imgui.

Throwing the external editor out, I now focus on building the in game editor for the game engine while my team build the physics, graphics, logic systems. Here, my team member proposed to me about having handles for game objects so that we will not be dealing with the pointers and I agreed. I have seen that handles can be useful from this article by `Noel Llopis`_. Most of the object creation are done by one of my team mates while I focused on building the reflection/serialization systems.

In every engine, there is always a need for message passing and there are a few ways that we can do it. I did not want the tean to implement a complicated solution and end up not using it, so I made a clear decision here that our messaging system will be simple bubbling up messaging where all the systems will broadcast to the core (root) and then broadcast downwards to all systems again. This kind of messaging works for most of our use cases where we have things like window resizing and moving and we need to notify the graphics system that this is happening. We do not use this for our logic components.

Components is also another thing that was on my mind constantly when I started to build our engine, because I do not know how to store all this components and if so, how do these components interact with one another. The choice that I made was to have different systems for each type of components, e.g. transform systems will have an array of transform components and is responsible of managing them. So I have a graphics, transform, physics, collision and logic system with their own components. The logic system is a little special because it holds different subsystems or factories for different logic 'scripts' which are just .cpp files and then manages them.

Back to the editor, it is just an object with all the functionality inside, so the editor sits on top of the scene management system and can be removed from the engine without affecting the game engine at all.

Basically that's most of the things that we have done for RE. Be sure to keep a lookout for the game Basket Brawl. That's the name of the game that we are developing at DigiPen. Stay tuned for more "How I Make Games"!

.. _FlashPunk: http://useflashpunk.net/
.. _Haxe: https://haxe.org/
.. _wxWidgets: https://wxwidgets.org/
.. _Handmade Hero: https://handmadehero.org/
.. _ZE2D: https://github.com/zine92/ZE2D
.. _game loop: https://www.koonsolo.com/news/dewitters-gameloop/
.. _Casey Muratori: https://mollyrocket.com/casey/about.html
.. _Randy Gaul: http://www.randygaul.net/2012/10/01/c-reflection-type-metadata-introduction
.. _reflection system: http://gamedev.stackexchange.com/questions/76257/c-property-system-interface-for-game-editors-reflection-system
.. _Sean Middleditch: http://gamedev.stackexchange.com/users/7697/sean-middleditch
.. _dear imgui: https://github.com/ocornut/imgui
.. _AntTweakBar: http://anttweakbar.sourceforge.net
.. _Noel Llopis: http://gamesfromwithin.com/managing-data-relationships