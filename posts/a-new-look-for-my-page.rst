.. title: A new look for my page
.. slug: a-new-look-for-my-page
.. date: 2019-03-27 23:28:54 UTC+08:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text

How this all started was a link that I saw on Hacker News, which happens to be a post on Bulma_. Bulma_ is a css framework based on
flexbox. I thought it looked interesting and hence I took an interest. I decided to (re)build my page using the framework, just to see
how it works. I have long wanted to have my own blog with my own skin. Previously when I was using Jekyll, I was also using an online
theme and I wanted to have my own theme.

Anyhow, I managed to create a simple page with Bulma and some sample page on how the posts looks like and such. I even contemplated on
creating my own static generator for my bulma page. Luckily I did not went ahead with it. I realized that creating a somewhat usable
static generator with all my needs is going to take up much time. And so, I decided to implement my new theme on Nikola.

My first attempt at creating the theme was not so successful, I had try to read the documentation but I failed to understand how to create
a theme in Nikola. I decided to look at other static generators and I decided to give Hugo a try. One thing I like about Hugo is it's
documentations that is built and maintained by the community. It is really useful to look something up. Once I had Hugo set up, I went
ahead and create a new empty theme and proceed to build the necessary parts.

I managed to replicate the theme in Hugo and I went on to create the templates for the posts. This turned out to be tougher than I thought.
The reason being, I was using reStructed text with Nikola and Hugo wasn't detecting my rSt files properly and I try to port my posts to 
markdown format. When I was done with that, I realized that the formatting of the text was not as nice as when generated with Nikola.

At this point, I decided to try creating a Nikola theme again. The second attempt, I tried to read the theme creation tutorial and the theme
reference. I realized that there are many useful commands and information in both documents. Also the idea of the theme inheritance kinda
confused me a little in the beginning but I think what it means is we can copy over the templates and overwrite them any way we like.

I found out that I can call 

.. code-block:: batch

   nikola theme -c base.tmpl

to copy the current template from the parent which is base (by default). I found this from the theme reference_. The page describes the
template that can be copied and modified. This is super useful and I copied a few templates to modify. Initially I wanted to stick to my
original design made in bulma, I decided to go with a simplistic design instead. I like the general feel of the hack theme however there
are a few things that I dislike and I felt like I wanted to try changing them. Thankfully, it was not too hard and I managed to get a
somewhat nice theme out.

I am currently satisfied with my simple theme. I might improve it in the future. Right now I want a blog that is distraction free and easy
on the eyes and I think this theme has achieved that purpose.

Funny how a link on Hacker News prompted me to (re)design my blog and prompted me to (almost create a static generator and) trying out a new
static generator. Not liking it, I decided to come back to Nikola.

.. _Bulma: https://bulma.io
.. _reference: https://getnikola.com/theming.html
