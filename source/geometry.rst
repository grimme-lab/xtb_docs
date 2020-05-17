.. _geometry:

--------------
Geometry Input
--------------

.. contents::

Supported Geometry Formats
==========================

Currently the following input formats are supported by ``xtb``.

======================= ================= ======================= =========== ==========
 Format                  Basename          Suffix                  molecular   periodic
======================= ================= ======================= =========== ==========
 Turbomole               coord             coord, tmol             x           3D
 xyz file                                  xyz                     x
 mol file                                  mol                     x
 Structure-Data file                       sdf                     x
 Protein Database file                     pdb                     x
 Vasp's POSCAR/CONTCAR   poscar, contcar   vasp, poscar, contcar               3D
 genFormat                                 gen                     x           3D
 Gaussian external                         ein                     x
======================= ================= ======================= =========== ==========

.. note:: The default format is always Turbomole and neither the suffix nor the
          base name is case-sensitive for file type detection.

Turbomole Coordinate Input
--------------------------

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

.. note:: ``crest`` only supporting a plain coord data group without modifiers.

xyz Input
---------

The well-known xyz or xmol format is supported by ``xtb`` as well.
The atom is identified by its element symbol and followed by its cartesian
coordinates in Ångström.

An example for caffeine is given with:

.. code-block:: text
   :caption: caffeine.xyz

   24

   C          1.07317        0.04885       -0.07573
   N          2.51365        0.01256       -0.07580
   C          3.35199        1.09592       -0.07533
   N          4.61898        0.73028       -0.07549
   C          4.57907       -0.63144       -0.07531
   C          3.30131       -1.10256       -0.07524
   C          2.98068       -2.48687       -0.07377
   O          1.82530       -2.90038       -0.07577
   N          4.11440       -3.30433       -0.06936
   C          5.45174       -2.85618       -0.07235
   O          6.38934       -3.65965       -0.07232
   N          5.66240       -1.47682       -0.07487
   C          7.00947       -0.93648       -0.07524
   C          3.92063       -4.74093       -0.06158
   H          0.73398        1.08786       -0.07503
   H          0.71239       -0.45698        0.82335
   H          0.71240       -0.45580       -0.97549
   H          2.99301        2.11762       -0.07478
   H          7.76531       -1.72634       -0.07591
   H          7.14864       -0.32182        0.81969
   H          7.14802       -0.32076       -0.96953
   H          2.86501       -5.02316       -0.05833
   H          4.40233       -5.15920        0.82837
   H          4.40017       -5.16929       -0.94780


mol and sdf Input
-----------------

The mol and sdf format of the `ct-file`_ formats are partly supported in ``xtb``.
Some limitations apply to those input formats are applied to those formats to
make them play nicely together with QM nature of the xTB methods.
This means not any mol or sdf input will be accepted as geometry input.

.. _ct-file: http://c4.cabrillo.edu/404/ctfile.pdf

A valid sdf input is given for the water molecule with

.. code-block:: text
   :caption: h2o.sdf

   Water
     xtb     11041909383D
   Comment line
     3  2  0     0  0            999 V2000
      -0.2191   -0.3098    0.0000  O  0  0  0  0  0  0  0  0  0  0  0  0
       0.7400   -0.2909   -0.0000  H  0  0  0  0  0  0  0  0  0  0  0  0
      -0.5210    0.6007    0.0000  H  0  0  0  0  0  0  0  0  0  0  0  0
     1  2  1  0  0  0  0
     1  3  1  0  0  0  0
   M  END
   > <Formula>
   H2 O

   > <Mw>
   18.01528

   > <SMILES>
   O([H])[H]

   > <CSID>
   937

   $$$$

The input reader is strict in differentiating mol and sdf input, mol input with
the sdf extension will be rejected by the reader. The topology and the sdf
key-value pairs will be preserved and printed again in the final optimized
geometry.

Multiple entries in an sdf input will be ignored by the reader.

Protein Database Input
----------------------

The input reader supports parts of the `pdb-format`_ for reading single PDB
file. Minimal sanity checks on the PDB input will be performed, *i.e.* the
reader will outright reject any geometry without hydrogen atoms.

.. _pdb-format: http://www.wwpdb.org/documentation/file-format-content/format33/v3.3.html

