.. title: Porting Space Invaders clone from C++ to D
.. slug: porting-space-invaders-clone-from-c++-to-d
.. date: 2018-07-23 16:42:55 UTC+08:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text


Few weeks ago, I came across a series of `blog`_ posts by Nick Tasios. In that post, he wrote a simple Space Invaders with OpenGL and GLFW. 
I thought it was a good exercise for myself to port it into D. and it was a good opportunity to get `Derelict`_ to work. 

Previously I had tried to set it up, 
but the lack of documentation and examples got to me. This time round, I am determined to get it to work and port Space Invaders to D. Just for the fun of it.

Setting up Derelict-GLFW turned out to be easier than imagined. Turned out, I didn't include the dlls for the GLFW for it to work properly the other time.
I also managed to find some code to initialize the GLFW as well as getting it to run.

Once I got GLFW to run, the rest was porting the code from the tutorial to D. I kept the looks and feel similar of the original tutorial. Overall, building a simple game like
Space Invaders is a good exercise to learn about D. Anyhow, I have finished porting the tutorial from 1 to 5, however in the near future, I hope to refactor the code somemore
to make it more beginner friendly.

You can check out my code `here`_.

.. _blog: http://nicktasios.nl/posts/space-invaders-from-scratch-part-1.html
.. _Derelict: https://derelictorg.github.io/
.. _here: https://github.com/zgoh/d_space_invaders