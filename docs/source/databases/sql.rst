SQL
===

Installing MySQL
################

https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-22-04

Installing PostgreSQL
#####################

https://www.postgresql.org/download/linux/ubuntu/


To quickly get a testing mysql or PostgreSql environment you can also use dockers:

.. code-block:: bash

    # PostgreSQL
    # Create and Run an instance of postgreSQL image latest version
    sudo docker run --name dock-postgres -e POSTGRES_PASSWORD=password -d postgres
    # Get an interactive PostgreSQL shell by attaching to the container and running the psql command
    sudo docker exec -it dock-postgres psql -U postgres -W

    # MySQL
    # Create and Run an instance of mysql image latest version
    sudo docker run --name dock-mysql -e MYSQL_ROOT_PASSWORD=password -d mysql:latest
    # Get an interactive MySQL shell by attaching to the container and running the mysql command
    sudo docker exec -it dock-mysql mysql -u root -p
    # Then once you get mysql prompt create a temporary database
    # CREATE DATABASE testdb;
    # use testdb;


Creating a Table
################

As an example, we'll create a table based on the following data:

Table name: **Persons**

+---------------+------------------+-----------------+------------------+------------+------------+---------------+
|   Person_ID   |    First Name    |     Surname     |    Birth date    |   Weight   |   Height   |   Earthling   |
+===============+==================+=================+==================+============+============+===============+
|      1        |      Robert      |    Dubouchard   |    19/12/1975    |    89.1    |    134     |     True      |
+---------------+------------------+-----------------+------------------+------------+------------+---------------+
|      2        |      Sarah       |    Dupont       |    14/01/1991    |    69.3    |    175.4   |     True      |
+---------------+------------------+-----------------+------------------+------------+------------+---------------+
|      3        |      Xioului     |    Wukikoku     |    02/11/2003    |    NULL    |    165.9   |     False     |
+---------------+------------------+-----------------+------------------+------------+------------+---------------+
|      4        |      Xiao        |    Li           |    15/03/1987    |    39.7    |    123.8   |     False     |
+---------------+------------------+-----------------+------------------+------------+------------+---------------+
|      5        |      Robert      |    Amomo        |    13/12/1998    |    123.2   |    210.3   |     True      |
+---------------+------------------+-----------------+------------------+------------+------------+---------------+
|      6        |      Jeanne      |    Klopens      |    19/11/1985    |    94.6    |    192.9   |     False     |
+---------------+------------------+-----------------+------------------+------------+------------+---------------+
|      7        |      Ludovic     |    Perudo       |    09/09/1999    |    46.2    |    135.8   |     True      |
+---------------+------------------+-----------------+------------------+------------+------------+---------------+
|      8        |      Bryan       |    Baboulo      |    12/10/2005    |    51.9    |    145.7   |     True      |
+---------------+------------------+-----------------+------------------+------------+------------+---------------+
|      9        |      Hadia       |    Wuxiko       |    02/09/2001    |    71.2    |    174.3   |     False     |
+---------------+------------------+-----------------+------------------+------------+------------+---------------+
|      10       |      Youcef      |    Vicompte     |    31/01/1990    |    NULL    |    93.3    |     True      |
+---------------+------------------+-----------------+------------------+------------+------------+---------------+
|      11       |      Jade        |    Haynak       |    08/04/1970    |    91.3    |    190.4   |     False     |
+---------------+------------------+-----------------+------------------+------------+------------+---------------+
|      12       |      Mehdi       |    Mezizi       |    05/06/1996    |    24.8    |    75.7    |     False     |
+---------------+------------------+-----------------+------------------+------------+------------+---------------+
|      13       |      Claire      |    Suniza       |    23/08/1973    |    37.7    |    112.1   |     False     |
+---------------+------------------+-----------------+------------------+------------+------------+---------------+
|      14       |      Robert      |    Blanchard    |    30/07/1978    |    43.8    |    156.6   |     True      |
+---------------+------------------+-----------------+------------------+------------+------------+---------------+
|      15       |      Hadia       |    Emopolo      |    29/03/1956    |    66.6    |    169.9   |     False     |
+---------------+------------------+-----------------+------------------+------------+------------+---------------+
|      16       |      Xiao        |    Quezara      |    26/10/2000    |    60.7    |    147.1   |     True      |
+---------------+------------------+-----------------+------------------+------------+------------+---------------+


Let's also consider this table containing additional data for some of the people:

Table name: **Persons_Comments**

+------------------+-----------------+-------------------------------------------------------------------------+
|    Comment_ID    |    Person_ID    |    Comment                                                              |
+==================+=================+=========================================================================+
|        1         |        2        |    Not a very nice person                                               |
+------------------+-----------------+-------------------------------------------------------------------------+
|        2         |        2        |    Likes to read with the head upside down                              |
+------------------+-----------------+-------------------------------------------------------------------------+
|        3         |        4        |    Don't talk about piano with her !                                    |
+------------------+-----------------+-------------------------------------------------------------------------+
|        4         |        4        |    Such a lovely Martian                                                |
+------------------+-----------------+-------------------------------------------------------------------------+
|        5         |        4        |    Goes to pee every 30 minutes                                         |
+------------------+-----------------+-------------------------------------------------------------------------+
|        6         |        6        |    Do not trust this girl                                               |
+------------------+-----------------+-------------------------------------------------------------------------+
|        7         |        12       |    Likes to drink wine mixed with kombucha                              |
+------------------+-----------------+-------------------------------------------------------------------------+
|        8         |        13       |    A child in an adult body                                             |
+------------------+-----------------+-------------------------------------------------------------------------+
|        9         |        14       |    Doesn't like to be bothered while eating                             |
+------------------+-----------------+-------------------------------------------------------------------------+
|        10        |        14       |    A cute weirdo                                                        |
+------------------+-----------------+-------------------------------------------------------------------------+
|        11        |        25       |    This person does't exist                                             |
+------------------+-----------------+-------------------------------------------------------------------------+
|        12        |        27       |    I already like the person with this ID even if he/she doesn't exist  |
+------------------+-----------------+-------------------------------------------------------------------------+


Creating the tables
*******************

