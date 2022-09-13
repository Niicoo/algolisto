Divers
======

Debugging option
################

Debug option to include in a bash script toprint the executed commands.

.. code-block:: python

    set -x 

Shebang line
############

Line at the beginning of a file to indicate that the file is a script, unix-like operating systems will try to execute the file using the interpreter specified by that line.

.. code-block:: python

    #!/usr/bin/env bash


Create an empty file
####################

.. code-block:: bash

    touch new_file.txt


Create a file and its content
#############################

.. code-block:: bash

    cat <<EOM >new_file.txt
    Content of the file!
    You can use new lines and write whatever you want.
    Until you write the keyword 'EOM' to the last line.
    "EOM" was arbitrary chosen, you can use the keyword you want.
    EOM


Basic Arithmetic operations
###########################

.. note::
    Bash only works with integer.

.. code-block:: bash

    declare -i FIRST_DAY=19090
    declare -i LAST_DAY=22744
    declare -i NB_DAYS_PER_JOB=50

    NB_DAYS=$(( LAST_DAY - FIRST_DAY ))
    NB_JOBS=$(( (NB_TOTAL_DAYS / NB_DAYS_PER_JOB) + 1 ))


Loops
#####

.. code-block:: bash

    for (( ind = 0; ind < 10; ind++ )); do echo $ind; done
    for ind in {0..10}; do echo $ind; done
    for ind in {0..10..2}; do echo $ind; done
    for i in 0 2 7 8 10 25 67; do echo $i; done
