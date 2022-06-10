---
lesson_title: 'Principles of Software Engineering'
lesson_schedule_slug: dirac-software-engineering-schedule
title: "Functions and Classes"
slug: dirac-software-engineering-classes
teaching: 15
exercises: 15
questions:
- "How can we structure our data and code to help us keep track of larger programmes?"
objectives:
- "Use functions to structure code which performs a particular task."
- "Use classes to encapsulate structured data."
keypoints:
- "Functions allow us to decompose a problem down into smaller tasks."
- "Classes allow us to organise data which represents a distinct concept."
---

## Functions

From our previous section, we've actually now got everything we need to write any program, but if we stopped here we'd quickly find that larger programs become unmanageable.
What we're missing is a way of structuring our code, so that we can separate out specific parts with specific functionality.
The first and most important tool for doing this are **functions**.

Much like in mathematics, functions represent an operation which can be applied to some data, to receive some other data as output.

~~~ python
def add_one(x):
    """Add one to a number."""
    return x + 1

print(add_one(3))
~~~
{: .language-python}

~~~
4
~~~
{: .output}

In the example above, we define a function `add_one` which adds one to a number.
To define a function, we need to start with the `def` keyword, then the function name, the function **arguments** in parentheses and the colon to start a new block.
Within the function's code block we can do anything we could outside of a function, but in order to get any data back out of the function we need to `return` it.

The arguments of a function are the values it takes as input - when we **call** the function inside the `print()`, we provide the values of any required parameters, in this case just `x`.
The value returned by the function is then passed on to `print()`, just as it would be if we'd put the value there directly.

The last component of the function definition above is the **docstring**.
Docstrings aren't a requirement, but can make it much easier to understand what your code is doing, especially if it's complex or you haven't looked at it for a while.
A docstring needs to be the first thing inside a function's code block and should be enclosed within triple double quotes (i.e. `"""`).

> ## A Function for Pi
>
> Take our existing example for the Monte Carlo approximation of pi and convert it into a function.
>
> > ## Solution
> > 
> > ~~~ python
> > import random
> >
> > def approximate_pi(num_points=1000000):
> >     """Monte-carlo approximation of pi by counting points within a circle."""
> >     inside = 0
> > 
> >     for i in range(num_points):
> >         x = random.uniform(-1, 1)
> >         y = random.uniform(-1, 1)
> > 
> >         r2 = x**2 + y**2
> > 
> >         if r2 <= 1:
> >             inside += 1
> > 
> >     return = 4 * inside / num_points
> > 
> > pi = approximate_pi()
> > print(pi)
> > ~~~
> > {: .language-python}
> > 
> > ~~~
> > 3.142464
> > ~~~
> > {: .output}
> >
> > In this example solution, we've provided a default value for the number of points, meaning that when someone calls the function they may choose not to specify how many points, in which case the default value will be used.
> {: .solution}
{: .challenge}

> ## Sum of Squares
> 
> Write a function which accepts an integer and returns the sum of the squares of integers up to **and including** this number.
> 
> > ## Solution
> > 
> > ~~~ python
> > def sum_of_squares(limit):
> >     total = 0
> >     for i in range(limit + 1):
> >         total += i * i
> > 
> >     return total
> > ~~~
> > {: .language-python}
> {: .solution}
{: .challenge}

### Function Composition

One of the main benefits of breaking our code up into functions is that it allows us to use **composition**.
Often, we find that a task is **composed** of several smaller sub-tasks - an example of this can be seen when we convert temperatures between Fahrenheit, Celsius and Kelvin.
Writing two functions to perform temperature conversion from Fahrenheit, we might have:

~~~ python
def fahr_to_celsius(fahr):
    # apply standard Fahrenheit to Celsius formula
    celsius = ((fahr - 32) * (5/9))
    return celsius

def fahr_to_kelvin(fahr):
    # apply standard Fahrenheit to Kelvin formula
    kelvin = ((fahr - 32) * (5/9)) + 273.15
    return kelvin
~~~
{: .language-python}

