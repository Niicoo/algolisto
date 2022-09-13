Divers
======

- The use of zip and zip(\*data)
- the use of the locals() and globals()
- enumerate
- np.ndarray
- isinstance()
- include a path to the pythonpath absolute OR relative to the current file
- get all attributes of a class

Shebang line
############

Line at the beginning of a file to indicate that the file is a script, unix-like operating systems will try to execute the file using the interpreter specified by that line.

.. code-block:: python

    #!/usr/bin/env python

Xarray
######

.. code-block:: bash

    conda install -c conda-forge xarray

Basic example using xarray:

.. code-block:: python

    import xarray as xr
    import numpy as np

    # Example 1D data
    t = np.arange(0, 100, 0.1) # Seconds
    f = 0.13 # Hertz
    data = np.sin(2 * np.pi * 0.13 * t)

    darray_1D = xr.DataArray(
        data=data,
        dims=["time"],
        coords={"time": t},
        attrs={"frequency": f"{f} Hz"}
    )

    # Example 2D data
    lat = np.arange(-90, 90)
    lon = np.arange(-180, 180)

    llat, llon = np.meshgrid(lat, lon)
    data = np.logical_and(np.abs(llat) < 45, llon > 30)

    darray_2D = xr.DataArray(
        data=data,
        dims=["lon", "lat"],
        coords={"lat": lat, "lon": lon},
    )

    # Creating a dataset
    ds = xr.Dataset(
        data_vars={"signal": darray_1D, "map_mask": darray_2D}
    )

.. code-block:: bash

    In [55]: darray_1D
    Out[55]: 
    <xarray.DataArray (time: 1000)>
    array([ 0.00000000e+00,  8.15906116e-02,  1.62637165e-01,  2.42599231e-01,
            3.20943610e-01,  3.97147891e-01,  4.70703932e-01,  5.41121252e-01,
            6.07930298e-01,  6.70685577e-01,  7.28968627e-01,  7.82390811e-01,
            8.30595899e-01,  8.73262455e-01,  9.10105971e-01,  9.40880769e-01,
            9.65381639e-01,  9.83445205e-01,  9.94951017e-01,  9.99822352e-01,
            9.98026728e-01,  9.89576119e-01,  9.74526873e-01,  9.52979342e-01,
            9.25077207e-01,  8.91006524e-01,  8.50994482e-01,  8.05307886e-01,
            7.54251381e-01,  6.98165419e-01,  6.37423990e-01,  5.72432126e-01,
            5.03623202e-01,  4.31456046e-01,  3.56411879e-01,  2.78991106e-01,
            1.99709981e-01,  1.19097160e-01,  3.76901827e-02, -4.39681183e-02,
            ...
            -1.99709981e-01, -2.78991106e-01, -3.56411879e-01, -4.31456046e-01,
            -5.03623202e-01, -5.72432126e-01, -6.37423990e-01, -6.98165419e-01,
            -7.54251381e-01, -8.05307886e-01, -8.50994482e-01, -8.91006524e-01,
            -9.25077207e-01, -9.52979342e-01, -9.74526873e-01, -9.89576119e-01,
            -9.98026728e-01, -9.99822352e-01, -9.94951017e-01, -9.83445205e-01,
            -9.65381639e-01, -9.40880769e-01, -9.10105971e-01, -8.73262455e-01,
            -8.30595899e-01, -7.82390811e-01, -7.28968627e-01, -6.70685577e-01,
            -6.07930298e-01, -5.41121252e-01, -4.70703932e-01, -3.97147891e-01,
            -3.20943610e-01, -2.42599231e-01, -1.62637165e-01, -8.15906116e-02])
    Coordinates:
    * time     (time) float64 0.0 0.1 0.2 0.3 0.4 0.5 ... 99.5 99.6 99.7 99.8 99.9
    Attributes:
        frequency:  0.13 Hz


    In [56]: darray_2D
    Out[56]: 
    <xarray.DataArray (lon: 360, lat: 180)>
    array([ [False, False, False, ..., False, False, False],
            [False, False, False, ..., False, False, False],
            [False, False, False, ..., False, False, False],
            ...,
            [False, False, False, ..., False, False, False],
            [False, False, False, ..., False, False, False],
            [False, False, False, ..., False, False, False]])
    Coordinates:
    * lat      (lat) int64 -90 -89 -88 -87 -86 -85 -84 ... 83 84 85 86 87 88 89
    * lon      (lon) int64 -180 -179 -178 -177 -176 -175 ... 175 176 177 178 179


    In [57]: ds
    Out[57]: 
    <xarray.Dataset>
    Dimensions:   (time: 1000, lat: 180, lon: 360)
    Coordinates:
    * time      (time) float64 0.0 0.1 0.2 0.3 0.4 ... 99.5 99.6 99.7 99.8 99.9
    * lat       (lat) int64 -90 -89 -88 -87 -86 -85 -84 ... 83 84 85 86 87 88 89
    * lon       (lon) int64 -180 -179 -178 -177 -176 -175 ... 175 176 177 178 179
    Data variables:
        signal    (time) float64 0.0 0.08159 0.1626 ... -0.2426 -0.1626 -0.08159
        map_mask  (lon, lat) bool False False False False ... False False False



