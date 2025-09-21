---
layout: post
title: "Enhancing your python code with decorators"
date: 2024-06-03
categories: python backend webdev learning
---

## Introduction
In this article, you'll learn about decorators in Python: how they work, why they are useful, and when to use them. We'll also explore some common decorators and their use cases.

While I aim to explain concepts from a beginner's perspective, this article is not tailored for absolute beginners. A fundamental understanding of OOP in Python is required to fully benefit from this article. With that in mind, let's dive in.

In Python and most programming languages, a decorator is a tool that allows you to modify the behavior of a function or method without changing it’s main behavior.

They help add more functionality before a function is called or after it has been called. Decorators give you the ability to modify the behavior of functions without altering their main implementation.

## Creating Decorators
A decorator in Python is a function that:

- Accepts another function as an argument,
- Defines a new function inside itself,
- Returns that new function. This is the basic template for creating decorators in Python.

For example, if we want to create a simple decorator function that multiplies the return value of any function by 10, we can do it like this:
{% highlight python linenos %}
def mul_by_ten_decorator(func):

    def wrapper_function():
        return func() * 10

    return wrapper_function

# target function
def demo():
    return 2

result =  mul_by_ten_decorator(demo)
print(result())
{% endhighlight %}

{% highlight python linenos %}
john@doe:~/Desktop$ python3 main.py
output: 20
{% endhighlight %}

### note📝
{% highlight linenos %}
We have the mul_by_ten_decorator that takes a function as an argument.
Inside, the wrapper_function calls func() (i.e., the passed function)
and multiplies the result by 10.

Moving to the demo function, which we use to test our decorator,
it simply returns 2. The mul_by_ten_decorator wraps the demo function,
so when demo is passed as an argument, the decorator multiplies its return value
by 10. Calling result() is equivalent to calling wrapper_function(),
which returns 20. This value is then printed to the screen when we run the code
from the terminal.
{% endhighlight %}

We can further simplify our code by introducing the @ symbol

## Introducing @ symbol in decorators
{% highlight python linenos %}
def mul_by_ten_decorator(func):

    def wrapper_function():
        return func() * 10

    return wrapper_function

# target function
@mul_by_ten_decorator
def demo():
    return 2


result = demo()
print(result)
{% endhighlight %}

{% highlight python linenos %}
john@doe:~/Desktop$ python3 main.py
output: 20
{% endhighlight %}

### note📝
{% highlight linenos %}
When we introduce the decorator symbol @, we can wrap our target function
with our decorator function, making our code more encapsulated. By simply calling
the demo() function, it is similar to calling the wrapper_function(),
which internally calls func() (the demo function). The demo function returns 2,
which is then multiplied by 10. The wrapper_function returns 20, which is assigned
to result and printed out.
{% endhighlight %}

## Passing Arguments to Wrapper Functions
Sometimes, functions can take multiple arguments. If we modify our demo function to take two arguments, a and b, and return their product multiplied by 2, it would look like this:
{% highlight python linenos %}
def mul_by_ten_decorator(func):

    def wrapper_function():
        return func() * 10

    return wrapper_function


@mul_by_ten_decorator
def demo(a, b):
    return 2 * (a * b)


result = demo(4, 5)
print(result)
{% endhighlight %}
When we run our code, we encounter a type error.

{% highlight python linenos %}
john@doe:~/Desktop$ python3 demo.py
TypeError: mul_by_ten.<locals>.wrapper() takes 0 positional arguments but 2 were given
{% endhighlight %}

The error message implies that the wrapper_function is also expecting a number of arguments. We can modify our wrapper_function as follows: 

{% highlight python linenos %}
def wrapper_function(a, b):
   return func(a, b) * 10
{% endhighlight %}

so the entire code block can further be modified like this

{% highlight python linenos %}
def mul_by_ten_decorator(func):

    def wrapper_function(a, b):
        return func(a, b) * 10

    return wrapper_function


# target function
@mul_by_ten_decorator
def demo(a, b):
    return 2 * (a * b)


result = demo(4, 5)
print(result)
{% endhighlight %}

