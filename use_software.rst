Installing and using software
==================================

We recommend that you install and use the Conda package manager to install software on NIBSC HPC.

Why Conda?
----------------

The interesting thing about Conda is that it allows you to use separate environments for separate projects. If you have a project where you’ve installed a number of packages for Python or R you might not need to use them on another project, or you might need different versions of those packages: in this case, you can just create separate environments for them instead of installing and uninstalling multiple times. With separate environments you force yourself to make the dependencies for each project explicit which in turn makes it easier for collaborators to run your code and improves reproducibility.

Conda also provides access to thousands of packages used in data science and bioinformatics. These packages can be installed with a single command, so you don’t have to worry about compilers, dependencies, and where to put binaries.
Conda is already available to every user of the NIBSC HPC.


Configuring Conda
--------------------------

Conda can install packages from different *channels*. This system is similar to *repositories* in other package managers. Here we’ll add a few channels that are commonly used in bioinformatics:

.. code-block:: bash

  [hpc-head]$ conda config --add channels defaults
  [hpc-head]$ conda config --add channels bioconda
  [hpc-head]$ conda config --add channels conda-forge
  [hpc-head]$ conda config --add channels NIBSC HPC


Searching for packages
-------------------------

You can easily search for Conda packages through the website anaconda.org or using the conda search command::

  [hpc-head]$ conda search rstudio


Remember that the Conda package may not be called in the same way as the exact official name of the software. For example, the Conda package for the tool biobambam2 is just called biobambam, so searching for biobambam2 would not return any results.


Using environments
------------------------

Conda comes with a single environment known as the *base* environment. To activate the base environment, just type::

  [hpc-head]$ conda activate
  (base) [hpc-head]$

You now have access to the software installed in the base environment.

If we wanted to create a new environment with the newest version of PySAM, we should follow the steps below:

.. code-block:: bash

    [hpc-head]$ conda create --name amazing-project pysam
    Solving environment: done

    ## Package Plan ##

      environment location: /Users/das/.conda/envs/amazing-project

      added / updated specs:
        - pysam
        - python=3


    The following packages will be downloaded:

        package                    |            build
        ---------------------------|-----------------
        pysam-0.15.1               |   py36h0380709_0         2.0 MB  bioconda
        bcftools-1.9               |       h4da6232_0         789 KB  bioconda
        samtools-1.9               |       h8ee4bcc_1         526 KB  bioconda
        setuptools-40.4.3          |           py36_0         556 KB
        certifi-2018.10.15         |           py36_0         138 KB
        libcurl-7.61.1             |       hf30b1f0_0         457 KB
        libffi-3.2.1               |                1          41 KB  bioconda
        htslib-1.9                 |       hc238db4_4         1.2 MB  bioconda
        curl-7.61.1                |       ha441bb4_0         135 KB
        wheel-0.32.2               |           py36_0          35 KB
        libdeflate-1.0             |       h470a237_0          44 KB  bioconda
        bzip2-1.0.6                |       h1de35cc_5         149 KB
        ------------------------------------------------------------
                                               Total:         6.0 MB

    The following NEW packages will be INSTALLED:

        bcftools:        1.9-h4da6232_0          bioconda
        bzip2:           1.0.6-h1de35cc_5
        ca-certificates: 2018.03.07-0
        certifi:         2018.10.15-py36_0
        curl:            7.61.1-ha441bb4_0
        htslib:          1.9-hc238db4_4          bioconda
        libcurl:         7.61.1-hf30b1f0_0
        libcxx:          4.0.1-hcfea43d_1
        libcxxabi:       4.0.1-hcfea43d_1
        libdeflate:      1.0-h470a237_0          bioconda
        libedit:         3.1.20170329-hb402a30_2
        libffi:          3.2.1-1                 bioconda
        libssh2:         1.8.0-h322a93b_4
        ncurses:         6.1-h0a44026_0
        openssl:         1.0.2p-h1de35cc_0
        pip:             10.0.1-py36_0
        pysam:           0.15.1-py36h0380709_0   bioconda
        python:          3.6.6-hc167b69_0
        readline:        7.0-h1de35cc_5
        samtools:        1.9-h8ee4bcc_1          bioconda
        setuptools:      40.4.3-py36_0
        sqlite:          3.25.2-ha441bb4_0
        tk:              8.6.8-ha441bb4_0
        wheel:           0.32.2-py36_0
        xz:              5.2.4-h1de35cc_4
        zlib:            1.2.11-hf3cbc9b_2

    Proceed ([y]/n)? y


    Downloading and Extracting Packages
    pysam-0.15.1         | 2.0 MB    | ################################## | 100%
    bcftools-1.9         | 789 KB    | ################################## | 100%
    samtools-1.9         | 526 KB    | ################################## | 100%
    setuptools-40.4.3    | 556 KB    | ################################## | 100%
    certifi-2018.10.15   | 138 KB    | ################################## | 100%
    libcurl-7.61.1       | 457 KB    | ################################## | 100%
    libffi-3.2.1         | 41 KB     | ################################## | 100%
    htslib-1.9           | 1.2 MB    | ################################## | 100%
    curl-7.61.1          | 135 KB    | ################################## | 100%
    wheel-0.32.2         | 35 KB     | ################################## | 100%
    libdeflate-1.0       | 44 KB     | ################################## | 100%
    bzip2-1.0.6          | 149 KB    | ################################## | 100%
    Preparing transaction: done
    Verifying transaction: done
    Executing transaction: done