.. tabs::

    .. tab:: PostgreSQL

        .. code-block:: sql

            CREATE TABLE Persons (
                PERSON_ID       INTEGER PRIMARY KEY generated by default as identity,
                "First Name"    VARCHAR(255) NOT NULL,
                Surname         VARCHAR(255),
                "Birth date"    DATE,
                Weight          FLOAT,
                Height          FLOAT,
                Earthling       BOOL DEFAULT TRUE
            );
            CREATE TABLE Persons_Comments (
                COMMENT_ID      INTEGER PRIMARY KEY generated by default as identity,
                PERSON_ID       INTEGER,
                Comment         text
            );
            --To add a constraint on PERSON_ID to be sure it matchs an existing PERSON_ID in the Persons table.
            --FOREIGN KEY (PERSON_ID) REFERENCES Persons(PERSON_ID)

    .. tab:: MySQL

        .. code-block:: sql

            CREATE TABLE Persons (
                PERSON_ID INT PRIMARY KEY AUTO_INCREMENT,
                `First Name` VARCHAR(255) NOT NULL,
                Surname VARCHAR(255),
                `Birth date` DATE,
                Weight FLOAT,
                Height FLOAT,
                Earthling BOOLEAN DEFAULT TRUE
            );
            CREATE TABLE Persons_Comments (
                COMMENT_ID INT PRIMARY KEY AUTO_INCREMENT,
                PERSON_ID INT,
                Comment TEXT
            );
            --To add a constraint on PERSON_ID to be sure it matchs an existing PERSON_ID in the Persons table:
            --FOREIGN KEY (PERSON_ID) REFERENCES Persons(PERSON_ID)


You can find more info about data types available here:

- PostgreSQL: https://www.postgresql.org/docs/current/datatype.html
- MySQL: https://dev.mysql.com/doc/refman/8.0/en/data-types.html

.. important::
    :code:`FOREIGN KEY` : This constraint will prevent you from creating values with an reference key that doesn't exist. Before to delete a parent row, you will first need to delete all references in childs tables.


.. important::
    Not a good practice to set column names with space inside.

We added a field containing a primary key auto with auto incremented value.

It's a better practice to set a dedicaded column as a primary key but you can also use the combination of multiple column as a primary key. Example below:

.. tabs::

    .. tab:: PostgreSQL

        .. code-block:: sql

            CREATE TABLE Persons (
                "First Name"    VARCHAR(255) NOT NULL,
                Surname         VARCHAR(255),
                "Birth date"    DATE,
                Weight          FLOAT,
                Height          FLOAT,
                Earthling       BOOL DEFAULT TRUE,
                PRIMARY KEY     ("First Name", Surname)
            );

    .. tab:: MySQL

        .. code-block:: sql

            CREATE TABLE Persons (
                `First Name`    VARCHAR(255) NOT NULL,
                Surname         VARCHAR(255),
                `Birth date`    DATETIME,
                Weight          FLOAT,
                Height          FLOAT,
                Earthling       BOOLEAN DEFAULT TRUE,
                PRIMARY KEY     (`First Name`, Surname)
            );


Data Manipulation: INSERT, UPDATE, DELETE
*****************************************

INSERT
------

.. tabs::

    .. tab:: PostgreSQL

        .. code-block:: sql

            INSERT INTO Persons (
                "First Name",
                Surname,
                "Birth date",
                Weight,
                Height,
                Earthling
            )
            VALUES
                ('Robert',  'Dubouchard', '1975-12-19',  89.1,    134,     True),
                ('Sarah',   'Dupont',     '1991-01-14',  69.3,    175.4,   True),     
                ('Xioului', 'Wukikoku',   '2003-11-02',  54.8,    165.9,   False),    
                ('Xiao',    'Li',         '1987-03-15',  39.7,    123.8,   False),    
                ('Robert',  'Amomo',      '1998-12-13',  123.2,   210.3,   True),     
                ('Jeanne',  'Klopens',    '1985-11-19',  94.6,    192.9,   False),    
                ('Ludovic', 'Perudo',     '1999-09-09',  46.2,    135.8,   True),     
                ('Bryan',   'Baboulo',    '2005-10-12',  51.9,    145.7,   True),     
                ('Hadia',   'Wuxiko',     '2001-09-02',  71.2,    174.3,   False),    
                ('Youcef',  'Vicompte',   '1990-01-31',  34.0,    93.3,    True),     
                ('Jade',    'Haynak',     '1970-04-08',  91.3,    190.4,   False),    
                ('Mehdi',   'Mezizi',     '1996-06-05',  24.8,    75.7,    False),    
                ('Claire',  'Suniza',     '1973-08-23',  37.7,    112.1,   False),    
                ('Robert',  'Blanchard',  '1978-07-30',  43.8,    156.6,   True),     
                ('Hadia',   'Emopolo',    '1956-03-29',  66.6,    169.9,   False),    
                ('Xiao',    'Quezara',    '2000-10-26',  60.7,    147.1,   True);

            INSERT INTO Persons_Comments (
                PERSON_ID,
                Comment
            )
            VALUES
                ((SELECT PERSON_ID FROM Persons WHERE "First Name" = 'Sarah'   AND Surname = 'Dupont'),    'Not a very nice person'),
                ((SELECT PERSON_ID FROM Persons WHERE "First Name" = 'Sarah'   AND Surname = 'Dupont'),    'Likes to read with the head upside down'),
                ((SELECT PERSON_ID FROM Persons WHERE "First Name" = 'Xiao'    AND Surname = 'Li'),        'Don''t talk about piano with her !'),
                ((SELECT PERSON_ID FROM Persons WHERE "First Name" = 'Xiao'    AND Surname = 'Li'),        'Such a lovely Martian'),
                ((SELECT PERSON_ID FROM Persons WHERE "First Name" = 'Xiao'    AND Surname = 'Li'),        'Goes to pee every 30 minutes'),
                ((SELECT PERSON_ID FROM Persons WHERE "First Name" = 'Jade'    AND Surname = 'Haynak'),    'Do not trust this girl'),
                ((SELECT PERSON_ID FROM Persons WHERE "First Name" = 'Mehdi'   AND Surname = 'Mezizi'),    'Likes to drink wine mixed with kombucha'),
                ((SELECT PERSON_ID FROM Persons WHERE "First Name" = 'Claire'  AND Surname = 'Suniza'),    'A child in an adult body'),
                ((SELECT PERSON_ID FROM Persons WHERE "First Name" = 'Robert'  AND Surname = 'Blanchard'), 'Doesn''t like to be bothered while eating'),
                ((SELECT PERSON_ID FROM Persons WHERE "First Name" = 'Robert'  AND Surname = 'Blanchard'), 'A cute weirdo'),
                (25, 'This person does''t exist'),
                (27, 'I already like the person with this ID even if he/she doesn''t exist');

    .. tab:: MySQL

        .. code-block:: sql

            INSERT INTO Persons (
                `First Name`,
                Surname,
                `Birth date`,
                Weight,
                Height,
                Earthling
            )
            VALUES
                ('Robert',  'Dubouchard', '1975-12-19',  89.1,    134,     True),
                ('Sarah',   'Dupont',     '1991-01-14',  69.3,    175.4,   True),     
                ('Xioului', 'Wukikoku',   '2003-11-02',  54.8,    165.9,   False),    
                ('Xiao',    'Li',         '1987-03-15',  39.7,    123.8,   False),    
                ('Robert',  'Amomo',      '1998-12-13',  123.2,   210.3,   True),     
                ('Jeanne',  'Klopens',    '1985-11-19',  94.6,    192.9,   False),    
                ('Ludovic', 'Perudo',     '1999-09-09',  46.2,    135.8,   True),     
                ('Bryan',   'Baboulo',    '2005-10-12',  51.9,    145.7,   True),     
                ('Hadia',   'Wuxiko',     '2001-09-02',  71.2,    174.3,   False),    
                ('Youcef',  'Vicompte',   '1990-01-31',  34.0,    93.3,    True),     
                ('Jade',    'Haynak',     '1970-04-08',  91.3,    190.4,   False),    
                ('Mehdi',   'Mezizi',     '1996-06-05',  24.8,    75.7,    False),    
                ('Claire',  'Suniza',     '1973-08-23',  37.7,    112.1,   False),    
                ('Robert',  'Blanchard',  '1978-07-30',  43.8,    156.6,   True),     
                ('Hadia',   'Emopolo',    '1956-03-29',  66.6,    169.9,   False),    
                ('Xiao',    'Quezara',    '2000-10-26',  60.7,    147.1,   True);

                INSERT INTO Persons_Comments (
                    PERSON_ID,
                    Comment
                )
                VALUES
                    ((SELECT PERSON_ID FROM Persons WHERE `First Name` = 'Sarah'   AND Surname = 'Dupont'),    'Not a very nice person'),
                    ((SELECT PERSON_ID FROM Persons WHERE `First Name` = 'Sarah'   AND Surname = 'Dupont'),    'Likes to read with the head upside down'),
                    ((SELECT PERSON_ID FROM Persons WHERE `First Name` = 'Xiao'    AND Surname = 'Li'),        'Don''t talk about piano with her !'),
                    ((SELECT PERSON_ID FROM Persons WHERE `First Name` = 'Xiao'    AND Surname = 'Li'),        'Such a lovely Martian'),
                    ((SELECT PERSON_ID FROM Persons WHERE `First Name` = 'Xiao'    AND Surname = 'Li'),        'Goes to pee every 30 minutes'),
                    ((SELECT PERSON_ID FROM Persons WHERE `First Name` = 'Jade'    AND Surname = 'Haynak'),    'Do not trust this girl'),
                    ((SELECT PERSON_ID FROM Persons WHERE `First Name` = 'Mehdi'   AND Surname = 'Mezizi'),    'Likes to drink wine mixed with kombucha'),
                    ((SELECT PERSON_ID FROM Persons WHERE `First Name` = 'Claire'  AND Surname = 'Suniza'),    'A child in an adult body'),
                    ((SELECT PERSON_ID FROM Persons WHERE `First Name` = 'Robert'  AND Surname = 'Blanchard'), 'Doesn''t like to be bothered while eating'),
                    ((SELECT PERSON_ID FROM Persons WHERE `First Name` = 'Robert'  AND Surname = 'Blanchard'), 'A cute weirdo'),
                    (25, 'This person does''t exist'),
                    (27, 'I already like the person with this ID even if he/she doesn''t exist');


