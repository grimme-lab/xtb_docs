.. _sp:

----------------------------
Singlepoint Calculations
----------------------------

.. contents::

.. note:: Generally, a singlepoint calculation will be carried out automatically before every other calculation done with ``xtb``.

Input
========================


To start a singlepoint calculation with ``xtb`` only a molecular geometry is needed. ``xtb`` supports the ``TURBOMOLE`` coordinates (coord), any valid Xmol (e.g. .xyz) and Structure-Data files (.sdf).


Example ``TURBOMOLE`` input coordinates for H\ :sub:`2`\ O (e.g. coord):

.. code:: bash

   $coord
      0.00000000000000      0.00000000000000     -0.73578586109551      o
      1.44183152868459      0.00000000000000      0.36789293054775      h
     -1.44183152868459      0.00000000000000      0.36789293054775      h
   $end

Example Xmol input coordinates for H\ :sub:`2`\ O (e.g. h2o.xyz):   

.. code:: bash

   3
   Comment Line
   O     0.0000000    0.0000000   -0.3893611 
   H     0.7629844    0.0000000    0.1946806 
   H    -0.7629844    0.0000000    0.1946806
   
.. note:: For any valid Xmol file ``xtb`` will actually count the lines and double check
          the number of atoms specified, here the suffix .xyz is optional since ``xtb``
          will automatically detect the file type.   
   
Example SDF input for H\ :sub:`2`\ O (e.g. h2o.sdf)

.. code:: bash

   Water
   Comment Line 1
   Comment Line 2
     3  2  0  0  0               999 V2000
       0.0021   -0.0041    0.0020 H   0  0  0  0  0  0  0  0  0  0  0  0
      -0.0110    0.9628    0.0073 O   0  0  0  0  0  0  0  0  0  0  0  0
       0.8669    1.3681    0.0011 H   0  0  0  0  0  0  0  0  0  0  0  0
     1  2  1  0  0  0  0
     2  3  1  0  0  0  0
   M  END
   $$$$

.. note:: To use input coordinates in SDF format the .sdf suffix is required.     
   

Charge and Multiplicity
=================================

By default ``xtb`` will search for ``.CHRG`` and ``.UHF`` files which contain the molecular charge 
and the number of unpaired electrons as an integer, respectively.

Example ``.CHRG`` file for a molecule with a molecular charge of +1:

.. code:: bash

   > cat .CHRG
   1

Example ``.CHRG`` file for a molecule with a molecular charge of -2:   
   
.. code:: bash
   
   > cat .CHRG
   -2

Example ``.UHF`` file for a molecule with two unpaired electrons:   
   
.. code:: bash

   > cat .UHF
   2

The molecular charge can also be specified directly from the command line:

.. code:: sh

  > xtb coord --chrg <INTEGER>
  
which is equivalent to

.. code:: sh

  > echo <INTEGER> > .CHRG && xtb coord


This also works for the unpaired electrons as in

.. code:: sh

  > xtb coord --uhf <INTEGER>

being equivalent to

.. code:: sh

  > echo <INTEGER> > .UHF && xtb molecule.xyz
  
Example for a +1 charged molecule with 2 unpaired electrons:

   
.. code:: bash

  > xtb --chrg 1 --uhf 2


.. note:: The molecular charge or number of unpaired electrons specified from the command line will override specifications provided by ``.CHRG``, ``.UHF`` and the ``xcontrol`` input!    
   
   
The imported specifications are documented in the output file in the *Calculation Setup* section.

.. code:: bash

   
           -------------------------------------------------
          |                Calculation Setup                |
           -------------------------------------------------

          program call               : xtb molecule.xyz
          hostname                   : user
          coordinate file            : molecule.xyz
          omp threads                :                     4
          number of atoms            :                     3
          number of electrons        :                     7
          charge                     :                     1    # Specified molecular charge
          spin                       :                   1.0    # Total spin from number of unpaired electrons (S=2*0.5=1)
          first test random number   :      0.54680533077496



.. note:: Note that the position of the input coordinates is totally unaffected
          by any command-line arguments, if you are not sure, whether ``xtb`` tries
          to interpret your filename as flag use ``--`` to stop the parsing
          as command-line options for all following arguments.

          .. code:: sh

            > xtb -- -oh.xyz

To select the parametrization of the xTB method you can currently choose
from three different geometry, frequency and non-covalent interactions (GFN)
parametrization, which differ mostly in the cost--accuracy ratio,

.. code:: sh

  > xtb --gfn 2 coord

to choose GFN2-xTB, which is also the default parametrization. Also
available are GFN1-xTB, and GFN0-xTB.

Accuracy and Iterations
=================================

Accuracy
---------

The accuracy of the SCC calculation can be adjusted from the command line:

.. code:: sh

  > xtb coord --acc <REAL>
  
By default the accuracy multiplier is set to 1 resulting in the following settings:

+---------------------------+------------------------------------+
| Accuracy                  |           1                        |
+===========================+====================================+
| Integral neglect          |     0.1000000E-07                  |
+---------------------------+------------------------------------+
| SCC convergence           |     0.1000000E-05 E\ :sub:`h`\     |
+---------------------------+------------------------------------+
| Wavefunction convergence  |     0.1000000E-03 e                |
+---------------------------+------------------------------------+

Setting the accuracy level to 3 will result in:

+---------------------------+------------------------------------+
| Accuracy                  |           3                        |
+===========================+====================================+
| Integral neglect          |     0.3000000E-07                  |
+---------------------------+------------------------------------+
| SCC convergence           |     0.3000000E-05 E\ :sub:`h`\     |
+---------------------------+------------------------------------+
| Wavefunction convergence  |     0.3000000E-03 e                |
+---------------------------+------------------------------------+


Iterations
------------

