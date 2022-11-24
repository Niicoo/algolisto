Kernel
======

This is valid on Ubuntu based distributions only.

Manually install a kernel
#########################

#. Download the kernel you want using this link: https://kernel.ubuntu.com/~kernel-ppa/mainline/

    Make sure to download all the packages corresponding to your architecture ("amd64" in most case)

#. Install the packages using:

    .. code-block:: bash

        sudo dpkg -i *.deb


List installed Kernels
######################

.. code-block:: bash

    dpkg --list | grep linux-image
    # or 
    find /boot/vmli*


Uninstall/Remove a kernel
#########################

Source: https://askubuntu.com/questions/1253347/how-to-easily-remove-old-kernels-in-ubuntu-20-04-lts

List install kernel(s) module(s):

.. code-block:: bash
    
    dpkg --list | egrep -i --color 'linux-image|linux-headers|linux-modules' | awk '{ print $2 }'


Uninstall kernel you don't need with :code:`sudo apt purge`:

.. code-block:: bash

    sudo apt purge linux-headers-5.6.11-050611  linux-headers-5.6.11-050611-lowlatency linux-image-unsigned-5.6.11-050611-lowlatency linux-modules-5.6.11-050611-lowlatency

.. warning:: 
    Of course, do not uninstall the kernel you are currently using, you can check which kernel you're running with the command :code:`uname -a`.


------------------------------------------------------------

**Sources**:

- https://ostechnix.com/list-or-check-all-installed-linux-kernels-from-commandline/
- https://askubuntu.com/questions/1253347/how-to-easily-remove-old-kernels-in-ubuntu-20-04-lts