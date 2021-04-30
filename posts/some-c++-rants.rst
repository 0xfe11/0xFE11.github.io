.. title: Some C++ Rants
.. slug: some-c++-rants
.. date: 2018-04-11 17:56:27 UTC+08:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text

Hello, World! Lately I have been rather busy with work and stuff. I am currently working as an intern in an enterprise software company. It is a relatively small team with only three of us right now. 
As you can imagine, the code base is rather new at about three years I think. Not counting the libraries that we used internally. Any C/C++ codebase that are large enough will be facing these issues, mainly 
build, debugging tools. 

Firstly, my experience with the build tool is not pleasant, the issue I face right now is using an old (and patched version by my company, yes) version of this open watcom build tool. Honestly this is the first
time I have ever hear of it and the build is so unfriendly and little documentation. Currently the team is currently adding CMake support to the project which will hopefully solve this issue as well as the next issue
I wanted to talk about, the debugging tool. 

Right now the team is using open watcom to build the project, we do not use Visual Studio to build and debug the project which is something I really miss. Honestly I think Visual Studio
has some of the better debugging facility around. Also Visual Studio have better code intellisense. Currently I am using VSCode and it just doesn't feel as good as having Visual Studio with me.

I agree that C/C++ is a powerful language, however to wield such power, one would need to have powerful tool at his disposal and right now, I feel like I am not using the right tool for the job.
