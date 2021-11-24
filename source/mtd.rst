.. _mtd:

-------------------------------
 Meta-Dynamics Simulations
-------------------------------

In this guide, all necessary information will be given in order to perform
meta-dynamics (MTD) simulations with the ``xtb`` program.
For the theory behind our MTD approach please refer to:

    S. Grimme,
    *Exploration of Chemical Compound, Conformer, and Reaction Space with
    Meta-Dynamics Simulations Based on Tight-Binding Quantum Chemical
    Calculations*,
    *J. Chem. Theory Comput.*, **2019**, *155*, 2847-2862.
    **DOI:** `10.1021/acs.jctc.9b00143`__
    Publication Date (Web): April 3, 2019

__ https://doi.org/10.1021/acs.jctc.9b00143

.. contents::

From a practical point of view the application of meta-dynamics is quite similar to molecular-dynamic simulations.
In MTD simulations a biasing potential given as a sum of Gaussian functions is additionally expressed. 
The root-mean-square deviation (RMSD) in Cartesian space is chosen as a metric for the collective variables.  

All adjustable parameters will be discussed and a guide to how to change them will be given as well as an example.

General MTD Setup
=================

For any MTD calculation a :ref:`detailed-input` file is necessary to enter
the correct calculation mode. The basic parameters for dynamics are taken
from the ``$md`` block as described in the section regarding :ref:`md`.
The ``$metadyn`` data group has to be present in the input file.
All available instructions for this data group are shown here:

  +---------+---------+-----------------------------------------------------------+
  |  key    | value   | description                                               |
  +=========+=========+===========================================================+
  | save    | integer | maximal number of structures for rmsd criteria            |
  +---------+---------+-----------------------------------------------------------+
  | kpush   | real    | scaling factor for rmsd criteria                          |
  +---------+---------+-----------------------------------------------------------+
  | alp     | real    | width of the gaussian potential used in the rmsd criteria |
  +---------+---------+-----------------------------------------------------------+
  | coord   | file    | external structure file to initialize rmsd criteria       |
  +---------+---------+-----------------------------------------------------------+
  | atoms   | list    | atoms to include in the rmsd calculation (default: all)   |
  +---------+---------+-----------------------------------------------------------+

To avoid accidental activation of the bias potential conservative default values
are chosen in the program. So you cannot simply use a commandline-only approach
to perform a MTD calculation. First of all you want to create a ``metadyn.inp``
file with this content

.. code::

   $metadyn
      save=10
      kpush=1.0
      alp=0.2
   $end

You can start the metadynamic calculation now by using the ``--md`` commandline
flag as

.. code:: bash

   > xtb --md --input metadyn.inp coord


By using the flag ``--metadyn integer``, the number of saved structures may
also be entered via the commandline and need not to be present in the
detailed input:

.. code:: bash

   > xtb --metadyn 10 --input metadyn.inp coord

MTD specific Files
------------------

After the ``xtb`` program has performed the desired MTD simulation the trajectory of the structures can be found in ``xtb.trj``.
Furthermore, files with the names ``scoord.*`` are generated. After every picosecond of simulation the structure at this point 
will be written into these files. After a successful completion of the MTD simulation a ``xtbmdok`` file will be touched. 
The structure and velocities at the end of the simulation will be written into a ``mdrestart`` file.  

Restart
-------
The ``mdrestart`` file can be used to restart an MTD simulation. This can be very helpful for equilibration purposes. 
In order to achive this, in the ``$md`` block the ``restart`` parameter has to be set to ``true``.

.. code::

   > cat xcontrol
   $md
    restart=true

Example/Case study
==================

To summarize the most important topics of this chapter we will perform an MTD simulation of the water dimer molecule with `xTB`.
Make sure that ``xtb`` is properly set up and you have the following files in your working directory

.. code::

 > cat coord
 $coord
   -3.41057596145012      0.01397950240733     -2.48858246259228       o
   -4.61882519451466     -0.37951683073135     -3.80598534178089       h
   -3.93472948277574      1.59474363012965     -1.77520048297923       h
   -6.45960866143682     -1.52443579539520     -6.62024481959292       o
   -5.26218381176973     -1.40667057754076     -7.97781013203256       h
   -6.78373759577982     -3.28799737179945     -6.34039886662289       h
 $end

 > cat metadyn.inp
 $md
    time=10
    step=1
    temp=298
 $end   
 $metadyn
    atoms: 1-3
    save=10
    kpush=0.02
    alp=1.2
 $end   