An valid example input (with hydrogen atoms and partial occupied sides removed)
is given here:

.. code-block:: text
   :caption: 4qxx.pdb

   ATOM      1  N   GLY Z   1      -0.821  -2.072  16.609  1.00  9.93           N
   ATOM      2  CA  GLY Z   1      -1.705  -2.345  15.487  1.00  7.38           C
   ATOM      3  C   GLY Z   1      -0.968  -3.008  14.344  1.00  4.89           C
   ATOM      4  O   GLY Z   1       0.258  -2.982  14.292  1.00  5.05           O
   ATOM      5  HA2 GLY Z   1      -2.130  -1.405  15.135  1.00  0.00           H
   ATOM      6  HA3 GLY Z   1      -2.511  -2.999  15.819  1.00  0.00           H
   ATOM      7  H1  GLY Z   1      -1.364  -1.742  17.394  1.00  0.00           H
   ATOM      8  H2  GLY Z   1      -0.150  -1.365  16.344  1.00  0.00           H
   ATOM      9  H3  GLY Z   1      -0.334  -2.918  16.868  1.00  0.00           H
   ATOM     10  N   ASN Z   2      -1.721  -3.603  13.425  1.00  3.53           N
   ATOM     11  CA  ASN Z   2      -1.141  -4.323  12.291  1.00  1.85           C
   ATOM     12  C   ASN Z   2      -1.748  -3.900  10.968  1.00  3.00           C
   ATOM     13  O   ASN Z   2      -2.955  -3.683  10.873  1.00  3.99           O
   ATOM     14  CB  ASN Z   2      -1.353  -5.827  12.446  1.00  5.03           C
   ATOM     15  CG  ASN Z   2      -0.679  -6.391  13.683  1.00  5.08           C
   ATOM     16  OD1 ASN Z   2       0.519  -6.202  13.896  1.00  6.10           O
   ATOM     17  ND2 ASN Z   2      -1.448  -7.087  14.506  1.00  8.41           N
   ATOM     18  H   ASN Z   2      -2.726  -3.557  13.512  1.00  0.00           H
   ATOM     19  HA  ASN Z   2      -0.070  -4.123  12.263  1.00  0.00           H
   ATOM     20  HB2 ASN Z   2      -0.945  -6.328  11.568  1.00  0.00           H
   ATOM     21  HB3 ASN Z   2      -2.423  -6.029  12.503  1.00  0.00           H
   ATOM     22 HD21 ASN Z   2      -2.427  -7.218  14.293  1.00  0.00           H
   ATOM     23 HD22 ASN Z   2      -1.056  -7.487  15.346  1.00  0.00           H
   ATOM     24  N   LEU Z   3      -0.907  -3.803   9.944  1.00  3.47           N
   ATOM     25  CA  LEU Z   3      -1.388  -3.576   8.586  1.00  3.48           C
   ATOM     26  C   LEU Z   3      -0.783  -4.660   7.709  1.00  3.29           C
   ATOM     27  O   LEU Z   3       0.437  -4.788   7.643  1.00  3.80           O
   ATOM     28  CB  LEU Z   3      -0.977  -2.185   8.081  1.00  3.88           C
   ATOM     29  CG  LEU Z   3      -1.524  -1.669   6.736  1.00  8.66           C
   ATOM     30  CD1 LEU Z   3      -1.225  -0.191   6.570  1.00  9.89           C
   ATOM     31  CD2 LEU Z   3      -0.962  -2.409   5.541  1.00 13.56           C
   ATOM     32  H   LEU Z   3       0.086  -3.888  10.109  1.00  0.00           H
   ATOM     33  HA  LEU Z   3      -2.475  -3.661   8.568  1.00  0.00           H
   ATOM     34  HB2 LEU Z   3      -1.284  -1.469   8.843  1.00  0.00           H
   ATOM     35  HB3 LEU Z   3       0.111  -2.162   8.026  1.00  0.00           H
   ATOM     36  HG  LEU Z   3      -2.606  -1.798   6.737  1.00  0.00           H
   ATOM     37 HD11 LEU Z   3      -1.623   0.359   7.423  1.00  0.00           H
   ATOM     38 HD12 LEU Z   3      -1.691   0.173   5.654  1.00  0.00           H
   ATOM     39 HD13 LEU Z   3      -0.147  -0.043   6.513  1.00  0.00           H
   ATOM     40 HD21 LEU Z   3      -1.168  -3.475   5.643  1.00  0.00           H
   ATOM     41 HD22 LEU Z   3      -1.429  -2.035   4.630  1.00  0.00           H
   ATOM     42 HD23 LEU Z   3       0.115  -2.250   5.489  1.00  0.00           H
   ATOM     43  N   VAL Z   4      -1.635  -5.424   7.029  1.00  3.17           N
   ATOM     44  CA  VAL Z   4      -1.165  -6.460   6.119  1.00  3.61           C
   ATOM     45  C   VAL Z   4      -1.791  -6.230   4.755  1.00  5.31           C
   ATOM     46  O   VAL Z   4      -3.014  -6.209   4.620  1.00  7.31           O
   ATOM     47  CB  VAL Z   4      -1.567  -7.872   6.593  1.00  5.31           C
   ATOM     48  CG1 VAL Z   4      -1.012  -8.934   5.633  1.00  6.73           C
   ATOM     49  CG2 VAL Z   4      -1.083  -8.120   8.018  1.00  5.48           C
   ATOM     50  H   VAL Z   4      -2.628  -5.282   7.146  1.00  0.00           H
   ATOM     51  HA  VAL Z   4      -0.080  -6.402   6.034  1.00  0.00           H
   ATOM     52  HB  VAL Z   4      -2.655  -7.939   6.585  1.00  0.00           H
   ATOM     53 HG11 VAL Z   4      -1.303  -9.926   5.980  1.00  0.00           H
   ATOM     54 HG12 VAL Z   4      -1.414  -8.766   4.634  1.00  0.00           H
   ATOM     55 HG13 VAL Z   4       0.075  -8.864   5.603  1.00  0.00           H
   ATOM     56 HG21 VAL Z   4      -1.377  -9.121   8.333  1.00  0.00           H
   ATOM     57 HG22 VAL Z   4       0.003  -8.032   8.053  1.00  0.00           H
   ATOM     58 HG23 VAL Z   4      -1.529  -7.383   8.686  1.00  0.00           H
   ATOM     59  N   SER Z   5      -0.966  -6.052   3.736  1.00  7.53           N
   ATOM     60  CA  SER Z   5      -1.526  -5.888   2.407  1.00 11.48           C
   ATOM     61  C   SER Z   5      -1.207  -7.085   1.529  1.00 16.35           C
   ATOM     62  O   SER Z   5      -0.437  -7.976   1.902  1.00 14.00           O
   ATOM     63  CB  SER Z   5      -1.031  -4.596   1.767  1.00 13.36           C
   ATOM     64  OG  SER Z   5       0.361  -4.652   1.540  1.00 15.80           O
   ATOM     65  OXT SER Z   5      -1.737  -7.178   0.429  1.00 17.09           O
   ATOM     66  H   SER Z   5       0.033  -6.031   3.880  1.00  0.00           H
   ATOM     67  HA  SER Z   5      -2.610  -5.822   2.504  1.00  0.00           H
   ATOM     68  HB2 SER Z   5      -1.543  -4.449   0.816  1.00  0.00           H
   ATOM     69  HB3 SER Z   5      -1.254  -3.759   2.428  1.00  0.00           H
   ATOM     70  HG  SER Z   5       0.653  -3.831   1.137  1.00  0.00           H
   TER      71      SER Z   5
   HETATM   72  O   HOH Z 101       0.935  -5.175  16.502  1.00 18.83           O
   HETATM   73  H1  HOH Z 101       0.794  -5.522  15.621  1.00  0.00           H
   HETATM   74  H2  HOH Z 101       1.669  -4.561  16.489  1.00  0.00           H
   HETATM   75  O   HOH Z 102       0.691  -8.408  17.879  1.00 56.55           O
   HETATM   76  H1  HOH Z 102       1.392  -8.125  18.466  1.00  0.00           H
   HETATM   77  H2  HOH Z 102       0.993  -8.356  16.972  1.00  0.00           H
   CONECT   73   72
   CONECT   74   72
   CONECT   72   73   74
   CONECT   76   75
   CONECT   77   75
   CONECT   75   76   77
   END

