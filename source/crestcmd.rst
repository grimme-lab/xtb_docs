.. _crestcmd:

-------------------
 Basic Usage
-------------------

.. contents::


``CREST`` is usually invoked via commandline, and requires only a coordinate input file.
The program supports the ``TURBOMOLE`` coordinates (coord, Bohr) and Xmol (`*`.xyz, Angstroem) format 
and can be called via

.. code:: bash

   > crest [INPUT] [OPTIONS]
   
If no file is given as ``[INPUT]``, then ``CREST`` automatically searches for a file called *coord*
in the ``TURBOMOLE`` format. The different ``[OPTIONS]`` are listed below and refer to
``Version 2.7`` of the ``CREST`` code.



Runtypes
========

Several different applications are availabile within the ``CREST`` program.
The most important usage are the two different conformational search algorithms MF-MD-GC and iMTD-GC,
but there are also some smaller utility tools that can be used, such as an CRE sorting function (CREGEN),
or a standalone z-martix sorting function (ZSORT).
The different runtypes are:

MF-MD-GC algorithm
   :flag: ``-v1``
   :description:
     First generation of the GFN\ *n*-xTB driven conformational search algorithm, consisting
     out of mode following, molecular dynamics sampling and genetic structure crossing.

MTD-GC algorithm
   :flag: ``-v2``
   :description:
     Second generation of the GFN\ *n*-xTB driven conformational search algorithm, consisting
     out of a meta-dynamics approach and genetic structure crossing.

iMTD-GC algorithm ``[DEFAULT]``
   :flag: ``-v2i, -v3``
   :description:
     Iterative version of the MTD-GC workflow, which is the default runtype of ``CREST``.

CREGEN ensemble sorting tool
   :flag: ``-cregen <FILE>``
   :description:
     Tool to sort a given ensemble ``<FILE>`` according to energy, atomic RMSD and
     rotational constant. A reference structure (e.g. *coord*) has to be provided.

ZSORT z-matrix sorting tool
   :flag: ``-zsort``
   :description:
     The atom order of the given input file is sorted in order to yield a more consistent z-matrix,
     i.e., atoms are grouped together according to the molecular structure (e.g. methyl groups).

MDOPT parallel ensemble optimization
   :flag: ``-mdopt <FILE>``
   :description:
     Optimize each point on a given trajectory or ensemble file ``<FILE>`` with GFN\ *n*--xTB.

SCREEN ensemble screening tool
   :flag: ``-screen <FILE>``
   :description:
     Optimize each point on a given trajectory or ensemble file ``<FILE>`` with GFN\ *n*--xTB
     in a multilevel approach and sort the resulting ensemble (CREGEN).

Other structure screening modes
    :flag: ``-protonate``
    :description:
      A tool that can be used to find protonation sites, i.e., the protomers of the input structure.
      In the approach first localized molecular orbitals (LMOs) are calculated and LP- and π-centers
      are identified. Then, a proton is added to each of these centers and the resulting structures are
      optimized and sorted.
    :flag: ``-deprotonate``
    :description:
      A tool to find deprotomers of the input structure. Each H atom is removed and the resulting 
      structures are optimized and sorted.
    :flag: ``-tautomerize``
    :description:
      A tool that combines the ``-protonate`` and ``-deprotonate`` options to find (prototropic)
      tautomers of the input structure.
      

Options
=======

General Options
---------------

-h, -help
    Show help page

-chrg INT
    Specify molecular charge as *INT*, overrides ``.CHRG`` file

-uhf INT
    Specify :math:`N_{\alpha}-N_{\beta}` as *INT*, overrides ``.UHF`` file

-gfn0 
    Use GFN0-xTB


-gfn1
    Use GFN1-xTB


-gfn2 ``[DEFAULT]``
    Use GFN2-xTB, which is the default.

-g, -gbsa SOLVENT
    Generalized born (GB) model with solvent accessable surface (SASA) model,
    available solvents are *acetone*, *acetonitrile*, *benzene* (only GFN1-xTB),
    *CH2Cl2*, *CHCl3*, *CS2*, *DMF* (only GFN2-xTB), *DMSO*, *ether*, *H2O*,
    *methanol*, *n-hexane* (only GFN2-xTB), *THF* and *toluene*.
    The solvent input is not case-sensitive.

