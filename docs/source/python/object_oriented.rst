Object-Oriented Programming
===========================


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
####################

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



------------------------------------------------------------

**Sources**:

- Multiple Inheritance: https://stackoverflow.com/questions/9575409/calling-parent-class-init-with-multiple-inheritance-whats-the-right-way
- Mixins Class: https://stackoverflow.com/questions/533631/what-is-a-mixin-and-why-is-it-useful
