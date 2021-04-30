.. title: BeeSeiged Postmortem
.. slug: beeseiged-postmortem
.. date: 2016-12-12 14:27:41 UTC+08:00
.. tags: 
.. category: Postmortem
.. link: 
.. description: 
.. type: text

This is the latest game that I have been working on for my school project (GAM 300). We are supposed to make a 3D game
from a custom game engine that we build on our own. We had submitted the first playable in the engine and now I will talk
about how it went for this project. This project is my first time building a 3D game from scratch. As with the previous project
I worked on the tools and editor for the game.

So what went wrong for this project? There are a lot of problems that we faced as a team, firstly our team was too big. In a
typical school project 6-8 people is the optimal however our team size is 10. Not saying that 10 is a lot but we had no prior
experience in working in a big team and we had to coordinate whos working on what. Besides that, because our team is big, the
expectations are naturally higher. Initially we had a very big idea because we thought that it would be possible for a big
team like ours to pull it off however I think we bite off more than we can chew and we had to keep taking away mechanics until
the game was nothing like the original concept.

Another issue with cutting away mechanics is prototyping. When we change the mechanics, we didn't really test out the mechanics
before we implement it. And because we constantly change our game ideas and mechanics, our game becomes a mess. Meanwhile over at
the technical side, we are also facing a lot of issues with the game engine. Our team chose to use the multicore engine provided
to us which was not exactly production ready and have a lot of issues.

We were quite inexperienced with multi core engine and thus we often make mistakes and assumed something and tons of bugs happened
in the engine which leads to a very slow development. Contributing to the slow development is the tools, I think that as a tools
programmer I have to make a good assets pipeline for the designers and developers to use however the pipeline is a mess right now.

Apart from that our game engine currently uses something called the HTN which is a hierarchy task network which is like a behavior
tree but more powerful. This is not a bad thing however I think that it adds unnecessary complexity to our game logic. Initially
the plan was to make the HTN accessible to the designers so that they can use it to create the AI in the game however as of late,
only we are using it. Therefore I think that we can do away with the unnecessary complexity and just code the game logic.

Now, on to the things that went right. Definitely multi core! I feel that multi core is the way to go and we as programmer don't
tend to think of things in parallel. In fact the real world is parallel and so we need to think in terms of multi core. Programming
with a multi core engine has let me to learn a lot.

Our current plans is to discard the original game idea and continue with a new game idea because we want to focus and make something
fun. As for the technical side, we are intending to rewrite the game engine from scratch (hopefully we have time).
