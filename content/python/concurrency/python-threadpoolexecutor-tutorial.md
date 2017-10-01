+++
date = "2017-10-01T13:36:48+01:00"
title = "Python ThreadPoolExecutor Tutorial"
draft = true
tags = ["python", "concurrency"]
series = [ "python" ]
desc = "In this article take a look at how you can use the ThreadPoolExecutor in Python to speed up your programs."
author = "Elliot Forbes"
twitter = "https://twitter.com/Elliot_F"
+++

> This tutorial has been taken and adapted from my book: [Learning Concurrency in Python](https://www.packtpub.com/application-development/learning-concurrency-python)


## Creating a ThreadPoolExecutor

The first step we need to know is how we can define our own `ThreadPoolExecutor’s`. This is a rather simple one-liner which looks something like so:

~~~py
executor = ThreadPoolExecutor(max_workers=3) 
~~~

Here we instantiate an instance of our `ThreadPoolExecutor` and pass in the maximum number of workers that we want it to have. In this case we’ve defined it as 3 which essentially means this thread pool will only have 3 concurrent threads that can process any jobs that we submit to it. 

In order to give the threads within our `ThreadPoolExecutor` something to do we can call the submit() function which takes in a function as its primary parameter like so:

~~~py
executor.submit(myFunction())
~~~

## Example

In this example we put together both the creation of our `ThreadPoolExecutor` object and the submission of tasks to this newly instantiated object. We’ll have a very simple task function that will which will simply sum the numbers from 0 to 9 and then print out the result. Not the most cutting edge software I’m sure you’ll agree but it serves as a fairly adequate example.

Below our defined task function we have our standard main function. It’s within this that we define our executor object in a similar fashion to above before then submitting two tasks to this new pool of threads.

~~~py
from concurrent.futures import ThreadPoolExecutor
import threading
import random

def task():
    print("Executing our Task")
    result = 0
    i = 0
    for i in range(10):
        result = result + i
    print("I: {}".format(result))
    print("Task Executed {}".format(threading.current_thread()))

def main():
    executor = ThreadPoolExecutor(max_workers=3)
    task1 = executor.submit(task)
    task2 = executor.submit(task)

if __name__ == '__main__':
    main()
~~~

## Output

If we were to execute our Python program above then we should see the rather bland output of both our tasks being executed and the result of our computation being printed out on the command line. 

We then utilize the `threading.current_thread()` function in order to determine which thread has performed this task. You should see that the two values outputted are distinct daemon threads. 

~~~py
$ python3.6 05_threadPool.py
Executing our Task
I: 45
Executing our Task
I: 45
Task Executed <Thread(<concurrent.futures.thread.ThreadPoolExecutor object at 0x102abf358>_1, started daemon 123145333858304)>
Task Executed <Thread(<concurrent.futures.thread.ThreadPoolExecutor object at 0x102abf358>_0, started daemon 123145328603136)>
~~~