-opt LEVEL
    Set the optimization accuracy for final GFN\ *n*--xTB optimizations.
    See :ref:`geometry optimization` for valid *LEVEL* arguments.
    The ``[DEFAULT]`` is *vtight*.

-zs ``[DEFAULT]``
    Perform z-matrix sorting (i.e. ZSORT) for the input coordinate file.

-nozs
    Do not perform z-matrix sorting of the input file.

-ewin REAL
    Set the energy threshold to *REAL* kcal/mol. This affects several runtypes and
    the ``[DEFAULT]`` is depending on the application (6 kcal/mol conformational searches,
    30 kcal/mol screening tools).


-xnam BIN
    Specify the name (and path) of the ``xtb`` binary that
    sould be used as *BIN*. The ``[DEFAULT]`` is *xtb*.

-prsc
    Create a scoord.`*` file for each conformer in the ``TURBOMOLE`` format.

-niceprint
    In-line progress bar printout for optimizations.

-scratch <DIR>
    Performs the entire calculation in the specified <DIR>. If <DIR> is not existing it will be created.

-T INT
    Specify the number of CPU threads *INT* that shall be used.
    ``CREST`` automatically adjusts the number of processes according to this variable
    in each step, in order to achieve optimal parallelization of the calculations.

-dry
    Perfrom a "dry" run, i.e., nothing is actually done but instead an overview of the 
    settings that would be applied in the calculation is given.


Options for MF-MD-GC
-------------------------

.. warning:: The MF-MD-GC workflow is outdated

-nomf
    Skip modefollowing
    
-nomd
    Skip MD part

-nocross            
    Skip genetic crossing part.

-loose              
    Decrease used number of selected modes

-vloose             
    Decrease used number of selected modes a lot

-tight              
    Increase used number of selected modes

-mdlen, -len REAL
    Set length of the molecular dynamics simulation to *REAL* ps.
    The ``[DEFAULT]`` is 40 ps.

-shake INT        
    Set SHAKE mode for MD. *INT* can be 0(= off), 1(= H-only), 2(= all bonds)
    The ``[DEFAULT]`` is 2.

-quick              
    Conduct only one MF/MD (no GC) run to obtain a crude conformer ensemble.


Options for iMTD-GC
-----------------------------
-cross ``[DEFAULT]``
    Do the genetic structure crossing (GC) part.

-nocross
    Don´t do the GC part.

-mrest INT
    Maximum number of MTD restarts in iMTD-GC algorithm. The ``[DEFAULT]`` is 5 cycles.

-shake INT
    Set SHAKE mode for MD. *INT* can be 0(= off), 1(= H-only), 2(= all bonds)
    The ``[DEFAULT]`` is 2.

-tstep INT
    Set MD time step to *INT* fs. The ``[DEFAULT]`` is 5 fs.

-mdlen, -len REAL
    Set length of the meta-dynamics simulations (MTD) to *REAL* ps.
    The ``[DEFAULT]`` is depending on the size and flexibility of the system.

-mddump INT
    Set dumpstep in which coordinates are written to the trajectory file to *INT* fs.
    The ``[DEFAULT]`` is 100 fs.

-vbdump REAL 
    Set dump frequency in which a new reference structure is taken for :math:`V_{bias}` to *REAL* ps.
    The ``[DEFAULT]`` is 1.0 ps.
                     
-tnmd REAL
    Set temperature for the additional normal MDs on the lowest conformers after the MTD step.
    The ``[DEFAULT]`` is 400 K.

-norotmd           
    Don´t do the additional  MDs on the lowest conformers after the MTD step.

-quick
    Perform a search with reduced settings for a crude conformer ensemble.

-squick, -superquick
    Perform an even more crude conformational search than with ``-quick``.

-mquick
    Perform an even more crude conformational search than with ``-quick`` or ``-squick``.

-origin ``[DEFAULT]``           
    Track the step of generation for each conformer/rotamer.

-keepdir
    Keep sub-directories of the conformer production run.

