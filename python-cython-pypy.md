# CPython, Cython, PyPy... An introductory guide to the different Python variants

If you're into Python programming, chances are that you have heard the words CPython, Cython, Jython, PyPy... However, what are they exactly?

In this article we will do a small introduction to some of this Python variants, so you can at least understand what they exactly refer to if you see them on any other context.

### CPython
CPython is the original Python implementation, the one you will probably use daily and that you can download officially from [python.org]. Its name comes from the fact that the Python code is compiled to bytecode using C.

### Jython
Jython is an alternative implementation that uses Java, instead of C, to obtain the bytecode. This enables this bytecode to run in the JVM, the Java Virtual Machine, like other languages such as Kotlin, Scala or Java itself.

As Jython its just another implementation, the syntax remains unchanged: you can take all your CPython code and run it on Jython without needing to change a single line.

### Cython
Cython is compiler that enables to write C extensions for Python, usually with the goal of making it more efficient. Unlike the previous examples, is not a different implementation: it uses CPython to run the Python code. It can be considered a superset of Python, as it contains all its functionality and adds the extra C capabilities on top of it.

This extensions are written in a specific syntax, much similar to the one you'll use for writing the equivalent C code. Let's see this example (from [Cython's Wikipedia site](https://en.wikipedia.org/wiki/Cython))

```python
def sum_up_to_n(n):
    a = 0
    
    for i in range(n):
        a += i

    return a

cpdef sum_up_to_n(int n):
    cdef int a = 0, i
    
    for i in range(n):
        a += i

    return a
```

### PyPy
With PyPy, we have another Python implementation like CPython or Jython. The interesting point of this implementation is that it is implemented in... Python itself _(yes, probably this blew your mind, so keep reading to understand what we mean by that)_

The core of PyPy is a Just-In-Time (JIT) compiler that it uses to compile the most repeated parts of your source code to the machine's native code (instead of bytecode, like CPython or Jython did). Running native code is more efficient than running bytecode, so this can result in great speed improvements even considering the time that will be used for compiling the code. 

PyPy has a lot of detailed information about its advantages (and disadvantages) in its website, as well as some performance tips and advices on which cases may not be best suited for it, so I encourage you to check it if you're interested.

## Summary
After this article, you should be more familiar with the concepts of CPython, Jython, Cython and PyPy. However, chances are that you are interested in learning more about them, so I've prepared a list of links that can be really useful if want to dive deeper in the topic. I hope you find them useful!

#### Useful links
- [Python.org - Alternative Python implementations](https://www.python.org/download/alternatives/)
- [The definitive guide to Jython](http://www.jython.org/jythonbook/en/1.0/index.html)
- [Cython official website](https://cython.org/)
- [PyPy official website](https://pypy.org/)
- [Just-in-time compilation](https://en.wikipedia.org/wiki/Just-in-time_compilation)
- [Article on Toptal about Python implementations](https://www.toptal.com/python/why-are-there-so-many-pythons)
