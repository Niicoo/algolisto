Useful Commands
===============


Archive Compression/Decompression
#################################


zip
***

**Compression**

.. code-block:: bash
    
    zip -r output.zip file1.txt file2.txt folder1/

**Decompression**

.. code-block:: bash

     unzip input.zip -d /output/directory



tar.gz || tar.xz
****************

**Compression**

.. code-block:: bash

    # tar.gz
    tar -czvf output.tar.gz file1.txt file2.txt folder1/
    # tar.xz
    tar -cJvf output.tar.xz file1.txt file2.txt folder1/

**Decompression**

.. code-block:: bash

    tar -xvf input.tar.gz -C /output/directory


File Manipulation
#################

**Renaming multiple file in a folder**

.. code-block:: bash

    for f in E1_c*IFREMER.dat; do mv $f ${f:0:16}_ORF.dat ;done


**Search a specific text in files + subdirectory**

.. code-block:: bash

    grep -nr "text to search" --include \*.py


**Counting the number of files**

.. code-block:: bash

    find . -type f -name "*.nc" | wc -l

You can use the option :bash:`-maxdepth 1` to only consider file in the current directory.


**View Disk Free space of a folder**

.. code-block:: bash

    df -h /path/to/folder


**Show the disk usage of a folder**

.. code-block:: bash

    du /path/to/folder -h -d 1



Packages
########

**Installing a package**

.. code-block:: bash

    sudo apt-get install package-name

**Remove/Uninstall a package**

.. code-block:: bash

    # Remove the package only
    sudo apt-get remove package_name
    # Remove the package and its config files
    sudo apt-get remove --purge package_name

**Installing a deb package locally**

.. code-block:: bash

    dpkg -x package.deb /output/dir


**List available packages**

.. code-block:: bash

    # List all available packages
    apt list
    # List only the installed packages
    apt list --installed


PDF Manipulation
################

Join/Merge PDF files:

.. code-block:: bash

    pdftk input_1.pdf input_2.pdf input_3.pdf cat output output.pdf
    # To concatenate only desired pages
    pdftk A=input_1.pdf B=input_2.pdf cat A1 B2-20even output output.pdf


ImageMagick
###########

Convert images to pdf

.. code-block:: bash

    convert Image1.png Image2.png Image.pdf

You may need to change the ImageMagick policy if you have problem with the convert command.

In :code:`/etc/ImageMagick-6/policy.xml` update the line:

FROM:

.. code-block:: bash

    <policy domain="coder" rights="none" pattern="PDF" />

TO:

.. code-block:: bash

    <policy domain="coder" rights="read|write" pattern="PDF" />


source: https://linuxhint.com/convert-image-to-pdf-command-line/

Divers
######

**Synchronizing 2 paths**

The files are copied only if they are different than the destination files.

.. code-block:: bash

    rsync -av /source/path /destination/path
    # Also works with SSH 
    rsync -avz /source/path/ user@XX.XXX.XX.XXX:/destination/path

**To check if you are in an interactive shell**

.. code-block:: bash
    
    [[ $- == *i* ]] && echo 'Interactive' || echo 'Not interactive'

**To check if you are in a login shell**

.. code-block:: bash
    
    shopt -q login_shell && echo 'Login shell' || echo 'Not login shell'


**Show which process are using a specific file**

.. code-block:: bash
    
    fuser /path/to/file


**Execute a command in the background**

*(Execute a command and being able to close the terminal)*

.. code-block:: bash
    
    nohup python script.py > out.log &


**View the header of a Netcdf file**

.. code-block:: bash

    ncdump -h file.nc

**List Environment variables**

.. code-block:: bash

    printenv

**Get the location of a library**

.. code-block:: bash

    ldconfig -p | grep libname*

