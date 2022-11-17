Typing
======



Type aliases
############

.. code-block:: python

    from typing import List

    Vector = List[float]

    def scale(scalar: float, vector: Vector) -> Vector:
        return [scalar * num for num in vector]



NewType
#######

.. code-block:: python

    from typing import NewType

    UserId = NewType('UserId', int)



Callable
########

Frameworks expecting callback functions of specific signatures might be type hinted using:

:code:`Callable[[Arg1Type, Arg2Type], ReturnType]`

For example:

.. code-block:: python

    from collections.abc import Callable

    def feeder(get_next_item: Callable[[], str]) -> None:
        # Body

    def async_query(on_success: Callable[[int], None],
                    on_error: Callable[[int, Exception], None]) -> None:
        # Body

    async def on_update(value: str) -> None:
        # Body
    
    callback: Callable[[str], Awaitable[None]] = on_update

It is possible to declare the return type of a callable without specifying the call signature by substituting a literal ellipsis for the list of arguments in the type hint:

:code:`Callable[..., ReturnType]`



Generics
########

Generics can be parameterized by using a factory available in typing called TypeVar.

.. code-block:: python

    from collections.abc import Sequence
    from typing import TypeVar

    T = TypeVar('T')      # Declare type variable

    def first(l: Sequence[T]) -> T:   # Generic function
        return l[0]

Here the return type is "linked" to the parameter type: whatever you put into the function, the same thing comes out.

.. code-block:: python

    T = TypeVar('T')  # Can be anything
    S = TypeVar('S', bound=str)  # Can be any subtype of str
    A = TypeVar('A', str, bytes)  # Must be exactly str or bytes
    U = TypeVar('U', bound=str|bytes)  # Can be any subtype of the union str|bytes
    V = TypeVar('V', bound=SupportsAbs)  # Can be anything with an __abs__ method
    X = TypeVar('X', covariant=True)  # Allowing Subclasses to be used
    Y = TypeVar('Y', contravariant=True)  # Allowing Parent class to be used

More info about covariant and contravariant parameters in the PEP 484: https://peps.python.org/pep-0484/#covariance-and-contravariance



User-defined generic types
##########################

A user-defined class can be defined as a generic class. The type is stated when we instantiate the class.

.. code-block:: python

    from typing import Dict, Generic, TypeVar

    T = TypeVar("T")

    class Registry(Generic[T]):
        def __init__(self) -> None:
            self._store: Dict[str, T] = {}
            
        def set_item(self, k: str, v: T) -> None:
            self._store[k] = v
        
        def get_item(self, k: str) -> T:
            return self._store[k]
    
    if __name__ == "__main__":
        family_name_reg = Registry[str]()
        family_age_reg = Registry[int]()
        
        family_name_reg.set_item("husband", "steve")
        family_name_reg.set_item("dad", "john")
        
        family_age_reg.set_item("steve", 30)




Nominal vs structural subtyping
###############################

.. code-block:: python

    from collections.abc import Iterator, Iterable

    class Bucket:  # Note: no base classes
        ...
        def __len__(self) -> int: ...
        def __iter__(self) -> Iterator[int]: ...


    def collect(items: Iterable[int]) -> int: ...

    result = collect(Bucket())  # Passes type check

:code:`Bucket` is implicitly considered a subtype of both **Sized** (:code:`from collections.abc import Sized`) and **Iterable[int]** (:code:`from collections.abc import Iterable`) by static type checkers thanks to the definition of methods :code:`__len__` and :code:`__iter__`. This is known as structural subtyping (or static duck-typing).



MyPy
####

You can use **mypy** (:code:`conda install -c conda-forge mypy`) to check the correct typing of your project/script.




Examples
########

Multiple possible input type:

.. code-block:: python

    from tying import Union
    from numbers import Number

    Union[Number, str]


Generator function (yield):

.. code-block:: python

    from collections.abc import Generator

    def test(N: int) -> Generator[int]:
        for k in range(N):
            yield k


Iterable object:

.. code-block:: python

    from collections.abc import Iterable

    def test(params: Iterable[int]):
        for p in params:
            print(p)


------------------------------------------------------------

**Sources**:

- Official Typing Python 3 Documentation https://docs.python.org/3/library/typing.html
- Typevars explained: https://dev.to/decorator_factory/typevars-explained-hmo
- Understanding Generics Typing: https://medium.com/@steveYeah/using-generics-in-python-99010e5056eb
- Understanding covariance and contravariance in TypeVar: https://peps.python.org/pep-0484/#covariance-and-contravariance

