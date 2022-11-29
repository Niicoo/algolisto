Timer
=====

The example below is an generic class to run a task at regular interval in a thread:


.. code-block:: python

    from abc import ABC, abstractmethod
    from threading import Thread, Event
    import datetime as dt
    import time
    import random


    class IntervalTimer(ABC):
        def __init__(self, interval: int):
            self.interval: int = interval # seconds
            self.stop_event = Event()
            self.mainth = None

        def _start(self):
            self.actionsBeforeStarting()
            interval_dt = dt.timedelta(seconds=self.interval)
            last_run = dt.datetime.utcnow().replace(microsecond=0)
            th = Thread(target=self.actionsInterval)
            while not self.stop_event.isSet():
                cur_time = dt.datetime.utcnow().replace(microsecond=0)
                if( (cur_time.second % self.interval == 0) and
                    ((cur_time - last_run) >= interval_dt)):
                    last_run = cur_time
                    if th.is_alive():
                        print("\t\tPast process is not over !!")
                    else:
                        th = Thread(target=self.actionsInterval)
                        th.start()
            self.actionsBeforeStopping()
        
        def start(self):
            self.mainth = Thread(target=self._start)
            self.mainth.start()

        def stop(self):
            self.stop_event.set()
            self.mainth.join()


        @abstractmethod
        def actionsBeforeStarting(self):
            pass

        @abstractmethod
        def actionsInterval(self):
            pass

        @abstractmethod
        def actionsBeforeStopping(self):
            pass


    class TimeTest(IntervalTimer):
        def __init__(self, interval):
            super().__init__(interval)
        
        def actionsBeforeStarting(self):
            print("\tINITIALISATION")

        def actionsInterval(self):
            print("\tRUNNING [START]")
            print("\tTime: %s" % dt.datetime.utcnow())
            nb_seconds = random.randint(3, 7)
            print("\tRequired time: %d s" % nb_seconds)
            time.sleep(nb_seconds)
            print("\tRUNNING [STOP]")
        
        def actionsBeforeStopping(self):
            print("\tSTOPPING")

    interval = 5 # seconds
    test = TimeTest(interval)
    print("Starting the timer [%d second(s) interval]" % interval)
    test.start()
    time.sleep(27)
    print("Stopping the timer")
    test.stop()


Output:

.. code-block:: text

    Starting the timer [5 second(s) interval]
            INITIALISATION
            RUNNING [START]
            Time: 2022-11-29 12:00:00.005237
            Required time: 3 s
            RUNNING [STOP]
            RUNNING [START]
            Time: 2022-11-29 12:00:05.000148
            Required time: 4 s
            RUNNING [STOP]
            RUNNING [START]
            Time: 2022-11-29 12:00:10.000150
            Required time: 7 s
                    Past process is not over !!
            RUNNING [STOP]
            RUNNING [START]
            Time: 2022-11-29 12:00:20.005245
            Required time: 5 s
    Stopping the timer
            STOPPING

            RUNNING [STOP]


------------------------------------------------------------

**Sources**:

- threading.Thread: https://docs.python.org/3/library/threading.html?highlight=thread#threading.Thread
- threading.Event: https://docs.python.org/3/library/threading.html?highlight=event#threading.Event