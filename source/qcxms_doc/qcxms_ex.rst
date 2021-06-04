.. _qcxms_example:

----------
Quickstart
----------

The default settings of QCxMS provide a good starting point for the calculations. However, if more 
control for :ref:`run_qcxms` is needed, example input files for each run type can be found below.

EI - positive Ion Electron Ionization
-------------------------------------


.. code:: 

   ei
   xtb
   ntraj xxx
   tmax  xxx

DEA - negative Ion Electron Ionization
--------------------------------------

.. code::

   dea
   scan 1
   upper 15
   lower 0
   <functional>
   <basis>

Instead of the IEE distribution, the excess energy is scanned by switching on the scan keyword, with upper/lower bounds in eV. The
negative ion mode is enabled with the the xtb method or the orca program for DFT methods. The ma-def2-SVP/ma-def2-TZVP basis sets 
are recommended for anion calculations. The xtb negative ion simulations are not recommended for production level for accuracy reasons.


CID - positive Ion Collision Induced Dissociation
-------------------------------------------------

.. code::

   cid
   xtb2
   eimpact 60
   lchamb 0.25
   fullauto

The run-type must explicitly be defined so the program knows how to handle the number of collisions. For the *general activation method*, use the `fullauto` command. For the *forced activation run-type*, the number of collisions are set by using *collno x* to set all collisions (including fragment-gas collisions (*fgc*)). For setting ONLY mol. ion-gas collisions, use *maxcoll x*. 