Dict of List TO/FROM List of Dict
#################################

Below is an example converting:

- a dict of list TO a list of dict (each list of the dictionary must have the same lengths)
- a list of dict TO a dict of list (each dictionary of the list must contain the same keys)


.. code-block:: python

    DATADICO = {
        "index":  [0,    1,     2,     3,    4,    5],
        "letter": ["a",  "f",   "f",   "z",  "x",  "t"],
        "float":  [5.4,  1.1,   2.9,   8.8,  0.1,  5.0],
        "bool":   [True, False, False, True, True, False],
    }

    # Convert dict of list TO list of dict
    NB_ITEMS = len(DATADICO[list(DATADICO.keys())[0]])
    [{key: DATADICO[key][k] for key in DATADICO} for k in range(NB_ITEMS)]


    DATALIST = [
        {"index": 0, "letter": 'a', "float": 5.4, "bool": True},
        {"index": 1, "letter": 'f', "float": 1.1, "bool": False},
        {"index": 2, "letter": 'f', "float": 2.9, "bool": False},
        {"index": 3, "letter": 'z', "float": 8.8, "bool": True},
        {"index": 4, "letter": 'x', "float": 0.1, "bool": True},
        {"index": 5, "letter": 't', "float": 5.0, "bool": False},
    ]

    # Convert list of dict TO dict of list
    {key: [i[key] for i in DATALIST] for key in DATALIST[0]}


Reduce
######

**Reduce** is a nummpy feature that is very useful when you need to apply some consecutive sequential operations, below is an example with an **OR** logical gate:

.. code-block:: python

    import numpy as np

    MASK_1 = np.array([True,  False, False, False, True, False, False], dtype=bool)
    MASK_2 = np.array([True,  True,  False, False, True, False, True], dtype=bool)
    MASK_3 = np.array([False, False, False, False, True, True,  False], dtype=bool)
    MASK_4 = np.array([True,  False, False, False, True, False, True], dtype=bool)

    MASK = np.logical_or.reduce([MASK_1, MASK_2, MASK_3, MASK_4])
    # MASK => array([ True,  True, False, False,  True,  True,  True])

This example is specific to numpy, you can use your own function using the :code:`functools.reduce` function.
(Learn more using the `geeksforgeeks reduce page <https://www.geeksforgeeks.org/reduce-in-python/>`_)


.. code-block:: python

    import functools
    
    A = [1, 3, 5, 6, 2, 5]
    
    # Sum of the list
    sum_A = functools.reduce(lambda a, b: a+b, A)


Bisect
######

**Bisect** provides support for maintaining a list in sorted order without having to sort the list after each insertion.

.. code-block:: python

    from bisect import bisect_left, bisect_right

    A = [ -6, 2, 0, 1, 5, 11, 13, 17, 23, 45, 45, 45, 54]

    bisect_left(A, -19)     # 0
    bisect_right(A, -19)    # 0

    bisect_left(A, 11)      # 5
    bisect_right(A, 11)     # 6

    bisect_left(A, 45)      # 9
    bisect_right(A, 45)     # 12

    bisect_left(A, 67)      # 13
    bisect_right(A, 67)     # 13

    # More examples and use cases here: https://docs.python.org/3/library/bisect.html


Flatten a multi-dimensional array except the last dimension
###########################################################

