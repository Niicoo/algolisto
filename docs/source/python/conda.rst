Conda
=====


Create an environment
#####################

.. code-block:: bash

    conda create -n myenv python=3.6 scipy=0.15.0 numpy


Create an environment from an environment file:

.. code-block:: bash

    conda env create -f environment.yml


You can create the environment file from an existing environment using the command:

.. code-block:: bash

    conda env export > environment.yml

Exemple of an environment file:

.. code-block:: yaml

    name: myenv
    channels:
      - conda-forge
      - anaconda
    dependencies:
      - python=3.6
      - bokeh=0.9.2
      - numpy=1.9.*
      - nodejs>=0.10.*
      - conda-forge::matplotlib
      - anaconda::numpy
      - flask
      - pip
      - pip:
        - Flask-Testing
        - package_name==2.3.0
    variables:
      PATH: "/example/additional/path:$PATH"


Delete an environment
#####################

.. code-block:: bash

    conda remove --name myenv --all


List the environments
#####################

.. code-block:: bash

    conda info --envs


View the list of packages in an environment
###########################################

.. code-block:: bash

    conda list -n myenv


Search for package availables
#############################

.. code-block:: bash

    conda search -f package_name


Install a specific package
##########################

.. code-block:: bash

    conda install [-c channel_name] package_name


Remove a specific package
#########################

.. code-block:: bash

    conda remove [--force] package_name

Use the --force argument to remove only the package without removing its dependencies.


Configure your environment with .condarc
########################################

You can get info on your settings using the following command

.. code-block:: bash

    conda info


To update the default path of the folder where the environments will be created you can edit the `envs_dirs` variable in your your .condarc using the following syntax:
(You may need to update the `pkgs_dirs` if you don't have write access to the packages directory)

.. code-block:: yaml

    envs_dirs:
      - ~/.conda/envs  # The first path will be the one chosen by default to create new environments
      - /data/envs
    
    pkgs_dirs:
      - ~/.conda/pkgs


(More infos on condarc here: https://docs.conda.io/projects/conda/en/latest/user-guide/configuration/use-condarc.html)


------------------------------------------------------------

**Sources**:

- Official Conda Documentation https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html
