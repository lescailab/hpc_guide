Using Nextflow on HPC
=========================

Nextflow is a workflow language that enables scalable and reproducible workflow management.  It allows the adaptation of pipelines written in the most common scripting languages.
Nextflow allows you to write a computational pipeline by making it simpler to put together many different tasks.

Nextflow supports Docker and Singularity containers technology. This, along with the integration of the GitHub code sharing platform, allows you to write self-contained pipelines, manage versions and to rapidly reproduce any former configuration. Additionally, Nextflow also supports Conda environments so it will work very nicely with the environments you might have setup already for your projects.

More importantly,  Nextflow provides an abstraction layer between your pipeline's logic and the execution layer, so that it can be executed on multiple platforms without it changing.

You can read more about Nextflow on `their website`_.

.. _their website: https://www.nextflow.io/


Launching a Nextflow pipeline
------------------------------

Nextflow is already installed on the cluster. To use it, you need to activate the module installed by typing the following command::

  [hpc-head]$ module load NextFlow/latest

Once the module is activated, you might test Nextflow simply by typing::

  [hpc-head]$ nextflow -C /path/to/myconfig.config run /path/to/myscript.nf


.. warning::

  Running Nextflow, particularly when running longer and more complex pipelines, can be memory intensive if not computing intensive. Like any other computations, please **do not run on hpc-head** as this might impede other uses to access the login node and therefore use the HPC.
  Please run Nextflow either with an interactive job for testing, requesting minimal amount of resources, or within a job script for larger scale testing or production analyses.


Testing your pipelines
~~~~~~~~~~~~~~~~~~~~~~~

If you need to test your code, particularly when writing a new pipeline, we recommend launching an interactive job as explained in other pages::

  [hpc-head]$ srun --mem=2g --pty /bin/bash
  srun: job 17129453 queued and waiting for resources
  srun: job 17129453 has been allocated resources
  [b1s3]$


And within the new session, use nextflow as indicated above::

  [b1s3]$ module load NextFlow/latest
  [b1s3]$ nextflow run /path/my/test_script.nf


Large testing or Production
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

For larger tests, or for your established analyses, you will first have your consolidated nextflow script (let's call it *mypipeline.nf*) and then you will create a job script (let's call it *launch_nf.job*) that will launch the pipeline as in the following example:

.. code-block:: bash

    #!/bin/bash
    #SBATCH --partition WORK
    #SBATCH --mem 16G
    #SBATCH -c 8
    #SBATCH -t 12:00:00

    PIPELINE=$1
    CONFIG=$2

    module load NextFlow/latest

    nextflow -C ${CONFIG} run ${PIPELINE}


Which you then can launch from hpc-head as a normal *sbatch* submission::

    [hpc-head]$ sbatch launch_nf.job /home/me/code/mypipeline.nf /home/me/code/myconfig_file.conf

When preparing your job script of course, make sure to estimate correctly the resources necessary and particularly the time. The closest is your estimate to reality, the fairer your usage is towards all other HPC users.
