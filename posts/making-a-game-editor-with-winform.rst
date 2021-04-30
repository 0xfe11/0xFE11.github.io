.. title: Making a game editor with WinForm
.. slug: making-a-game-editor-with-winform
.. date: 2016-01-12 04:03:00 UTC+08:00
.. tags: 
.. category: unknown
.. link: 
.. description: 
.. type: text

One of the many things that I feel that is very important in making a game engine is making the editor together with the engine. The editor should be a tool that is done early to aid the game designers. The engine tools is to support the game but the editor is to support the creation of the game which I think is very important.

So far I have seen various tools that are used to build the editor, ranging from in-game editor to external editors. In this post, I will mainly be talking about the external editors which can be build with libraries like the wxWidgets, Qt, WinForms or really any windows toolkit. In this post, I will talk about how to build an editor in WinForms(C#) and glue it to your engine code(C++) using some easy way. The reason I chose WinForms is because of the environment that I will be working on will be Windows mostly and it is included in Visual Studio by default, so no rebuilding of libraries and stuff.

Initially when I was looking for ways to make the editor in WinForms, I can only find a few tutorials on how to do this. There are a few good tutorials but I do not understand what was going on but after I setup myself, I got a pretty good understanding of what's going on. (If you want to know how to setup, read the next part.) We know that our WinForms application will be in C#, so how do we get it to run code from our engine? The answer is using managed C++, because C# is a (.NET) managed library and the CLR C++ is also a (.NET) library. We can create an interface layer in CLR C++ and include that library that we build into the editor. This was the editor can call our engine function via the layer and the layer is communicating with the engine directly (because both are C++).

To setup the project, we will need Visual Studio (2015 is the one that I use but any version from 2010 should suffice.). Firstly, create a new (Visual C# Windows Forms Application). Next we need the C++ interface layer for our editor, add a new (Visual C++ CLR Class Library) project to the solution. Now we need to add the reference to our C# project so that we can access the C++ interface. Build the whole solution and make sure that a dll file is generated in the folder and then selecting the C# project, go to the References  and right click to add references, make sure to add the correct dll file in the build folder.

Next, we need 2 more projects, one contains the WinMain for starting up the game engine without the editor and the other project for storing only the game engine which can be started with either with main or through the editor. Let's create the project for the WinMain, for this we need to add a (Visual C++ Empty Project). Lastly, we add another (Visual C++ Win32 Application Static Library) project.

The most important thing to note when building is, always build the static library first because this is needed by all the other 3 projects. We can set the build dependencies by right clicking the project and setting it (Build Dependencies Project Dependencies). Make sure all the other 3 projects depends on the static lib which will be your engine code, and additional note for your editor project, it will need to depend on your C++ interface project as well, so add that to the dependency.

After setting the build order, make sure the project can be build without any errors, and we can move on to set the include folders for the interface and WinMain project because they will need to use the headers inside of the static lib.

Now build the project again, make sure that it builds cleanly and fix any errors. Now that we get the project working, we need to think of a way to call our engine from the editor. That's why we have to expose our engine functionality to the interface project which will be included by our editor as an assembly and we can call it from there.

To see how to setup the engine in your editor, you can download the sample project and take a look at how it is being setup.

There you have it, this will form the base for your game editor and engine. Sorry I did not include the codes to for the editor and the interface layer, to have a look below for the download link for the project or take a look at the extra links below.

Thanks to these tutorials which helped me through with the setup of the projects.


`Hosting a D3D in WinForm`__.

.. _Winforms: http://www.gamedev.net/page/resources/_/technical/directx-and-xna/hosting-a-c-d3d-engine-in-c-winforms-r2526

__ Winforms_

`Help using directx with winforms/wpf`__.

.. _WPF: http://www.gamedev.net/page/resources/_/technical/directx-and-xna/hosting-a-c-d3d-engine-in-c-winforms-r2526

__ WPF_

`Download link for project`__.

.. _Download: http://www.gamedev.net/page/resources/_/technical/directx-and-xna/hosting-a-c-d3d-engine-in-c-winforms-r2526

__ Download_