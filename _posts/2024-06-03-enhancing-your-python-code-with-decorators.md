---
layout: post
title: "Enhancing your python code with decorators"
date: 2024-06-03
categories: python backend webdev learning
---

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

## Setters and Getters
A getter method is responsible for retrieving the current value of a property, decorated with the @property decorator, while a setter method is responsible for setting the value of a property. The setter method is called when the property is assigned a new value.

One major flaw in how we've implemented validation above is that if we have other methods that rely on the radius being positive, this current approach might not handle invalid values properly. This can lead to subtle bugs or crashes. For example, consider a circumference method:

{% highlight python linenos %}
@property
def circumference(self):
    return 2 * 3.14159 * self.radius
{% endhighlight %}

The circumference property does not currently validate whether the radius is a positive number or not. It would be redundant to validate the radius within the circumference method. Instead, we can introduce setter and getter properties for our radius.

{% highlight python linenos %}
class Circle:
    def __init__(self, radius):
        self.radius = radius

    @property
    def radius(self):
        return self._radius


    @radius.setter
    def radius(self, value):
        if value <= 0:
            raise ValueError('Positive numbers only')
        self._radius = value


    @property
    def diameter(self):
        return 2 * self.radius


    @property
    def circumference(self):
        return 2 * 3.14159 * self.radius


circle = Circle(10)
print(f'Diameter: {circle.diameter}')
print(f'Circumference: {circle.circumference}')
{% endhighlight %}

{% highlight python linenos %}
john@doe:~/Desktop$ python3 main.py
Diameter: 20
Circumference: 62.8318
{% endhighlight %}

note📝
{% highlight linenos %}
Our getter method retrieves the current value of the radius and returns it,
while our setter method decorator checks the validity of the value.
If the value is valid, it is assigned to the radius self._radius.

Notice that in our getter and setter methods,
we use _radius to avoid the function calling itself repeatedly.
{% endhighlight %}

### Deleters
{% highlight linenos %}
The deleter function in a Python property decorator is used to define the behavior
when an attribute is deleted using the del statement.
Like the getter and setter functions,
it allows you to control access to an attribute,
but specifically handles what happens when you delete the attribute.

Returning to our circle class, let’s include a deleter property that
defines how the class should behave when a radius in a circle object is deleted.
{% endhighlight %}

{% highlight python linenos %}
class Circle:
    def __init__(self, radius):
        self.radius = radius

    @property
    def radius(self):
        return self._radius


    @radius.setter
    def radius(self, value):
        if value <= 0:
            raise ValueError('Positive numbers only')
        self._radius = value


    @radius.deleter
    def radius(self):
        print('Radius deleted')
        del self._radius


    @property
    def diameter(self):
        return 2 * self.radius


    @property
    def circumference(self):
        return 2 * 3.14159 * self.radius


circle = Circle(10)
print(f'Diameter: {circle.diameter}')
print(f'Circumference: {circle.circumference}')
del circle.radius
{% endhighlight %}

{% highlight python linenos %}
john@doe:~/Desktop$ python3 main.py
Diameter: 20
Circumference: 62.8318
Radius deleted
{% endhighlight %}

## Class method (@classmethod)
A class method is a method that is bound to the class rather than the instance of the class. It takes the class itself as its first argument (cls) instead of the instance (self). This allows the method to access and modify class state that applies across all instances of the class. A class method can be called by both the class and its instances.

Let's see how to use a class method in a class by modeling a Person:

{% highlight python linenos %}
from datetime import datetime

class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    @classmethod
    def birth_year(cls, name, year):
        return cls(name, datetime.today().year - year)

    def __str__(self):
        return f'Name: {self.name}, Year: {self.age}'

person = Person.birth_year('Doe', 34)
print(person)
{% endhighlight %}

{% highlight python linenos %}
john@doe:~/Desktop$ python3 main.py
Name: Doe, Year: 1990
{% endhighlight %}

## note📝
{% highlight linenos %}
Notice that we can call the birth_year() method directly on the class without
creating a Person instance, e.g., person = Person('Doe', 34).
{% endhighlight %}

Let's take a look at another example that explains the use of @classmethod in a code block.