.. important::
    Character constants need single quotes ! (Single quotes are escaped by doubling them up)

UPDATE
------

.. code-block:: sql

    UPDATE Persons_Comments
    SET comment = 'Such a lovely Martian, am I in love ?'
    WHERE Comment_ID = 4;

DELETE
------

.. code-block:: sql

    DELETE FROM Persons_Comments
    WHERE Comment_ID = 4;


Example using SELECT FROM, WHERE, LIKE, ORDER BY
################################################

SELECT: This is the primary command used to retrieve data from a database.
WHERE: To extract specific data from a database.
ORDER BY: To sort data using ASC or DESC keyword.
LIKE: To match a pattern

.. tabs::

    .. tab:: PostgreSQL

        .. code-block:: sql

            SELECT "First Name", Surname, "Birth date"
            FROM Persons
            WHERE Earthling = True AND "First Name" LIKE '%o%'
            ORDER BY "Birth date" ASC;

    .. tab:: MySQL

        .. code-block:: sql

            SELECT `First Name`, Surname, `Birth date`
            FROM Persons
            WHERE Earthling = True AND `First Name` LIKE '%o%'
            ORDER BY `Birth date` ASC;

Output:

+------------+------------+------------+
| First Name | Surname    | Birth date |
+============+============+============+
| Robert     | Dubouchard | 1975-12-19 |
+------------+------------+------------+
| Robert     | Blanchard  | 1978-07-30 |
+------------+------------+------------+
| Youcef     | Vicompte   | 1990-01-31 |
+------------+------------+------------+
| Robert     | Amomo      | 1998-12-13 |
+------------+------------+------------+
| Ludovic    | Perudo     | 1999-09-09 |
+------------+------------+------------+
| Xiao       | Quezara    | 2000-10-26 |
+------------+------------+------------+


Example using built-in functions and subquery
#############################################

SQL has a variety of built-in functions that are commonly used in queries. Some of the most important main built-in functions in SQL are:

- COUNT(): Used to count the number of rows that match a specific condition.
- SUM(): Used to calculate the sum of a column.
- AVG(): Used to calculate the average of a column.
- MIN(): Used to find the minimum value of a column.
- MAX(): Used to find the maximum value of a column.
- ROUND(): Used to round a number to a specified number of decimal places.
- CONCAT(): Used to concatenate two or more strings together.
- UPPER(): Used to convert a string to uppercase.
- LOWER(): Used to convert a string to lowercase.
- SUBSTRING(): Used to extract a substring from a string.
- DATE(): Used to retrieve the current date.
- DATEPART(): Used to extract a specific part of a date, such as the year or month.

You can find a more exhaustive list of built in functions `here <https://www.tutorialsteacher.com/sqlserver/builtin-functions>`_

Example:

.. code-block:: sql

    SELECT COUNT(*) FROM Persons
    WHERE Height > (SELECT AVG(Height) FROM Persons);

Output: 8


Example joining tables
######################

SQL allows you to join two or more tables to retrieve data that is spread across multiple tables.

Inner Join
**********

Returns only the matching rows from both tables.

.. code-block:: sql

    SELECT Persons.*, Persons_Comments.Comment FROM Persons
    INNER JOIN Persons_Comments
    ON Persons.PERSON_ID = Persons_Comments.PERSON_ID;

