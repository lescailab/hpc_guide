Best practices
================================

Structure and document your projects
-------------------------------------

Using the same project structure for all of your projects is a good way to maintain consistency and to facilitate when writing custom scripts. We recommend a structure similar to the one in the following example. It is flexible enough to work for a wide variety of data analysis projects, but feel free to adapt it to your needs.

.. code-block:: bash

    myproject/
        raw_data/
           README.txt
           sample001/
           sample002/
           ...
        qc/
           README.txt
           ...
        alignments/
           README.txt
           sample001/
           sample002/
           ...
        analysis/
           README.txt
           method001/
           method002/
           ...
        scripts/
           README.txt
           ...
        docs/
           1-installing-dependencies.txt
           2-running-some-analysis.txt
           3-running-some-other-analysis.txt
           NOTEBOOK.Rmd
           ...
        environment.yml
        README.txt
        ...

This may seem like a large structure for a simple project, but you can always scale down this structure. E.g. for small projects you may drop the docs folder and simply document how to run your analyses in myproject/README.txt. Each folder contains a README.txt file which documents the purpose and contents of the folder. You don’t have to write a lot, just what’s necessary to understand your project and its different parts.

.. warning::

  If you consider modifying the basic project structure, make sure that you thought about what it means for the reproducibility of your project! Consider whether you're responding to a specific need, and how such a change affects reproducibility.

Let’s have a look at the goal of each folder:

raw_data
~~~~~~~~~

This folder should contain your raw/original data files, organised in subfolders each by sample name or identifier. You should consider this directory read-only (even better: make it read-only). However, it is okay to add new files to the folder, as long as you don’t modify or delete other files in the folder.

qc
~~~

This folder contains the outputs of different QC tools you might be running (fastQC, FastqScreen, BamStats, multiQC and the likes). It might contain several sub-folders, depending on the outputs produced by the different tools.

analysis
~~~~~~~~~

This folder contains the results of any analyses you might be running on your dataset. We recommend to structure it by method and/or analysis run, so you can quickly recognise what results you may find there and how they have been generated.

scripts
~~~~~~~~~~~

This directory contains the scripts you wrote for your analysis, e.g. those used by your workflow. This folder is **optional** because we recommend to use git or bitbucket for versioning control of your code: if you're using version-control and you want to keep all your scripts into a single repository, then you might want to clone the repository in dataset-independent folder. However, if you maintain your code in a project-specific repository you might clone your .git in this location.

docs
~~~~~~~~

This folder is mainly for large projects where it may be nice to document each analysis in its own file. If you use plain text files as shown above, remember to give them numbers to make it easier for the reader to figure out where to begin.
We recommend however to document all activities performed on your data in a reproducible research format (a Jupyter notebook or a Markdown notebook/document): this will facilitate complete understanding of your analyses and the rationale behind all choices you made during the project (including file renaming, folder creation, etc.).

If you're interested in how we document our own workflow, you may have a look at this `page`_: we run many different projects and therefore we maintain a specific repository for all projects to version control both the project-specific code as well as our markdown notes.

.. _page: http://hpc.nibsc.ac.uk/wiki/new_wiki/howto/reporting_workflow.html

In addition to the folders described above, the project root should also contain at least two files: a file documenting your project structure (this could just contain a short introduction to the project and a link to this page) and a file describing your environment.


Multi-user project
--------------------

In the case of multi-user projects where many people collaborate on the same data, the following structure may be used.

.. code-block:: bash

    myproject/
        data/
            README.txt
            ...
        people/
            username1/
            username2/
            username3/
            ...
        results/
            README.txt
            ...
        README.txt

In this scenario the root folder contains a people folder named after the username of each person working on the project. Each of these folders uses the same directory structure as described for single-user projects. This means that each user his/her own scripts, sandbox, docs, results, steps and data folders. The user-specific data folder can contain data files that are not used by everyone associated with the project, but it can also contain symbolic links to the root data folder.

The root results folder is used to aggregate results from different users by creating symbolic links to specific result files. For example, say that user A produced a result foo.txt and user B wants to use this file. User B can then create a symlink from myproject/people/A/results/foo.txt to /myproject/analysis/method001/foo.txt.


Use project-specific environments
-----------------------------------

An environment is a isolated collection of programs and libraries. You can have multiple environments (e.g. one for each project) and these environments can have different software and even different versions of the same software installed simultaneously. To use an environment you must activate it. This will load all of the software available in the environment into your shell so that it is available as any other program installed on the machine.

Detailed documentation on how to use the conda command can be found `here`_. Then run:

.. _here: http://hpc.nibsc.ac.uk/wiki/hpcdoc/use_software.html

.. code-block:: bash

    [hpc-head]$ conda activate
    (base) [hpc-head]$ conda create -n myproject python=3.5


This will create an environment called myproject with Python 3.5 installed. To enter the environment, use this command::

    [hpc-head]$ conda activate myproject
    (myproject) [hpc-head]$

Now check that the environment has been activated correctly by starting Python::

    [hpc-head]$ python
    Python 3.5.1 |Continuum Analytics, Inc.| (default, Dec 7 2015, 11:24:55)
    [GCC 4.2.1 (Apple Inc. build 5577)] on darwin
    Type "help", "copyright", "credits" or "license" for more information.
    >>> import numpy
    Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
    ImportError: No module named 'numpy'

As you can see running the python command now opens Python 3.5.1 and we can also see that the Python installation was provided by Continuum, the company providing Anaconda. However, if we try to import e.g. numpy we get an error because this package has not been installed in the environment. Let’s try to install it. Press Control-d to close the Python interpreter and then run this command::

    [hpc-head]$ conda install numpy

This will install the latest version of the numpy package into the current environment (you may have to say yes to installing the packages). Now try to open Python again and import numpy. It should work this time.

The conda install command lets you choose exactly which version of the package to install. When we created the myproject environment which chose to specifically install Python version 3.5 using the *=* character. This syntax also works for conda install, e.g. *conda install numpy=1.9.1*.

Before leaving the environment, it might be good to export the environment and its packages to make it available to other people. We can do this with::

    (myproject) [hpc-head]$ conda env export > environment.yml

Which allows other members of the project to recreate your exact environment::

    [hpc-head]$ conda env create -f environment.yml
    [hpc-head]$ conda activate myproject


When you are done working with your project, or you want to switch to another environment for working with another project, run the command::

    [hpc-head]$ conda deactivate


You may think that Anaconda only works for Python and Python packages, however, Anaconda actually works for any program that is available as an Anaconda package (which may Python, R or any other language, including binaries). Packages are provided through channels. While the official Anaconda channel contains thousands of popular packages, other channels provide even more packages. One such channel is the R channel which provides access to the R programming language and many popular libraries used with R. To get access to the R channel run::

    [hpc-head]$ conda config --add channels r

Another great channel is the **Bioconda** channel which provides access to hundreds of packages related to bioinformatics such as BWA, samtools, BLAST etc.::

    [hpc-head]$ conda config --add channels bioconda


To make things more reproducible, rather than installing packages one by one in an interactive session, Anaconda allows you to specify a list of channels and packages with specific versions in an environment file. You can create a file called environment.yml in the project folder and put this in the file:

.. code-block:: bash

    name: myproject
    channels:
      - r
      - bioconda
    dependencies:
      - python=3.4
      - numpy=1.9.2
      - r-essentials=1.4
      - bwa=0.7.15

As you work you may need to change your environment, e.g. update a package to a more recent version, add or remove a package. To do this, just modify the environment.yml file and then run::

    [hpc-head]$ conda env update --prune
