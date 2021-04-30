.. title: How to use git amend to push your blog?
.. slug: how-to-use-git-amend-to-push-your-blog
.. date: 2018-06-04 23:13:03 UTC+08:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text

Recently I learned something new with Git. Yes, it's the amend option that you can append to your commit. What it does is roughly (from the Git docs).

.. code-block:: shell

    $ git reset --soft HEAD^
    $ ... do something else to come up with the right tree ...
    $ git commit -c ORIG_HEAD

So apparently, at my company, whenever we commit our code, we will submit our own branch and create a pull request (pr) from it. And when there is any changes, we will edit our code and amend our commit. This way it looks as if we only committed once. 

Likewise, I also do it on my blog. What I did in the past was stage and commit everything and push. This resulted in many commits, whenever I wanted to add a new blog posts. Ater I have learnt about amend, I will just amend my repository for my blog and do a force push to the origin so that, on the Github website, I will only see one commit.

I hope you enjoy this tip.