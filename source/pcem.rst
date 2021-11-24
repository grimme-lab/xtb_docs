.. _pcem:

-----------------------------------
 External Potentials and Embedding
-----------------------------------

``xtb`` supports external electrostatic potentials for GFN1-xTB and GFN2-xTB.

.. contents::

Example: The Water Tetramer in Pieces
=====================================

As input geometry for the QM half of the water cluster we use

.. code-block:: none

   $coord
      -2.75237178376284     2.43247309226225    -0.01392519847964      O
      -0.93157260886974     2.79621404458590    -0.01863384029005      H
      -3.43820531288547     3.30583608421060     1.42134539425148      H
      -2.43247309226225    -2.75237178376284     0.01392519847964      O
      -2.79621404458590    -0.93157260886974     0.01863384029005      H
      -3.30583608421060    -3.43820531288547    -1.42134539425148      H
   $end

The setup will look somewhat similar to this

.. code-block:: bash

   > ls
   pcem.input    water_4.coord   water_4.pc
   > cat pcem.input
   $embedding
      input=water_4.pc
   $end
   > xtb water_4.coord -I pcem.input
   ...

The file ``water_4.pc`` contains the partial charges and its positions as

.. code-block:: none

   6
     -0.69645733    2.75237178376284    -2.43247309226225    -0.01392519847964  O
      0.36031084    0.93157260886974    -2.79621404458590    -0.01863384029005  H
      0.33614649    3.43820531288547    -3.30583608421060     1.42134539425148  H
     -0.69645733    2.43247309226225     2.75237178376284     0.01392519847964  O
      0.36031084    2.79621404458590     0.93157260886974     0.01863384029005  H
      0.33614649    3.30583608421060     3.43820531288547    -1.42134539425148  H

The first column contains the partial charge, the second to fourth columns
contain the cartesian coordinates in Bohr (or in Ångström if ``interface=orca``
is used in the input file).
The fifth column is optional, but can contain, like here, element symbols
to specify the chemical hardnesses of the partial charges.
Note that we are not using real point charges here but a damped Coulomb interaction
consistent to the electrostatic interactions used in the respective
xTB Hamiltonian.

The read in point charges are shown in the setup block of the SCC as

.. code-block:: none
   :emphasize-lines: 18-20

   ... skip ...

              ------------------------------------------------- 
             |        Self-Consistent Charge Iterations        |
              ------------------------------------------------- 
   
             ...................................................
             :                      SETUP                      :
             :.................................................:
             :  # basis functions                  12          :
             :  # atomic orbitals                  12          :
             :  # shells                            8          :
             :  # electrons                        16          :
             :  max. iterations                   250          :
             :  Hamiltonian                  GFN2-xTB          :
             :  restarted?                      false          :
             :  GBSA solvation                  false          :
             :  PC potential                     true          :
             :  -> # point charges                  6          :
             :  -> sum of PC                0.0000000     e    :
             :  electronic temp.          300.0000000     K    :
             :  accuracy                    1.0000000          :
             :  -> integral cutoff          0.2500000E+02      :
             :  -> integral neglect         0.1000000E-07      :
             :  -> SCF convergence          0.1000000E-05 Eh   :
             :  -> wf. convergence          0.1000000E-03 e    :
             :  Broyden damping             0.4000000          :
             ...................................................
   
    iter      E             dE          RMSdq      gap      omega  full diag
      1    -10.2141283 -0.102141E+02  0.418E+00   13.25       0.0  T
      2    -10.2169842 -0.285597E-02  0.241E+00   13.15       1.0  T
      3    -10.2175075 -0.523314E-03  0.392E-01   12.35       1.0  T
      4    -10.2176392 -0.131616E-03  0.871E-02   12.57       1.0  T
      5    -10.2176471 -0.790990E-05  0.417E-02   12.50       1.0  T
      6    -10.2176488 -0.172257E-05  0.237E-03   12.52      17.3  T
      7    -10.2176488 -0.524712E-08  0.835E-04   12.52      48.9  T
      8    -10.2176488  0.134836E-09  0.623E-04   12.52      65.5  T
   
      *** convergence criteria satisfied after 8 iterations ***
   
   ... skip ...
   
            :::::::::::::::::::::::::::::::::::::::::::::::::::::
            ::                     SUMMARY                     ::
            :::::::::::::::::::::::::::::::::::::::::::::::::::::
            :: total energy             -10.155934019173 Eh    ::
            :: gradient norm              0.024959693967 Eh/a0 ::
            :: HOMO-LUMO gap             12.520985724021 eV    ::
            ::.................................................::
            :: SCC energy               -10.217648797540 Eh    ::
            :: -> isotropic ES            0.066792092832 Eh    ::
            :: -> anisotropic ES         -0.002669047466 Eh    ::
            :: -> anisotropic XC         -0.001310421597 Eh    ::
            :: -> dispersion             -0.000808230531 Eh    ::
            :: repulsion energy           0.061714760838 Eh    ::
            :: add. restraining           0.000000000000 Eh    ::
            :::::::::::::::::::::::::::::::::::::::::::::::::::::

   ... skip ...

To obtain point charge like behaviour for the partial charges the chemical
hardness can be set to a large value. This can be done by specifying the chemical
hardnesses in the fifth column instead of giving an element symbol.
For this setup the ``water_4.pc`` would look like

.. code-block:: none

   6
     -0.69645733    2.75237178376284    -2.43247309226225    -0.01392519847964  99
      0.36031084    0.93157260886974    -2.79621404458590    -0.01863384029005  99
      0.33614649    3.43820531288547    -3.30583608421060     1.42134539425148  99
     -0.69645733    2.43247309226225     2.75237178376284     0.01392519847964  99
      0.36031084    2.79621404458590     0.93157260886974     0.01863384029005  99
      0.33614649    3.30583608421060     3.43820531288547    -1.42134539425148  99
