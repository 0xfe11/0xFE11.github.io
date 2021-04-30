.. title: Make for small projects
.. slug: make-for-small-projects
.. date: 2018-10-06 18:23:06 UTC+08:00
.. tags:
.. category:
.. link:
.. description:
.. type: text

Some back story, I learned of make and makefile in my first year of my University programme. I didn't really like and understand what I was doing.

I went on and picked up other kind of make/build system. Until recently, I found it quite useful to have. Here I hope to demystify a make file and hopefully you can use for your other projects.

Honestly I know the title says make for small projects, it does not represent my view that make is for small projects, because make have certainly been used for large projects. I found make easy to use for small projects.

To start with a simple makefile, create a file named makefile and save this inside:

.. code-block:: makefile

    all:

With that, you have already created your makefile. You can now type make and your terminal should say:

.. code-block:: shell

    make: Nothing to be done for 'all'.

We see that make have finished with the message that there is nothing to do for 'all'. 'all' is a rule in this case. Make works by scanning the makefile and finding all the rules. Rules are basically a work with a colon in front. You can change the all to any word you want.

What can we do with a rule, you may ask. A rule can depend on other rule and also execute certain action for each rule.

For example, we can have a simple make file that builds a C source file and run it.

.. code-block:: makefile

    all: build
      ./main

    build:
      gcc main.c -o main

In this example, we have two rules, all and build. When you run make, it first sees the rule all, and it found out that it has a dependecy of build rule. It will then see if build has any dependecy. In this case build does not rely on any other thing as well, it will execute the gcc command. When that is done, all will move on to the next dependecy, until there is no more dependecy and execute its action which will run main.

There is much more to make file than what I described here. I like to use make because it allows me to define my own set of rules and actions for example currently I am using it with nimble, a nim build and package manager. Nimble allows me to build but not run the application therefore I have a makefile to invoke nimble build and another rule to run the application.

I really think it's useful than writing shell scripts/batch file. Make more today.
