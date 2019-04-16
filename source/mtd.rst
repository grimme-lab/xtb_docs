.. _mtd:

-------------------------------
 Meta-Dynamics Simulations
-------------------------------

In this chapter, all necessary information will be given in order to perform meta-dynamics (MTD) simulations with `xTB`.

.. contents::

From a practical point of view the application of meta-dynamics is quite similar to molecular-dynamic simulations.
In MTD simulations a biasing potential given as a sum of Gaussian functions is additionally expressed. 
The root-mean-square deviation (RMSD) in Cartesian space is chosen as a metric for the collective variables.  

All adjustable parameters will be discussed and a guide to how to change them will be given as well as an example.


General command-line control
============================
    
By using the flag ``--metadyn`` the MD simulation will be performed directly with the user given input structure (coord).

.. code:: bash

  > xtb coord --metadyn

It is strongly recommended to start the MTD simulation from an xTB
optimized structure. 
Otherwise there may be instabilities during the MD and the equilibration
will be severely hindered. 


General MD Parameters
=====================

Basic parameters for MTD simulations such as time and temperature are adjustable via the ``$md`` block.

In order to change the parameters of the MTD simulation the ``$metadyn`` block
in the xcontrol/input file has to be modified.

  +---------+---------+-----------+-----------------------------------------+
  |  key    | value   | default   | description                             |
  +=========+=========+===========+=========================================+
  | dump    | real    | 50 fs     | interval for trajectory printout        |
  +---------+---------+-----------+-----------------------------------------+
  | hmass   | integer | 4 times   | mass of hydrogen atoms                  |
  +---------+---------+-----------+-----------------------------------------+
  | nvt     | boolean | true      | perform simulation in NVT ensemble      |
  +---------+---------+-----------+-----------------------------------------+
  | restart | boolean | false     | read velocities from ``mdrestart``      |
  +---------+---------+-----------+-----------------------------------------+
  | temp    | real    | 298.15 K  | thermostat temperature                  |
  +---------+---------+-----------+-----------------------------------------+
  | time    | real    | 50 ps     | total run time of simulation            |
  +---------+---------+-----------+-----------------------------------------+
  | sccacc  | real    | 2.0       | accuracy of xTB calculation in dynamics |
  +---------+---------+-----------+-----------------------------------------+
  | shake   | integer | all bonds | use SHAKE algorithm to constrain bonds  |
  +---------+---------+-----------+-----------------------------------------+
  | step    | real    | 4 fs      | time step for propagation               |
  +---------+---------+-----------+-----------------------------------------+
  | velo    | boolean | false     | also write out velocities               |
  +---------+---------+-----------+-----------------------------------------+

Specific MTD Parameters
=======================

At this point it should be mentioned that the default settings are already optimized and there is no further need
to change the MTD specific paramters.
If one still wants to do so, the parameters of the MTD simulation the ``$metadyn`` block
in the xcontrol/input file has to be modified.

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

By using the flag ``--metadyn integer``, the number of saved structures may also be entered via the command line.

.. code:: bash

  > xtb coord --metadyn 10


The above xtb call with all other settings on default should look like below in your input file

.. code::

   $md
      temp=298.15 # in K
      time= 50.0  # in ps
      dump= 50.0  # in fs
      step=  4.0  # in fs
      velo=false
      nvt =true
      hmass=4
      shake=2
      sccacc=2.0
   $metadyn
      save=10
      factor=0.000000000


MTD specific Files
==================

After the ``xtb`` program has performed the desired MTD simulation the trajectory of the structures can be found in ``xtb.trj``.
Furthermore, files with the names ``scoord.*`` are generated. After every picosecond of simulation the structure at this point 
will be written into these files. After a successful completion of the MTD simulation a ``xtbmdok`` file will be touched. 
The structure and velocities at the end of the simulation will be written into a ``mdrestart`` file.  

Restart
-------
The ``mdrestart`` file can be used to restart an MTD simulation. This can be very helpful for equilibration purposes. 
In order to achive this, in the ``$md`` block the ``mdrestart`` parameter has to be set to ``true``.

.. code::

   > cat xcontrol
   $md
    mdrestart=true

Example/Case study
------------------

To summarize the most important topics of this chapter we will perform an MD simulation of the water dimer molecule with `xTB`.
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

 > cat xcontrol
 $md
    time=10
    step=1
    temp=298
 $metadyn
    atoms=1-3 

As you can see, we will run the MTD simulation for 10 ps with a timestep of 1 fs at a temperature of 298 Kelvin. 
For the meta-dynamics, only the structure of the second water molecule will be taken into account in the rmsd criteria. 
To start the simulation we call xtb as follows

.. code:: bash

 > xtb coord --input xcontrol --metadyn

In the file ``xtb.trj`` we can find our trajectory. 
We can analyze the structures now by displaying them in a molecular graphics editor (e.g., `MOLDEN`_, `VMD`_ etc. ) 
or a trajectory analyzer (e.g. `TRAVIS`_).

.. _MOLDEN: http://cheminf.cmbi.ru.nl/molden/
.. _VMD: https://www.ks.uiuc.edu/Research/vmd/
.. _TRAVIS: https://www.chemie.uni-bonn.de/pctc/mulliken-center/software/travis/travis
