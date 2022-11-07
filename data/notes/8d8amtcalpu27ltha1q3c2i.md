
Higher-order functions are functions that take other functions as input and return functions as output.

Some examples of built-in higher-order function in Python include:  `map()`, `filter()`, and `reduce()`.

The `functools` module in Python also provides built-in higher-order functions and operations for working with them.

## Higher-order functions and decorators

Decorators are a type of higher-order function that extends the functionality of a given function.

Let's start with a simple decorator example. In this simplest case let's decorate a function that takes no arguments:

```python
# This is a regular function
def hello():
    print("Hello world")

# This is a decorator function:
def goodbye(func):
    def inner():
        func()
        print ("Goodbye world!")
    return inner
```

The common pattern of decorator functions is that they contain an inner and an outer function. The job ouf the outer function is simply to return the inner function. The inner function calls and extends the input function.

When we pass the `hello` function to the `goodbye` function, we get a new function:

```python
hello_goodbye = goodbye(hello)
hello_goodbye()
>>> Hello world
>>> Goodbye world!
```

The next step up in terms of complexity is adding arguments to the inner function:

```python
def greet(name):
    print(f"Hello, {name}!")

def decorate(func):
    def inner(name):
        func(name)
        print("It's nice to meet you!")
    return inner

decorate(greet)("Ricardo")
>>> Hello, Ricardo!
>>> It's nice to meet you!
```

That's not too hard! Notice that the syntax `decorate(greet)("Ricardo")` sends the string "Ricardo" to the inner function of the `decorate` function.

Finally we can add some syntactic sugar to our decorators by using Python's `@` operator. In this case, if we wanted to decorate the function `greet` above, we could write it like this:

```python
def goodbye(func):
    def inner(name):
        func(name)
        print(f"Goodbye, {name}")
    return inner

@goodbye
def greet(name):
    print(f"Hello, {name}!")

greet("Ricardo")
>>> Hello, Ricardo!
>>> Goodbye, Ricardo
```

As you can see, `@goodbye` essentially modifies the function to return the output of `goodbye(greet)`.

That is basically all there is to know about decorators!


## Partial functions

Partial functions are a type of higher-order function that extends functions by pre-defining part of their arguments.

Let's say we have a function that takes two numeric arguments and sums them together:

```python
def sum(a, b)
    return a + b

sum(1, 2)
>>> 3
```

Now lets say we want to create a new version of this fuction where the argument `a` is a fixed value, say `5` , and we only need to input the argument `b`.

The `functools` module provides an easy way to do this:

```python
from functools import partial

sum_five = partial(sum, 5)
sum_five(2)  # Sums 2 + 5
>>> 7
```

The function `partial` takes a function as its first argument, and passes the rest of its arguments to that function. It returns a new function with a lower number of inputs. To learn how it does this, we can actually implement `partial` ourselves using decorators! We also need to learn about the special `*args` and `**kwargs` parameters.


Let's start by implementing the sample above from scratch:

```python
def input_five(func):
    def inner(b):
        return func(5, b)
    return inner

def sum(a, b):
    return a + b

sum_five = input_five(sum)
sum_five(4)
>>> 9
```

As you can see, decorators can be used to create partial functions! 
Of course, we could have used the decorator syntax `@input_five`, but I wanted to drive home the point that `sum_five` is a new function that takes less arguments than `sum`.

## Variable arguments

The only issue with the implementation above is that sometimes you don't know how many arguments the input function will have. Say we instead passed a function that sums three numbers the function `input_five` above, in this case it will fail, because `func` will expect three arguments instead of two.

To solve this we can use the special parameter `*args`. `*args` is a function parameter that catches any arbitrary number of **un-named** input parameters as a list. Note that `args` is just a variable name (by convention we use the name `args` or `argv`), and `*` is the important part of this parameter. It is the operator `*` that denotes packing the input arguments into an iterable and assigning them to the variable that follows it.

A simple function using `*args`:

```python
def print_all_arguments(*args):
    for arg in args:
        print(arg)

print_all_arguments(1, 2, 3)
>>> 1
>>> 2
>>> 3
```

From this, you can proably get an idea of how this is useful for building decorators that can be applied across functions with varying number of arguments.
Here is the same partial function decorator using `*args`:

```python
def input_five(func):
    def inner(*args):
        return func(5, *args)
    return inner

def sum(a, b, c, d):
    return a + b + c +d

sum_five = input_five(sum)
sum_five(1, 10, 100)
>>> 116
```

To wrap things up, let's learn about **kwargs:

`**kwargs` is similar to `*args`, but catches **named** input parameters into a dictionary. Just as in `*args`, the important part is the operator `**` which denotes packing name-value pairs into a dictonary and assigning them to a variable named `kwargs`. The list of arguments can then be accessed through the dictionary's `.items()` method.

An example:

```python
def print_all_arguments(**kwargs):
    for k, v in kwargs.items():
        print(f"Argument {k} has the value {v}")

print_all_arguments(a=1, b=2, c=3)
>>> Argument a has the value 1
>>> Argument b has the value 2
>>> Argument c has the value 3
```

I leave it as an exercise to think how you can use `**kwargs` to build a decorator that can be applied to functions with varying number of named arguments.
