Netcdf
======


Writing
#######


.. code-block:: python

    import netCDF4 as nc
    import numpy as np

    root = nc.Dataset("file.nc", mode='w')

    # Attributes
    root.history = "Created on June 13th 2022"
    root.cycles = [1, 2, 3, 8, 9]

    # Dimensions
    len_time = 10
    len_wf = 128
    root.createDimension("time", len_time)
    root.createDimension("index_waveform", len_wf)

    # You can define a dimension value
    time = root.createVariable("time", np.float64, ("time", ))
    time.calendar = "Gregorian"
    time.coordinates = "longitude latitude"
    time.leap_second = "0000-00-00_00:00:00"
    time.long_name = "time in TAI"
    time.standard_name = "time"
    time.tai_utc_difference = 37.
    time.units = "seconds since 2000-01-01 00:00:00"
    time[:] = np.arange(len_time)

    # 1D Variable
    latitude = root.createVariable("latitude", np.int32, ("time", ), fill_value=-2147483648)
    latitude.scale_factor = 1.e-06
    latitude.comment = "Latitude of measurement [-90, +90]. Positive latitude is North latitude, negative latitude is South latitude"
    latitude.coordinates = "longitude latitude"
    latitude.long_name = "latitude"
    latitude.standard_name = "latitude"
    latitude.units = "degrees_north"
    latitude[:] = np.random.rand(len_time) * 180 - 90

    # 2D Variable
    waveform = root.createVariable("waveform", np.float64, ("time", "index_waveform", ))
    waveform.comment = "Waveform computed on board (acquired from the table)"
    waveform.long_name = "waveform computed on board"
    waveform.standard_name = "wf_lrm_board"
    waveform[:] = np.random.random((len_time, len_wf))
    
    # Create a group
    group_one = root.createGroup("group_one")

    # You can add a variable in this group using a dimenion created in the parent group
    longitude = group_one.createVariable("longitude", np.int32, ("time", ), fill_value=-2147483648)
    longitude.scale_factor = 1.e-06
    longitude.comment = "Longitude of measurement [0, 360). East longitude relative to Greenwich meridian"
    longitude.coordinates = "longitude latitude"
    longitude.long_name = "longitude"
    longitude.standard_name = "longitude"
    longitude.units = "degrees_east"
    longitude[:] = np.random.rand(len_time) * 360

    # Variable with add_offset
    var_x = group_one.createVariable("var_x", np.int8, ("time", ), fill_value=-128)
    var_x.add_offset = 100
    var_x.scale_factor = 1.e-01
    # This netcdf variable accepts values in the range [100 - 2**7 * 0.1 ; 100 + (2**7 - 1) * 0.1]
    # All values out of this range won't be encoded correctly
    var_x_values = 100 + (np.random.rand(len_time) - 0.5) * 256 * 1e-1
    # UNSAFE: This value WON'T be automatically masked if var_x_values is not a masked array
    var_x_values[0] = np.nan
    var_x_values = np.ma.array(var_x_values)
    var_x_values.mask = np.zeros(len_time, dtype=bool)
    # SAFE: This value WILL be masked
    var_x_values.mask[1] = True
    var_x[:] = var_x_values

    # A group can have its own dimension or override a parent dimension
    group_one.createDimension("dim_0", 100)

    root.close()



Reading
#######

.. code-block:: python

    import netCDF4 as nc
    import numpy as np

    root = nc.Dataset("file.nc", mode='r')

    # Read an attribute
    history = root.history

    # Read a variable
    latitude = np.array(root["latitude"])

    # Read a variable in a group
    longitude = np.array(root["group_one"]["longitude"])

    longitude = np.array(root["group_one"]["var_x"])

    root.close()

.. role:: raw-html(raw)
    :format: html

.. note::
    Valid range of :code:`fill_value`: :raw-html:`<br />`
    * :code:`[-2 ** (N-1) : (2 ** (N-1)) - 1]` for signed type :raw-html:`<br />`
    * :code:`[0 : (2 ** N) - 1]` for unsigned type :raw-html:`<br />`
    With N = Number of bits


| The use of attributes (:code:`add_offset`) and (:code:`scale_factor`) set the lower and upper limits of the encoded variable, as well as its resolution.
| By default (:code:`add_offset = 0`).
| For example, if we choose an unsigned integer on 16 bits (:code:`np.uint16`) with a scale factor of 1e-2 (:code:`scale_factor=1e-2`) and an offset of 100 (:code:`add_offset=100`), then the valid range is:


.. code-block:: python

    [100 - 0 * 1e-2: 100 + ((2**16) - 1) * 1e-2] = [100 : 755.35]


.. warning::
    If the input contains NaN values, it won't be automatically masked when writting the netcdf variable unless the input is a masked array (see the example above, weird behavior).
    It is safer to explicitly mask the required sample of the input array before writting it in the netcdf file.