Output:

+-----------+------------+-----------+------------+--------+--------+-----------+------------------------------------------+
| PERSON_ID | First Name | Surname   | Birth date | Weight | Height | Earthling | Comment                                  |
+===========+============+===========+============+========+========+===========+==========================================+
|         2 | Sarah      | Dupont    | 1991-01-14 |   69.3 |  175.4 |         1 | Not a very nice person                   |
+-----------+------------+-----------+------------+--------+--------+-----------+------------------------------------------+
|         2 | Sarah      | Dupont    | 1991-01-14 |   69.3 |  175.4 |         1 | Likes to read with the head upside down  |
+-----------+------------+-----------+------------+--------+--------+-----------+------------------------------------------+
|         4 | Xiao       | Li        | 1987-03-15 |   39.7 |  123.8 |         0 | Don't talk about piano with her !        |
+-----------+------------+-----------+------------+--------+--------+-----------+------------------------------------------+
|         4 | Xiao       | Li        | 1987-03-15 |   39.7 |  123.8 |         0 | Such a lovely Martian                    |
+-----------+------------+-----------+------------+--------+--------+-----------+------------------------------------------+
|         4 | Xiao       | Li        | 1987-03-15 |   39.7 |  123.8 |         0 | Goes to pee every 30 minutes             |
+-----------+------------+-----------+------------+--------+--------+-----------+------------------------------------------+
|        11 | Jade       | Haynak    | 1970-04-08 |   91.3 |  190.4 |         0 | Do not trust this girl                   |
+-----------+------------+-----------+------------+--------+--------+-----------+------------------------------------------+
|        12 | Mehdi      | Mezizi    | 1996-06-05 |   24.8 |   75.7 |         0 | Likes to drink wine mixed with kombucha  |
+-----------+------------+-----------+------------+--------+--------+-----------+------------------------------------------+
|        13 | Claire     | Suniza    | 1973-08-23 |   37.7 |  112.1 |         0 | A child in an adult body                 |
+-----------+------------+-----------+------------+--------+--------+-----------+------------------------------------------+
|        14 | Robert     | Blanchard | 1978-07-30 |   43.8 |  156.6 |         1 | Doesn't like to be bothered while eating |
+-----------+------------+-----------+------------+--------+--------+-----------+------------------------------------------+
|        14 | Robert     | Blanchard | 1978-07-30 |   43.8 |  156.6 |         1 | A cute weirdo                            |
+-----------+------------+-----------+------------+--------+--------+-----------+------------------------------------------+


Right [OUTER] Join
******************

Returns all the rows from the right table and matching rows from the left table.

.. code-block:: sql

    SELECT Persons.*, Persons_Comments.Comment FROM Persons
    RIGHT JOIN Persons_Comments
    ON Persons.PERSON_ID = Persons_Comments.PERSON_ID;

Output:

+-----------+------------+-----------+------------+--------+--------+-----------+---------------------------------------------------------------------+
| PERSON_ID | First Name | Surname   | Birth date | Weight | Height | Earthling | Comment                                                             |
+===========+============+===========+============+========+========+===========+=====================================================================+
|         2 | Sarah      | Dupont    | 1991-01-14 |   69.3 |  175.4 |         1 | Not a very nice person                                              |
+-----------+------------+-----------+------------+--------+--------+-----------+---------------------------------------------------------------------+
|         2 | Sarah      | Dupont    | 1991-01-14 |   69.3 |  175.4 |         1 | Likes to read with the head upside down                             |
+-----------+------------+-----------+------------+--------+--------+-----------+---------------------------------------------------------------------+
|         4 | Xiao       | Li        | 1987-03-15 |   39.7 |  123.8 |         0 | Don't talk about piano with her !                                   |
+-----------+------------+-----------+------------+--------+--------+-----------+---------------------------------------------------------------------+
|         4 | Xiao       | Li        | 1987-03-15 |   39.7 |  123.8 |         0 | Such a lovely Martian                                               |
+-----------+------------+-----------+------------+--------+--------+-----------+---------------------------------------------------------------------+
|         4 | Xiao       | Li        | 1987-03-15 |   39.7 |  123.8 |         0 | Goes to pee every 30 minutes                                        |
+-----------+------------+-----------+------------+--------+--------+-----------+---------------------------------------------------------------------+
|        11 | Jade       | Haynak    | 1970-04-08 |   91.3 |  190.4 |         0 | Do not trust this girl                                              |
+-----------+------------+-----------+------------+--------+--------+-----------+---------------------------------------------------------------------+
|        12 | Mehdi      | Mezizi    | 1996-06-05 |   24.8 |   75.7 |         0 | Likes to drink wine mixed with kombucha                             |
+-----------+------------+-----------+------------+--------+--------+-----------+---------------------------------------------------------------------+
|        13 | Claire     | Suniza    | 1973-08-23 |   37.7 |  112.1 |         0 | A child in an adult body                                            |
+-----------+------------+-----------+------------+--------+--------+-----------+---------------------------------------------------------------------+
|        14 | Robert     | Blanchard | 1978-07-30 |   43.8 |  156.6 |         1 | Doesn't like to be bothered while eating                            |
+-----------+------------+-----------+------------+--------+--------+-----------+---------------------------------------------------------------------+
|        14 | Robert     | Blanchard | 1978-07-30 |   43.8 |  156.6 |         1 | A cute weirdo                                                       |
+-----------+------------+-----------+------------+--------+--------+-----------+---------------------------------------------------------------------+
|      NULL | NULL       | NULL      | NULL       |   NULL |   NULL |      NULL | This Person does't exist                                            |
+-----------+------------+-----------+------------+--------+--------+-----------+---------------------------------------------------------------------+
|      NULL | NULL       | NULL      | NULL       |   NULL |   NULL |      NULL | I already like the person with this ID even if he/she doesn't exist |
+-----------+------------+-----------+------------+--------+--------+-----------+---------------------------------------------------------------------+

.. note::
    Note that **PERSON_ID** is **NULL** for the 2 comments with non existing persons.

You can solve that problem by using CASE statement:

.. tabs::

    .. tab:: PostgreSQL

        .. code-block:: sql

            SELECT
                CASE 
                    WHEN Persons.PERSON_ID IS NULL THEN Persons_Comments.PERSON_ID 
                    WHEN Persons_Comments.PERSON_ID IS NULL THEN Persons.PERSON_ID 
                    WHEN Persons.PERSON_ID = Persons_Comments.PERSON_ID THEN Persons.PERSON_ID 
                    ELSE -1
                END AS PERSON_ID,
                "First Name",
                Surname,
                "Birth date",
                Weight,
                Height,
                Earthling,
                Persons_Comments.Comment
            FROM Persons
            RIGHT JOIN Persons_Comments
            ON Persons.PERSON_ID = Persons_Comments.PERSON_ID;

    .. tab:: MySQL

        .. code-block:: sql

            SELECT
                CASE 
                    WHEN Persons.PERSON_ID IS NULL THEN Persons_Comments.PERSON_ID 
                    WHEN Persons_Comments.PERSON_ID IS NULL THEN Persons.PERSON_ID 
                    WHEN Persons.PERSON_ID = Persons_Comments.PERSON_ID THEN Persons.PERSON_ID 
                    ELSE 'Error: values are different'
                END AS PERSON_ID,
                `First Name`,
                Surname,
                `Birth date`,
                Weight,
                Height,
                Earthling,
                Persons_Comments.Comment
            FROM Persons
            RIGHT JOIN Persons_Comments
            ON Persons.PERSON_ID = Persons_Comments.PERSON_ID;

Output:

+-----------+------------+-----------+------------+--------+--------+-----------+---------------------------------------------------------------------+
| PERSON_ID | First Name | Surname   | Birth date | Weight | Height | Earthling | Comment                                                             |
+===========+============+===========+============+========+========+===========+=====================================================================+
| 2         | Sarah      | Dupont    | 1991-01-14 |   69.3 |  175.4 |         1 | Not a very nice person                                              |
+-----------+------------+-----------+------------+--------+--------+-----------+---------------------------------------------------------------------+
| 2         | Sarah      | Dupont    | 1991-01-14 |   69.3 |  175.4 |         1 | Likes to read with the head upside down                             |
+-----------+------------+-----------+------------+--------+--------+-----------+---------------------------------------------------------------------+
| 4         | Xiao       | Li        | 1987-03-15 |   39.7 |  123.8 |         0 | Don't talk about piano with her !                                   |
+-----------+------------+-----------+------------+--------+--------+-----------+---------------------------------------------------------------------+
| 4         | Xiao       | Li        | 1987-03-15 |   39.7 |  123.8 |         0 | Such a lovely Martian                                               |
+-----------+------------+-----------+------------+--------+--------+-----------+---------------------------------------------------------------------+
| 4         | Xiao       | Li        | 1987-03-15 |   39.7 |  123.8 |         0 | Goes to pee every 30 minutes                                        |
+-----------+------------+-----------+------------+--------+--------+-----------+---------------------------------------------------------------------+
| 11        | Jade       | Haynak    | 1970-04-08 |   91.3 |  190.4 |         0 | Do not trust this girl                                              |
+-----------+------------+-----------+------------+--------+--------+-----------+---------------------------------------------------------------------+
| 12        | Mehdi      | Mezizi    | 1996-06-05 |   24.8 |   75.7 |         0 | Likes to drink wine mixed with kombucha                             |
+-----------+------------+-----------+------------+--------+--------+-----------+---------------------------------------------------------------------+
| 13        | Claire     | Suniza    | 1973-08-23 |   37.7 |  112.1 |         0 | A child in an adult body                                            |
+-----------+------------+-----------+------------+--------+--------+-----------+---------------------------------------------------------------------+
| 14        | Robert     | Blanchard | 1978-07-30 |   43.8 |  156.6 |         1 | Doesn't like to be bothered while eating                            |
+-----------+------------+-----------+------------+--------+--------+-----------+---------------------------------------------------------------------+
| 14        | Robert     | Blanchard | 1978-07-30 |   43.8 |  156.6 |         1 | A cute weirdo                                                       |
+-----------+------------+-----------+------------+--------+--------+-----------+---------------------------------------------------------------------+
| 25        | NULL       | NULL      | NULL       |   NULL |   NULL |      NULL | This Person does't exist                                            |
+-----------+------------+-----------+------------+--------+--------+-----------+---------------------------------------------------------------------+
| 27        | NULL       | NULL      | NULL       |   NULL |   NULL |      NULL | I already like the person with this ID even if he/she doesn't exist |
+-----------+------------+-----------+------------+--------+--------+-----------+---------------------------------------------------------------------+


Left [OUTER] Join
*****************

Returns all the rows from the left table and matching rows from the right table.

.. code-block:: sql

    SELECT Persons.*, Persons_Comments.COMMENT_ID , Persons_Comments.Comment FROM Persons
    LEFT JOIN Persons_Comments
    ON Persons.PERSON_ID = Persons_Comments.PERSON_ID;