.. code-block:: python

    import numpy as np

    data = np.zeros((23, 12, 4))
    # data shape: (23, 12, 4)
    data_flatten = data.reshape(-1, data.shape[-1])
    # data_flatten shape: (276, 4)


Flatten only the first N dimensions of an array
###############################################

.. code-block:: python

    import numpy as np

    data = np.zeros((9, 5, 7, 8, 4))
    # data shape: (9, 5, 7, 8, 4)
    N = 2 # Number of dimension to flatten at the beginning
    data_flatten = data.reshape(-1, *data.shape[N:])
    # data_flatten shape: (45, 7, 8, 4)


Convert a 2D array to a list of tuples
######################################

.. code-block:: python

    import numpy as np

    data = np.zeros((100, 10))
    data = list(map(tuple, np.vstack(data)))


Duplicate an array to create a 2D array
#######################################

.. code-block:: python

    import numpy as np

    data = np.arange(10)
    # data shape: (10,)

    # Two equivalent method
    new_data = np.tile(data, (5, 1))
    new_data = np.repeat(data[np.newaxis], 5, axis=0)
    # new_data shape: (5, 10)

    new_data = np.repeat(data[:, np.newaxis], 5, axis=1)
    # new_data shape: (10, 5)


Measuring time elapsed
######################

.. code-block:: python

    import time
    
    t0 = time.perf_counter()
    time.sleep(5)
    t1 = time.perf_counter()
    print(f"Time elapsed: {t1 - t0:0.4f} seconds")


Progress bar
############

TO DO : erase it and do my own progress bar

.. code-block:: python

    def progressbar(it, prefix="", size=60, out=sys.stdout): # Python3.6+
        count = len(it)
        def show(j):

            x = int(size*j/count)
            print(f"{prefix}[{u'█'*x}{('.'*(size-x))}] {j}/{count}", end='\r', file=out, flush=True)
        
        show(0)
        for i, item in enumerate(it):
            yield item
            show(i+1)
        print("\n", flush=True, file=out)


Star and double star operators
##############################

When passed in a function definition (often :code:`*args*` and :code:`**kwargs*` are used but it is not mandatory to name them like that):

.. code-block:: python

    def test(*args, **kwargs):
        print("args:", args)
        print("kwargs:", kwargs)
    
    test(34, "salut", param=0, xoxo=None)
    # args: (34, 'salut')
    # kwargs: {'param': 0, 'xoxo': None}


When used to unpack sequence/collection/dictionary:

.. code-block:: python

    def test(a, b, c, d):
        return a + b + c + d
    
    params = (1, 2)
    dico = {"c": 3, "d": 4}

    test(*params, **dico)


Conversion of Earth coordinate systems
######################################

Install pyproj using the command:

.. code-block:: bash

    conda install -c conda-forge pyproj

.. code-block:: python

    from pyproj import Transformer, CRS

    CRS_4326 = CRS("epsg:4326") # WGS84 (lat/lon)
    CRS_3857 = CRS("epsg:3857") # Mercator (meter)
    CRS_ECEF = CRS(proj='geocent', ellps='WGS84', datum='WGS84') # Earth-centered, Earth-fixed coordinate system
    T_LATLON_TO_M = Transformer.from_crs(CRS_4326, CRS_3857, always_xy=True)
    T_M_TO_LATLON = Transformer.from_crs(CRS_3857, CRS_4326, always_xy=True)
    T_LATLON_TO_ECEF = Transformer.from_crs(CRS_4326, CRS_ECEF, always_xy=True)

    # Convert from Lon/Lat to x/y
    x, y = T_LATLON_TO_M.transform(lon, lat)
    # Convert from x/y to Lon/Lat
    lon, lat = T_M_TO_LATLON.transform(x, y)
    # Convert from Lon/Lat/Elevation to ECEF
    x, y, z = T_LATLON_TO_ECEF.transform(lon, lat, elevation)

------------------------------------------------------------

**Sources**:

- progressbar -> StackOverFlow: (https://stackoverflow.com/questions/3160699/python-progress-bar)
- bisect: https://docs.python.org/3/library/bisect.html
- functools.reduce: https://www.geeksforgeeks.org/reduce-in-python/
- single star, double star: https://stackoverflow.com/questions/2921847/what-does-the-star-and-doublestar-operator-mean-in-a-function-call
- single star, double star: https://docs.python.org/3/tutorial/controlflow.html#unpacking-argument-lists
