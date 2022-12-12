Installation
============

This tutorial shows how to install and configure `Reaper <https://www.reaper.fm/>`_ and `Yabridge <https://github.com/robbert-vdh/yabridge>`_ (or `Carla <https://kx.studio/Applications:Carla>`_) in order to get a working DAW with both Linux VST and windows VST.


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


Install and Configure Yabridge
##############################

Download latest version and follow installation instruction from https://github.com/robbert-vdh/yabridge.

Example for Version 5.0.2:

.. code-block:: bash

    wget https://github.com/robbert-vdh/yabridge/releases/download/5.0.2/yabridge-5.0.2.tar.gz
    tar -xvf yabridge-5.0.2.tar.gz -C ~/.local/share/
    export PATH="$PATH:$HOME/.local/share/yabridge"
    yabridgectl add "/home/ndejax/.wine/drive_c/Program Files/Common Files/VST3"
    yabridgectl add "/home/ndejax/.wine/drive_c/Program Files/VstPlugins"
    yabridgectl add "/home/ndejax/.wine/drive_c/Program Files/Ample Sound"
    yabridgectl sync

Ater that, the windows VST should appears just like native linux VSTs in your DAW.


Install and Configure Carla
###########################

If `Yabridge <https://github.com/robbert-vdh/yabridge>`_ is not working for you, you can try with `Carla <https://kx.studio/Applications:Carla>`_:

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


Install and Configure Jack
##########################

Below is the packages you need to install to get jackd audio sound server + its GUI tool `qjackctl` and

.. code-block:: bash

    sudo apt-get install jackd qjackctl pulseaudio-module-jack

In **qjackctl**:

- set **Sample Rate** to 48000 in ``Setup`` **>** ``Settings`` **>** ``Parameters``
- set **Frames/Period** to 128 in ``Setup`` **>** ``Settings`` **>** ``Parameters`` (lower if your latency is still to high)
- select your microphone in **Input Device** in ``Setup`` **>** ``Settings`` **>** ``Advanced``
- check and edit ``Setup`` **>** ``Options`` **>** ``Execute script after startup`` as below:
    :code:`pactl load-module module-jack-sink channels=2; pactl load-module module-jack-source; pacmd set-default-sink jack_out`
    (It allows you to continue to hear the sound of other applications)

.. important::
    After that, each time you run jack, you may need to go to the sound settings in Ubuntu/Mint and recheck the input and output devices (Jack Sink and Jack Source) and also check the volume levels of the devices you're using (micro and speakers).


After Launching Reaper, you should get a graph like that:

.. image:: /assets/reaper/installation/graph_qjackctl.png
    :width: 400pt

In my case the system playback 1 and 2 correspond to the jack output of my laptop and playback 3 and 4 correspond to my laptop internal speakers.



Install and Configure Reaper
############################

#. Download Linux x64 linux binaries in Reaper website: https://www.reaper.fm/

#. Install it running the following commands:

    .. code-block:: bash

        tar -xvf reaper*_linux_x86_64.tar.xz && cd reaper_linux_x86_64
        ./install-reaper.sh

#. Add the Carla VST path:

    Open ``Options`` **>** ``Preferences`` **>** ``VST`` **>** ``Edit path list ...``

    (If you're using carla, the plugins in my case were located in: `/lib/vst/carla.vst`)

#. Choose the audio device:

    Open ``Options`` **>** ``Preferences`` **>** ``Audio`` **>** ``Device``
    Check ``Auto-start Jackd``

    .. note::
        If you still want to hear the sound from the rest of the applications (Pulse Audio), you first need to start jack using ``qjackctl`` (configured previously) before launching Reaper.



Install VST
###########

Native Linux VST:

    #. Copy the files to one of the search path of Reaper
    #. Rescan the plugins in Reaper

Native Windows VST [DLL]:

    #. Copy the files to one of the search path of Yabridge/Carla
    #. Rescan the plugins in Yabridge/Carla
    #. [Carla only] use it through **Carla-Rack** in your track

Native Windows VST [EXE]:

    #. Install the VST using **wine**
    #. If required, add the VST path in Yabridge/Carla
    #. Rescan the plugins in Yabridge/Carla
    #. [Carla only] use it through **Carla-Rack** in your track


------------------------------------------------------------

**Sources**:

- Wine installation: https://wiki.winehq.org/Ubuntu
- Reaper: https://www.reaper.fm/
- Carla: https://kx.studio/Applications:Carla
- Yabridge: https://github.com/robbert-vdh/yabridge