As you can see, we will run the MTD simulation for 10 ps with a timestep of 1 fs at a temperature of 298 Kelvin. 
For the meta-dynamics, only the structure of the first water molecule will be taken into account in the rmsd criteria. 
To start the simulation we call xtb as follows

.. code:: bash

 > xtb --md --input metadyn.inp coord
 
The output for the example MTD simulation of the water dimer will look like this:

.. code:: bash

           -------------------------------------------------      
          |                  Meta Dynamics                  |
           ------------------------------------------------- 
 trajectories on xtb.trj or xtb.trj.<n>
 
 MD time /ps        :   10.00
 dt /fs             :    1.00
 SCC accuracy       :    1.00
 temperature /K     :  298.00
 max steps          : 10000
 block length (av.) :  5000
 dumpstep(trj) /fs  :  100.00   100
 dumpstep(coords)/fs: 1000.00  1000
 H atoms mass (amu) :     2
 # deg. of freedom  :    14
 SHAKE on. # bonds  :           4  all: T
 Berendsen THERMOSTAT on
 kpush  :    0.020
 alpha  :    1.200
 update :  10
         time (ps)    <Epot>      Ekin   <T>   T     Etot
      0    0.00      0.00000   0.0198    0.    0.   -10.10916
 est. speed in wall clock h for 100 ps :  0.01
    200    0.20    -10.09118   0.0116  559.  524.   -10.12881
    400    0.40    -10.11436   0.0105  454.  471.   -10.13041
    600    0.60    -10.12260   0.0070  431.  316.   -10.13157
    800    0.80    -10.12671   0.0071  412.  321.   -10.13081
    ...    ...      ...        ...     ...   ...     ...
   4800    4.80    -10.13763   0.0084  469.  379.   -10.13198
 block <Epot> / <T> :     -10.13978  465.     drift:  0.99D+02   Tbath : 298.
   5000    5.00    -10.13775   0.0082  465.  368.   -10.13253
   5200    5.20    -10.13783   0.0129  469.  582.   -10.12808
   5400    5.40    -10.13794   0.0105  471.  474.   -10.13014
   5600    5.60    -10.13804   0.0090  470.  407.   -10.13140
   ...     ...      ...        ...     ...   ...     ...
   9800    9.80    -10.13918   0.0083  462.  376.   -10.13258
 average properties 
 Epot               :  -10.1392169717059     
 Epot (accurate SCC):  -10.1402473210558     
 Ekin               :  1.019492766065306E-002
 Etot               :  -10.1290220440452     
 T                  :   459.900938472654     
 thermostating problem
 normal exit of md()
              

In the file ``xtb.trj`` we can find our trajectory. 
We can analyze the structures now by displaying them in a molecular graphics editor (e.g., `MOLDEN`_, `VMD`_ etc. ) 
or a trajectory analyzer (e.g. `TRAVIS`_).

.. _MOLDEN: http://cheminf.cmbi.ru.nl/molden/
.. _VMD: https://www.ks.uiuc.edu/Research/vmd/
.. _TRAVIS: https://www.chemie.uni-bonn.de/pctc/mulliken-center/software/travis/travis

Constrained MD/MTD simulations
==============================

As you may have noticed in the example given above by checking the file ``xtb.trj``, the water dimer dissociates within 
the MTD simulation due to the applied bias potential. If you run dynamics for systems that are non-covalently bound, 
you may encounter this problem from time to time. To avoid dissociation you can try to confine the simulation in a sphere by 
a repulsive potential. For further details check how to confine a cavity in :ref:`detailed-input`.

To avoid dissociation of the water dimer by a logfermi potential, the input file has to be modified:

.. code::

 > cat metadyn.inp
 $md
    time=10
    step=1
    temp=298
 $end   
 $metadyn
    atoms: 1-3
    save=10
    kpush=0.02
    alp=1.2
 $end
 $wall
    potential=logfermi
    sphere: auto, all
 $end   
 
To start the constrained MTD simulation we call xtb as follows:

.. code:: bash

 > xtb --md --input metadyn.inp coord

If you now check the trajectory file, you will see that the water molecules do not separate.

.. note:: The wall potential does not only work for MD/MTD simulations.
          It may also be applied in the same manner for single point calculations and geometry optimizations.