+-----------+------------+------------+------------+--------+--------+-----------+------------+------------------------------------------+
| PERSON_ID | First Name | Surname    | Birth date | Weight | Height | Earthling | COMMENT_ID | Comment                                  |
+===========+============+============+============+========+========+===========+============+==========================================+
|         1 | Robert     | Dubouchard | 1975-12-19 |   89.1 |    134 |         1 |       NULL | NULL                                     |
+-----------+------------+------------+------------+--------+--------+-----------+------------+------------------------------------------+
|         2 | Sarah      | Dupont     | 1991-01-14 |   69.3 |  175.4 |         1 |          2 | Likes to read with the head upside down  |
+-----------+------------+------------+------------+--------+--------+-----------+------------+------------------------------------------+
|         2 | Sarah      | Dupont     | 1991-01-14 |   69.3 |  175.4 |         1 |          1 | Not a very nice person                   |
+-----------+------------+------------+------------+--------+--------+-----------+------------+------------------------------------------+
|         3 | Xioului    | Wukikoku   | 2003-11-02 |   54.8 |  165.9 |         0 |       NULL | NULL                                     |
+-----------+------------+------------+------------+--------+--------+-----------+------------+------------------------------------------+
|         4 | Xiao       | Li         | 1987-03-15 |   39.7 |  123.8 |         0 |          5 | Goes to pee every 30 minutes             |
+-----------+------------+------------+------------+--------+--------+-----------+------------+------------------------------------------+
|         4 | Xiao       | Li         | 1987-03-15 |   39.7 |  123.8 |         0 |          4 | Such a lovely Martian                    |
+-----------+------------+------------+------------+--------+--------+-----------+------------+------------------------------------------+
|         4 | Xiao       | Li         | 1987-03-15 |   39.7 |  123.8 |         0 |          3 | Don't talk about piano with her !        |
+-----------+------------+------------+------------+--------+--------+-----------+------------+------------------------------------------+
|         5 | Robert     | Amomo      | 1998-12-13 |  123.2 |  210.3 |         1 |       NULL | NULL                                     |
+-----------+------------+------------+------------+--------+--------+-----------+------------+------------------------------------------+
|         6 | Jeanne     | Klopens    | 1985-11-19 |   94.6 |  192.9 |         0 |       NULL | NULL                                     |
+-----------+------------+------------+------------+--------+--------+-----------+------------+------------------------------------------+
|         7 | Ludovic    | Perudo     | 1999-09-09 |   46.2 |  135.8 |         1 |       NULL | NULL                                     |
+-----------+------------+------------+------------+--------+--------+-----------+------------+------------------------------------------+
|         8 | Bryan      | Baboulo    | 2005-10-12 |   51.9 |  145.7 |         1 |       NULL | NULL                                     |
+-----------+------------+------------+------------+--------+--------+-----------+------------+------------------------------------------+
|         9 | Hadia      | Wuxiko     | 2001-09-02 |   71.2 |  174.3 |         0 |       NULL | NULL                                     |
+-----------+------------+------------+------------+--------+--------+-----------+------------+------------------------------------------+
|        10 | Youcef     | Vicompte   | 1990-01-31 |     34 |   93.3 |         1 |       NULL | NULL                                     |
+-----------+------------+------------+------------+--------+--------+-----------+------------+------------------------------------------+
|        11 | Jade       | Haynak     | 1970-04-08 |   91.3 |  190.4 |         0 |          6 | Do not trust this girl                   |
+-----------+------------+------------+------------+--------+--------+-----------+------------+------------------------------------------+
|        12 | Mehdi      | Mezizi     | 1996-06-05 |   24.8 |   75.7 |         0 |          7 | Likes to drink wine mixed with kombucha  |
+-----------+------------+------------+------------+--------+--------+-----------+------------+------------------------------------------+
|        13 | Claire     | Suniza     | 1973-08-23 |   37.7 |  112.1 |         0 |          8 | A child in an adult body                 |
+-----------+------------+------------+------------+--------+--------+-----------+------------+------------------------------------------+
|        14 | Robert     | Blanchard  | 1978-07-30 |   43.8 |  156.6 |         1 |         10 | A cute weirdo                            |
+-----------+------------+------------+------------+--------+--------+-----------+------------+------------------------------------------+
|        14 | Robert     | Blanchard  | 1978-07-30 |   43.8 |  156.6 |         1 |          9 | Doesn't like to be bothered while eating |
+-----------+------------+------------+------------+--------+--------+-----------+------------+------------------------------------------+
|        15 | Hadia      | Emopolo    | 1956-03-29 |   66.6 |  169.9 |         0 |       NULL | NULL                                     |
+-----------+------------+------------+------------+--------+--------+-----------+------------+------------------------------------------+
|        16 | Xiao       | Quezara    | 2000-10-26 |   60.7 |  147.1 |         1 |       NULL | NULL                                     |
+-----------+------------+------------+------------+--------+--------+-----------+------------+------------------------------------------+

If you don't want to create multiple rows for each comment on the same person, you can use the **GROUP_CONCAT** method in Mysql or the **array_agg** in psql as below:

.. tabs::

    .. tab:: PostgreSQL

        .. code-block:: sql

            SELECT
                Persons.*,
                array_agg(Persons_Comments.COMMENT_ID) AS comments_ids,
                array_agg(Persons_Comments.Comment) AS comments
            FROM Persons
            LEFT JOIN Persons_Comments
            ON Persons.PERSON_ID = Persons_Comments.PERSON_ID
            GROUP BY Persons.PERSON_ID;

    .. tab:: MySQL

        .. code-block:: sql

            SELECT
                Persons.*,
                GROUP_CONCAT(Persons_Comments.COMMENT_ID) AS comments_ids,
                GROUP_CONCAT(Persons_Comments.Comment) AS comments
            FROM Persons
            LEFT JOIN Persons_Comments
            ON Persons.PERSON_ID = Persons_Comments.PERSON_ID
            GROUP BY Persons.PERSON_ID;

+-----------+------------+------------+------------+--------+--------+-----------+--------------+--------------------------------------------------------------------------------------+
| PERSON_ID | First Name | Surname    | Birth date | Weight | Height | Earthling | comments_ids | comments                                                                             |
+===========+============+============+============+========+========+===========+==============+======================================================================================+
|         1 | Robert     | Dubouchard | 1975-12-19 |   89.1 |    134 |         1 | NULL         | NULL                                                                                 |
+-----------+------------+------------+------------+--------+--------+-----------+--------------+--------------------------------------------------------------------------------------+
|         2 | Sarah      | Dupont     | 1991-01-14 |   69.3 |  175.4 |         1 | 2,1          | Likes to read with the head upside down,Not a very nice person                       |
+-----------+------------+------------+------------+--------+--------+-----------+--------------+--------------------------------------------------------------------------------------+
|         3 | Xioului    | Wukikoku   | 2003-11-02 |   54.8 |  165.9 |         0 | NULL         | NULL                                                                                 |
+-----------+------------+------------+------------+--------+--------+-----------+--------------+--------------------------------------------------------------------------------------+
|         4 | Xiao       | Li         | 1987-03-15 |   39.7 |  123.8 |         0 | 5,4,3        | Goes to pee every 30 minutes,Such a lovely Martian,Don't talk about piano with her ! |
+-----------+------------+------------+------------+--------+--------+-----------+--------------+--------------------------------------------------------------------------------------+
|         5 | Robert     | Amomo      | 1998-12-13 |  123.2 |  210.3 |         1 | NULL         | NULL                                                                                 |
+-----------+------------+------------+------------+--------+--------+-----------+--------------+--------------------------------------------------------------------------------------+
|         6 | Jeanne     | Klopens    | 1985-11-19 |   94.6 |  192.9 |         0 | NULL         | NULL                                                                                 |
+-----------+------------+------------+------------+--------+--------+-----------+--------------+--------------------------------------------------------------------------------------+
|         7 | Ludovic    | Perudo     | 1999-09-09 |   46.2 |  135.8 |         1 | NULL         | NULL                                                                                 |
+-----------+------------+------------+------------+--------+--------+-----------+--------------+--------------------------------------------------------------------------------------+
|         8 | Bryan      | Baboulo    | 2005-10-12 |   51.9 |  145.7 |         1 | NULL         | NULL                                                                                 |
+-----------+------------+------------+------------+--------+--------+-----------+--------------+--------------------------------------------------------------------------------------+
|         9 | Hadia      | Wuxiko     | 2001-09-02 |   71.2 |  174.3 |         0 | NULL         | NULL                                                                                 |
+-----------+------------+------------+------------+--------+--------+-----------+--------------+--------------------------------------------------------------------------------------+
|        10 | Youcef     | Vicompte   | 1990-01-31 |     34 |   93.3 |         1 | NULL         | NULL                                                                                 |
+-----------+------------+------------+------------+--------+--------+-----------+--------------+--------------------------------------------------------------------------------------+
|        11 | Jade       | Haynak     | 1970-04-08 |   91.3 |  190.4 |         0 | 6            | Do not trust this girl                                                               |
+-----------+------------+------------+------------+--------+--------+-----------+--------------+--------------------------------------------------------------------------------------+
|        12 | Mehdi      | Mezizi     | 1996-06-05 |   24.8 |   75.7 |         0 | 7            | Likes to drink wine mixed with kombucha                                              |
+-----------+------------+------------+------------+--------+--------+-----------+--------------+--------------------------------------------------------------------------------------+
|        13 | Claire     | Suniza     | 1973-08-23 |   37.7 |  112.1 |         0 | 8            | A child in an adult body                                                             |
+-----------+------------+------------+------------+--------+--------+-----------+--------------+--------------------------------------------------------------------------------------+
|        14 | Robert     | Blanchard  | 1978-07-30 |   43.8 |  156.6 |         1 | 10,9         | A cute weirdo,Doesn't like to be bothered while eating                               |
+-----------+------------+------------+------------+--------+--------+-----------+--------------+--------------------------------------------------------------------------------------+
|        15 | Hadia      | Emopolo    | 1956-03-29 |   66.6 |  169.9 |         0 | NULL         | NULL                                                                                 |
+-----------+------------+------------+------------+--------+--------+-----------+--------------+--------------------------------------------------------------------------------------+
|        16 | Xiao       | Quezara    | 2000-10-26 |   60.7 |  147.1 |         1 | NULL         | NULL                                                                                 |
+-----------+------------+------------+------------+--------+--------+-----------+--------------+--------------------------------------------------------------------------------------+

