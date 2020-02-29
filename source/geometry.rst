.. _geometry:

--------------
Geometry Input
--------------

.. contents::

Supported Geometry Formats
==========================

Currently the following input formats are supported by ``xtb``.

======================= ======================= =========== ==========
 Format                  Suffix                  molecular   periodic
======================= ======================= =========== ==========
 Turbomole               coord, tmol             x           3D
 xyz file                xyz                     x
 mol file                mol                     x
 Structure-Data file     sdf                     x
 Protein Database file   pdb                     x
 Vasp's POSCAR/CONTCAR   vasp, poscar, contcar               3D
 genFormat               gen                     x           3D
 Gaussian external       ein                     x
======================= ======================= =========== ==========

.. note:: The default format is always Turbomole.

Turbomole Coordinates
---------------------

The default input format is the Turbomole coordinate file as a ``$coord`` data
group.

.. code-block:: text
   :caption: h2o.coord

   $coord
    0.00000000000000      0.00000000000000     -0.73578586109551      o
    1.44183152868459      0.00000000000000      0.36789293054775      h
   -1.44183152868459      0.00000000000000      0.36789293054775      h
   $end

Like in Turbomole the coordinates can be given in Bohr (default)
or Ångström (``$coord angs``) or fractional coordinates (``$coord frac``).
Optional data groups like the systems periodicity (``$periodic``),
lattice parameters (``$lattice [bohr|angs]``) and cell parameters
(``$cell [bohr|angs]``) can be provided as well.

The following input for a MgO crystal utilize this data groups:

.. code-block:: text
   :caption: mgo.coord

   $coord frac
       0.00      0.00      0.00      mg
       0.50      0.50      0.50      o
   $periodic 3
   $cell
    5.798338236 5.798338236 5.798338236 60. 60. 60.
   $end

xyz Files
---------

mol Files
---------

Structure-Data file
-------------------

Protein Database file
---------------------

Vasp's POSCAR/CONTCAR
---------------------

genFormat
---------

Gaussian External
-----------------

Custom Annotations in Geometries
================================

The element type is detected by the element symbol, ``xtb`` filters the input
string in the respective format for letters and uses them to figure out the
element type in a case-insensitive way.
While reading the geometry input the actual element symbol will be preserved
and not normalized, a buffer of four characters is available to hold the symbol
which will be used when referring to the element and printing the final geometry.

.. note:: Prior to version 6.3 only a two character buffer was available.

This allows to add annotations to the geometry input which will not affect the
calculation, but show up in the output, log files and the final geometry printout.
Consider the following xyz example:

.. code-block:: text

   24

   13C        1.07317        0.04885       -0.07573
   N          2.51365        0.01256       -0.07580
   C*         3.35199        1.09592       -0.07533
   N          4.61898        0.73028       -0.07549
   C          4.57907       -0.63144       -0.07531
   C          3.30131       -1.10256       -0.07524
   C          2.98068       -2.48687       -0.07377
   O(1)       1.82530       -2.90038       -0.07577
   N          4.11440       -3.30433       -0.06936
   C          5.45174       -2.85618       -0.07235
   O          6.38934       -3.65965       -0.07232
   N          5.66240       -1.47682       -0.07487
   C          7.00947       -0.93648       -0.07524
   C          3.92063       -4.74093       -0.06158
   H          0.73398        1.08786       -0.07503
   D          0.71239       -0.45698        0.82335
   D          0.71240       -0.45580       -0.97549
   D          2.99301        2.11762       -0.07478
   H          7.76531       -1.72634       -0.07591
   2H         7.14864       -0.32182        0.81969
   3H         7.14802       -0.32076       -0.96953
   H          2.86501       -5.02316       -0.05833
   H          4.40233       -5.15920        0.82837
   H          4.40017       -5.16929       -0.94780

Which is a valid input for ``xtb``. Note that D and T can be used as synonyms
for hydrogen (H).
