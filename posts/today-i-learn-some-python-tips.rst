.. title: Today I learn some Python tips!
.. slug: today-i-learn-some-python-tips
.. date: 2020-07-26 22:37:00 UTC+08:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text

I came across a YouTube_ video in April that showed some of the (not so common) features and tips and tips.

1. Hello World
--------------

The following code will print hello world. How apt to start.

.. code-block:: python

  def hello_fn():
    import __hello__

   hello_fn()



2. Zen of Python
----------------

This is actually an easter egg, calling zen_fn() will print a zen message.

.. code-block:: python
    
  ## Zen of python
  def zen_fn():
    import this

  # Will print a zen message
  zen_fn()



3. Python Enums
---------------

This is how you can emulate enums in python,

.. code-block:: python

  def enum_fn():
    class Enum:
      Red, Green, Blue = range(3)
      a, b, c = range(1, 4)
	
    print(Enum.Green)
    print(Enum.Red == 0)
    print(Enum.c)

  enum_fn()



4. Multiple assignment
----------------------

In this example, you can assign multiple values to multiple values as you like, you can also do so for a list or tuple.

.. code-block:: python

  def mult_assignment_fn():
    x, y = 1, 4
    print(x, y)
    
    # Using this to decouple the list
    x, y, z = [4, 5, 6]
    print(x, y, z)

    # Likewise for tuple
    x, y, z = (4, 5, 6)
    print(x, y, z)

  mult_assignment_fn()



5. Format strings
-----------------

This is actually quite useful and I have been using it quite a lot. Basically, you add a f in front of your string and inside, you can use
the variable name directly.

.. code-block:: python

  def fstring_fn():
    age = 10

    # Print only 10
    print(f"{age}")

    # Print the result of evaluation age + 10
    print(f"{age + 10}")

    # Same as above
    print(F"{age + 10}")

  fstring_fn()



6. Enumerate function
---------------------

Enumerate function can be useful if you want to do something like iterate through the entire list and printing the items in the list.

.. code-block:: python

  def enumerate_fn():
    x = [2,3,4,5]

    # not using enumerate
    for i in range(len(x)):
      print(i, x[i])

    # using enumerate
    for i, e in enumerate(x):
      print(i, e)

  enumerate_fn()



7. Zip function
---------------

Another helpful function for "zipping" the items from different list together. The items at the index of the different list to be zipped will
be together.

.. code-block:: python

  def zip_fn():
    names = ["Jane", "Henry", "Tom"]
    ages = [20, 30, 10]
    fav_color = ["green", "blue", "red"]

    # Without Zip
    for i in range(len(names)):
      print(names[i], ages[i], fav_color[i])
    
    print()
    
    # With Zip
    print(list(zip(names, ages, fav_color)))
    
    for name, age, color in zip(names, ages, fav_color):
      print(name, age, color)

  zip_fn()



8. Show built in help
---------------------

This is particularly useful when you want to read the help from the command line.

.. code-block:: python
   
  def help_fn():
    help(list)
    
    # Even help on modules
    import sys
    help(sys)

  help_fn()



9. Print object attributes
--------------------------

To get the object attributes, you can use dir() function.

.. code-block:: python

  def dir_fn():
    x = ""
    # Print attribute of x
    print(dir(x))

    # Print constant integer literal attribute
    print(dir(4))
    
    # Even works on modules
    import queue
    print(dir(queue.Queue))

  dir_fn()



10. List comprehension
----------------------

This syntax is just Python's way of writing for inside of a list. The result will be a list object from the resulting for loop execution.

.. code-block:: python

  def list_comprehension_fn():
    # Normal list comprehension
    x = [i for i in range(5)]
    print (x)
    
    # List comprehension with conditions
    x = [i for i in range(5) if i %2 == 0]
    print (x)
    
    # List comprehension with other initializers (empty list)
    x = [[] for i in range(5)]
    print (x)
    
    # Nested list comprehension
    x = [[j for j in range(i)] for i in range(5)]
    print (x)
    
    # Using zip
    x = [i for i in zip(range(5), range(5, 10))]
    print (x)
    
    x = [[x, y] for x, y in zip(range(5), range(5, 10))]
    print (x)
    
    x = [y for _, y in zip(range(5), range(5, 10))]
    print (x)

  list_comprehension_fn()