But on closer inspection, we find that the second of these functions can be broken down into two sub-tasks: firstly, convert Fahrenheit to Celsius, then convert Celsius to Kelvin.
Since we already have a function which converts from Fahrenheit to Celsius, we can make use of this:

~~~ python
def fahr_to_celsius(fahr):
    # apply standard Fahrenheit to Celsius formula
    celsius = ((fahr - 32) * (5/9))
    return celsius

def fahr_to_kelvin(fahr):
    # apply standard Fahrenheit to Kelvin formula
    kelvin = fahr_to_celsius(fahr) + 273.15
    return kelvin
~~~
{: .language-python}

This breaking down of a problem into smaller components is a core concept in the practice of software engineering and many other techniques are based around this idea.

## Classes

Classes are another tool available to us in Python which allow us to structure our **code** and our **data** at the same time.
A class is effectively a **template** for a structured piece of data and the **behaviour** which is associated with it.

As an example here, let's imagine some software we might need to analyse the results of a clinical trial.
For each patient, we might need to keep track of:

- Their name
- Their dosage
- Some general health measurements
- Measurements of the trial outcome indicator

Using the data structures we've seen so far, we might implement this using a dictionary for each patient - so all of our patients would be represented in a list of dictionaries:

~~~ python
alice = {
    "name": "Alice",
    "dosage_mg": 40,
    "weight_kg": 65,
    "measurements": [
        10, 10, 6, 4, 2, 1, 0
    ],
}

bob = {
    "name": "Bob",
    "dosage_mg": 0,
    "weight_kg": 75,
    "measurements": [
        10, 8, 8, 9, 8, 9, 9
    ],
}
~~~
{: .language-python}

However, having to replicate the structure like this each time is error prone and overly verbose.
By using a class, we have a better way to structure this:

~~~ python
class Patient:
    def __init__(self, name, dosage_mg, weight_kg):
        self.name = name
        self.dosage_mg = dosage_mg
        self.weight_kg = weight_kg

        self.measurements = []

    def add_measurement(self, value):
        self.measurements.append(value)

alice = Patient("Alice", 40, 65)
alice.add_measurement(10)
alice.add_measurement(8)
print(alice.name)
print(alice.dosage_mg)

bob = Patient("Bob", 0, 75)
bob.add_measurement(10)
bob.add_measurement(8)
print(bob.name)
print(bob.dosage_mg)
~~~
{: .language-python}

~~~
Alice
40
Bob
0
~~~
{: .output}

In this example, the `self.name`, `self.dosage_mg`, `self.weight_kg` and `self.measurements` **attributes** are the structured data we want our class to contain.
The function `add_measurement` is a **behaviour** that we have chosen to define for our class - something that the data can do, or something that can be done to the data.

We can then create an **instance** of the class by using similar syntax to calling a function.
When we create instances for Alice and Bob, we provide the values to the parameters of the `__init__` method.

When a function belongs to a class like this, we often refer to it as a **method**.
Normal methods will have `self` as their first parameter, but notice that we don't ever provide a value for this when we call the `__init__` method (implicitly) or the `add_measurement` method.
This is because it gets filled in for us, to refer to the instance of the class that we're operating on.
In the case of the line `alice.add_measurement(10)`, the value of the `self` parameter, will be the class instance `alice`.

> ## Adding a Method
> 
> Something we might need to calculate during our clinical trial is the dosage per body mass, often reported in units of milligrams per kilogram (mg/kg).
> 
> Add a new method to our patient class which will calculate the measure for us.
> 
> > ## Solution
> > ~~~ python
> > class Patient:
> >     def __init__(self, name, dosage_mg, weight_kg):
> >         self.name = name
> >         self.dosage_mg = dosage_mg
> >         self.weight_kg = weight_kg
> > 
> >         self.measurements = []
> > 
> >     def add_measurement(self, value):
> >         self.measurements.append(value)
> > 
> >     def dosage_per_kg(self):
> >         return self.dosage_mg / self.weight_kg
> > ~~~
> > {: .language-python}
> {: .solution}
{: .challenge}
