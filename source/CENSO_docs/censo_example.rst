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
=======================

.. code::
   glycerol.xyz

To generate the ensemble using CREST:

.. code:: sh
   crest glycerol.xyz --nmr --alpb chcl3 > crest.out

To run CENSO on the ensemble:

.. code:: sh
   censo -i crest_conformers.xyz --screening --optimization --nmr > censo.out

To set up the ``anmr`` directory:

.. code:: sh
   c2anmr

Then configure the ``.anmrrc`` file located in the ``anmr`` directory according to your requirements.
Then change into the ``anmr`` directory and run:

.. code:: sh
   anmr -plain > anmr.out

To plot the spectrum:

.. code:: sh
   nmrplot
