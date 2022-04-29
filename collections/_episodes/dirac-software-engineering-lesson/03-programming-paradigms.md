---
title: "Programming Paradigms"
slug: dirac-software-engineering-programming-paradigms
teaching: 15
exercises: 20
questions:
- "How does the structure of a problem affect the structure of our code?"
objectives:
- "Briefly describe the major paradigms we can use to classify programming languages."
- "Decompose the flow of data within a program into a sequence of data transformations"
- "Use classes to encapsulate data within a more complex program"
keypoints:
- "A Paradigm describes a way of structuring reasoning about code."
- "Different programming languages are suited to different paradigms."
- "Different paradigms are suited to solving different classes of problems."
- "*Pure functions* are functions with deterministic behaviour and no side effects."
- "Classes allow us to organise data into distinct concepts."
---

## Introduction

In the episode on software lifecycles, we spoke briefly about the design of software and how the design is impacted by the problem being solved and the environment in which the software is expected to run.

One of the major topics in the design of software is **Programming Paradigms**.

> In science and philosophy, a paradigm ... is a distinct set of concepts or thought patterns ...
>
> -- Wikipedia - Paradigm

Each paradigm represents a slightly different way of thinking about and structuring our code and each has certain strengths and weaknesses when used to solve particular classes of problem.
Once your software begins to get more complex it's common to use aspects of different paradigms to handle different subtasks.
Because of this, it's useful to know about the major paradigms, so you can recognise which you're using and when it might be appropriate to switch to another.

There's a long history behind this, but to skip straight to now, there's currently three dominant paradigms.

## Procedural Programming

**Procedural programming** is the simplest conceptually and is typically the paradigm most beginners will start with, so this is probably the style you're most familiar with up to this point, where we group code into *procedures performing a single task*.
In most modern languages we call these **functions**, instead of procedures - so if you're grouping your code into functions, this might be the paradigm you're using.

By grouping code like this, we make it easier to reason about the overall structure, since we should be able to tell roughly what a function does just by looking at its name.
These functions are also much easier to reuse than code outside of functions, since we can call them from any part of our program.

As the structure of code here is simpler than the following paradigms, this is an appropriate choice for smaller scripts and software that we're writing just for a single use.
Aside from smaller scripts, Procedural Programming is also commonly seen in code focused on high performance, with relatively simple data structures, such as in High Performance Computing (HPC).
These programs tend to be written in C (which doesn't support Object Oriented Programming) or Fortran (which didn't until recently).
HPC code is also often written in C++, but C++ code would more commonly follow an Object Oriented style, though it may have procedural sections.

Note that you may sometimes hear people refer to this paradigm as "Functional Programming" to contrast it with Object Oriented Programming, because it uses functions rather than objects, but this is incorrect.
Functional Programming places much stronger constraints on the behaviour of a function.

## Functional Programming

**Functional Programming** is built around a more strict definition of the term "function" borrowed from mathematics.
A function in this context can be thought of as a mapping that transforms its input data into output data.
Anything a function does other than produce an output is known as a **side effect** and should be avoided wherever possible.

Being strict about this definition allows us to break down the distinction between **code** and **data**, for example by writing a function which accepts and transforms other functions - in Functional Programming *code is data*.

The most common application of Functional Programming in research is in data processing, especially when using **Big Data**.
A popular definition of Big Data is data which is too large to fit in the memory of a single computer, with a single dataset sometimes being multiple terabytes or larger.
With datasets like this, we can't move the data around easily, so we often want to send our code to where the data is instead.
By writing our code in a functional style, we also gain the ability to run many operations in parallel as it's guaranteed that each operation won't interact with any of the others - this is essential if we want to process this much data in a reasonable amount of time.

### Pure Functions and Side Effects

We define a **pure function** as one which satisfies two criteria:

1. The data returned must be the same each time the same arguments are provided
2. Calling the function has no **side effects**

Side effects cover any action that a function performs which affects anything other than the value they return.
Examples include: printing text, modifying the value of an argument, or changing the value of a global variable.

> ## Pure Functions
>
> Which of these functions are pure?
> If you're not sure, explain your reasoning to someone else, do they agree?
>
> ~~~
> def add_one(x):
>     return x + 1
>
> def say_hello(name):
>     print('Hello', name)
>
> def append_item_1(a_list, item):
>     a_list.append(item)
>     return a_list
>
> def append_item_2(a_list, item):
>     result = a_list + [item]
>     return result
> ~~~
> {: .language-python}
>
> > ## Solution
> >
> > 1. `add_one` is pure - it has no effects other than to return a value and this value will always be the same when given the same inputs
> > 2. `say_hello` is not pure - printing text counts as a side effect, even though it is the clear purpose of the function
> > 3. `append_item_1` is not pure - the argument `a_list` gets modified as a side effect - try this yourself to prove it
> > 4. `append_item_2` is pure - the result is a new variable, so this time `a_list` doesn't get modified - again, try this yourself
> {: .solution}
>
{: .challenge}

### MapReduce in Python - Comprehensions

Often, when working with data you'll find that you need to apply a transformation to each datapoint, and/or filter the data, before performing some aggregation across the whole dataset.
This process is often referred to as **MapReduce**, particularly when working within the context of **Big Data** using tools such as Spark or Hadoop.
The MapReduce style of data processing relies heavily on the composability and parallelisability that we get when using functional programming.
This name comes from applying or **mapping** an operation to each value, then performing a **reduction** operation which collects the data together to produce a single result.

