PyTorch Install
===============

Pytorch install guide: https://pytorch.org/get-started/locally/

Procedure to install the most recent pytorch package compatible with your machine:

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

#. [OPTIONAL - Not Recommended] Install CUDA manually:

    Do this step only if you don't want to install cuda using conda.

    Check Which version of CUDA you need to install using this link: https://pytorch.org/get-started/locally/

    .. image:: /assets/machine_learning/install_pytorch/pytorch_install.png
        :width: 500pt


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

#. Install PyTorch

    .. code-block:: bash

        # If you already installed CUDA previously
        conda install pytorch torchvision torchaudio -c pytorch
        conda env config vars set PATH="/usr/local/cuda-11.6/bin:$PATH"
        conda env config vars set LD_LIBRARY_PATH="/usr/local/cuda-11.6/lib64:/usr/local/cuda-11.6/include:$LD_LIBRARY_PATH"
        # If you didn't install CUDA yet (check the link to get the command for the latest version available)
        conda install pytorch torchvision torchaudio cudatoolkit=11.6 -c pytorch -c conda-forge

    You can check pytorch properly detect your GPU using the command:

    .. code-block:: python

        import torch
        is_available = torch.cuda.is_available()
        cuda_version = torch.version.cuda
        nb_devices = torch.cuda.device_count()
        ind_device = torch.cuda.current_device()
        device_name = torch.cuda.get_device_name(ind_device)


------------------------------------------------------------

**Sources**:

- check Pytorch is using GPU: https://stackoverflow.com/questions/48152674/how-do-i-check-if-pytorch-is-using-the-gpu