Vasp's POSCAR/CONTCAR Input
---------------------------

For periodic input Vasp's POSCAR / CONTCAR input files are supported, for more
information on the format visit the `vasp-wiki`_.

.. _vasp-wiki: https://www.vasp.at/wiki/index.php/POSCAR

For a molecular crystal of ammonia the input would look like:

.. code-block:: text
   :caption: ammonia.poscar

    H  N
    1.0000000000000000
        5.0133599999999996    0.0000000000000000    0.0000000000000000
        0.0000000000000000    5.0133599999999996    0.0000000000000000
        0.0000000000000000    0.0000000000000000    5.0133599999999996
     12   4
   Cartesian
     2.1985588943999996  1.7639005823999998  0.8801454815999999
     1.7639005823999998  0.8801454815999999  2.1985588943999996
     0.8801454815999999  2.1985588943999996  1.7639005823999998
     4.8411510839999998  1.6194155471999998  4.9398140088000000
     4.3563090384000001  2.4998116967999997  3.6324801215999996
     3.5195792543999995  1.1535741359999998  4.0840334567999994
     4.0840334567999994  3.5195792543999995  1.1535741359999998
     4.9398140088000000  4.8411510839999998  1.6194155471999998
     3.6324801215999996  4.3563090384000001  2.4998116967999997
     2.4998116967999997  3.6324801215999996  4.3563090384000001
     1.1535741359999998  4.0840334567999994  3.5195792543999995
     1.6194155471999998  4.9398140088000000  4.8411510839999998
     1.3746131783999997  1.3746131783999997  1.3746131783999997
     3.9981545999999994  1.9910559239999999  4.4636450759999997
     4.4636450759999997  3.9981545999999994  1.9910559239999999
     1.9910559239999999  4.4636450759999997  3.9981545999999994