Assuming we have an employee record, we can use a class method to retrieve individual employee records based on the PRIMARY KEY passed.

{% highlight python linenos %}
import sqlite3

class Employee:
    def __init__(self, id, name, salary):
        self.id = id
        self.name = name
        self.salary = salary

    @classmethod
    def from_database(cls, id):

        conn = sqlite3.connect('employees.db')
        cursor = conn.cursor()
        cursor.execute('SELECT name, salary FROM employees WHERE id=?', (id,))
        row = cursor.fetchone()
        conn.close()

        if row:
            name, salary = row
            return cls(id, name, salary)
        else:
            raise ValueError('Employees not found')

    def __str__(self):
        return f'{self.id} {self.name} {self.salary}'

employee_1 = Employee.from_database(3)
print(employee_1)
{% endhighlight %}

But before we run this code, let's create a database.py file that will set up our database of employees.

{% highlight python linenos %}
import sqlite3

conn = sqlite3.connect('employees.db')

cursor = conn.cursor()

cursor.execute('''CREATE TABLE IF NOT EXISTS employees
               (id INTEGER PRIMARY KEY, name TEXT, salary REAL)''')

cursor.execute("INSERT INTO employees (id, name, salary) VALUES (1, 'Ohemaa', 10000)")
cursor.execute("INSERT INTO employees (id, name, salary) VALUES (2, 'Nana', 3000)")
cursor.execute("INSERT INTO employees (id, name, salary) VALUES (3, 'Kofi', 15000)")

conn.commit()

cursor.execute("SELECT * FROM employees")
rows = cursor.fetchall()
for row in rows:
    print(row)

cursor.close()
conn.close()
{% endhighlight %}

When we run database.py for the first time it creates an employee.db file as shown below

{% highlight python linenos %}
john@doe:~/Desktop$ python3 database.py
(1, 'Ohemaa', 10000.0)
(2, 'Nana', 3000.0)
(3, 'Kofi', 15000.0)
{% endhighlight %}

Now, when we run our main.py file we can get the individual associated to the PRIMARY_KEY passed

{% highlight python linenos %}
john@doe:~/Desktop$ python3 database.py
3 Kofi 15000.0
{% endhighlight %}

### note📝
{% highlight linenos %}
In the classmethod in our main.py file,
{% endhighlight %}

{% highlight python linenos %}
 conn = sqlite3.connect("employees.db")
       cursor = conn.cursor()
       cursor.execute("SELECT name, salary FROM employees WHERE id=?", (id,))
       row = cursor.fetchone()
       conn.close()
{% endhighlight %}

{% highlight linenos %}
We establish and close the connection with our employees.db and then unpack
the values of the row into name and salary. This allows us to use a classmethod
without creating an employee instance to access an employee record.
If we were to implement this using an instance method,
it would require us to create an empty employee object first,
like employee = Employee(0, '', ''), before fetching the record,
which might seem a bit too complex.
{% endhighlight %}

## Static method (@staticmethod)
In a static method, you don’t need to pass an explicit first argument like cls in a classmethod or self in an instance method. A static method is also bound to a class and somewhat behaves like a class method. The major differences are:

- A static method doesn’t take an instance of the class as its first argument, unlike a classmethod or instance method.
- A static method cannot modify the state of the class.
It is primarily used as a utility function that does not depend on the state of the class or its instances.
- A static method is defined using the @staticmethod decorator.
- A staticmethod is most suitable in scenarios where you need to perform a function on a class without keeping a record of the instances of the class, as shown below:

{% highlight python linenos %}
class Calculator:
    @staticmethod
    def add(a,b):
        return a + b

    @staticmethod
    def multiply(a, b):
        return a * b

print(f'Addition: {Calculator.add(11, 15)}')
print(f'Multiply: {Calculator.multiply(18, 15)}')
print()
# you can also create a calculator instance
results = Calculator()
print(f'Addition: {results.add(21, 13)}')
print(f'Multiply: {results.multiply(20, 10)}')
{% endhighlight %}
{% highlight python linenos %}
john@doe:~/Desktop$ python3 main.py
Addition: 26
Multiply: 270

Addition: 34
Multiply: 200
{% endhighlight %}
You notice we do not need to rely on a class instance to use a static method.

