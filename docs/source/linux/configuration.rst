Configuration
=============


Set the terminal language to English
####################################

Add the following lines to your .profile:

.. code-block:: bash

    export LANG="en_US.UTF-8"
    export LANGUAGE="en_US:"


Softwares
#########

**Mullvad**: https://mullvad.net/en/download/linux/

.. code-block:: bash

    sudo apt install MullvadVPN-2022.2_amd64.deb

**Visual Studio code**: https://code.visualstudio.com/

.. code-block:: bash

    sudo apt install code_1.69.2-1658162013_amd64.deb

**Mega**: https://mega.io/desktop

.. code-block:: bash

    sudo apt install megasync-xUbuntu_22.04_amd64.deb


**Enpass**: https://www.enpass.io/support/kb/general/how-to-install-enpass-on-linux/

.. code-block:: bash

    sudo -i
    echo "deb https://apt.enpass.io/ stable main" > /etc/apt/sources.list.d/enpass.list
    wget -O - https://apt.enpass.io/keys/enpass-linux.key | tee /etc/apt/trusted.gpg.d/enpass.asc
    apt-get update
    apt-get install enpass
    exit


**Spotify**: https://www.spotify.com/us/download/linux/

.. code-block:: bash

    snap install spotify
    # OR
    curl -sS https://download.spotify.com/debian/pubkey_5E3C45D7B312C643.gpg | sudo apt-key add - 
    echo "deb http://repository.spotify.com stable non-free" | sudo tee /etc/apt/sources.list.d/spotify.list
    sudo apt-get update && sudo apt-get install spotify-client

If you get the problem where Spotify is running in fullscreen mode without being able to access the exit/minimize icons, in the launcher :code:`/usr/share/applications/spotify.desktop` you can edit the line `Exec`:

From:

.. code-block:: bash

    Exec=spotify %U

To:

.. code-block:: bash

    Exec=sh -c 'sed -i "/\b\(app.window.position\)\b/d" -- $HOME/.config/spotify/prefs ; spotify %U'

(source: https://askubuntu.com/questions/1200000/cannot-exit-fullscreen-on-spotify & https://askubuntu.com/questions/60379/how-to-combine-two-commands-as-a-launcher)

**Geany**

.. code-block:: bash

    sudo apt-get install geany

**Git**

.. code-block:: bash

    sudo apt-get install git

**Miniconda**: https://docs.conda.io/en/latest/miniconda.html

.. code-block:: bash

    bash Miniconda3-latest-Linux-x86_64.sh

**Filezilla**

.. code-block:: bash

    sudo apt install filezilla

**Inkscape**: https://inkscape.org/

.. code-block:: bash

    sudo apt install inkscape

**PDFtk**

.. code-block:: bash

    sudo snap install pdftk
    # OR
    sudo apt-get install pdftk-java

**ImageMagick**

.. code-block:: bash

    sudo apt install imagemagick

**Draw.io**

Download latest DEB package at: https://github.com/jgraph/drawio-desktop/releases/

Then run:

.. code-block:: bash

    sudo apt install drawio-amd64-20.3.0.deb


