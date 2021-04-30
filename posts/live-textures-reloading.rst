.. title: Live Textures Reloading
.. slug: live-textures-reloading
.. date: 2016-03-28 01:55:21 UTC+08:00
.. tags: 
.. category: unknown
.. link: 
.. description: 
.. type: text

While I was drawing for my game, BasketBrawl, I would always draw, export the art, (close the game if it's opened and) start the game and repeat this whole work flow. Notice how I need to constantly close and restart my game just to see a particular image in my game and I find this process too tedious that I thought to myself, why not make the game reload the textures when it needs?

Thus, I came out with a solution for my game engine, though this can be expanded to reloading of other assets as well but for my use case, I only extend it for reloading of images file.

Initially what I thought of was to keep track of list of all files and it's last modified dates and use it to compare, however after some research, I came across ReadDirectoryChangesW api that Windows supplied. There is `one page`_ that gives a detailed explanation on how to use ReadDirectoryChangesW and he has code samples as well.

To use it in my game engine, I put the code in my main file where all my windows platform codes are. There is certain things that one must take note when using this api, because we cannot just run this function on our main thread because this will block the main thread and that is not what we want. The method that I used is just using regular c++11 thread_ and run it there in my WinMain function. When the thread receives a file modification event, the file watcher will be called and it will notify my game engine to unload and reload the textures on the fly.

I have also use the file watcher to monitor file adding/removing because whenever we add or remove a texture, currently I need to manually update all the textures by running a batch file inside the textures folder, so what I did was when there is a file adding/removal event, I will use ShellExecute_ function to run my batch file and then reload the textures.

Implementing live reload is actually very simple but do take note of some issues when live reloading, mainly my game crashes a few times because of textures loading issues because we are using the DirectXTK's texture loader and it crashes a few time. So I think it would be better to pause the game engine while doing the updating and resume the game engine when all necessary updating is done.

This can easily extend to reloading of audio or scripts as well.

.. _one page: https://developersarea.wordpress.com/2014/09/26/win32-file-watcher-api-to-monitor-directory-changes/
.. _thread: http://www.cplusplus.com/reference/thread/thread/
.. _ShellExecute: https://msdn.microsoft.com/en-us/library/windows/desktop/bb762153%28v=vs.85%29.aspx