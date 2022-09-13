Installation
============

This tutorial shows how to install and configure `Reaper <https://www.reaper.fm/>`_ and `Carla <https://kx.studio/Applications:Carla>`_ in order to get a working DAW with both Linux VST and windows VST.


Install and Configure Carla
###########################

To be able to use Windows VST working, you will need to install and configure `Carla <https://kx.studio/Applications:Carla>`_.

#. Install **kx studio repositories** using the DEB provided here: https://kx.studio/Repositories

#. Install **Carla** and the required bridge:

    .. code-block:: bash

        sudo apt-get update
        sudo apt-get install carla-git carla-bridge-win64 carla-bridge-win32 carla-bridge-linux64 carla-bridge-linux32 carla-bridge-wine64 carla-bridge-wine32

#. Configuring Carla to allow using bridges

    Open ``Settings`` **>** ``Configure Carla`` **>** ``Main``
        Tick **Enable Experimentals features**

    Open ``Settings`` **>** ``Configure Carla`` **>** ``Experimental``
        Tick **Enable Plugin Bridges** and **Enable Wine Bridges**


Install and Configure Reaper
############################


#. Download Linux x64 linux binaries in Reaper website: https://www.reaper.fm/

#. Install it running the following commands:

    .. code-block:: bash

        tar -xvf reaper*_linux_x86_64.tar.xz && cd reaper_linux_x86_64
        ./install-reaper.sh

#. Add the Carla VST path:

    Open ``Options`` **>** ``Preferences`` **>** ``VST`` **>** ``Edit path list ...``

    (In my case carla vst plugins were located in: `/lib/vst/carla.vst`)

#. Choose the audio device:

    Open ``Options`` **>** ``Preferences`` **>** ``Audio`` **>** ``Device``


[If Needed] Re-Installing Wine
##############################

#. Remove any trace of past wine installation

    Check package names that need to be removed:

    .. code-block:: bash

        apt list --installed | grep wine
    
    Remove all wine package identified in the previous command:

    .. code-block:: bash
        
        # Example
        sudo apt remove fonts-wine libwine wine-stable wine32 wine64 wine --purge

    .. warning::
        Careful not to remove unwanted packages (example: **carla-bridge-wine64**, **carla-bridge-wine32** installed for Carla)

#. Install the latest version of wine using this link:

    https://wiki.winehq.org/Ubuntu

    .. note::
        In my case, wine stable version was still not available for the Jammy release (ubuntu 22.04 / Mint 21), so I installed wine with the staging branch (**winehq-staging**).


Install VST
###########

Native Linux VST:

    #. Copy the files to one of the search path of Reaper
    #. Rescan the plugins in Reaper

Native Windows VST [DLL]:

    #. Copy the files to one of the search path of Carla
    #. Rescan the plugins using carla
    #. To use it in Reaper, use it through **Carla-Rack** in your track

Native Windows VST [EXE]:

    #. Install the VST using **wine** and make sure the plugins are installed in one of the search path of Carla
    #. Rescan the plugins using carla
    #. To use it in Reaper, use it through **Carla-Rack** in your track


------------------------------------------------------------

**Sources**:

- Wine installation: https://wiki.winehq.org/Ubuntu
- Reaper: https://www.reaper.fm/
- Carla: https://kx.studio/Applications:Carla
