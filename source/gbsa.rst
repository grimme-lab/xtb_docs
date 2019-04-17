.. _gbsa:

--------------------
 Implicit Solvation
--------------------

In this chapter, all neccessary information will be given in order
to use the implicit solvent model GBSA in `xTB` calculations.
Parameterized solvents and available grids are given as well.

.. contents::

General command-line control
============================

The generalized born (GB) model with solvent accessable surface area
(SASA) termed GBSA is envoked with the flag ``--gbsa [Solvent]`` or 
alternative ``-g [Solvent]``. As an example the single point calculation employing the 
GBSA model for solvation in toluene would be started by

.. code:: bash

  > xtb coord --gbsa toluene

Optimizing a geometry with the GBSA model can be done with the following input

.. code:: bash

  > xtb coord --opt --gbsa toluene

The order of the flags can be altered and the input
is not case sensitive.
Like in a optimization without GBSA the optimized coordinates are
written to a new file (``xtbopt.coord``).
In General the GBSA can be used in combination with all runtypes 
implemented in the GFN-xTB.

Parameterized Solvents
======================

The GBSA model is parameterized for the Hamiltonian of 
GFN1-xTB and GFN2-xTB, but not for GFN0-xTB. 
Also some solvents were parameterized only for GFN1 or GFN2.
Here is a list of the available solvents.

+---------------+-------+-------+
| solvents      | GFN1  | GFN2  |
+===============+=======+=======+
| Acetone       |   x   |   x   |
+---------------+-------+-------+
| Acetonitrile  |   x   |   x   |
+---------------+-------+-------+
| Benzene       |   x   |       |
+---------------+-------+-------+
| CH2Cl2        |   x   |   x   |
+---------------+-------+-------+
| CHCl3         |   x   |   x   |
+---------------+-------+-------+
| CS2           |   x   |   x   |
+---------------+-------+-------+
| DMF           |       |   x   |
+---------------+-------+-------+
| DMSO          |   x   |   x   |
+---------------+-------+-------+
| Ether         |   x   |   x   |
+---------------+-------+-------+
| Water (H2O)   |   x   |   x   |
+---------------+-------+-------+
| Methanol      |   x   |   x   |
+---------------+-------+-------+
| n-Hexan       |       |   x   |
+---------------+-------+-------+
| THF           |   x   |   x   |
+---------------+-------+-------+
| Toluene       |   x   |   x   |
+---------------+-------+-------+


Available Grids
===============

Different Lebedev grids for the calculation of the SASA term are 
implemented in GFN-xTB. The grids are independent of the used GFNn method 
and are called as example like this

.. code:: bash

  > xtb coord --opt --gbsa toluene tight

The default grid level is ``normal``. 
The available grid levels are given in the table below
with the corresponding numer of gridpoints.

+---------------+--------------+
| Gridlevel     |   Gridpoints |
+===============+==============+
| normal        |      230     |
+---------------+--------------+
| tight         |      974     |
+---------------+--------------+
| verytight     |     2030     |
+---------------+--------------+
| extreme       |     5810     |
+---------------+--------------+

Larger grids increase the computation time and
reduce numerical noise in the energy. They may help to converge 
geometry optimizations with GBSA for large molecules which 
would otherwise not converge due to numerical noise.

Reference States
================
 
The default reference state option is ``bar1M`` which should not
be changed for normal production runs.
In order to compare the solvation free energy with COSMO-RS
solvation free energies the reference state can be set to ``reference`` which corresponds
to the same option in COSMO-RS. This could be done with

.. code:: bash

  > xtb coord --opt --gbsa toluene reference