{% highlight python linenos %}
john@doe:~/Desktop$ python3 main.py
output: 400
{% endhighlight %}
### note📝
{% highlight linenos %}
Calling demo() is the same as calling the wrapper_function(),
which in turn calls func(). Therefore, we must ensure these functions accept the
same number of arguments.
{% endhighlight %}

What if we want to add 20 numbers using this same decorator? Do we need to pass 20 arguments to both the target function and the wrapper function? We cannot always define the number of arguments for both functions explicitly. Instead, we can use Python's *args and **kwargs keywords.

The *args keyword allows us to pass a variable number of positional arguments to the function, while **kwargs allows us to pass a variable number of keyword arguments.

We can make our code block more Pythonic by introducing these keywords.

{% highlight python linenos %}
def mul_by_ten_decorator(func):

    def wrapper_function(*args, **kwargs):
        return func(*args, **kwargs) * 10

    return wrapper_function


# target function
@mul_by_ten_decorator
def demo(a, b):
    return 2 * (a * b)


result = demo(4, 5)
print(result)
{% endhighlight %}
### note📝  
{% highlight linenos %}
Now, our target function takes in two arguments,
while our wrapper function accepts an arbitrary number of arguments.
{% endhighlight %}

With a good understanding of how to use decorator functions, we can proceed to explore some use cases and the importance of using decorator functions.

## Use Case of Decorators in Python
Let's examine some real-world examples where using a decorator comes in handy.

## Logging
It is often helpful to have logs of which functions are executed, along with relevant information. A logger is useful when you're trying to debug your code.

Here is a simple example of how to create a logger using Python's built-in logging package. The information about the script is saved to a file named test.log:

{% highlight python linenos %}
import logging

