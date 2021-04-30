.. title: 3D Buzz is closing down
.. slug: 3dbuzz-is-closing-down
.. date: 2020-01-12 11:35:58 UTC+08:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text

A few days ago on `Hacker News`_, I came across a post that says `3D Buzz`_ is shutting down and releasing all their contents for free. This 
was a website that was quite popular for 3D tutorials and stuff. 

This is how it looks like when I visited, note: downloads are temporarily disabled as of current writing. 

.. image:: /images/3dbuzz-is-closing-down_0.png

There are a list of courses released all available for download, so I decided this is a good exercise to write a simple scraper to download
the resources. I wrote a simple python script that use `Beautiful Soup`_ and Requests_ that will download the html page of 3D Buzz and parse 
the HTML. After which it will attempt to download the contents. You can check out the project here_.

[Update:] 3D Buzz have instead released a torrent file which is in my opinion better for such distribution.


.. _Hacker News: https://news.ycombinator.com/item?id=21982283
.. _3D Buzz: https://www.3dbuzz.com/
.. _Beautiful Soup: https://www.crummy.com/software/BeautifulSoup/
.. _Requests: https://2.python-requests.org/en/master/
.. _here: https://github.com/0xfe11/-3dbuzz_content_scraper
