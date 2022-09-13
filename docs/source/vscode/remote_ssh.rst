Remote SSH
==========

**=> [NEED TO BE UPDATED !!!]**

Connect to a linux server using a windows machine
#################################################

.. warning::
    **Current issue**: Remote x11 does not read that environment variable, even when hard coding the path in remote x11 , I got an error "couldn't query Security extension on display :0"
    For the moment, it just work if I set the x11 permission level to "trusted" in the remoe x11 settings


#. **Creating an SSH key**

    #. Generate a SSH key on the client machine (Windows)  
    #. Add the public key on the server machine in the file :code:`$HOME/.ssh/authorized_keys`  (you need to add the public key for each user you want to login)


#. **Install an X server**

    #. Install cygwin using this tutorial: https://x.cygwin.com/docs/ug/setup.html, make sure to install all required packages + openssh.

    #. Modify the shortcut of "XWin Server" to accept IP connection:
        :code:`C:\cygwin64\bin\run.exe --quote /usr/bin/bash.exe -l -c "cd; exec /usr/bin/startxwin -- -listen tcp"`

        If required, you can also modify the display number:
            :code:`C:\cygwin64\bin\run.exe --quote /usr/bin/bash.exe -l -c "cd; exec /usr/bin/startxwin -- :0 -listen tcp"`  

    #. Add Xwin server as startup program (https://support.microsoft.com/en-us/windows/add-an-app-to-run-automatically-at-startup-in-windows-10-150da165-dcd9-7230-517b-cf3c295d89dd)


#. **Configure Remote SSH**

    #. Install the extension "Remote Development" in Visual Studio Code (Follow this guide: https://code.visualstudio.com/docs/remote/ssh)  

    #. In order to get a login shell, add the following line to the **remote - SSH** :code:`settings.json`:

        .. code-block:: yaml

            {
                "terminal.integrated.profiles.linux": {
                    "bash": {
                        "path": "bash",
                        "args": ["-l"]
                    }
                },
                "terminal.integrated.defaultProfile.linux": "bash",
            }

    #. To use the same ssh configuration as Cygwin, we'll use the same config ssh file: :code:`C:\cygwin64\home\lpoutou\.ssh\config` (if it doesn't exists, create it). In the setting of the Remote SSH extension, enter the path of this config file. (the content of the config file is described below)  

    #. Connecting to the host:

        **[Preferred way]** To be able to connect to different account on the same machine, you can directly edit the config file as below: (Don't use the @ character for 'Host'!)  
        
        .. code-block:: bash

            Host lpoutou-px-power
                User lpoutou
                HostName px-power.px.algo.fr
            Host lpoutou-px-weak
                User lpoutou
                HostName px-weak.px.algo.fr
            Host axiao-px-power
                User axiao
                HostName px-power.px.algo.fr
        
        **[Alternative way]** Connect to the desired host using the remote SSH extension (eg: :code:`ssh lpoutou@px-power.px.algo.fr`). You won't be able to connect with different account on the same machine.

    #. If asked, specify the config file where to save the connexion parameters (use the same as previously)

#. **Install the extension "Remote X11" and "Remote X11 (SSH)"**

#. **Edit the Timeout setting of the Remote X11 extension to '15' (seconds)**

#. **Add the following envionment variables:**

    .. code-block:: bash

        DISPLAY :0.0
        XAUTHORITY C:\\cygwin64\\home\\lpoutou\\.Xauthority

#. **Add the Cygwin bin path to the path environment variable** (to make the 'xauth' program detectable) (:code:`C:\cygwin64\bin`)



Example of a config file:

.. code-block:: bash
        
    Host lpoutou-px-power
        User lpoutou
        HostName px-power.px.algo.fr
    Host lpoutou-px-weak
        User lpoutou
        HostName px-weak.px.algo.fr
    Host axiao-px-power
        User axiao
        HostName px-power.px.algo.fr
    Host *
        ForwardAgent yes
        Compression yes
        PreferredAuthentications publickey,password,keyboard-interactive,hostbased
        ForwardX11 yes
        ForwardX11Trusted yes
        Ciphers aes128-ctr,aes192-ctr,aes256-ctr,aes256-gcm@openssh.com,aes128-gcm@openssh.com,aes128-cbc,aes192-cbc,aes256-cbc,3des-cbc,chacha20-poly1305@openssh.com,rijndael-cbc@lysator.liu.se
        #Ciphers arcfour256,arcfour128,arcfour,blowfish-cbc,cast128-cbc
        MACs hmac-md5,hmac-sha1,umac-64@openssh.com,umac-128@openssh.com,hmac-sha2-256,hmac-sha2-512,hmac-sha1-96,hmac-md5-96,umac-64-etm@openssh.com,umac-128-etm@openssh.com,hmac-sha2-256-etm@openssh.com,hmac-sha2-512-etm@openssh.com,hmac-md5-etm@openssh.com,hmac-sha1-etm@openssh.com,hmac-sha1-96-etm@openssh.com,hmac-md5-96-etm@openssh.com
        #MACs hmac-ripemd160,hmac-ripemd160-etm@openssh.com,hmac-ripemd160@openssh.com
        HostKeyAlgorithms ssh-rsa-cert-v01@openssh.com,ssh-rsa,ecdsa-sha2-nistp256-cert-v01@openssh.com,ssh-dss-cert-v01@openssh.com,ssh-dss,ecdsa-sha2-nistp384-cert-v01@openssh.com,ecdsa-sha2-nistp521-cert-v01@openssh.com,ssh-ed25519-cert-v01@openssh.com,ecdsa-sha2-nistp256,ecdsa-sha2-nistp384,ecdsa-sha2-nistp521,ssh-ed25519
        NoHostAuthenticationForLocalhost yes
        StrictHostKeyChecking no
        CheckHostIP no
        UseRoaming no

(Some Ciphers and MACs have been deactivated because they are not available in Windows, you can also comment them all, as ssh will automatically choose one available anyway) 