.. title: Trying out Nikola
.. slug: trying-out-nikola
.. date: 2018-05-16 22:01:36 UTC+08:00
.. tags: 
.. category: unknown
.. link: 
.. description: First post on Nikola
.. type: text

This is my first post on my experience with using Nikola_. I have been thinking
about migrating my current blog from Jekyll_ to something Python_ based. This is because I am more familiar with python than Ruby. Since my internship last year, I have been exposed to Python_ and I really like it. 

As for static site generators, I have come across the following,
    - Pelican (Most starred python static gen)
    - Lektor (Have heard before)
    - Nikola (New, I think)

I tried Pelican but I found it lacking in terms of documentations and tools. The build scripts are also rather weird. Pelican uses a makefile to build the site and serve, which makes it harder for me to use on a Windows machine.

Also I wanted to give Lektor a try as well but I ended up going with Nikola instead. I really love the simplicity of the build/serve tool. Also installing theme/plugin is easy. The only trouble I had was running a Jekyll to Nikola post migration tool. Nevertheless, I manually ported my posts, which are in the 20s. 

And I really love the fact that it supports multiple templating engine as well as markdown and restructured text for writing posts. After I finished porting, I came to appreciate the elegance of restructured text. It can be confusing at first. Thankfully, I found this cheatsheet_ which teaches me all the fundamental stuff I need to port my posts.

There is one nice feature that I like that Nikola offers, which is reloading of my browser whenever the page has changed. I think this is a rather neat feature where the pages will just hot reload rather than having to reload manually.

For this blog, I am currently using the Hack_ theme hosted at `Nikola Theme`_.

.. _Hack: https://themes.getnikola.com/v7/hack/
.. _Nikola: https://getnikola.com/
.. _Jekyll: https://jekyllrb.com/
.. _Python: http://python.org/
.. _cheatsheet: https://github.com/ralsina/rst-cheatsheet/blob/master/rst-cheatsheet.rst#id17
.. _Nikola Theme: https://themes.getnikola.com/