.. title: Level creation workflow
.. slug: level-creation-workflow
.. date: 2016-11-30 05:18:20 UTC+08:00
.. tags: 
.. category: unknown
.. link: 
.. description: 
.. type: text

It's been some time since I discussed about my game. Currently we are developing a third person action/stealth kind of game with multiple characters and mechanics. In this project, I am in charge of the tools/editor. Making a 3D level is much harder than making a 2D level because 3D have much more items and it is important to have an user friendly editor to place the props and levels.

In the beginning, we had to manually place the levels piece by piece into the game using the tools that I make, however for larger levels, it starts to become a pain to use because certain features was bugged and it was hard to place the items without snapping and cloning. I know we had to use another way to build our levels.

That's when we learn about how we can build our levels in 3ds max and export them out into the game. What I am referring here is not about exporting the level as one mesh and import into the game. We had thought of doing that at the start but that was not a feasible idea because we would have to build/generate our collision data somehow from the level meshes. We had to scrap that idea and we came across a better idea.

Instead of exporting the whole level as a mesh, we will export the whole levels as just transform data. This is to say, we would export the position, rotation and scaling into a file and that file will be read by our editor to reproduce the levels. Of course all of this isn't automatic, it requires the designer to create the prefabs that are needed by the level before hand. The xml file will contain the name of the prefab that will be instantiated and the data as to where to place them.

This method is slow to begin because I had to create all the prefabs for the items that will be used in the beginning. However subsequently all I need to do is export the level and reimport the data provided by Max to rebuild the levels.

Though this method is good, it has some things that the designer/artist need to take note while putting the items. Our game uses a right handed coordinate system similar to opengl similar to Maya but Max uses a different system. First thing to note is the Max to our engine coordinate system. What we did was rotate the models by -90 about the x axis. This will ensure the model will be in the correct coordinate. Next we need to apply a transformation to the coordinates when we export the level as xml so that the the coordinate will be correct when reading into the editor.

