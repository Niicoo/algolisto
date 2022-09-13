Shapely
=======

You can install **shapely** package with the following command:

.. code-block:: bash

    conda install -c conda-forge shapely


**Basic example:**

.. code-block:: python

    import matplotlib.pyplot as plt
    from shapely.geometry import Polygon, Point, LineString, MultiPoint
    from shapely.ops import transform
    from pyproj import Transformer, CRS

    # Point
    point_in = Point(6, 3.5)
    point_out = Point(7, 6)

    # MultiPoint
    multipoint = MultiPoint([(4.5, 4.5), (4.5, 6), (3.5, 6.5), (4, 7), (5, 7), (5.5, 6.5)])

    # Line
    line = LineString([(1.5, 6), (1.5, 1.5), (4, 1.5), (5, 3), (5, 1)])

    # Polygon
    polygon = Polygon([(1, 1), (2.5, 4), (4.5, 5), (7, 4), (7, 1), (5, 3), (3, 2), (3, 1)])

    # Circle
    radius = 2
    circle = point_in.buffer(radius)

    # Plot
    fig = plt.figure(figsize=(6, 4), tight_layout=True)
    ax1 = fig.add_subplot(111)
    ax1.grid()
    ax1.set_xlabel('x')
    ax1.set_ylabel('y')
    ax1.set_title("Shapely objects plot")
    ax1.scatter(*point_in.xy, marker='^', color='k', label="point IN")
    ax1.scatter(*point_out.xy, marker='^', color='g', label="point OUT")
    ax1.scatter([i.x for i in multipoint.geoms], [i.y for i in multipoint.geoms], marker='o', color='c', label="multipoint")
    ax1.plot(*line.xy, linewidth=2.0, color='m', label="line")
    ax1.plot(*polygon.exterior.xy, linewidth=2.0, color='r', label="polygon")
    ax1.plot(*circle.exterior.xy, linewidth=1.0, linestyle="--", color='b', label="circle")
    ax1.legend(loc='center left', bbox_to_anchor=(1.04, 0.5))
    ax1.axis('equal')
    fig.savefig("shapely.png")

    # Operations
    inter_line_polygon = line.intersection(polygon.boundary)
    area_polygon = polygon.area

    # Example of conversion If coordinates were corresponding to Lon/Lat or x/y
    CRS_4326 = CRS("epsg:4326") # WGS84 (lat/lon)
    CRS_3857 = CRS("epsg:3857") # Mercator (meter)
    T_LATLON_TO_M = Transformer.from_crs(CRS_4326, CRS_3857, always_xy=True)
    T_M_TO_LATLON = Transformer.from_crs(CRS_3857, CRS_4326, always_xy=True)

    polygon_lonlat = transform(T_M_TO_LATLON.transform, polygon)


.. image:: /assets/shapely/shapely.png
    :width: 450pt

Shapely includes a lot of features to calculate area, intersections, transformations, etc... you can see how it works using the `documentation <https://shapely.readthedocs.io/en/stable/manual.html>`_.

------------------------------------------------------------

**Sources**:

- Shapely documentation: https://shapely.readthedocs.io/en/stable/manual.html
- Pyproj: https://pyproj4.github.io/pyproj/stable/