In Python, we do have the built-in functions `map`, `filter`, but we'll skip over those and go straight to the recommended approach.
If you're particularly interested in this form of data processing, it might be worth looking up the documentation for these functions, but in general we use **comprehensions** instead.

~~~
integers = range(5)
double_ints = [2 * i for i in integers]

print(double_ints)
~~~
{: .language-python}

~~~
[0, 2, 4, 6, 8]
~~~
{: .output}

The above example uses a **list comprehension** to double each number in a sequence.
Notice the similarity between the syntax for a list comprehension and a for loop - in effect, this is a for loop compressed into a single line.

We can also use list comprehensions to filter data, by adding the filter condition to the end.

~~~
double_even_ints = [2 * i for i in integers if i % 2 == 0]

print(double_even_ints)
~~~
{: .language-python}

~~~
[0, 4, 8]
~~~
{: .output}

Similarly, we have **set** and **dictionary comprehensions**, which look similar to list comprehensions, but use the **set literal** or **dictionary literal** syntax.

~~~
double_int_set = {2 * i for i in integers}

print(double_int_set)
~~~
{: .language-python}

~~~
{0, 2, 4, 6, 8}
~~~
{: .output}

~~~
double_int_dict = {i: 2 * i for i in integers}

print(double_int_dict)
~~~
{: .language-python}

~~~
{0: 0, 1: 2, 2: 4, 3: 6, 4: 8}
~~~
{: .output}

These 'comprehensions' cover the map and filter components of MapReduce, but not the reduce component.
For that we either need to rely on a built in reduction operator, or use the `reduce` function with a custom reduction operator.

In many cases, what we want to do is to sum the values in a collection - for this we have the built in `sum` function:

~~~
l = [1, 2, 3]

print(sum(l))
~~~
{: .language-python}

~~~
6
~~~
{: .output}

> ## Sum of Squares
>
> Using the MapReduce model and a list comprehension, write a function that calculates the sum of the squares of the values in a list.
> You can use the builtin Python function `sum` to sum a sequence of numbers.
> Your function should behave as below:
>
> ~~~
> def sum_of_squares(l):
>     # Your code here
>
> print(sum_of_squares([0]))
> print(sum_of_squares([1]))
> print(sum_of_squares([1, 2, 3]))
> print(sum_of_squares([-1]))
> print(sum_of_squares([-1, -2, -3]))
> ~~~
> {: .language-python}
>
> ~~~
> 0
> 1
> 14
> 1
> 14
> ~~~
> {: .output}
>
> > ## Solution
> >
> > ~~~
> > def sum_of_squares(l):
> >     squares = [x * x for x in l]
> >     return sum(squares)
> > ~~~
> > {: .language-python}
> >
> {: .solution}
>
> Now let's assume we're reading in these numbers from an input file, so they arrive as a list of strings.
> Modify your function so that it passes the following tests:
>
> ~~~
> print(sum_of_squares(['1', '2', '3']))
> print(sum_of_squares(['-1', '-2', '-3']))
> ~~~
> {: .language-python}
>
> ~~~
> 14
> 14
> ~~~
> {: .output}
>
> > ## Solution
> >
> > ~~~
> > def sum_of_squares(l):
> >     integers = [int(x) for x in l]
> >     squares = [x * x for x in integers]
> >     return sum(squares)
> > ~~~
> > {: .language-python}
> >
> {: .solution}
>
> Finally, like comments in Python, we'd like it to be possible for users to comment out numbers in the input file they give to our program.
> Extend your function so that the following tests pass (don't worry about passing the first set of tests with lists of integers):
>
> ~~~
> print(sum_of_squares(['1', '2', '3']))
> print(sum_of_squares(['-1', '-2', '-3']))
> print(sum_of_squares(['1', '2', '#100', '3']))
> ~~~
> {: .language-python}
>
> ~~~
> 14
> 14
> 14
> ~~~
> {: .output}
>
> > ## Solution
> >
> > ~~~
> > def sum_of_squares(l):
> >     integers = [int(x) for x in l if x[0] != '#']
> >     squares = [x * x for x in integers]
> >     return sum(squares)
> > ~~~
> > {: .language-python}
> {: .solution}
{: .challenge}

## Object Oriented

In **Object Oriented Programming**, we first think about the structure of the data and the things that we're modelling.
For example, if we're writing a simulation for our chemistry research, we're probably going to need to represent atoms and molecules.
Each of these has a set of properties which we need to know about in order for our code to perform the tasks we want - in this case, for example, we often need to know the mass and electric charge of each atom.
So with Object Oriented Programming, we'll have some **object** structure which represents an atom and all of its properties, another structure to represent a molecule, and a relationship between the two (a molecule contains atoms).
This structure also provides a way for us to associate code with an object, representing any **behaviours** it may have.

As an example here, let's imagine some software we might need to analyse the results of a clinical trial.
For each patient, we might need to keep track of:

- Their name
- Their dosage
- Some general health measurements
- Measurements of the trial outcome indicator e.g. inflammation

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
By using the Object Oriented paradigm, we have a better way to structure this:

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

bob = Patient("Bob", 0, 75)
bob.add_measurement(10)
bob.add_measurement(8)
~~~
{: .language-python}

In this second example, we define a **class** to represent our patients.
A class is a template for a piece of structured data, which may optionally provide some **behaviour** by implementing additional functions, like our `add_measurement` function.

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