11. Unnamed variable
--------------------

Sometimes a variable need not be created because it might not be used, hence you can use _ as the name of variable to define it as 
unused/unnamed.

.. code-block:: python

  def unnamed_var_fn():
    for _ in range(3):
      # Not able to use _ here
      print("hello world")

  unnamed_var_fn()



12. String join function
------------------------

This will join the strings together.

.. code-block:: python

  def join_fn():
    words = ["hello", "world", "My", "name", "is"]
    # Join all words
    print("".join(words))
    
    # Join all words with space separated
    print(" ".join(words))
    
    # Join all words with comma separated
    print(",".join(words))

  join_fn()



13. Reverse a string
--------------------

Sometimes, you might need to reverse a string, note this method will create a new string copy and not in place.

.. code-block:: python

  def reverse_str_fn():
    st = "hello"
    # New copy of string is returned, not in place
    print(st[::-1])

  reverse_str_fn()



14. sizeof in Python
--------------------

Sometimes you need to check the size of an object, just import sys and do a getsizeof().

.. code-block:: python

  def get_bytes_fn():
    import sys
    x = 100
    
    # Get size of object
    print(sys.getsizeof(x))
    print(sys.getsizeof("hello"))

  get_bytes_fn()



15. Get most freq in list using key
-----------------------------------

Sometimes you want to get an item in the list, you can make use of the key in max.

.. code-block:: python

  def get_most_freq_in_list():
    x = [1,2,3,4,4,5,5,6,6,1,1,1,1,1,1,1,2,2,2,3,4,5]

    # x.count is a iterator function that takes in a param to check the count of items in x
    # When passed into key, it will check which item in the set is biggest based on the count of items
    print(max(set(x), key=x.count))

  get_most_freq_in_list()



16. Lambda function
-------------------

Lambda or unnamed function can be written in python with the lambda keyword.

.. code-block:: python

  def lambda_fn():
    x = [(1,2), (3,4), (1,9)]
    print(max(x, key=lambda y: y[1]))

  lambda_fn()



17. XKCD Comic (Anti gravity)
-----------------------------

This is for fun, it will show a comic from XKCD.

.. code-block:: python

  def anti_gravity_fn():
    import antigravity
  
  anti_gravity_fn()
	


18. String multiplication
-------------------------

Did you know, you can actually multiply string by an integer, the result is just the string repeated that many times.

.. code-block:: python

  def string_multiplication_fn():
    # Print hellohellohello....
    print("hello " * 10)
    # Print hellohello....hehe
    print("hello " * 10 + "he " * 2)
    
    # Works with fstring as well
    name = "Kent"
    print(f"{name * 2}")

  string_multiplication_fn()
	


19. Splat and unpack
--------------------

This is useful for variable args as well as with kwargs functions.

.. code-block:: python

  def splat_and_unpack_fn():
    x = [1,2,3,4,5]
    # Unpack x to become 1 2 3 4 5
    print(*x)
    
    # Use on any amount of arguments
    def arg_fn(*args):
      print(args)
      
    arg_fn(2, 3)
    arg_fn(10, 3, 1)
    
    # Use on dict as wells
    def dict_fn(**kwargs):
      print(kwargs)
    
    
    dict_fn(x=10, y=20)
    
    def arg_dict_fn(*args, **kwargs):
      print(args, kwargs)
      
    
    arg_dict_fn(10, "aa", name="aa", age=10)

  splat_and_unpack_fn()



Conclusion
----------

Hopefully, this small list of python snippets is useful for you. Hopefully you learn something new today.



.. _YouTube: https://www.youtube.com/watch?v=sbtbIqEG4nI
