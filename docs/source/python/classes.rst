Classes
=======


Inheritance
###########

Parent class (Base class) & Child Class (Derived class) example:

.. code-block:: python

    class Car:
        def __init__(self, brand: str, year: int):
            self.brand = brand
            self.year = year
        
        def print_brand(self):
            print(self.brand)
        
        def print_infos(self):
            print("{} year {}".format(self.brand, self.year))
    

    class Ferrari(Car):
        def __init__(self, model: str, year: int):
            super().__init__("Ferrari", year)
            self.model = model
        
        def print_infos(self):
            print("{} model: {} - year {}".format(self.brand, self.model, self.year))


You can create abstract class / method using the *abc* module:


.. code-block:: python

    from abc import ABC, abstractmethod

    class Car(ABC):
        def __init__(self, brand: str, year: int):
            self.brand = brand
            self.year = year
        
        def print_brand(self):
            print(self.brand)
        
        @abstractmethod
        def print_infos(self):
            pass

Multiple Inheritance
********************

.. code-block:: python

    class A:
        def __init__(self):
            pass
    
    class B:
        def __init__(self):
            pass

    class C:
        def __init__(self):
            pass

    class D(A, B, C):
        def __init__(self):
            # With Super
            super().__init__()         # call A init method
            super(A, self).__init__()  # call B init method (call C if B does not have a __init__ method)
            super(B, self).__init__()  # call C init method
            # Without Super
            A.__init__(self)           # call A init method
            B.__init__(self)           # call B init method
            C.__init__(self)           # call C init method

Magic Class Methods
###################

Overload comparison operators
*****************************

Comparison operators:

- :code:`<`   with  :code:`__lt__(self, other)`
- :code:`<=`  with  :code:`__le__(self, other)`
- :code:`>`   with  :code:`__gt__(self, other)`
- :code:`>=`  with  :code:`__ge__(self, other)`
- :code:`==`  with  :code:`__eq__(self, other)`
- :code:`!=`  with  :code:`__ne__(self, other)`

If you don't want to implement all the six rich comparison methods, you can use the **decorator** `total_ordering <https://docs.python.org/3/library/functools.html?highlight=total_ordering#functools.total_ordering>`_ from the :code:`functools` library.

.. code-block:: python

    from functools import total_ordering

    @total_ordering
    class Student:
        def __init__(self, grade: float):
            self.grade: float = grade

        def __eq__(self, other: "Student"):
            return (self.grade == other.grade)

        def __lt__(self, other: "Student"):
            return (self.grade < other.grade)

        # Thanks to the total_ordering decorator, the methods:
        # - __le__(self, other)
        # - __gt__(self, other)
        # - __ge__(self, other)
        # - __ne__(self, other)
        # are also automatically supplied 

You can also overload all of the others operators:

- :code:`+`  with  :code:`__add__(self, other)`
- :code:`-`  with  :code:`__sub__(self, other)`
- :code:`^`  with  :code:`__xor__(self, other)`
- :code:`&`  with  :code:`__and__(self, other)`
- etc ... (Get the full list `here <https://www.geeksforgeeks.org/operator-overloading-in-python/>`_)

Make a class Hashable
*********************

From the python documentation, `hashable <https://docs.python.org/3/glossary.html#term-hashable>`_:

    An object is hashable if it has a hash value which never changes during its lifetime (it needs a :code:`__hash__()` method), and can be compared to other objects (it needs an :code:`__eq__()` method). Hashable objects which compare equal must have the same hash value.
    Hashability makes an object usable as a dictionary key and a set member, because these data structures use the hash value internally.
    [...]

.. code-block:: python

    class MyClass():
        def __init__(self, a, b, c):
            self.a = a
            self.b = b
            self.c = c

        def __eq__(self, other: "MyClass"):
            return (self.a == other.a) and (self.b == other.b)

        def __hash__(self):
            # Attributes used for hash must never changes during the object lifetime
            return hash((self.a, self.b))

    inst1 = MyClass(1, 2, 3)
    inst2 = MyClass(1, 3, 3)
    inst3 = MyClass(2, 3, 3)
    inst4 = MyClass(1, 2, 4)
    dico = {inst1: 1, inst2: 2, inst3: 3, inst4: 4} # Here inst4 key override inst1 item
    # So: dico[inst1] = dico[inst4] = 4

Make a class Iterable
*********************

`Iterable Python Documentation <https://docs.python.org/3/glossary.html#term-iterable>`_

.. code-block:: python

    class IterableClass:
        def __init__(self, data):
            self.data = data
            self.index = 0

        def __iter__(self):
            # Need to return an object with the __next__ method defined
            # You could also directly yield the next values here
            self.index = 0
            return self

        def __next__(self):
            if self.index > len(self.data) - 1:
                raise StopIteration
            output = self.data[self.index]
            self.index += 1
            return output

        iterable_class = IterableClass([1, 2, 3, 4, 5])
        for k in iterable_class: print(k)
        # Ok
        for k in iterable_class: print(k)
        # Ok too: To check that the index is correctly reset to 0

