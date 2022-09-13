Path Manipulations
==================

Get script path
###############

.. code-block:: bash

    SCRIPT_DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" &> /dev/null && pwd )"

Iterate over files in a directory
#################################

.. code-block:: bash

    for file in /path/to/directory/*; do echo $file; done