def function_logger(func):
    logging.basicConfig(level=logging.INFO, filename='test.log')
    def wrapper(*args, **kwargs):
        result = func(*args, **kwargs)
        logging.info(f'{func.__name__} ran with positional arguments:
        {args} and keyword arguments: {kwargs}. Return value: {result}')

        return result
    return wrapper


# target function
@function_logger
def addition(a, b):
    return (a + b)


print(addition(2, 5))
{% endhighlight %}

{% highlight python linenos %}
john@doe:~/Desktop$ python3 demo.py
output: 7
{% endhighlight %}


When you first run this code, a test.log file is created, which looks like this.

{% highlight python linenos %}
INFO:root:addition ran with positional arguments: (2, 5) and keyword arguments: {}. Return value: 7
{% endhighlight %}

To see different log messages, we can change the values of the arguments passed, for example, (11, 12).

### note📝 
{% highlight linenos %}
You can see our repeated template for creating a decorator:

- The decorator takes a function as an argument. ✅
- The wrapper function calls and returns a function. ✅

We import Python’s logging module with import logging, which includes a
BasicConfig method that sets up the logging configuration. We pass the
logging level, which can be logging.INFO, logging.DEBUG, or logging.ERROR.

Once we wrap this logger decorator around a function, we can get information
about the function, including the arguments passed and the returned value.
This can be a very useful tool for debugging and error tracking.
{% endhighlight %}

For more about logging in Python, checkout the official documentation
<a href="https://docs.python.org/3/howto/logging.html">https://docs.python.org/3/howto/logging.html</a>

## Caching
Caching is another use case where the knowledge of decorators comes in very handy. Caching is a technique used to store the results of expensive functions that take the same arguments and return the same value each time they are called. Instead of always recalculating the results, we can cache the process. This approach ensures that too many resources aren’t used up on such expensive functions.

To implement a caching function in Python, we use the @lru_cache decorator from functools.

### note📝
{% highlight linenos %}
LRU stands for Least Recently Used. The LRU function has a default maximum size,
of 128, which is the maximum number of calls to cache. Once this limit is reached,
older results are discarded to make space for new ones.
{% endhighlight %}


The Fibonacci sequence is a great example to illustrate the concept of caching because it depicts a recursive function, where calculating a Fibonacci sequence recalculates the same values multiple times.
{% highlight python linenos %}
from functools import lru_cache

@lru_cache(maxsize=120)
def fibonacci(n):
  if n < 2:
     return n
  return fibonacci(n-1) + fibonacci(n-2)


result = fibonacci(10)
print(result)
{% endhighlight %}
{% highlight python linenos %}
john@doe:~/Desktop$ python3 demo.py
output: 55
{% endhighlight %}

### note📝
{% highlight linenos %}
We use the lru_cache decorator from functools. When we call fibonacci(10),
it recursively calls fibonacci(9), fibonacci(8), fibonacci(7), and so on until it
reaches 1 or 0, ultimately outputting 55 to the screen. If n is less than 1,
it returns n; otherwise,
it returns the sum of the function called with n-1 and n-2.

When we call fibonacci(30) for the first time,
it stops calling the Fibonacci function when it reaches 10,
since we’ve previously run fibonacci(10). Similarly, when we run fibonacci(60)
for the first time, it stops calling the Fibonacci function at 30
since we’ve previously run fibonacci(30).
{% endhighlight %}

Therefore, a cached fibonacci() function will execute faster compared to one that isn’t cached. We can write a script to demonstrate the time difference between a cached function and one that isn't cached.

{% highlight python linenos %}
import time
from functools import lru_cache

# Fibonacci function without caching
def fibonacci_no_cache(n):
   if n < 2:
       return n
   return fibonacci_no_cache(n-1) + fibonacci_no_cache(n-2)

# Fibonacci function with caching
@lru_cache(maxsize=None)  # Use LRU cache with unlimited size
def fibonacci_with_cache(n):
   if n < 2:
       return n
   return fibonacci_with_cache(n-1) + fibonacci_with_cache(n-2)

# Calculate Fibonacci(30) without caching and measure the time
start_time = time.time()
fibonacci_no_cache(10)
no_cache_time = time.time() - start_time

# Calculate Fibonacci(30) with caching and measure the time
start_time = time.time()
fibonacci_with_cache(10)
with_cache_time = time.time() - start_time

print(f"Time without cache: {no_cache_time}")
print(f"Time with cache: {with_cache_time}")
{% endhighlight %}
{% highlight python linenos %}
john@doe:~/Desktop$ python3 main.py
Time without cache: 3.886222839355469e-05
Time with cache: 2.2172927856445312e-05
{% endhighlight %}

### note📝
{% highlight linenos %}
We see the differences in time it takes to run a cached function
and one that isn't: cached 0.0000221729, not cached 0.0000388622.
You can see we reduce the time by almost half.
{% endhighlight %}

Caching is particularly effective for recursive functions like Fibonacci, where the same inputs are used repeatedly. It saves time by avoiding redundant calculations, especially for large or frequently accessed values.

Now that we've explored some use cases where knowledge of decorators in Python can be very handy, let's further look at some of the built-in decorators that come with Python.

## Python Decorators
### Properties
Properties are built-in Python functions for managing methods of a class. They allow you to define methods that get and set the values of attributes, providing a way to enforce rules and validation when accessing or modifying these attributes. Properties are typically used to encapsulate private attributes and control access to them.

Let's see an example of how properties work in a class method:

{% highlight python linenos %}
class Circle:
    def __init__(self, radius):
        self.radius = radius

    @property
    def diameter(self):

        return 2 * self.radius

circle = Circle(4)
print(circle.diameter).
{% endhighlight %}
{% highlight python linenos %}
john@doe:~/Desktop$ python3 main.py
Output: 8
{% endhighlight %}

We can access the diameter method as an attribute, instead of calling the function with circle.diameter(), thanks to the @property decorator.

The beauty of having property decorators is how they make our code more concise. We can choose to make our diameter method stricter by ensuring the radius doesn’t take numbers less than or equal to zero, like this:

{% highlight python linenos %}
class Circle:
    def __init__(self, radius):
        self.radius = radius

    @property
    def diameter(self):
        if self. radius <= 0:
            raise ValueError('Only positive numbers')
        return 2 * self.radius

circle = Circle(-3)
print(circle.diameter)
{% endhighlight %}

when we run the code, a value error is raised as shown below

{% highlight python linenos %}
john@doe:~/Desktop$ python3 main.py
    raise ValueError('Only positive numbers')
ValueError: Only positive numbers
{% endhighlight %}