Full Outer Join
***************

Returns all the rows from both tables, including non-matching rows.

.. tabs::

    .. tab:: PostgreSQL

        .. code-block:: sql

            SELECT * FROM Persons
            FULL OUTER JOIN Persons_Comments
            ON Persons.PERSON_ID = Persons_Comments.PERSON_ID;

    .. tab:: MySQL

        .. code-block:: sql

            SELECT * FROM Persons
            LEFT JOIN Persons_Comments ON Persons.PERSON_ID = Persons_Comments.PERSON_ID
            UNION ALL
            SELECT * FROM Persons
            RIGHT JOIN Persons_Comments ON Persons.PERSON_ID = Persons_Comments.PERSON_ID
            WHERE Persons.PERSON_ID IS NULL;

Output:

+-----------+------------+------------+------------+--------+--------+-----------+------------+-----------+---------------------------------------------------------------------+
| PERSON_ID | First Name | Surname    | Birth date | Weight | Height | Earthling | COMMENT_ID | PERSON_ID | Comment                                                             |
+===========+============+============+============+========+========+===========+============+===========+=====================================================================+
|         1 | Robert     | Dubouchard | 1975-12-19 |   89.1 |    134 |         1 |       NULL |      NULL | NULL                                                                |
+-----------+------------+------------+------------+--------+--------+-----------+------------+-----------+---------------------------------------------------------------------+
|         2 | Sarah      | Dupont     | 1991-01-14 |   69.3 |  175.4 |         1 |          2 |         2 | Likes to read with the head upside down                             |
+-----------+------------+------------+------------+--------+--------+-----------+------------+-----------+---------------------------------------------------------------------+
|         2 | Sarah      | Dupont     | 1991-01-14 |   69.3 |  175.4 |         1 |          1 |         2 | Not a very nice person                                              |
+-----------+------------+------------+------------+--------+--------+-----------+------------+-----------+---------------------------------------------------------------------+
|         3 | Xioului    | Wukikoku   | 2003-11-02 |   54.8 |  165.9 |         0 |       NULL |      NULL | NULL                                                                |
+-----------+------------+------------+------------+--------+--------+-----------+------------+-----------+---------------------------------------------------------------------+
|         4 | Xiao       | Li         | 1987-03-15 |   39.7 |  123.8 |         0 |          5 |         4 | Goes to pee every 30 minutes                                        |
+-----------+------------+------------+------------+--------+--------+-----------+------------+-----------+---------------------------------------------------------------------+
|         4 | Xiao       | Li         | 1987-03-15 |   39.7 |  123.8 |         0 |          4 |         4 | Such a lovely Martian                                               |
+-----------+------------+------------+------------+--------+--------+-----------+------------+-----------+---------------------------------------------------------------------+
|         4 | Xiao       | Li         | 1987-03-15 |   39.7 |  123.8 |         0 |          3 |         4 | Don't talk about piano with her !                                   |
+-----------+------------+------------+------------+--------+--------+-----------+------------+-----------+---------------------------------------------------------------------+
|         5 | Robert     | Amomo      | 1998-12-13 |  123.2 |  210.3 |         1 |       NULL |      NULL | NULL                                                                |
+-----------+------------+------------+------------+--------+--------+-----------+------------+-----------+---------------------------------------------------------------------+
|         6 | Jeanne     | Klopens    | 1985-11-19 |   94.6 |  192.9 |         0 |       NULL |      NULL | NULL                                                                |
+-----------+------------+------------+------------+--------+--------+-----------+------------+-----------+---------------------------------------------------------------------+
|         7 | Ludovic    | Perudo     | 1999-09-09 |   46.2 |  135.8 |         1 |       NULL |      NULL | NULL                                                                |
+-----------+------------+------------+------------+--------+--------+-----------+------------+-----------+---------------------------------------------------------------------+
|         8 | Bryan      | Baboulo    | 2005-10-12 |   51.9 |  145.7 |         1 |       NULL |      NULL | NULL                                                                |
+-----------+------------+------------+------------+--------+--------+-----------+------------+-----------+---------------------------------------------------------------------+
|         9 | Hadia      | Wuxiko     | 2001-09-02 |   71.2 |  174.3 |         0 |       NULL |      NULL | NULL                                                                |
+-----------+------------+------------+------------+--------+--------+-----------+------------+-----------+---------------------------------------------------------------------+
|        10 | Youcef     | Vicompte   | 1990-01-31 |     34 |   93.3 |         1 |       NULL |      NULL | NULL                                                                |
+-----------+------------+------------+------------+--------+--------+-----------+------------+-----------+---------------------------------------------------------------------+
|        11 | Jade       | Haynak     | 1970-04-08 |   91.3 |  190.4 |         0 |          6 |        11 | Do not trust this girl                                              |
+-----------+------------+------------+------------+--------+--------+-----------+------------+-----------+---------------------------------------------------------------------+
|        12 | Mehdi      | Mezizi     | 1996-06-05 |   24.8 |   75.7 |         0 |          7 |        12 | Likes to drink wine mixed with kombucha                             |
+-----------+------------+------------+------------+--------+--------+-----------+------------+-----------+---------------------------------------------------------------------+
|        13 | Claire     | Suniza     | 1973-08-23 |   37.7 |  112.1 |         0 |          8 |        13 | A child in an adult body                                            |
+-----------+------------+------------+------------+--------+--------+-----------+------------+-----------+---------------------------------------------------------------------+
|        14 | Robert     | Blanchard  | 1978-07-30 |   43.8 |  156.6 |         1 |         10 |        14 | A cute weirdo                                                       |
+-----------+------------+------------+------------+--------+--------+-----------+------------+-----------+---------------------------------------------------------------------+
|        14 | Robert     | Blanchard  | 1978-07-30 |   43.8 |  156.6 |         1 |          9 |        14 | Doesn't like to be bothered while eating                            |
+-----------+------------+------------+------------+--------+--------+-----------+------------+-----------+---------------------------------------------------------------------+
|        15 | Hadia      | Emopolo    | 1956-03-29 |   66.6 |  169.9 |         0 |       NULL |      NULL | NULL                                                                |
+-----------+------------+------------+------------+--------+--------+-----------+------------+-----------+---------------------------------------------------------------------+
|        16 | Xiao       | Quezara    | 2000-10-26 |   60.7 |  147.1 |         1 |       NULL |      NULL | NULL                                                                |
+-----------+------------+------------+------------+--------+--------+-----------+------------+-----------+---------------------------------------------------------------------+
|      NULL | NULL       | NULL       | NULL       |   NULL |   NULL |      NULL |         11 |        25 | This Person does't exist                                            |
+-----------+------------+------------+------------+--------+--------+-----------+------------+-----------+---------------------------------------------------------------------+
|      NULL | NULL       | NULL       | NULL       |   NULL |   NULL |      NULL |         12 |        27 | I already like the person with this ID even if he/she doesn't exist |
+-----------+------------+------------+------------+--------+--------+-----------+------------+-----------+---------------------------------------------------------------------+


