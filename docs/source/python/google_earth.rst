Google Earth
============

Google earth files are written in KML (or KMZ = KML compressed archive).

The best way to generate a simple kml file is to use the **simplekml** package that you can install with:

.. code-block:: bash

    conda install -c conda-forge simplekml

Below is a basic example generating a simple kml file for google earth:

Rescaled photos used in this example:

- :download:`Photo Pont Neuf </assets/google_earth/pont_neuf.png>`: http://www.freemages.fr/browse/photo-351-toulouse-pont-neuf.html
    .. image:: /assets/google_earth/pont_neuf.png
        :width: 150pt
- :download:`Photo Pont Saint Pierre </assets/google_earth/pont_saint_pierre.png>`: http://www.freemages.fr/browse/photo-237-pont-st-pierre.html
    .. image:: /assets/google_earth/pont_saint_pierre.png
        :width: 150pt
- :download:`Photo Pont des Catalans </assets/google_earth/pont_des_catalans.png>`: By Maxime Lafage — Travail personnel, CC BY-SA 3.0, https://commons.wikimedia.org/w/index.php?curid=21617340
    .. image:: /assets/google_earth/pont_des_catalans.png
        :width: 150pt
- :download:`Photo Pont Saint Michel </assets/google_earth/pont_saint_michel.png>`: By Serydicule — Travail personnel, CC BY-SA 3.0, https://commons.wikimedia.org/w/index.php?curid=4268437
    .. image:: /assets/google_earth/pont_saint_michel.png
        :width: 150pt

.. code-block:: python

    import simplekml

    # Bridges Coordinates ((Lon, Lat), img_path)
    BRIDGES_COORDS = {
        "Pont Neuf": ((1.439001277058114, 43.59926197664164), "pont_neuf.png"),
        "Pont Saint-Pierre": ((1.434699883317263, 43.60213926211102), "pont_saint_pierre.png"),
        "Pont Saint-Michel": ((1.437952346786093, 43.59238591463478), "pont_saint_michel.png"),
        "Pont des Catalans": ((1.427956087350026, 43.60311850398945), "pont_des_catalans.png"),
    }

    # Parcs Coordinates ((Lon0, Lat0), (Lon1, Lat1), ...)
    PARCS_POLYGONS = {
        "Prairie des filtres": [
            (1.437818056276741, 43.59893829861551),
            (1.435179725302889, 43.59283349466352),
            (1.43583380929208, 43.59276697714191),
            (1.437246891614994, 43.59456400502385),
            (1.437936652075082, 43.59666502555274),
            (1.43811494255768, 43.59766059035145),
            (1.437818056276741, 43.59893829861551)
        ],
        "Jardin des plantes": [
            (1.452354489577692, 43.59119785881381),
            (1.452659571836548, 43.59135000093642),
            (1.452122004633536, 43.5946235820627),
            (1.451817292056345, 43.59482726005042),
            (1.451707911270457, 43.59476125937645),
            (1.451509541177225, 43.59481733753255),
            (1.451300782103038, 43.59471257924214),
            (1.451458830464756, 43.59450710323053),
            (1.451421878391774, 43.59404188360258),
            (1.45013011773917, 43.59353787477147),
            (1.450346226785248, 43.59327238908971),
            (1.4488536541275, 43.59241223864972),
            (1.452354489577692, 43.59119785881381),
        ]
    }

    # Path From Capitol to Iles du Ramier
    PATHS_COORDS = {
        "Capitol To Iles du Ramier": [
            (1.447480733498216, 43.60481386672822),
            (1.445735295910373, 43.60376328940693),
            (1.445687936142166, 43.60045898926229),
            (1.443785127425334, 43.60034852819621),
            (1.44249978028699, 43.60006341259311),
            (1.440549726305658, 43.59953279021803),
            (1.440097984310407, 43.59774349832251),
            (1.440511134417293, 43.59469725181547),
            (1.440850363525377, 43.5922190664575),
            (1.439840653155942, 43.59224402025578),
            (1.43893960439148, 43.59049390337003),
        ]
    }

    root = simplekml.Kml()

    # Create folders/Documents
    bridges = root.newdocument(name="Bridges")
    parcs = root.newdocument(name="Parcs")

    # Check if a folder already exists
    already_exists = any([doc.name == "Bridges" for doc in root.containers])

    # Add Points in the 'bridges' document
    for bridge_name, (coords, img_path) in BRIDGES_COORDS.items():
        point = bridges.newpoint(name=bridge_name, coords=[coords])
        # Edit Point style
        point.style.labelstyle.color = simplekml.Color.red
        point.style.iconstyle.color = simplekml.Color.red
        # Adding the photo in the description
        pic = root.addfile(img_path)
        point.description = f'<img src="{pic}" alt="picture" width=300 height=225 align="left" />'

    # Add Linestrings in the 'root' document
    for path_name, coords in PATHS_COORDS.items():
        line = root.newlinestring(name=path_name)
        line.coords = coords
        line.linestyle.width = 5
        line.linestyle.color = simplekml.Color.yellow

    # Add Polygons in the 'parcs' document
    for parc_name, coords in PARCS_POLYGONS.items():
        pol = parcs.newpolygon(name=parc_name)
        pol.outerboundaryis = coords
        pol.style.linestyle.color = simplekml.Color.blue
        pol.style.linestyle.width = 5
        pol.style.polystyle.color = simplekml.Color.changealphaint(100, simplekml.Color.blue)

    # Save as KMZ
    root.savekmz("example.kmz", format = False)

**Results in Google Earth**:

.. image:: /assets/google_earth/kmz_example_screenshot.png
    :width: 500pt

------------------------------------------------------------

**Sources**:

- Google KML Tutorial: https://developers.google.com/kml/documentation/kml_tut
- https://simplekml.readthedocs.io/en/latest/index.html
- Photo Pont Neuf: http://www.freemages.fr/browse/photo-351-toulouse-pont-neuf.html
- Photo Pont Saint Pierre: http://www.freemages.fr/browse/photo-237-pont-st-pierre.html
- Photo Pont des Catalans: Par Maxime Lafage — Travail personnel, CC BY-SA 3.0, https://commons.wikimedia.org/w/index.php?curid=21617340
- Photo Pont Saint Michel: Par Serydicule — Travail personnel, CC BY-SA 3.0, https://commons.wikimedia.org/w/index.php?curid=4268437