The number of iterations allowed for the SCC calculation can be adjusted from the command line:

.. code:: sh

  > xtb coord --iteration <INTEGER>
  
The default number of iterations in the SCC is set to 250.

Fermi-smearing
=================================

The electronic temperature :math:`T_{el}` is used as an adjustable parameter, employing so-called Fermi 
smearing to achieve fractional occupations for systems with almost degenerate orbital levels. 
This is mainly used to take static correlation into account or to e.g. investigate thermally forbidden reaction pathways.

:math:`T_{el}` enters the GFNn-xTB Hamiltonian as

.. math::

   -T_{el}S_{el}
   
and the orbital occupations for a spin orbital :math:`\psi_{i}` are given by

.. math::

   n_{i}(T_{el})=\frac{1}{exp[(\epsilon _{i}- \epsilon _{F})/(k_{B}T_{el})]+1}

The default electronic temperature is :math:`T_{el}` = 300 K.

:math:`T_{el}` can be adjusted by the command line:

.. code:: sh

  > xtb --etemp <REAL> molecule.xyz

            
The specified electronic temperature is documented in the output file in the *Self-Consistent Charge Iterations* section

.. code:: bash           
            
             -------------------------------------------------
            |        Self-Consistent Charge Iterations        |
             -------------------------------------------------
   
   Ncao       : 6
   Nao        : 6
   Nshell     : 4
   Nel        : 8
   T(el)      :  5000.0   # Specified electronic temperature
   intcut     :    25.0
   scfconv/Eh :  0.100E-05
     qconv/e  :  0.100E-03
   intneglect :  0.100E-07
   broydamp   :      0.400

      
.. note:: Sometimes you may face difficulties converging the self consistent
          charge iterations. In this case increasing the electronic temperature 
          and restarting at the converged calculation with normal temperature can help.

          .. code:: sh

            > xtb coord --etemp 1000.0&& xtb coord --restart
  
  
Vertical Ionization Potentials and Electron Affinities
==================================================================================

``xtb`` can be used to calculate vertical ionization potentials (IP) and electron affinities (EA) applying
a specially reparameterized GFN1-xTB version. The special purpose parameters are documented in the ``.param_ipea.xtb``
parameter file.

.. note:: The vertical ionization potential or electron affinity is obtained as the energy difference between the corresponding   
          molecule groundstate and its ionized species in the same geometry.
  
          .. math::

             vIP = E(M^{n+1})-E(M^{n})              vEA = E(M^{n-1})-E(M^{n})
             
          Be aware that the sign of IP and EA can differ in the literature due to different definitions.   

The vertical IP and EA calculations can be evoked from the command line either separately or combined.

.. code:: sh

  > xtb coord --vip
  
.. code:: sh

  > xtb coord --vea

.. code:: sh

  > xtb coord --vipea


.. note:: To optimize the geometry prior to the vertical IP and/or EA calculation add an optimization flag prior to the vipea 
          one.
          
          .. code:: sh

            > xtb coord --opt && xtb xtbopt.coord --vipea

The calculated IP and/or EA are then corrected empirically, both the empirical shift and the final IP and/or EA are documented
in the output in the *vertical delta SCC IP calculation* and *vertical delta SCC EA calculation* sections.

Example output for the optimized Water molecule:

.. code:: bash

              -------------------------------------------------
             |        vertical delta SCC IP calculation        |
              -------------------------------------------------

              *** removed SETUP and SCC detailes for clarity ***

            :::::::::::::::::::::::::::::::::::::::::::::::::::::
            ::                     SUMMARY                     ::
            :::::::::::::::::::::::::::::::::::::::::::::::::::::
            :: total energy               -5.141603209729 Eh   ::
            :: gradient norm               0.051348781702 Eh/α ::
            :: HOMO-LUMO gap               6.668725933430 eV   ::
            ::.................................................::
            :: SCC energy                 -5.189558706232 Eh   ::
            :: -> electrostatic            0.159050410368 Eh   ::
            :: repulsion energy            0.048093066315 Eh   ::
            :: dispersion energy          -0.000137569813 Eh   ::
            :: halogen bond corr.          0.000000000000 Eh   ::
            :: add. restraining            0.000000000000 Eh   ::
            :::::::::::::::::::::::::::::::::::::::::::::::::::::

   ------------------------------------------------------------------------
   empirical IP shift (eV):    4.8455        # Empirical shift
   delta SCC IP (eV):   13.7897              # Finally calculated vertical IP (Exp.: 12.6 eV)
   ------------------------------------------------------------------------
              -------------------------------------------------
             |        vertical delta SCC EA calculation        |
              -------------------------------------------------

              *** removed SETUP and SCC detailes for clarity ***

            :::::::::::::::::::::::::::::::::::::::::::::::::::::
            ::                     SUMMARY                     ::
            :::::::::::::::::::::::::::::::::::::::::::::::::::::
            :: total energy               -5.929826433613 Eh   ::
            :: gradient norm               0.016238133270 Eh/α ::
            :: HOMO-LUMO gap               7.760066297206 eV   ::
            ::.................................................::
            :: SCC energy                 -5.977781930116 Eh   ::
            :: -> electrostatic            0.169754616317 Eh   ::
            :: repulsion energy            0.048093066315 Eh   ::
            :: dispersion energy          -0.000137569813 Eh   ::
            :: halogen bond corr.          0.000000000000 Eh   ::
            :: add. restraining            0.000000000000 Eh   ::
            :::::::::::::::::::::::::::::::::::::::::::::::::::::

   ------------------------------------------------------------------------
   empirical EA shift (eV):    4.8455     # Empirical shift
   delta SCC EA (eV):   -2.0320           # Finally calculated vertical EA
   ------------------------------------------------------------------------


