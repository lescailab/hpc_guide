Connecting to the cluster
==========================

Below you can find a few basic steps, to get you setup and ready to connect to EOS.

Terminal
-----------

If you are on a UNIX system (i.e. either Linux or Mac OS), a terminal will already be present natively in your operating system.

If you are on Windows, then we recommend you install `MobaXTerm`_ which is a very good UNIX emulator, and includes already useful things like an X server and a package management system to help you use languages like python or perl.

.. _MobaXTerm: https://mobaxterm.mobatek.net/download.html


Generate your ssh key
----------------------

First of all you should verify if you already have an SSH key in your system, which you might have generated earlier yourself or by using another software.
To do this, open your terminal and type :code:`ls -la ~/.ssh`

If you already have one or more keys, you will see a list, like in the example below

.. code-block:: bash

        [user@machine ~]$ ls -la ~/.ssh
        total 4
        drwx------  2 user group    3 Feb  9 15:42 .
        drwxr-x--- 17 user group   25 Feb 20 12:32 ..
        -rw-------  1 user group 1152 Feb  9 15:42 authorized_keys
        -rw-------  1 user group  227 Jan 14 10:01 id_ecdsa
        -rw-------  1 user group  178 Jan 14 10:01 id_ecdsa.pub

If you do not have a key already, and the command above does not list any *.pub* file, then you should generate one.

In order to generate a key, you should type 

.. code-block:: bash 

        ssh-keygen -t ed25519 -C "your_email@example.com"

In some systems, you might have to choose a different encription and use:

.. code-block:: bash 

        ssh-keygen -t rsa -C "your_email@example.com"

When you're prompted to "Enter a file in which to save the key," press Enter. This accepts the default file location.
Similarly, if you just enter when a passphrase is requested, it will generate the key without one.

Now you should print at screen the public key that you just generated, in order to use it in the next step.

.. code-block:: bash 

        cat ~/.ssh/id_rsa.pub

or use *id_ecdsa.pub* depending on the encryption you have used.

.. warning::
    
    You always want to copy the *.pub* file because this is the **public** key you can share. The other key is a private one, which you should never share.


Request an account
---------------------

Once you have generated or identified your **public** key, you will use that when requesting an account, following this _`link`.

.. _link: https://forms.gle/tiH9KDPakGPpGz2H8


Connect to the cluster 
-----------------------

When you receive confirmation, that your account has been activated, you can then connect to the cluster by openin a terminal and using:

.. code-block:: bash

        ssh user@eos.unipv.it

If you need to display any graphics, you can use instead 

.. code-block:: bash

        ssh -X user@eos.unipv.it

And a session on the terminal will be opened, within the login node of the cluster.

.. note::

        In order for a graphical window to be functional on MacOS an X server needs to be installed, because the OS does not have a native one like in Linux. We recommend _`XQuartz`.

.. _XQuartz: https://www.xquartz.org