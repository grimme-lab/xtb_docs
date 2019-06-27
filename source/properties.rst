.. _properties: 

-------------------------------
 Properties
-------------------------------

In this chapter, all necessary information about the properties  
of `xTB` will be given. Description of how to acquire different output information will be 
provided. Calculation of FOD will be described.

.. contents::

General printout
================

First the orbital energies and occupation are printed, where the highest occupied 
molecular orbital (HOMO) and the lowest unoccupied molecular orbital (LUMO) are marked. 
The HOMO-LUMO gap and the Fermi-level are summed up.

.. code-block:: none

           -------------------------------------------------
          |                Property Printout                |
           -------------------------------------------------

    * Orbital Energies and Occupations

         #    Occupation            Energy/Eh            Energy/eV
      -------------------------------------------------------------
         1        2.0000           -0.6801050             -18.5066
         2        2.0000           -0.5683264             -15.4649
         3        2.0000           -0.5108650             -13.9013
         4        2.0000           -0.4463919             -12.1469 (HOMO)
         5                          0.0826818               2.2499 (LUMO)
         6                          0.2518567               6.8534
      -------------------------------------------------------------
                  HL-Gap            0.5290737 Eh           14.3968 eV
             Fermi-level           -0.1818551 Eh           -4.9485 eV


The information provided by the printout can be modified and extended.
This can be done either by using the option-flags when calling the program (:ref:`commandline`), 
or by editing the input file (:ref:`detailed-input`). The kind of default information given is determined by the GFN-xTB version used. The default values called by the program are given:

--pop      
    requests printout of Mulliken population analysis 
--molden
    requests printout of molden file                  
--dipole   
    requests printout of dipole moments
--wbo
    requests Wiberg bond order printout              

GFN1-xTB
_________

Default settings for GFN1-xTB first prints the Mulliken and CM5 charges. n(x) denotes the 
population partioned to the x = s/p/d shells:

.. code-block:: none

    Mulliken/CM5 charges        n(s)   n(p)   n(d)
       1 O   0.67569  0.33312   1.682  4.994  0.000
       2 H  -0.33784 -0.16656   0.662  0.000  0.000
       3 H  -0.33784 -0.16656   0.662  0.000  0.000

Wiberg bond orders describe the partial bond orders and their disposition onto the atoms:

.. code-block:: none

  Wiberg/Mayer (AO) data.
  largest (>0.10) Wiberg bond orders for each atom
           total WBO            WBO to atom ...
     1  O   1.782        H    2 0.891    H    3 0.891
     2  H   0.892        O    1 0.891
     3  H   0.892        O    1 0.891

The molecular dipole moment and its cartesian components calculated from the electron density.
The components are given in atomic units while the total dipole moment is given in Debye, to convert from atomic units to Debye multiply by 2.5417 D/au.

.. code-block:: none

  dipole moment from electron density (au)
       X       Y       Z   
     0.8659   0.0000   0.6123  total (Debye):    2.696


GFN2-xTB
________

Default settings for GFN2-xTB first prints populations and coefficients.
From left to right, these are the atomic number Z, 
Coordination number CN,
Atomic partial charge q, 
Dispersion coefficient C6, 
Polarizability alpha:

.. code-block:: none

   #   Z        covCN         q      C6AA      α(0)
   1   8 O      1.613    -0.568    24.435     6.672
   2   1 H      0.806     0.284     0.771     1.379
   3   1 H      0.806     0.284     0.771     1.379


The C6, C8 and alpha coefficients are denoted explicitly in a.u.:

.. code-block:: none

 Mol. C6AA /au·bohr⁶  :         44.553640
 Mol. C8AA /au·bohr⁸  :        796.459844
 Mol. α(0) /au        :          9.429351

Wiberg bond orders:

.. code-block:: none

 Wiberg/Mayer (AO) data.
  largest (>0.10) Wiberg bond orders for each atom
           total WBO             WBO to atom ...
      1  O   1.839        H    3 0.919    H    2 0.919
      2  H   0.919        O    1 0.919
      3  H   0.919        O    1 0.919

Molecular dipole and quadropole moments. The contributions are seperated into their respective cartesian dimensions. 
'Full' represents the corresponding contributions of the molecular dipole or quadropole moments.


.. code-block:: none

 molecular dipole:
                 x           y           z       tot (Debye)
  q only:        0.481       0.000       0.340
    full:        0.696       0.000       0.492       2.167

 molecular quadrupole (traceless):
                 xx          xy          yy          xz          yz          zz
  q only:        0.305       0.000      -0.916      -0.432       0.000       0.610
   q+dip:        0.390       0.000      -1.177      -0.563       0.000       0.787
    full:        0.495      -0.000      -1.436      -0.632      -0.000       0.942


All is summed up in the end in both GFN-xTB versions:

.. code-block:: none

           -------------------------------------------------
          | TOTAL ENERGY               -5.070322476938 Eh   |
          | GRADIENT NORM               0.019484395925 Eh/α |
          | HOMO-LUMO GAP              14.652302902752 eV   |
           -------------------------------------------------


 
Density Properties
===================

Cube Files
__________

The ``xtb`` program is able to calculate the density, spin-density and the fractional occupation number weighted density (FOD). 
For these caclualtions, the program first creates a proper cube grid. The corresponding file is created in your working directory and marked as ``.cub`` file. 
It provides density and step size informations. An overview is already given in the printout:

.. code-block:: none

  cube file module (SG, 7/16)
  cube_pthr     :   0.050
  cube_step     :   0.400
  non-zero P (%):  76.190   nmat:      16
  Grid Boundaries (x y z) :
    4.69257109135830        3.00000000000000        4.79524030780751     
   -3.00000000000000       -3.00000000000000       -3.59840693802375     
  Total # of points        6720
  writing density.cub
 cpu  time for cube    0.01 s
 wall time for cube    0.01 s

Here, various information are provided, like the density matrix neglect threshold ``cube_pthr`` and the grid step size ``cube_step`` (in Bohr). These values can be changed in the input (xcontrol) file (:ref:`detailed-input`). 

For visualization, programs like chimera can be used, for which the ``.cub`` file can be loaded as volume data. 

Density and Spin-Density
________________________

To calculate the density or the spin denisty, the input (xcontrol) file has to be manipulated. Here, the bools ``density='bool'`` 
or respectively ``spin density='bool'`` have to be set to ``'true'``. This will create a ``.cub`` cube file, where the corresponding information is gathered.

For visualization, programs like *chimera* can be used, for which the ``.cub`` file can be loaded as volume data. 

Fractional Occupation Density (FOD) calculation
________________________________________________

The fractional occupation density analysis (FOD) is a diagnostic scheme that displays the static electron correlation localized on a molecule.
The density is hereby obtained by performing a computationally cheap Finite-Temperature DFT computation. 
The electrons are therefore self-consistenly smeared over the molecular orbitals according to a Fermi-Dirac distribution. For a more detailed insight and the theory behind 
the FOD analytics, please see the `original publication`_. To use FOD for selecting active spaces in CASSCF calculations, refer to our `later work`_ on this topic.

.. _original publication: https://onlinelibrary.wiley.com/doi/full/10.1002/anie.201501887
.. _later work: https://onlinelibrary.wiley.com/doi/full/10.1002/chem.201604682

To access the FOD analysis, simply use the flag ``--fod`` or set ``fod='true'`` in the input (xcontrol) file. This will create a ``fod.cub`` file and calculate the FOD on the cube grid. 
Be sure to set the electronic temperature to a higher value, e.g. 5000 K (``--etemp 5000``). The FOD population will be displayed in the printout section as:

.. code-block:: none

 NFOD :     0.6698

 Loewdin FODpop     n(s)   n(p)   n(d)
    1 C   0.1924   0.018  0.175  0.000
    2 C   0.0673   0.003  0.064  0.000
    3 C   0.0673   0.003  0.064  0.000
    4 C   0.1924   0.018  0.175  0.000
    5 C   0.0673   0.003  0.064  0.000
    6 C   0.0673   0.003  0.064  0.000
    7 H   0.0039   0.004  0.000  0.000
    8 H   0.0039   0.004  0.000  0.000
    9 H   0.0039   0.004  0.000  0.000
   10 H   0.0039   0.004  0.000  0.000

The NFOD number indicates the static electon correlation 

If you do not want to write a full ``fod.cub`` file, but still want to analyse the FOD population at least qualitatively, change the ``fod population ='bool'`` in the input (xcontrol) file to ``true``. This will display the fractional loewdin 
population of the system (see above) and only writes the ``fod`` file, where this information is stored.

Redirecting Property Printout
=============================

For large systems the property printout can become quite lenghty and will
clutter maybe thousands of lines in the standard output.
One possibility is to rigourously deactivate all printouts using the
``$write`` instruction in the input file,
but if one might need this information later it is hard to recover,
as an alternative the property printout can be redirected.
Simply add

.. code-block:: text

   $write
      output file=properties.out

to your input and specify the name for the redirection.
The calculations of the properties are performed as usual but the
standard output will show something like

.. code-block:: text

   Property printout bound to 'properties.out'

instead of the header, the usual printout can be found in ``properties.out``.
In the file the command line call and current time is saved additionally to
ensure that the printout is reproducable.

Machine Readable Data Dump
==========================

``xtb`` is able to dump parts of the calculated data in a machine-readable way
using the json-format. To activate the dump into a json file use the input

.. code:: text

   $write
      json=true

which will write a ``xtbout.json`` file containing partial charges,
cumulative atomic multipole moments, occupation number and orbtial energies
for single point calculations or frequencies, reduced masses and IR intensities
from hessian calculations.