-nci
    Specialized NCI mode that can be used to find aggregates of NCI complexes.
    The option generates an ellipsoide potential around the input structure and adds it to the MTD simulation.
    Also, settings for :math:`k` and :math:`\alpha` are adjusted and some settings are reduced,
    in order to achieve lower computation times.

-wscal REAL
    Scale the ellipsoide potential axes in the NCI mode  by factor *REAL*.



Options for CREGEN
------------------

.. note:: The CREGEN routine is also used to sort in between the steps of the conformational searches.
          Therefore the following options also affect the performance of the two conformer algorithms.

-rthr REAL
     Set RMSD threshold in Ångström. The ``[DEFAULT]`` is 0.125 Å.

-ethr REAL
     Set energy threshold between conformer pairs in kcal/mol. The ``[DEFAULT]`` is 0.10 kcal/mol.

-bthr REAL
     Set Rotational constant threshold to *REAL*. The ``[DEFAULT]`` is 0.02.

-athr REAL
     Similarity threshold to determine internal rotation equal atoms for NMR.
     The ``[DEFAULT]`` is 0.04.

-pthr REAL
     Boltzmann population threshold. The ``[DEFAULT]`` is 0.05 (= 5%).


-temp REAL
     Set temperature for the calculation of Boltzmann weights. The ``[DEFAULT]`` is 298.15 K.

-nmr, -eqv               
      Activate determination and printout of NMR-equivalencies. Writes the files
      ``anmr_rotamer`` and ``anmr_nucinfo``, which are required by the ``ENSO`` python script.

-metac 
      Automatic methyl group rotamer equivalence correction. 

-esort
     Sort only based on energy (i.e., no RMSD and rotational constant comparison)

-nowr 
     Don´t write new ensemble files (crest_rotamers_`*`.xyz, crest_conformers.xyz)

-rot
     Use only rotational constant for checks (and no RMSD)

-subrmsd
     Compare only those parts of the structure that were also included in the metadynamics bias potential.
     Can be important for constrained conformational searches. 


Options for other modes
-----------------------
-compare <FILE1> <FILE2>
     Compare two ensembles *<FILE1>* and *<FILE2>*. Both ensembles must have the same
     order of atoms of the molecule and should contain rotamers.

-maxcomp INT
     Selcect the lowest *INT* conformers out of each ensemble to be compared with ``-compare``.
     The ``[DEFAULT]`` is the 10 lowest conformers.

-iter INT
     Number of Protonation/Deprotonation Iterations for ``-tautomerize`` mode. The ``[DEFAULT]``
     is 2 iterations.
     
-swel STR
     Change H\ :math:`^+` in the protonation tool to some other ion specified by *STR*.
     *STR* has to contain the element symbol AND charge, e.g. ``Na+``


Options related to constrainment
--------------------------------
-cinp <FILE>
    Specify a <FILE> with additional constraints in the xTB syntax.
    This file will be used instead of any ``.xcontrol`` or ``.constrains`` file.

--constrain <atom list>
    Set up an example file in which the atoms in <atom list> shall be constrained.
    The file will be called ``.xcontrol.sample``. No calculations will be performed
    and the run is aborted after this sample is wirtten.

Options for the PROPERTY mode
-----------------------------

.. note:: The PROPERTY mode automatically performs additional calculations on the final
          conformer ensemble after the iMTD-GC (or a given input ensemble, see flag ``-forall``)


-prop STR
     This initializes the usage of the "property" mode.
     *STR* defines what shall be done with the ensemble.
     Valid options for *STR* are currently (case sensitive!):

``hess``   - performs a hessian calculation for all conformers and re-weights the ensemble on free energies

``reopt``  - reoptimization of the ensemble with vtight thresholds (usefull for "quick" runs)

``autoIR`` - calculate vib. modes for all conformers and average them (weighted by Boltzmann populations) in a single "crest.vibspectum" file.


-forall <FILE>
      Instead of starting the property calculation on the final conformer ensemble file after iMTD-GC
      the property mode can directly be started for a given input ensemble <FILE> in the Xmol (`*`.xyz) format.