Exemple with an iterable attribute:

.. code-block:: python

        class IterableClass:
            def __init__(self, iterable_attr):
                self.iterable_attr = iterable_attr

            def __iter__(self):
                yield from self.iterable_attr


Make a class Subscriptable
**************************

Making a class subscriptable is done with the defining the :code:`__getitem__` magic method:

.. code-block:: python

        class SubscriptableClass:
            def __init__(self, data):
                self.data = data

            def __getitem__(self, item):
                return self.data[item]
        
        inst = SubscriptableClass({"a": 1, "b": 2, "c": 3})
        # inst["a"] = 1, inst["b"] = 2, etc...

        # Doesn't work because inst.data is not a sequence with integers keys values
        for k in inst: print(k)
        
        # Works !
        inst = SubscriptableClass((4, 5, 6, 7, 8))
        for k in inst: print(k)


.. note::
    Making a class subsriptable by using the method :code:`__getitem__` automatically makes the class also iterable if the attribute is a sequence with integers keys values. `(For sequence types, the accepted keys should be integers and slice objects [...]) <https://docs.python.org/3/reference/datamodel.html?highlight=getitem#object.__getitem__>`_


Class String representation
***************************

Using the methods :code:`__str__` and :code:`__repr__`, see the example below.

.. note::
    The :code:`__str__` is intended to be as human-readable as possible, whereas the :code:`__repr__` should aim to be something that could be used to recreate the object.
    (`source <https://stackoverflow.com/a/3691806>`_)

.. code-block:: python

        class Person:
            def __init__(self, name: str, age: int):
                self.name = name
                self.age = age

            # Prefered method as it's called by __str__ when not defined
            def __repr__(self):
                return f"Person(name={self.name}, age={self.age})"

            def __str__(self):
                return f"{self.name}: {self.age} years old"

        jp = Person("Jean Paul", 89)
        print(jp) # => "Jean Paul: 89 years old"
        jp # => "Person(name=Jean Paul, age=89)"
        str(jp) # => "Jean Paul: 89 years old"

.. note::
    If you want to define only one method, define only the :code:`__repr__` method as :code:`__str__` calls it automatically when it's not defined.


Decorators
##########

Dataclasses
***********

Very succinctly :
- Class that contains mainly data, not much method (although there is no restriction)
- Automatically generates the :code:`__init__()` and :code:`__repr__()` methods = shorter definition

More info in the `official documentation <https://docs.python.org/3/library/dataclasses.html>`_

.. code-block:: python

    from dataclasses import dataclass, field
    from typing import List

    @dataclass
    class Person():
        name: str
        age: int
        # Argument with default value
        childrens: List[str] = field(default_factory=list)
        # Attributes not defined now
        is_adult: bool = field(init=False)

        def __post_init__(self):
            self.is_adult = self.age >= 18
    
    tati = Person("Tatiana", 32)
    nico = Person("Nicolas", 32, ["Robert", "Simone"])


Alternatives to dataclasses:

- Pydantic
- Attrs


Property
********

:code:`@property` decorator: https://docs.python.org/3/library/functions.html?highlight=property#property

.. code-block:: python

    class Student():
        def __init__(self, name, id, grade):
            self.name = name
            self.id = id
            self._grade = grade
            
        def get_name(self):
            return self.name

        # Read only attribute
        # Generated only when required
        @property
        def full_id(self):
            return self.name + " - " + str(self.id)

        # Defining a setter ang getter method for an attribute
        @property
        def grade(self):
            return self._grade

        # Allow to add a check on the value for example
        @grade.setter
        def grade(self, grade):
            if grade < 0:
                print("I know this guy is bad but less than 0 is mean")
                return
            self._grade = grade

    pollo = Student("Poulet", "AC2474", 12)

(source: https://www.askpython.com/python/built-in-methods/python-property-decorator)


------------------------------------------------------------

**Sources**:

- Multiple Inheritance: https://stackoverflow.com/questions/9575409/calling-parent-class-init-with-multiple-inheritance-whats-the-right-way
- Mixins Class: https://stackoverflow.com/questions/533631/what-is-a-mixin-and-why-is-it-useful
- Operators overloading: https://www.geeksforgeeks.org/operator-overloading-in-python/
- Purpose of :code:`__repr__` and :code:`__str__`: https://stackoverflow.com/questions/3691101/what-is-the-purpose-of-str-and-repr
- Difference between :code:`__repr__` and :code:`__str__`: https://stackoverflow.com/questions/1436703/what-is-the-difference-between-str-and-repr
- functools.total_ordering: https://docs.python.org/3/library/functools.html?highlight=total_ordering#functools.total_ordering
- dataclasses: https://docs.python.org/3/library/dataclasses.html
- property decorator: https://www.askpython.com/python/built-in-methods/python-property-decorator