Example Grouping data
#####################

Grouping data: SQL provides an easy way to aggregate data by grouping it based on specific criteria. You should understand how to use the GROUP BY clause to group data.

.. code-block:: sql

    SELECT Earthling, COUNT(*), AVG(Height) as mean_height
    FROM Persons
    GROUP BY Earthling;

Output:

+-----------+----------+--------------------+
| Earthling | COUNT(*) | mean_height        |
+===========+==========+====================+
|         1 |        8 |  149.7750015258789 |
+-----------+----------+--------------------+
|         0 |        8 | 150.62499713897705 |
+-----------+----------+--------------------+

Example using UNION
###################

Suppose you have two tables called "employees" and "customers" with the following data:


**Table: employees**

+-------------------+---------------------+------------------+
|    employee_id    |    employee_name    |    department    |
+===================+=====================+==================+
|    1              |    John             |    Sales         |
+-------------------+---------------------+------------------+
|    2              |    Jane             |    HR            |
+-------------------+---------------------+------------------+
|    3              |    Bob              |    Marketing     |
+-------------------+---------------------+------------------+


**Table: customers**

+-------------------+---------------------+------------------+
|    customer_id    |    customer_name    |    city          |
+===================+=====================+==================+
|    1              |    Acme Corp        |    New York      |
+-------------------+---------------------+------------------+
|    2              |    XYZ Corp         |    Chicago       |
+-------------------+---------------------+------------------+
|    3              |    ABC Inc          |    Dallas        |
+-------------------+---------------------+------------------+

If you want to combine the results of these two tables into a single table, you can use the UNION command like this:

.. code-block:: sql

    CREATE TABLE employees (employee_id INT PRIMARY KEY, employee_name VARCHAR(255), department VARCHAR(255));
    CREATE TABLE customers (customer_id INT PRIMARY KEY, customer_name VARCHAR(255), city VARCHAR(255));
    INSERT INTO employees VALUES (1, 'John', 'Sales'), (2, 'Jane', 'HR'), (3, 'Bob', 'Marketing');
    INSERT INTO customers VALUES (1, 'Acme Corp', 'New York'), (2, 'XYZ Corp', 'Chicago'), (3, 'ABC Inc', 'Dallas');

    SELECT employee_name as name, department as category, 'employee' as source
    FROM employees
    UNION
    SELECT customer_name as name, city as category, 'customer' as source
    FROM customers;


This will give you the following result.

+------------------+------------------+--------------+
|      name        |    category      |    source    |
+==================+==================+==============+
|      John        |    Sales         |   employee   |
+------------------+------------------+--------------+
|      Jane        |    HR            |   employee   |
+------------------+------------------+--------------+
|      Bob         |    Marketing     |   employee   |
+------------------+------------------+--------------+
|      Acme Corp   |    New York      |   customer   |
+------------------+------------------+--------------+
|      XYZ Corp    |    Chicago       |   customer   |
+------------------+------------------+--------------+
|      ABC Inc     |    Dallas        |   customer   |
+------------------+------------------+--------------+


.. note::
    Note that in the SELECT statement, we're using aliases to make the column names in both tables match. We're also adding a new column called "source" to differentiate between the data from the two tables. The UNION command removes duplicate rows, so the resulting table only contains distinct rows.


Views
#####

Views are virtual tables that can be used to simplify complex queries or restrict access to certain data.


Indexes
#######

Indexes are data structures that can be used to speed up data retrieval to improve query performance.


Useful commands
###############

Get the list of databases
*************************

.. tabs::

    .. tab:: PostgreSQL

        .. code-block:: psql

            \list

    .. tab:: MySQL

        .. code-block:: mysql

            SHOW DATABASES;


Get the name of the current database in use
*******************************************

.. tabs::

    .. tab:: PostgreSQL

        .. code-block:: psql

            SELECT current_database();

    .. tab:: MySQL

        .. code-block:: mysql

            SELECT db_name();


Create a new database
*********************

.. code-block:: sql

    CREATE DATABASE testdb;


Use a specific database
***********************

.. tabs::

    .. tab:: PostgreSQL

        .. code-block:: psql

            \connect testdb

    .. tab:: MySQL

        .. code-block:: mysql

            use testdb;


List tables in the current database
***********************************

.. tabs::

    .. tab:: PostgreSQL

        .. code-block:: psql

            \dt

    .. tab:: MySQL

        .. code-block:: mysql

            SHOW TABLES;


Delete/Remove/Drop a table
**************************

.. code-block:: sql

    DROP TABLE tablename;