This gives us a clean environment with just the minimal number of packages necessary to support PySAM. To use the software that was installed in the environment, the environment needs to be activated first:

.. code-block:: bash

    [hpc-head]$ conda activate amazing-project
    (amazing-project) [hpc-head]$ python -c 'import pysam; print(pysam.__version__)'
    0.6.0

You will notice that the prompt changed to show you that you’re now in the amazing-project environment.

Conda can install any kind of software, as long as its *recipe* (i.e. instructions) are available in the conda repositories we are using. This means that your entire setup can be installed through Conda (if all packages are available).
For example, you can create an environment with Rstudio, R, and ggplot2 with a single command.


Available Modules on the Cluster
---------------------------------

Before installing something on your own environment, it is always worth checking what has been already installed for everyone on the HPC. This can be done with the following command::

  [hpc-head]$ module avail

Which will show the available *modules*. You can then activate a specific tool by using the following command::

  [hpc-head]$ module load NAME

Where *NAME* corresponds **exactly** to the name in the list generated with the previous command.


Command reference
----------------------

To install software in the currently activated environment::

    (amazing-project) [hpc-head]$ conda install PACKAGE-NAME

To remove a software package from the currently activated environment::

    (amazing-project) [hpc-head]$ conda remove PACKAGE-NAME

To update a software package in the currently activated environment::

    (amazing-project) [hpc-head]$ conda update PACKAGE-NAME

Since Conda keeps track of what you are loading in the environment you created, it will tell you exactly which packages are used in the environment. This is very useful for collaborating with others, since your collaborators can create an exact copy of your environment with a single command.

To export your environment so that others can recreate it::

    (amazing-project) [hpc-head]$ conda env export > environment.yml

The **environment.yml** file contains an exact specification of your environment and the packages installed. You share this with other collaborators, and they will be able to recreate your environment by running::

    [hpc-head]$ conda env create -f environment.yml

You can read more about using environments for projects `here`_. There’s also also a `cheat sheet`_ with Conda commands available.

.. _here: http://hpc.nibsc.ac.uk/wiki/hpcdoc/best_practices.html
.. _cheat sheet: http://know.continuum.io/rs/387-XNW-688/images/conda-cheatsheet.pdf


I don’t think I can use Conda because…
------------------------------------------

A Conda package is not available
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If building a custom Conda package is not possible, we recommend using a `Singularity`_ image instead.

.. _Singularity: https://sylabs.io/docs/


I’m part of a project that specifies the software I should use
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In this case the project should and probably will supply you for either a set of Conda packages or Singularity images. If not, most or all of the software will probably be available through Conda anyway, so you can still set up an environment with the software.


Using graphical interfaces
----------------------------

In order to use programs with a graphical user interface on NIBSC HPC you should activate X-forwarding, when connecting to the cluster.

You can use X-forwarding to tunnel individual graphical programs to your local desktop. This works well for many programs, but programs that do fancy graphics or anything animated might not work well.

On Linux you simply need to tell SSH that you wish to enable X-forwarding. To do this, add -X to the ssh command when logging in to the cluster, for example::

    [local]$ ssh -X USERNAME@hpc-head

Since macOS does not include an X server, you will need to download and install XQuartz on your computer. When installed, reboot the computer. Now, you just need to tell SSH that you wish to enable X-forwarding. To do this, add -X to the ssh command when logging in to the cluster, for example::

  [local]$ ssh -X USERNAME@hpc-head

On Windows, we recommend that you use **MobaXterm** which has an integrated X server.