genFormat Input
---------------

The DFTB+ `genFormat`_ is supported for molecular and 3D periodic systems.

.. _genFormat: http://www.dftbplus.org/fileadmin/DFTBPLUS/public/dftbplus/latest/manual.pdf

A valid input file for a molecular system is given here:

.. code-block:: text
   :caption: 1,4-bromomethaneformaldehyde.gen

   9 C
   C Br H O
        1   1  -8.9147060000E-02  -6.6786080000E-02  -1.0432907000E-01
        2   2   1.7639746700E+00   2.6771621000E-01   4.2178865000E-01
        3   3  -2.6325805000E-01  -1.1300550700E+00  -1.3052621000E-01
        4   3  -7.4963702000E-01   3.9302570000E-01   6.1238499000E-01
        5   3  -2.6130022000E-01   3.5462634000E-01  -1.0812232600E+00
        6   4   4.7684499800E+00   7.6734388000E-01   1.2078966200E+00
        7   1   5.5165496700E+00   2.5437564000E-01   4.3331738000E-01
        8   3   6.6378745000E+00   3.1585526000E-01   5.3760272000E-01
        9   3   5.1708208600E+00  -3.3263252000E-01  -4.6451965000E-01

For a periodic system the input would look like

.. code-block:: text
   :caption: GaAs.gen

   2  F
   Ga As
   1 1 0.00 0.00 0.00
   2 2 0.25 0.25 0.25
   0.0000000E+00 0.0000000E+00 0.0000000E+00
   0.2713546E+01 0.2713546E+01 0.0000000E+00
   0.0000000E+00 0.2713546E+01 0.2713546E+01
   0.2713546E+01 0.0000000E+00 0.2713546E+01

Gaussian External Input
-----------------------

The `Gaussian`_ external format is supported to use ``xtb`` with the Gaussian
program. A thin wrapper around the ``xtb`` binary is required to convert the
external call to a valid ``xtb`` program call.

.. _Gaussian: https://gaussian.com/external/

An example input file is given here:

.. code-block:: text
   :caption: nh3.EIn

            4         1         0         1
            7      0.000000000000      0.000000000000     -0.114091591161      0.000000000000
            1     -1.817280998039      0.000000000000      0.528409372569      0.000000000000
            1      0.908640499019     -1.573811509290      0.528409372569      0.000000000000
            1      0.908640499019      1.573811509290      0.528409372569      0.000000000000

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

.. note:: Mangled names are not supported with ``crest`` and must be normalized.
