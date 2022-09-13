Notebook
========

To install jupyter notebook to your conda environment, use the following command:

.. code-block:: bash

    conda install -c anaconda jupyter


Start a server
##############

.. code-block:: bash

    # Start the notebook server
    jupyter notebook
    # Open a notebook (Automatically start the server if not running)
    # If a server is already running, it won't use it, it will create a new one
    jupyter notebook notebook.ipynb


Close a running server
######################

.. code-block:: bash

    # Identify the port number using
    jupyter notebook list
    # Close the server using the command (eg for port 8888)
    jupyter notebook stop 8888


Convert a python notebook(.ipynb) to python (.py)
#################################################

.. code-block:: bash

    jupyter nbconvert input.ipynb --to python


Add a conda environment to the list of kernels availables
#########################################################

.. code-block:: bash

    conda activate my_env
    conda install ipykernel
    ipython kernel install --user --name=kernel_my_env

------------------------------------------------------------

**Sources**:

- Official documentation: https://www.educative.io/blog/python-jupyter-notebook
- Add a kernel: https://stackoverflow.com/questions/53004311/how-to-add-conda-environment-to-jupyter-lab
