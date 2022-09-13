String Formatting
=================


Using F-strings (Literal String Interpolation)
##############################################

.. code-block:: python

    var_int = 4
    var_float = 3.14159

    print(f"""An example using F-Strings:
    - an integer filled with zeros =>{var_int:05d}<=
    - an integer filled with white spaces =>{var_int:>5d}<=
    - arithmetic operation =>{2 * (var_int + 1)}<=
    - lambda expression =>{(lambda x: x*2)(var_int)}<=
    - a float with 10 characters (6 before the comma, 1 for the comma + 3 after the comma) =>{var_float:> 10.3f}<=
    """)

    An example using F-Strings:
    - an integer filled with zeros =>00004<=
    - an integer filled with white spaces =>    4<=
    - arithmetic operation =>10<=
    - lambda expression =>8<=
    - a float with 10 characters (6 before the comma, 1 for the comma + 3 after the comma) =>     3.142<=



Using .format() method
######################

.. code-block:: python

    print("""A small example of .format() without parameters references:
    - an integer =>{:d}<=
    - a float =>{:.3f}<=
    - a string =>{:s}<=""".format(4, 3.14159, "salut"))

    A small example of .format() without parameters references:
    - an integer =>4<=
    - a float =>3.142<=
    - a string =>salut<=


.. code-block:: python

    print("""A small example of .format() using indexes:
    - an integer =>{0:d}<=
    - a float =>{1:>.3f}<=
    - a string =>{2:s}<=""".format(4, 3.14159, "salut"))

    A small example of .format() using indexes:
    - an integer =>4<=
    - a float =>3.142<=
    - a string =>salut<=


.. code-block:: python

    print("""A small example of .format() naming the parameters:
    - an integer =>{var_int:d} used twice  =>{var_int:05d}<=
    - a float =>{var_float:.3f}<=
    - a string  =>{var_str:s}<=
    """.format(var_int=4, var_float=3.14159, var_str="salut"))

    A small example of .format() naming the parameters:
    - an integer =>4 used twice  =>00004<=
    - a float =>3.142<=
    - a string  =>salut<=


.. code-block:: python

    print("""Choosing the number of digit/character:
    - an integer filled with zeros =>{var_int:05d}<=
    - an integer filled with white spaces =>{var_int:>5d}<=
    - a float with 10 characters (6 before the comma, 1 for the comma + 3 after the comma) =>{var_float:> 10.3f}<=
    """.format(var_int=4, var_float=3.14159, var_str="salut"))

    Choosing the number of digit/character:
    - an integer filled with zeros =>00004<=
    - an integer filled with white spaces =>    4<=
    - a float with 10 characters (6 before the comma, 1 for the comma + 3 after the comma) =>     3.142<=


Using % Module operator
#######################

.. code-block:: python

    print("""A small example using Modulo (%%) operator:
    - an integer =>%d<=
    - a float =>%.3f<=
    - a string =>%s<=""" % (4, 3.14159, "salut"))

    A small example using Modulo (%) operator:
    - an integer =>4<=
    - a float =>3.142<=
    - a string =>salut<=

------------------------------------------------------------

**Sources**:

- https://www.geeksforgeeks.org/string-formatting-in-python/
