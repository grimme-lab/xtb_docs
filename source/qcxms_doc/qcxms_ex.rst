.. _qcxms_example:

----------
Quickstart
----------

Default settings of QCxMS provide a good starting point for the calculations.
For the different run modes, use the settings described in section :ref:`run_qcxms`.

The `keywords`_ have to be put into a *qcxms.in* file. 
The file can be created manually, or is created automatically by first running the program. 
In the latter case all default settings are used. 
To use other run modes, see the examples below. 
The keyword provided are points of orientation and there are more that can be used to tune the calculations. 

.. note::
  The default values are **not always** giving the best results, but were determined to give the best outcome for most 
  chemical systems.  
  Molecules have different characteristics that significantely influence the calculations. 
  Changing the input settings can improve upon the final result, e.g. decreasing the molecular ion peak abundance or
  producing more low *m/z* fragments. 

EI - positive Ion Electron Ionization
-------------------------------------


.. code:: 
    ei
    xtb2
    ntraj *<integer>*
    tmax  *<real>*

The EI run mode is automatically used with the [*default*] settings described by the `keywords`_, leading to a run using
`xtb2`, so actually there is no keyword required in the *qcxms.in* file. 
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

The negative ion mode is enabled with the xTB or ORCA. The ma-def2-SVP/ma-def2-TZVP basis sets are recommended automatically 
chosen for calculation of the electron affinity (EA). 
For EI in negative ion mode, production level runs are recommended to be conducted at DFT level. 
However, these are computationally very expensive and xTB can be used for a quantitative overview. 
As less IEE energy is required, it is recommended to reduce the IEE distribution limits by using the `upper` and `lower`
keywords.

The old keywords `dea` and `scan` are obsolete.


CID - positive Ion Collision Induced Dissociation
-------------------------------------------------

.. code::

   cid
   xtb2
   charge 1
   elab 40
   lchamb 0.25

The run-type must explicitly be defined so the program knows how to handle the number of collisions. 
For the *general activation method*, use the `fullauto` command. For the *forced activation run-type*, the number of collisions are 
set by using *collno x* to set all collisions (including fragment-gas collisions (*fgc*)). For setting ONLY mol. ion-gas collisions, use *maxcoll x*. 

