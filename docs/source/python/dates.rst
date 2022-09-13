Dates
=====

Get current date
################

.. code-block:: python

    import datetime as dt

    date = dt.datetime.now()


Date format Conversion
######################

In this section we'll consider the following date formats:

- **datetime.datetime**
- **numpy.datetime64**
- **Julian Day**: Number of decimal day elapsed since a reference date (often 1950, 1970 or 2000)
- **(Days, Seconds, Microseconds)**: elapsed since a reference date (often 1950, 1970 or 2000)
- **Unix epoch / Unix time / POSIX time / Unix timestamp**: Number of seconds elapsed since January 1, 1970 (midnight UTC/GMT)


datetime.datetime and numpy.datetime64
**************************************

.. code-block:: python

    import datetime as dt
    import numpy as np

    def dt64_to_dt(date: np.datetime64) -> dt.datetime:
        return date.astype('M8[ms]').astype('O')

    def dt_to_dt64(date: dt.datetime) -> np.datetime64:
        return np.datetime64(date)


Julian Day and (Days, Seconds, Microseconds)
********************************************

.. code-block:: python

    from typing import Tuple

    JulianDay = float
    DaySecUsec = Tuple[int, int, int]

    def daysecusec_to_jd(date:DaySecUsec) -> JulianDay:
        jdays = float(date[0])
        jdays += (date[1] / 86400.0)
        jdays += (date[2] / (86400.0 * 1e6))
        return jdays

    def jd_to_daysecusec(date:JulianDay) -> DaySecUsec:
        us_per_day = 24 * 3600 * 1e6
        julian_day_us = date * us_per_day
        nbdays = int(date)
        julian_day_us -= (nbdays * us_per_day)
        nbseconds = int(julian_day_us / 1e6)
        julian_day_us -= (nbseconds * 1e6)
        nbmicroseconds = int(julian_day_us)
        time_tuple = (nbdays, nbseconds, nbmicroseconds)
        return time_tuple


datetime.datetime and (Days, Seconds, Microseconds)
***************************************************

.. code-block:: python

    import datetime as dt
    from typing import Tuple

    DaySecUsec = Tuple[int, int, int]

    def daysecusec_to_dt(date:DaySecUsec, ref_date:dt.datetime=dt.datetime(1950, 1, 1)) -> dt.datetime:
        dt_obj = ref_date
        dt_obj += dt.timedelta(days=date[0])
        dt_obj += dt.timedelta(seconds=date[1])
        dt_obj += dt.timedelta(microseconds=date[2])
        return dt_obj

    def dt_to_daysecusec(date:dt.datetime, ref_date:dt.datetime=dt.datetime(1950, 1, 1)) -> DaySecUsec:
        delta = date - ref_date
        time_tuple = (delta.days, delta.seconds, delta.microseconds)
        return time_tuple


numpy.datetime64 and (Days, Seconds, Microseconds)
**************************************************

.. code-block:: python

    import numpy as np
    from typing import Tuple

    DaySecUsec = Tuple[int, int, int]

    def daysecusec_to_dt64(date:DaySecUsec, ref_date:np.datetime64=np.datetime64('1950')) -> np.datetime64:
        dt64_obj = ref_date
        dt64_obj += np.timedelta64(date[0], 'D')
        dt64_obj += np.timedelta64(date[1], 's')
        dt64_obj += np.timedelta64(date[2], 'us')
        return dt64_obj

    def dt64_to_daysecusec(date:np.datetime64, ref_date:np.datetime64=np.datetime64('1950')) -> DaySecUsec:
        delta = date - ref_date.astype("datetime64[us]")
        nbdays = int(delta / np.timedelta64(1, 'D'))
        delta = delta - np.timedelta64(nbdays, 'D')
        nbseconds = int(delta / np.timedelta64(1, 's'))
        delta = delta - np.timedelta64(nbseconds, 's')
        nbmicroseconds = int(delta / np.timedelta64(1, 'us'))
        time_tuple = (nbdays, nbseconds, nbmicroseconds)
        return time_tuple


datetime.datetime and Julian Days
*********************************

.. code-block:: python

    import datetime as dt

    JulianDay = float

    def jd_to_dt(date:JulianDay, ref_date:dt.datetime=dt.datetime(1950, 1, 1)) -> dt.datetime:
        return ref_date + dt.timedelta(days=date)

    def dt_to_jd(date:dt.datetime, ref_date:dt.datetime=dt.datetime(1950, 1, 1)) -> JulianDay:
        delta = date - ref_date
        jd = delta.days + (delta.seconds + (delta.microseconds / 1e6)) / 86400
        return jd


Unix Timestamp and datetime.datetime
************************************

.. code-block:: python

    import datetime as dt

    UnixTs = float

    def unixts_to_dt(date:UnixTs) -> dt.datetime:
        return dt.datetime.utcfromtimestamp(float(date))


    def dt_to_unixts(date:dt.datetime) -> UnixTs:
        dt_diff_obj = date - dt.datetime.utcfromtimestamp(0)
        secs = dt_diff_obj.total_seconds()
        return secs
