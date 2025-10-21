.. _censo_example:

===================
Example Calculation
===================

Introduction
------------

In this exmaple, we will calculate the NMR spectrum of glycerol. Inputs are provided, as well as all commands used.

Getting Started
---------------

Make sure you have CREST installed, since we are going to use some of the features to generate the spectrum in the end using ANMR.
Otherwise, you can also skip the ANMR step and use a different program to proceed, then you can use any other tool for conformational analysis.
The input for CENSO must however be in xyz-format.

Generating the ensemble
-----------------------

Coordinates for the glycerol molecule in xyz-format are provided below.
.. code:: text
    14

    C         -0.0332833509        0.7993977341       -0.5076202431
    C         -1.2462908476       -0.1281178757       -0.6917545421
    C          1.2858100282        0.0095303998       -0.6494477230
    O         -0.0916630568        1.3936456646        0.7706440001
    O          1.4328267729       -0.9389872239        0.3851401138
    O         -1.3668536124       -0.9918460280        0.4108920948
    H         -0.5405929490       -1.4991998599        0.4727537353
    H         -0.6540574400        0.8197630567        1.3150523847
    H          1.4768811463       -0.4464956300        1.2155408950
    H         -0.0489195612        1.5987895030       -1.2562724743
    H         -2.1662143011        0.4617838506       -0.7173498360
    H         -1.1547003795       -0.6842782853       -1.6332401341
    H          2.1278994642        0.7118806200       -0.6460126171
    H          1.2931938285       -0.5480483223       -1.5877298046

To generate the ensemble using CREST you should have these coordinates in a file called ``glycerol.xyz``:

.. code:: sh
   crest glycerol.xyz --nmr --alpb chcl3 > crest.out

In this example, we want to calculate the NMR spectrum in chloroform, which is why we use the ALPB solvation model in CREST.

.. hint::
   Note, that CREST is based on meta-dynamics and is inherently a stochastic process. 
   Therefore, you should always aggregate over multiple runs.

Running CENSO
-------------

To run CENSO on the ensemble, you need to specify all parts you want to use. In this case, we will stick to just running the screening step
and the geometry optimization step, and of course the NMR calculation. Specifying an input file explicitly is not necessary, unless it is not called
``crest_conformers.xyz``.

To run CENSO:

.. code:: sh
   censo -i crest_conformers.xyz --screening --optimization --nmr --solvent chcl3 > censo.out

CENSO should run out of the box without having to configure anything if all programs you want to use are found in your ``$PATH``. The defaults for screening
are to use TURBOMOLE with COSMOtherm. For the geometry optimization, the default uses TURBOMOLE with ANCOPT as driver (xtb). If you need to modify these settings,
you can use ``censo --new-config`` to generate a new configuration file or just use the example file provided in the CENSO GitHub repository. You can 
then use this configuration file with:

.. code:: sh
   censo -i crest_confermers.xyz -S -O --nmr --inprc censo2rc_NEW --solvent chcl3 > censo.out

.. hint::
   There are short flags for the ensemble optimization steps for the CLI: ``-P`` for prescreening, ``-S`` for screening, ``-O`` for optimization, ``-R`` for refinement.

In the end, CENSO will generate xyz files containing the ensembles at each ensemble optimization step, as well as json files containing information about energetics and
calculated spectroscopic parameters.

Configuring and running ANMR
----------------------------

ANMR requires a directory called ``anmr`` with a specific structure to be present in the current working directory. This directory will contain all the necessary files to run ANMR.
To set up the ``anmr`` directory, you can use the provided script:

.. code:: sh
   c2anmr

Then configure the ``.anmrrc`` file located in the ``anmr`` directory according to your requirements.
You can read more about the setup in :ref:`nmr`.

Then change into the ``anmr`` directory and run:

.. code:: sh
   anmr -plain > anmr.out

To plot the spectrum, you can use the ``nmrplot`` script. Run it in the ``anmr`` directory:

.. code:: sh
   nmrplot

This will provide you with a plot of the NMR spectrum. You can modify plotting options via the CLI of the script.
