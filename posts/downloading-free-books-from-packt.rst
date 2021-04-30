.. title: Downloading free books from Packt
.. slug: downloading-free-books-from-packt
.. date: 2020-07-23 10:08:02 UTC+08:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text

Packt have been having this `Free Learning`_ for a couple of years now. Everyday Packt will release a free ebook to get for 24 hours.
Personally I have accumulated quite a number of free books that way from them which is stored in their library under my account. I thought it
would be interesting to download (most of) them to my local libray so that I can put it into my device to read.

Packt do provide the links to the books that I have downloaded and they have a download button to download the book one by one. I don't
have the luxury of time to download one by one, hence I decided to automate this process.

Some disclaimer before I begin, this method is for downloading the books that are released via Free Learning and owned by you. As seen in 
the picture below.

.. image:: /images/downloading-free-books-from-packt_0.png



Getting Started
---------------

Packt actually supports downloading the resource from their server. From this we know that we can find out if there is an embed url in the 
page or find out if it is calling a REST api to get the files. For this, I decided to use the web browser's built
in network inspector and look at the network tab to see what is happening behind the scenes. I have clicked on the download mobi button and 
from the network tab, there is several calls to the backend. I am only interested in looking the xhr request that the webpage is sending.

.. image:: /images/downloading-free-books-from-packt_1.png

The API of interest here is the following, we can get all the APIs by clicking on all the download buttons.

.. code-block:: html

  [GET] /products-v1/products/1234567890123/files/mobi
  [GET] /products-v1/products/1234567890123/files/epub
  [GET] /products-v1/products/1234567890123/files/pdf
  [GET] /products-v1/products/1234567890123/files/code

Here we see the 1234567890123 which is our product id, (I have obfuscate the product id for privacy purpose). The product id is a unique string
of integers which is tagged to a title.

In the requests to the download API, we can see the usual stuff in a http request header with an additional field named Authorization which 
is a JSON Web Tokens (JWT) used to authenticate ourselves.



Getting the JWT
---------------

Now that we have the APIs to download one individual file, we will need to find a way to get the JWT token. Taking a step back, I went back
to the login page for Packt and look at the network calls made. I found the following call to generate the token,

.. code-block:: html

  [POST] /auth-v1/users/tokens
  {
    "username": "",
    "password": ""
  }

This will give us the Authorization token that we will need for the download APIs. 



Getting the list of files
-------------------------

Once again, with my network tab opened, I navigate around the website and I found out that when I clicked on my owned products, there will be
a REST call to the server to get my owned products. The parameters allow me to fetch up to a certain amount of books.

.. code-block:: html

  [GET] /entitlements-v1/users/me/products?sort=createdAt:DESC&offset=0&limit=10

The result is a json array with the product name, id, metadata etc.



Testing the API with Insomnia
-----------------------------

For most of my discovery, I am using the browser's network inspector tool and Insomnia for testing of the REST APIs. Some of the steps for
testing are,

1. Getting our JWT with the tokens API.
2. Getting the list of the books.
3. Using the JWT from 1. we can randomly use a product id from 2. to test any of the download APIs.

Once I am satisfied with this step, I can move on to the next.



Writing the Python script to get 'em' all
-----------------------------------------

Initially I have written the code using requests library, however I realized that downloading file serially is very slow and hence I written
a second async version to handle bulk downloading. The non-async version supports for downloading individual books while the async version is
for downloading of all the books. The code is rather buggy, I might improve it in the future if there is a need. The code can be found here on
GitHub_.



.. _Free Learning: https://www.packtpub.com/free-learning
.. _GitHub: https://github.com/0xfe11/packt_downloader
