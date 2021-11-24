.. _md:

-------------------------------
 Molecular Dyamics Simulations
-------------------------------

In this chapter, all necessary information will be given in order
to perform MD simulations with xTB. 
The adjustable parameters will be discussed and a guide to how to change
them will be given.

.. contents::

General command-line control
============================

There are two main possibilities how to evoke a MD simulation.
With the flag ``--omd`` geometry optimization will be performed
and this structure then will be used for the MD simulation, a loose
optimization level will be chosen.

.. code:: bash

  > xtb coord --omd
    
By using the flag ``--md`` the MD simulation will be performed directly with the user given input structure.

.. code:: bash

  > xtb coord --md

It is strongly recommended to start the MD simulation from an xTB
optimized structure. 
Otherwise there may be instabilities during the MD and the equilibration
will be severely hindered. 

Parameters
==========

In order to change the parameters of the MD simulation the ``$md`` block
in the input file has to be modified.

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
  | shake   | integer | 2         | use SHAKE algorithm to constrain bonds  |
  +         +         +           +                                         +
  |         |         |           | 0 = off, 1 = X-H only, 2 = all bonds    |   
  +---------+---------+-----------+-----------------------------------------+
  | step    | real    | 4 fs      | time step for propagation               |
  +---------+---------+-----------+-----------------------------------------+
  | velo    | boolean | false     | also write out velocities               |
  +---------+---------+-----------+-----------------------------------------+

The above default setting should look like below in your input file

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
   $end

.. important :: 
   For MD simulations with GFN-FF the time step must be reduced, for more information see section :ref:`gfnff`

MD specific Files
=================

After the ``xtb`` program has performed the desired MD simulation the trajectory of the structures can be found in ``xtb.trj``.
Furthermore, files with the names ``scoord.*`` are generated. After every picosecond of simulation the structure at this point will be written into these files. After a successful completion of the MD simulation a ``xtbmdok`` file will be touched. The structure and velocities at the end of the simulation will be written into a ``mdrestart`` file.

Trajectory
----------
The number of structures in the ``xtb.trj`` file depends on the ``dump`` variable and the propagation time step.
For practical purposes the two parameters are converted into a dump frequency *n* = (dump step/time step), e.g.,
a structure is written to the trajecotry equidistantly at every *n*-th propagation step. 
Due to this conversion the total number of structures in the ``xtb.trj`` file might be slightly larger
than the expected (total runtime/dump step).
The same applies to the ``scoord.*`` files.

Restart
-------
The ``mdrestart`` file can be used to restart an MD simulation. This can be very helpful for equilibration purposes. 
In order to achive this, in the ``$md`` block the ``restart`` parameter has to be set to ``true``.

.. code::

   > cat restart.inp
   $md
    restart=true

Example/Case study
------------------

To summarize the most important topics of this chapter we will perform an MD simulation of the ethane molecule with `xTB`.
Make sure that ``xtb`` is properly set up and you have the following files in your working directory

.. code::

 > cat coord
 $coord
  1.82409443250962  -0.02380488009596  0.17250251620479  c
  4.68095348739630  -0.02380488009596  0.17250308312263  c
  1.09744635742609   1.41159121722257 -1.12629926294082  h
  1.09744579050825   0.38329239564274  2.06499275150500  h
  1.09744635742609  -1.86629844212581 -0.42118612892243  h
  5.40760175145245   1.81868868193389  0.76619172824984  h
  5.40760212939767  -0.43090215583466 -1.71998734115020  h
  5.40760175145245  -1.45920097741449  1.47130486226824  h
 $end
 > cat md.inp
 $md
  time=10
  step=1
  temp=500
  shake=1

As you can see, we will run the simulation for 10 ps with a timestep of 1 fs at a temperature of 500 Kelvin. Furthermore, all hydrogen-containing bonds will be constrained using the *SHAKE* algorithm. To start the simulation we call xtb as follows

.. code:: bash

 > xtb coord --input md.inp --omd

The program will start with performing a geometry optimization,
the optimized structure used to start the dynamic can be found
and inspected in ``xtbopt.coord``.

In the file ``xtb.trj`` we can find our trajectory. We can analyze the structures now by displaying them in a molecular graphics editor (e.g., `MOLDEN`_, `VMD`_ etc. ) or a trajectory analyzer (e.g. `TRAVIS`_).

.. _MOLDEN: http://cheminf.cmbi.ru.nl/molden/
.. _VMD: https://www.ks.uiuc.edu/Research/vmd/
.. _TRAVIS: https://www.chemie.uni-bonn.de/pctc/mulliken-center/software/travis/travis