Let's take a look at another example that calculates the average salary of employees in a company.
{% highlight python linenos %}
class Company:

    @staticmethod
    def average_salary(employees):
        result = sum(employees.values())/ len(employees)
        return int(result)

data = {
    "Alice": 50000,
    "Bob": 60000,
    "Charlie": 70000
    }

print(f'Average salary: {Company.average_salary(data)}')
{% endhighlight %}
{% highlight python linenos %}
john@doe:~/Desktop$ python3 main.py
Average salary: 60000
{% endhighlight %}

You see that the staticmethod doesn’t need to know or modify anything in the class other than having access to the class name.

In summary, static methods are within a class and do not need access to the class (no self or cls keyword). They cannot change or look at any object attributes or call other methods within the class. Static methods are mostly suitable as helper or utility functions that are relevant to the class but do not need to access or modify class or instance data.

## Abstract method (@abstractmethod)
Lastly, let’s take a look at abstractmethod in Python.

An abstract class acts as an interface for other subclasses, serving as a blueprint and forcing all subclasses to implement all of its abstract methods. An abstract base class cannot be instantiated directly.

Python does not provide abstract classes natively, but rather comes with a module that provides the base for defining Abstract Base Classes (ABC).

You use an @abstractmethod when all children of a subclass are required to have the same method as the inherited abstract class. Like the example below
{% highlight python linenos %}
from abc import ABC, abstractmethod

class Employee(ABC):
    @abstractmethod
    def calculate_salary(self):
        pass

class FullTimeEmployee(Employee):
    def calculate_salary(self):
        return "Calculating salary for full-time employee"

class PartTimeEmployee(Employee):
    def calculate_salary(self):
        return "Calculating salary for part-time employee"

class ContractEmployee(Employee):
    def calculate_salary(self):
        return "Calculating salary for contract employee"
{% endhighlight %}

### note📝
{% highlight linenos %}
Employee(ABC) defines an abstract class, which cannot be instantiated.
If a class inherits from an abstract class,
it must implement all the abstract methods defined in the parent abstract class.
Otherwise, it will also be considered an abstract class and cannot be instantiated.
{% endhighlight %}

### Abstract property
Just like with abstractmethod, an abstract property must also be implemented in any subclass. This allows you to specify that a subclass must include a property with a getter (and optionally a setter) method.
{% highlight python linenos %}
from abc import ABC, abstractmethod

class Employee(ABC):
    @property
    @abstractmethod
    def salary(self):
        """The salary property must be implemented by all subclasses"""
        pass

class FullTimeEmployee(Employee):
    def __init__(self, base_salary):
        self.base_salary = base_salary

    @property
    def salary(self):
        return self.base_salary

class PartTimeEmployee(Employee):
    def __init__(self, hourly_rate, hours_worked):
        self.hourly_rate = hourly_rate
        self.hours_worked = hours_worked

    @property
    def salary(self):
        return self.hourly_rate * self.hours_worked

# Example usage
full_time = FullTimeEmployee(50000)
part_time = PartTimeEmployee(20, 1000)

print(f'Full_time Salary: {full_time.salary}') 
print(f'Part_time Salary: {part_time.salary}')
{% endhighlight %}
### note📝
{% highlight linenos %}
Every child class (FullTimeEmployee and PartTimeEmployee) has the salary property,
although with different implementations. One takes in one argument and the other
takes two. When we run the code, we get the following output:
{% endhighlight %}
{% highlight python linenos %}
john@doe:~/Desktop$ python3 main.py
Full_time Salary: 50000
Part_time Salary: 20000
{% endhighlight %}

{% highlight linenos %}
FullTimeEmployee and PartTimeEmployee are concrete subclasses
that provide specific implementations for the salary property.
The salary methods in both subclasses are concrete methods.
Concrete methods are methods that have a complete implementation within a class.
They contain actual code that defines what the method does,
as opposed to abstract methods,
which only declare the method's signature without providing an implementation.
{% endhighlight %}

### Conclusion
An understanding of decorators can help you write cleaner, more maintainable, and reusable code. Decorators are a flexible and readable way to modify the behavior of functions and methods. They are useful for a variety of tasks such as logging, authorization, and caching.

