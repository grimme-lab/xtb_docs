.._singlepoint:

----------------------------
Singlepoint Calculations
----------------------------

.. contents::

.. note:: Generally, a singlepoint calculation will be carried out automatically before every other calculation done with ``xtb``.

Geometry Input
========================


To start a singlepoint calculation with ``xtb`` only a molecular geometry is needed. ``xtb`` supports the ``TURBOMOLE`` coordinates, any valid Xmol files and Structure-Data files (.sdf).


Example ``TURBOMOLE`` input coordinates for H\ :sub:`2`\ O (e.g. ``coord``):

.. code:: bash

   $coord
      0.00000000000000      0.00000000000000     -0.73578586109551      o
      1.44183152868459      0.00000000000000      0.36789293054775      h
     -1.44183152868459      0.00000000000000      0.36789293054775      h
   $end

Example Xmol input coordinates for H\ :sub:`2`\ O (e.g. ``h2o.xyz``):   

.. code:: bash

   3
   Comment Line
   O     0.0000000    0.0000000   -0.3893611 
   H     0.7629844    0.0000000    0.1946806 
   H    -0.7629844    0.0000000    0.1946806
   
.. note:: For any valid Xmol file ``xtb`` will actually count the lines and double check
          the number of atoms specified, here the suffix ``.xyz`` is optional since ``xtb``
          will automatically detect the file type.   
   
Example SDF input for H\ :sub:`2`\ O (e.g. ``h2o.sdf``)

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

.. note:: To use input coordinates in SDF format the ``.sdf`` suffix is required.     
   

Charge and Multiplicity Input
=================================

By default ``xtb`` will search for ``.CHRG`` and ``.UHF`` files which contain the molecular charge 
and the number of unpaired electrons as an integer, respectively.

Example ``.CHRG`` file for a molecule with a molecular charge of +1:

.. code:: bash

   1

Example ``.CHRG`` file for a molecule with a molecular charge of -2:   
   
.. code:: bash

   -2

Example ``.UHF`` file for a molecule with two unpaired electrons:   
   
.. code:: bash

   2

The molecular charge can also be specified directly from the command line:

.. code:: sh

  > xtb molecule.xyz --chrg <INTEGER>
  
which is equivalent to

.. code:: sh

  > echo <INTEGER> > .CHRG && xtb molecule.xyz


This also works for the unpaired electrons as in

.. code:: sh

  > xtb --uhf <INTEGER> molecule.xyz

being equivalent to

.. code:: sh

  > echo <INTEGER> > .UHF && xtb molecule.xyz
  
Example for a +1 charged molecule with 2 unpaired electrons:

   
.. code:: bash

   xtb --chrg 1 --uhf 2


.. note:: The molecular charge or number of unpaired electrons specified from the command line will override specifications provided by ``.CHRG``, ``.UHF`` and the ``xcontrol`` input!    
   
   
The imported specifications are documented in the output file in the Calculation Setup section.

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
          charge                     :                     1
          spin                       :                   1.0
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

Sometimes you might face difficulties converging the self consistent
charge iterations, in this case it is usually a good idea to increase
the electronic temperature and to restart at normal temperature

.. code:: sh

  > xtb --etemp 1000.0 coord && xtb --restart coord
