.. title: Getting the correct voucher code for my online shopping
.. slug: getting-the-correct-voucher-code-for-my-online-shopping
.. date: 2020-03-28 14:39:08 UTC+08:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text

There is a online shopping platform that I always use, and just yesterday I came across an ad that caught my eye. The platform is having a 
lucky/lottery draw of some kind. The platform released a few items for grab along with a partly revealed coupon code that shoppers have to 
guess to get the item for free. 

An example of this was a certain bag with the voucher code 'MYBAG' was revealed and shoppers have to guess between '0000' to '9999' where the 
winning entry could be 'MYBAG26214'. On the surface this looks like a lottery ticket where shoppers can input any random numbers into the 
voucher cart redeemtion and see if they get the items discounted fully.

The bright side of this so called lottery is that, there is no cost to make before making a guess. Anybody can make any amount of guess within
the timeframe. The fastest person to guess the code will get the items. So this is a fastest winner wins competition.

Being a programmer at heart, I thought this was fair game and I do not think the winner(s) will be manually typing in the voucher code to guess.
Assuming it takes about 5 seconds to check one code, manually going at it will require 13 hours to crack it. Also randomly going at it will
also need a long time to break the code.

Knowing this, I sat myself down and thought about cracking the voucher codes. I was using the ecommerce platform on my mobile phone initially
and so I went online and start researching how to capture the network api requests made to the server so that I can start to write a script
that automate the sending of my voucher code to the platform to check.

I admit I spent quite some time stuck at this point, and this is because I wasn't able to use intercept my app and the server to check the 
network calls that I want to analyze. After some time spent here, I realized how foolish I was going on about here. I realized the ecommerce
platform not only have a mobile platform, it also offers a web version.

I was more familar with analyzing network calls from the web browser as compared to analyzing the mobile app. I launched my browser network
inspector tool and take a look at the request the page is making to the server to check the voucher. I found the API and tested in Insomnia
rest framework to trim down the request further.

Next I proceeded to write a simple script which sends the voucher code to the server to verify. The first version of the script is really slow,
it sends one requests at a time to the server and this takes a long time. I have actually tried running the script and it takes a long time. I
actually tried running the script and after 1 hour, it was still making request and it was not even halfway there.

It was at this point of time, I thought about threading my script to make it faster, though I have heard that threading is really a mess in
Python. I did some research and realized that there was a asynchronous programming model in python. I did some research and found out that
there is a few versions of async before it was finalize in Python 3.7. 

I went ahead and rewrite my first version and make it an async version, mainly using aiohttp for calling the http request instead of using 
request library. This time when I ran the async version, it finished in about 5 minutes which is in magnitudes faster. When I found out the
correct voucher code, I was so excited. I tried to key in the voucher code, and I got the dreaded message that the voucher code have been 
claimed. This only mean one thing, someone was faster than me and got to the code faster than me.

To conclude, synchronous programming is easier to understand but causes a lot of busy waiting for requests to finish or file to load whereas
asynchronous code let other request run in the background while it does other thing which makes it utilize the CPU as much as possible. 
Although, I didn't get to get anything, I learned about async programming in Python.





















