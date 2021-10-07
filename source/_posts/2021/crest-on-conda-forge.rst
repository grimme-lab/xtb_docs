:date: 2021-10-07
:author: Sebastian Ehlert
:category: conda
:tags: dftd4

CREST 2.11.1 available on conda-forge
=====================================

.. image:: https://github.com/grimme-lab/crest/raw/master/assets/crest.png
   :width: 30%
   :alt: CREST

CREST is now available from the conda-forge channel for Linux and MacOS platforms.
Builds for MacOS/ARM64 are currently not available due to missing libraries on this architecture.

For more details checkout the `package <https://anaconda.org/conda-forge/crest>`_ or the `feedstock repository <https://github.com/conda-forge/crest-feedstock>`_.

To install CREST with conda add conda-forge to your channels, if it is not available yet:

.. code::

   conda config --add channels conda-forge

Now you can create a new environment with both CREST and ``xtb`` using

.. code::

   conda create -n conf crest xtb
   conda activate conf
