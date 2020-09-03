.. _gbsa:

--------------------
 Implicit Solvation
--------------------

In this chapter, all neccessary information will be given in order
to use the implicit solvent model ALPB in xTB calculations.
Parameterized solvents and available grids are given as well.

.. contents::

General command-line control
============================

.. note::

   The ALPB solvation model is implemented in version 6.3.3 or newer, use ``--gbsa`` in older versions instead.

The analytical linearized Poisson-Boltzmann (ALPB) model is envoked with the flag ``--alpb [Solvent]``.
As an example the single point calculation employing the ALPB model for solvation in water would be started by

.. code:: bash

  > xtb coord --alpb water

As an example the energy printout of a singlepoint calculation of a H₂O molecule in implicit water is given.

.. code:: bash

 :::::::::::::::::::::::::::::::::::::::::::::::::::::
 ::                     SUMMARY                     ::
 :::::::::::::::::::::::::::::::::::::::::::::::::::::
 :: total energy               -5.080052453799 Eh   ::
 :: total w/o Gsasa/hb         -5.072629830168 Eh   ::
 :: gradient norm               0.004391355361 Eh/α ::
 :: HOMO-LUMO gap              14.784541887474 eV   ::
 ::.................................................::
 :: SCC energy                 -5.113963912352 Eh   ::
 :: -> isotropic ES             0.042951967946 Eh   ::
 :: -> anisotropic ES          -0.000414697277 Eh   ::
 :: -> anisotropic XC          -0.000390138125 Eh   ::
 :: -> dispersion              -0.000131341861 Eh   ::
 :: -> Gsolv                   -0.011759733450 Eh   ::
 ::    -> Gborn                -0.004337109820 Eh   ::
 ::    -> Gsasa                 0.000220003644 Eh   ::
 ::    -> Ghb                  -0.009500070401 Eh   ::
 ::    -> Gshift                0.001857443127 Eh   ::
 :: repulsion energy            0.033911458523 Eh   ::
 :: add. restraining            0.000000000000 Eh   ::
 :::::::::::::::::::::::::::::::::::::::::::::::::::::

The solvation free energy is printed as ``Gsolv`` and is also added
to all total energy printouts.

Optimizing a geometry with the ALPB model can be done with the following input

.. code:: bash

  > xtb coord --opt --alpb water

The order of the flags can be altered and the input
is not case sensitive.
Like in a optimization without ALPB the optimized coordinates are
written to a new file (``xtbopt.coord``).
In General the ALPB can be used in combination with all available run types
implemented in the `xtb`.

Parameterized Solvents
======================

The ALPB model is parameterized for the Hamiltonian of GFN1-xTB, GFN2-xTB, and the GFN-FF, but not for GFN0-xTB.
For the GFN1-xTB and GFN2-xTB Hamltonians also a generalized Born (GB) model with surface area (SA) contributions, dubbed GBSA is available.
Here is a list of the available solvents.

=============== ============ ============ ============ ============ ========
 solvents        GFN1(ALPB)   GFN1(GBSA)   GFN2(ALPB)   GFN2(GBSA)   GFN-FF
=============== ============ ============ ============ ============ ========
 Acetone         x            x            x            x            x
 Acetonitrile    x            x            x            x            x
 Aniline         x                         x                         x
 Benzaldehyde    x                         x                         x
 Benzene         x            x            x            x            x
 CH₂Cl₂          x            x            x            x            x
 CHCl₃           x            x            x            x            x
 CS₂             x            x            x            x            x
 Dioxane         x                         x                         x
 DMF             x                         x                         x
 DMSO            x            x            x            x            x
 Ether           x            x            x            x            x
 Ethylacetate    x                         x                         x
 Furane          x                         x                         x
 Hexandecane     x                         x                         x
 Hexane          x                         x            x            x
 Methanol                     x                         x
 Nitromethane    x                         x                         x
 Octanol         x                         x                         x
 Octanol (wet)   x                         x                         x
 Phenol          x                         x                         x
 Toluene         x            x            x            x            x
 THF             x            x            x            x            x
 Water (H₂O)     x            x            x            x            x
=============== ============ ============ ============ ============ ========

To get the legacy GBSA model setup a detailed input with

.. code-block:: text

   $gbsa
      kernel=still

and invoke the program with the ``--gbsa`` flag instead of the ``--alpb`` flag.


Available Grids
===============

Different Lebedev grids for the calculation of the SASA term are
implemented in ``xtb``. The grids are independent of the used GFNn method
and are set in the detailed input as

.. code-block:: text

  $gbsa
     gbsagrid=tight


The default grid level is ``normal``.
The available grid levels are given in the table below
with the corresponding number of gridpoints.

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
geometry optimizations with ALPB for large molecules which
would otherwise not converge due to numerical noise.

Reference States
================

The default reference state option is ``bar1M`` which should not
be changed for normal production runs.
In order to compare the solvation free energy with
solvation free energies from COSMO-RS the reference state can be set to ``reference`` which corresponds
to the same ``reference`` option as in COSMO-RS. This could be done with

.. code:: bash

  > xtb coord --opt --alpb water reference

Extended Functionality
======================

Solvent Accessable Surface Area
-------------------------------

.. note:: feature implemented in version 6.2

To get more insights and diagnostics for a ALPB calculation the Born radii
and the solvent accessable surface area can be printed by toggling the
property-printout with

.. code-block:: none

   $write
      gbsa=true

The printout for a branched octane isomer using ALPB(Water) looks like

.. code-block:: none

    * generalized Born model for continuum solvation

      #   Z   Born rad/Å    SASA/Å²    H-bond
      1   6 C      3.761     0.000     0.000
      2   6 C      3.761     0.000     0.000
      3   6 C      2.741     1.820    -0.000
      4   6 C      2.741     1.839    -0.000
      5   6 C      2.741     1.817    -0.000
      6   6 C      2.741     1.820    -0.000
      7   6 C      2.741     1.839    -0.000
      8   6 C      2.741     1.817    -0.000
      9   1 H      2.136    11.404    -0.015
     10   1 H      2.130    12.571    -0.017
     11   1 H      2.098    14.966    -0.020
     12   1 H      2.130    12.563    -0.017
     13   1 H      2.098    14.979    -0.020
     14   1 H      2.136    11.403    -0.015
     15   1 H      2.136    11.412    -0.015
     16   1 H      2.130    12.524    -0.017
     17   1 H      2.098    14.948    -0.020
     18   1 H      2.136    11.404    -0.015
     19   1 H      2.130    12.571    -0.017
     20   1 H      2.098    14.966    -0.020
     21   1 H      2.130    12.563    -0.017
     22   1 H      2.098    14.979    -0.020
     23   1 H      2.136    11.403    -0.015
     24   1 H      2.136    11.412    -0.015
     25   1 H      2.130    12.524    -0.017
     26   1 H      2.098    14.948    -0.020

    total SASA / Å² :      244.491

The quartary carbon atoms are shown with no solvent accessable surface area,
which means they are completely buried in the molecule leading to large
Born radii.
