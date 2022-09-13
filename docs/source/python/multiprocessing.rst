Multi-Processing & Threading
============================


Threading:

- Threads share the same memory and can write to and read from shared variables
- Due to Python Global Interpreter Lock, two threads won't be executed at the same time, but concurrently (for example with context switching)
- Effective for I/O-bound tasks
- Can be implemented with threading module

(asyncio_ *can also be considered for I/O bound with slow I/O with many different connections*)

.. _asyncio: https://docs.python.org/3/library/asyncio.html

Multi-processing:

- Every process has is own memory space
- Every process can contain one ore more subprocesses/threads
- Can be used to achieve parallelism by taking advantage of multi-core machines since processes can run on different CPU cores
- Effective for CPU-bound tasks
- Can be implemented with multiprocessing module (or concurrent.futures.ProcessPoolExecutor)

source: https://towardsdatascience.com/multithreading-multiprocessing-python-180d0975ab29


Below are examples showing the most important features for both Multi-processing and Threading:

- Get the number of CPU core available
- Set the number of process/thread desired
- Start the Multi-processing/Threading
- Get the output from the process/thread
- Stop/Kill the process/thread
- Communicate with the process/thread (ex: get progression, send signals)



Threading
#########

Basic example: The concurrent.futures way
*****************************************

[Ramarao Amara] -> https://stackoverflow.com/questions/6893968/how-to-get-the-return-value-from-a-thread-in-python

    In Python 3.2+, stdlib concurrent.futures module provides a higher level API to threading, including passing return values or exceptions from a worker thread back to the main thread.

.. code-block:: python

    import concurrent.futures
    from numbers import Number
    import time
    import numpy as np


    def task(value:Number) -> Number:
        progression = 0
        for k in range(0, 50):
            time.sleep(np.random.rand(1)[0])
            progression = ((k + 1) * 100) / 50
        progression = 100
        return value * 2


    with concurrent.futures.ThreadPoolExecutor() as executor:
        future = executor.submit(task, 4.5)
        return_value = future.result()
        print(return_value)


Basic example: The threading way
********************************

.. code-block:: python

    import threading
    from numbers import Number
    import time
    import numpy as np


    def task(value:Number) -> Number:
        progression = 0
        for k in range(0, 50):
            time.sleep(np.random.rand(1)[0])
            progression = ((k + 1) * 100) / 50
        progression = 100
        return value * 2

    input_value = 4.5
    x = threading.Thread(target=task, args=(input_value, ))
    x.start()
    # Do whatever you want
    x.join()


By using this method, we cannot directly get the return value from the thread, solutions are described in this stackoverflow question:
https://stackoverflow.com/questions/6893968/how-to-get-the-return-value-from-a-thread-in-python

Example using the solution of extending the **Thread** class:

.. code-block:: python

    import threading
    import sys
    from numbers import Number
    import time
    import numpy as np


    class ReturnValueThread(threading.Thread):
        def __init__(self, *args, **kwargs):
            super().__init__(*args, **kwargs)
            self.result = None

        def run(self):
            if self._target is None:
                return  # could alternatively raise an exception, depends on the use case
            try:
                self.result = self._target(*self._args, **self._kwargs)
            except Exception as exc:
                print(f'{type(exc).__name__}: {exc}', file=sys.stderr)  # properly handle the exception

        def join(self, *args, **kwargs):
            super().join(*args, **kwargs)
            return self.result


    def task(value:Number) -> Number:
        progression = 0
        for k in range(0, 50):
            time.sleep(np.random.rand(1)[0])
            progression = ((k + 1) * 100) / 50
        progression = 100
        return value * 2

    input_value = 4.5
    x = ReturnValueThread(target=task, args=(input_value, ))
    x.start()
    # Do whatever you want
    result = x.join()
    print("Result:", result)



Get data from the thread while he's alive
*****************************************

As an application example, in many application, we want to get the progression of the task and its state.
For example in the example above, we would like to get the value of **progression** variable.

One way to do this is to pass a mutable object in the function input argument (can also be done by accessing **self** if the function is within a class).

.. warning::
    **You can also use the methods described in the multiprocessing case ! (see :ref:`mp-get-data`).**


Example using an input mutable argument:

.. code-block:: python

    import concurrent.futures
    from numbers import Number
    from typing import Dict
    import time
    import numpy as np


    def task(value:Number, progression:Dict[int, float], index:int) -> Number:
        progression[index] = 0.0
        for k in range(0, 50):
            time.sleep(np.random.rand(1)[0])
            progression[index] = ((k + 1) * 100.0) / 50
        progression[index] = 100
        return value * 2


    with concurrent.futures.ThreadPoolExecutor() as executor:
        index_thread = 0
        progression = {index_thread: 0.0}
        future = executor.submit(task, 4.5, progression, index_thread)
        while not future.done():
            time.sleep(0.1)
            print(f"Thread progression: {progression[index_thread]:.2f}%%", end="\r")
        print("\n")
        return_value = future.result()
        print(return_value)


Example using a class attribute:

