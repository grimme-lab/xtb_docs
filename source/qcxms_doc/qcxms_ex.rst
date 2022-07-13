.. _qcxms_example:

----------
Quickstart
----------

The default settings provided in QCxMS are a good starting point for the calculations.
The default setting is performing an `EI`_ calculation.

To change the behavior of the program, keywords have to be put into a *qcxms.in* file.
The file can be created manually, or is created automatically by first running the program, using the *default*
settings.
If other run modes are required (*e.g.* CID or DEA), example inputs are given below. 
The keywords provided are points of orientation and there are more settings that can be used to tune the calculations which 
are described in detail in section :ref:`run_qcxms`.  

.. note::
  The default values are **not always** giving the best results, but are set to cover the majority of systems.  
  Molecules have different characteristics that can significantely influence the outcome. 
  Changing the input settings can improve upon the final result, e.g. decreasing the molecular ion peak abundance or
  producing more low *m/z* fragments. 

EI - positive Ion Electron Ionization
-------------------------------------
.. _EI:

.. code:: 

   ei
   xtb2
   ntraj *<integer>*
   tmax  *<real>*

The EI run mode is automatically used with the [*default*] settings described by the keywords, leading to a run using
``xtb2``, so actually there is no keyword required in the *qcxms.in* file. 
To provide some guidance, the setting showed above can be used and the *<integer>* and *<real>* placeholders switched to
actual values. 

DEA - negative Ion Electron Ionization
--------------------------------------

.. code::

   ei
   <functional>
   <basis>
   charge -1
   upper 15
   lower 0

For EA calculations, the ma-def2-SVP/ma-def2-TZVP basis sets are recommended.
Production runs are recommended to be conducted at DFT level, however, these are computationally very expensive and GFN-xTB can be 
used for a quantitative overview. 
The most cost-effordable DFT settings are *<functional>* = ``PBE`` | *<basis>* = ``ma-def2-svp``

Less IEE energy is required for DEA fragmentation, so it is recommended to reduce the IEE distribution limits 
by using the ``upper`` and ``lower`` keywords (the old keywords ``dea`` and ``scan`` are obsolete).


CID - positive Ion Collision Induced Dissociation
-------------------------------------------------

.. code::

   cid
   xtb2
   charge 1
   elab 40
   lchamb 0.25

The *general activation method* is automatically used, but can also be expicitely switched on using the ``fullauto`` keyword. 

For the *forced activation run-type*, use ``collauto``.
The number of collisions are  set by using ``collno`` *<integer>* to set number of collisions (including fragment-gas 
collisions (*fgc*)). 
For setting ONLY ion--gas collisions, use ``maxcoll`` *<integer>*. 

The *thermal activation method* is switched on with ``temprun`` and scales the internal energy to a value given by the
keyword ``esi`` *<real>*.
This is the fastest method, but requires a good initial guess for the internal energy value.

For more details, see section :ref:`run_qcxms`.

