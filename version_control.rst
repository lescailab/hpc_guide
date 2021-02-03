Code Version Control
======================


Version control systems are a category of software tools that help a software team manage changes to source code over time. Version control software keeps track of every modification to the code in a special kind of database. If a mistake is made, developers can turn back the clock and compare earlier versions of the code to help fix the mistake while minimizing disruption to all team members.
This is very nicely explained on Atlassian `tutorial pages`_.

.. _tutorial pages: https://www.atlassian.com/git/tutorials/what-is-version-control

We recommend to read the tutorial and download their `cheatsheet`_ which will provide an easy reference to look at, for the most common commands in git control.

.. _cheatsheet: https://www.atlassian.com/dam/jcr:8132028b-024f-4b6b-953e-e68fcce0c5fa/atlassian-git-cheatsheet.pdf


Configuring Git SSH
-------------------------

Using git via SSH you will be able to update your repository without typing your username and password everytime.

In order to do this, you need to generate your own SSH key as documented at `this page`_ and then configure github and record your SSH key on your own github profile settings, as explained in this `github page`_.

.. _this page: https://help.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent
.. _github page: https://help.github.com/en/github/authenticating-to-github/adding-a-new-ssh-key-to-your-github-account


Cloning a Repository
----------------------

Once you're all set with SSH keys and configuration, you can follow `these instructions`_ to clone a repository.

.. _these instructions: https://help.github.com/en/github/creating-cloning-and-archiving-repositories/cloning-a-repository

.. note::

    To work more comfortably, we recommend to clone your repository **both** on your laptop and on your HPC home. In this way you can conveniently edit the code on your computer using any editor or graphical interface you might prefer, *commit* and *push* the changes to the repository and then simply *pull* the changes on the cluster to update the code.
