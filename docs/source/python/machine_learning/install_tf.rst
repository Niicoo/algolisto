Tensorflow Install
==================


Procedure to install the most recent tensorflow package compatible with your nvidia machine:

#. Make sure your nvidia drivers is up to date.

    You can check that the Nvidia drivers is properly install with the command:

    .. code-block:: bash

        nvidia-smi

    It should print something like this:

    .. code-block:: bash

        +-----------------------------------------------------------------------------+
        | NVIDIA-SMI 515.65.01    Driver Version: 515.65.01    CUDA Version: 11.7     |
        |-------------------------------+----------------------+----------------------+
        | GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
        | Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
        |                               |                      |               MIG M. |
        |===============================+======================+======================|
        |   0  NVIDIA GeForce ...  Off  | 00000000:01:00.0 Off |                  N/A |
        | N/A   44C    P3    13W /  N/A |      4MiB /  4096MiB |      0%      Default |
        |                               |                      |                  N/A |
        +-------------------------------+----------------------+----------------------+
                                                                                    
        +-----------------------------------------------------------------------------+
        | Processes:                                                                  |
        |  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
        |        ID   ID                                                   Usage      |
        |=============================================================================|
        |    0   N/A  N/A      1090      G   /usr/lib/xorg/Xorg                  4MiB |
        +-----------------------------------------------------------------------------+


#. Check the most recent CUDA Drivers version you can install using this `cuda link <https://docs.nvidia.com/cuda/cuda-toolkit-release-notes/index.html>`_.

    .. image:: /assets/machine_learning/install_tf/CUDA_drivers.png
        :width: 500pt

    (*source:* https://docs.nvidia.com/cuda/cuda-toolkit-release-notes/index.html)


#. Now that you know the latest version of CUDA you can install, check which latest version of Tensorflow (GPU) you can install using this `tf link <https://www.tensorflow.org/install/source#gpu>`_.

    .. image:: /assets/machine_learning/install_tf/TF_versions.png
        :width: 500pt

    (*source:* https://www.tensorflow.org/install/source#gpu)


#. Install the CUDA version you choose in section **3.**

    CUDA Download link: https://developer.nvidia.com/cuda-toolkit-archive
    CUDA Installation guide: https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html

    .. warning::
        if you know your nvidia drivers matches with this CUDA version, when running the runfile installation of CUDA add :code:`--toolkit` (:code:`sudo sh cuda_11.2.2_460.32.03_linux.run --toolkit`) in the install command to make sure nvidia drivers won't be updated.

    You will need to update environment variables `PATH` and `LD_LIBRARY_PATH` in your `.bashrc`:
    (It is not mandatory to define these variable globally, you can also set them only when activating your conda environment, see below)

    .. code-block:: bash

        # These are symlinks ! be sure it point to the desired CUDA version you installed
        export PATH="/usr/local/cuda/bin:$PATH"
        export LD_LIBRARY_PATH="/usr/local/cuda/lib64:$LD_LIBRARY_PATH"
        export LD_LIBRARY_PATH="/usr/local/cuda/include:$LD_LIBRARY_PATH"
    

    You can check if CUDA is properly installed by running the command (won't work if you prefer to define environment variables for conda only): 

    .. code-block:: bash

        nvcc -V


#. Download the cuDNN version you choose in section **3.**

    cuDNN: https://docs.nvidia.com/deeplearning/cudnn/install-guide/index.html

    .. code-block:: bash

        tar -xf cudnn-linux-x86_64-*.tar.xz
        sudo cp cudnn-linux-x86_64-*/include/* /usr/local/cuda/include/
        sudo cp cudnn-linux-x86_64-*/lib/* /usr/local/cuda/lib64/

    .. warning::
        You may encounter problem if you don't have the official kernel version as cuDNN works with specific version of kernel: https://docs.nvidia.com/deeplearning/cudnn/support-matrix/index.html. (In my case cuDNN 8.5 is properly working with kernel 5.19 although 5.15 version is officialy supported)


#. Install tensorflow on conda:

    As of now, I've not being able to install tensorflow with GPU capability using conda: :code:`conda install -c conda-forge tensorflow=2.9` (from v2.0, the `tensorflow` package already include gpu capbility).
    Using pip did work:


    .. code-block:: bash
    
        pip install tensorflow-gpu==2.9
        # If you want to define the environment variables only for the current conda environment
        conda env config vars set PATH="/usr/local/cuda/bin:$PATH"
        conda env config vars set LD_LIBRARY_PATH="/usr/local/cuda/lib64:/usr/local/cuda/include:$LD_LIBRARY_PATH"

    You can check tensorflow properly detect your GPU using the command:

    .. code-block:: python

        import tensorflow as tf
        tf.test.gpu_device_name()
