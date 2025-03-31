# The Py-Row programming language
Did you ever open up someone else's Python project only to find it has tens of lines and is totally unreadable unless you literally dedicate a full 10 minutes to read it?
<br><br>
Well with Py-Row you can now write all of those tens of rows in just one line!

## Introduction
The Py-Row programming language is a subset of normal Python, meaning every valid Py-Row code is also valid Python code but the opposite is not true.
<br><br>
Py-Row is written just like normal python, except all of your code must fit into exactly one line (and no, semicolons are not allowed in this language, it has to be one statement).
<br>
All of your code must be an honest to god Python one-liner with just one statement containing everything you want your code to do.

## The 'main' function
The entry point for your program should look like this:
```python
(lambda: (  ) )()
```
Your program in its entirety should fit inside this lambda expression.
## Functions
In Py-Row every function should be a lambda expression.
```python
lambda: print("Hello, World!")
```
This function prints:
```
Hello, World!
```
Multiple statements can be used in a single function by putting them in a tuple:
```python
lambda: (print("this is printed by the first statement"), print("this is printed by the second statement"))
```
This function prints:
```
this is printed by the first statement
this is printed by the second statement
```
But keep in mind that every statement in the tuple is also a value that will be returned, meaning this function return:
```python
(None, None)
```
### Local scope memory 
You can declare a variable to be in the local scope of a function by simply doing this:
```python
lambda : (x := 1)
```
in this example we have declared a local scope variable by the name of x and assigned it a value of 1.
<br><br>
we can freely reassign the values of local scope variables:
```python
lambda : (x := 1, x := 2)
```
however, keep in mind that once a function has finished its runtime, every value in the tuple (so every value in local memory) will be returned:
```python
(lambda : (x := 1, x := 2))()  # (1, 2)
```
### Parameters
Function parameters should be given as simply parameters of the lambda
```python
lambda x: print(x)
```
This function for example takes 1 parameter called x and prints it.  
## Memory
While we already discussed local scope memory, what about global scope memory?
<br>
While it is not possible to write anything in Pythons global scope, we can still pass in as a parameter to a function whatever data we want it to have.
<br><br>
And so, we have one variable which holds all global scope memory (including every function) in a dict and pass it to every function:
```python
(lambda: (memory := {'foo': lambda mem: (print(mem['bar'])), 'bar': "Hello, World!"},  memory['foo'](memory)))()
```
The output for this Py-Row program:
```
Hello, World!
```
This program declares a function name `'foo'` and a global scope string variable named `'bar'` and when we run `'foo'`, it prints what's stored in `'bar'`.
### Reassignment of global scope variables
Global scope variables can be reassigned by using the dicts `update` method:
```python
(lambda: (memory := {'foo': lambda mem: (mem.update({'bar': "hi"}), print(mem['bar'])), 'bar': "Hello, World!"},  memory['foo'](memory)))()
```
In this example, we reassign `'bar'` to "hi" before printing, and so the programs output will be:
```
hi
```
And from this point on `'bar'`s value will be "hi" until it gets reassigned to something else.
## External libraries
Py-Row supports every Python module, however, you cannot use the standard `import` as it take one line. 
<br>
So to import modules, we need to use the `__import__(module)` method.
<br>
So for example, to check the current time, we will write:
```python
time := __import__('time').time()
```
## Logic
all logic operators (or, and, etc) are the same as regular python. 
### `if` statements
in Py-Row, `if` statements are done using pythons inline `if` feature.
<br>
```python
thing() if condition else something_else() 
```
## Loops
Loops are super important for many algorithms to work, Py-Row supports both `for` and `while` loops.
### `for` loops
In Py-Row, `for` loops are done using list comprehensions.
<br>
This means that for every iteration of the `for` loop a new variable needs to be created giving them a space complexity of O(n).
<br>
In many cases the `for` loop making a new list is not necessarily a bad thing as that list will genuinely be useful, but if you really need to make it more space efficient you should consider using `while` instead or using fuctools' `reduce`.
<br><br>
**Examples of `for`:**
<br>
This program prints every string in a list:
```python
(lambda: (memory := {'foo': lambda mem: ([print(string) for string in ["Hello", "World!"]])},  memory['foo'](memory)))()
```
Its output is:
```
Hello
World!
```
This program will print the range of 0 to 5:
```python
(lambda: (memory := {'foo': lambda mem: ([print(i) for i in range(5)])},  memory['foo'](memory)))()
```
Its output is:
```
0
1
2
3
4
```
### `while` loops
Unlike the `for` loop, we have to implement the `while` loop ourselves, however, thankfully it's not that complicated:
```python
'while': lambda condition, code: __import__('functools').reduce(lambda a, b: code(), iter(condition, False))
```
**Examples of `while`:**
<br>
This program prints the range of 0 to 5:
```python
(lambda: (memory := {'count': 0, 'foo': lambda mem: (mem['while'](lambda: (mem['count'] < 5), lambda: (print(mem['count']), mem.update({'count': mem['count']+1})))), 'while': lambda condition, code: __import__('functools').reduce(lambda a, b: code(), iter(condition, False))},  memory['foo'](memory)))()
```
Its output is:
```
0
1
2
3
4
```
## Compilation and running
Maybe one day I will the time and mental power to write a compiler that will actually remove comments and check that everything really is a single statement.
<br><br>
That day is not today, so for now simply write everything as `.py` files and just try not to break the rules of the language.
<br>
Pretty please 👉👈.  