.. code-block:: python

    import concurrent.futures
    from numbers import Number
    import time
    import numpy as np


    class TaskClass:
        def __init__(self):
            self.progression = 0.0

        def task(self, value:Number) -> Number:
            self.progression = 0.0
            for k in range(0, 50):
                time.sleep(np.random.rand(1)[0])
                self.progression = ((k + 1) * 100.0) / 50
            self.progression = 100
            return value * 2

        def getProgression(self) -> float:
            return self.progression


    task_inst = TaskClass()
    with concurrent.futures.ThreadPoolExecutor() as executor:
        future = executor.submit(task_inst.task, 4.5)
        while not future.done():
            time.sleep(0.1)
            print(f"Thread progression: {task_inst.getProgression():.2f}%%", end="\r")
        print("\n")
        return_value = future.result()
        print(return_value)



Multiprocessing
###############


Basic Example
*************

.. code-block:: python

    import multiprocessing as mp
    from numbers import Number
    import time
    import numpy as np


    def task(value:Number) -> Number:
        progression = 0
        for k in range(0, 50):
            time.sleep(np.random.rand(1)[0])
            progression = ((k + 1) * 100) / 50
        progression = 100
        return value * 2


    p = mp.Process(target=task, args=(4.5, ))
    p.start()
    # Do whatever you want
    p.join()


Get the return value from the process
*************************************

The best way to get the return value is to use an multiprocessing.Queue, example below:


.. code-block:: python

    import multiprocessing as mp
    from numbers import Number
    import time
    import numpy as np


    def task(value:Number) -> Number:
        progression = 0
        for k in range(0, 20):
            time.sleep(np.random.rand(1)[0])
            progression = ((k + 1) * 100) / 50
        progression = 100
        return value * 2

    def _taskWrapper(q:mp.Queue, *args, **kwargs) -> None:
        ret = task(*args, **kwargs)
        q.put(ret)

    q = mp.Queue()
    p = mp.Process(target=_taskWrapper, args=(q, 4.5, ))
    p.start()
    # Do whatever you want
    result = q.get()
    print(result)
    p.join()


.. _mp-get-data:

Get data from the process while he's alive
******************************************

As the process does not share the same memory area, we cannot use mutable object as we did for threading. So we also need to use Queue if we want to get data from the process while he's running.

.. code-block:: python

    import multiprocessing as mp
    from numbers import Number
    import time
    import numpy as np


    def task(value:Number, q:mp.Queue) -> Number:
        progression = 0
        q.put(("progression", progression))
        for k in range(0, 50):
            time.sleep(np.random.rand(1)[0])
            progression = ((k + 1) * 100) / 50
            q.put(("progression", progression))
        progression = 100
        q.put(("progression", progression))
        q.put(("result" , value * 2))

    q = mp.Queue()
    p = mp.Process(target=task, args=(4.5, q, ))
    p.start()
    while (ret := q.get())[0] == "progression":
        progression = ret[1]
        print(f"Process progression: {progression:.2f}%", end="\r")
    print("\n")
    result = ret[1]
    print(result)
    p.join()


.. warning::
    Queue works on the principle of First-In First-Out, you cannot get directly the last element added to the stack.


You can also use shared memory objects provided by the multiprocessing library (mp.Value and mp.Array):

.. code-block:: python

    import multiprocessing as mp
    from numbers import Number
    import time
    import numpy as np


    def task(value:Number, q:mp.Queue, progression:mp.Value) -> Number:
        progression.value = 0
        for k in range(0, 50):
            time.sleep(np.random.rand(1)[0])
            progression.value = ((k + 1) * 100) / 50
        progression.value = 100
        q.put(("result" , value * 2))


    q = mp.Queue()
    progression = mp.Value('f')
    p = mp.Process(target=task, args=(4.5, q, progression))
    p.start()
    while p.is_alive():
        time.sleep(0.05)
        print(f"Process progression: {progression.value:.2f}%", end="\r")
    print("\n")
    result = q.get()[1]
    print(result)
    p.join()


Locks
#####

If a part of your program has to be accessed by only one process at a time, you can use **Lock**, simple example below:

.. code-block:: python

    import multiprocessing as mp
    
    def process_function(lock):
        lock.acquire()
        # CRITICAL SECTION
        print("CRITICAL SECTION")
        print("Only One Process has to access at a given time")
        lock.release()

    lock = mp.Lock()
    p1 = mp.Process(target=process_function, args=(lock,))
    p2 = mp.Process(target=process_function, args=(lock,))
    p1.start()
    p2.start()
    p1.join()
    p2.join()



**A lot of basic features/concetps have not been mentioned (daemon, pool, pipes, etc...), you can learn more in the official multiprocessing documentation:**
https://docs.python.org/3/library/multiprocessing.html 

------------------------------------------------------------

**Sources**:

- Multi-threading and Multi-processing in Python (Giorgos Myrianthous): https://towardsdatascience.com/multithreading-multiprocessing-python-180d0975ab29
- StackOverFlow - How to get the return value from a thread in Python?: https://stackoverflow.com/questions/6893968/how-to-get-the-return-value-from-a-thread-in-python
- Alexandra Zaharia: https://alexandra-zaharia.github.io/posts/how-to-return-a-result-from-a-python-thread/
- https://stackoverflow.com/questions/27435284/multiprocessing-vs-multithreading-vs-asyncio-in-python-3
- Multiprocessing - Pipe VS Queue: https://stackoverflow.com/questions/8463008/multiprocessing-pipe-vs-queue
- Pipes, Queues and LOcks in python: https://www.educative.io/answers/pipes-queues-and-lock-in-multiprocessing-in-python
