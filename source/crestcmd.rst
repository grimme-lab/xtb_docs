.. _crestcmd:

-------------------
 Basic Usage
-------------------

.. contents::


``CREST`` is usually invoked via commandline, and requires only a coordinate input file.
The program supports the ``TURBOMOLE`` coordinates (coord) and Xmol (`*`.xyz) format and can
be called via

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
     show help page

-chrg INT
    specify molecular charge as *INT*, overrides ``.CHRG`` file

-uhf INT
    specify :math:`N_{\alpha}-N_{\beta}` as *INT*, overrides ``.UHF`` file

-gfn0 
  use GFN0-xTB


-gfn1
  use GFN1-xTB


-gfn2 ``[DEFAULT]``
    use GFN2-xTB, which is the default.

-g, -gbsa SOLVENT
    generalized born (GB) model with solvent accessable surface (SASA) model,
    available solvents are *acetone*, *acetonitrile*, *benzene* (only GFN1-xTB),
    *CH2Cl2*, *CHCl3*, *CS2*, *DMF* (only GFN2-xTB), *DMSO*, *ether*, *H2O*,
    *methanol*, *n-hexane* (only GFN2-xTB), *THF* and *toluene*.
    The solvent input is not case-sensitive.

-opt LEVEL
    set the optimization accuracy for final GFN\ *n*--xTB optimizations.
    See :ref:`geometry optimization` for valid *LEVEL* arguments.
    The ``[DEFAULT]`` is *vtight*.

-zs ``[DEFAULT]``
    perform z-matrix sorting (i.e. ZSORT) for the input coordinate file.

-nozs
  do not perform z-matrix sorting of the input file.

-ewin REAL
    set the energy threshold to *REAL* kcal/mol. This affects several runtypes and
    the ``[DEFAULT]`` is depending on the application (6 kcal/mol conformational searches,
    30 kcal/mol screening tools).


-xnam BIN
    specify the name (and path) of the ``xtb`` binary that
    sould be used as *BIN*. The ``[DEFAULT]`` is *xtb*.

-prsc
  create a scoord.`*` file for each conformer in the ``TURBOMOLE`` format.

-niceprint
     in-line progress bar printout for optimizations.


-T INT
    specify the number of CPU threads *INT* that shall be used.
    ``CREST`` automatically adjusts the number of processes according to this variable
    in each step, in order to achieve optimal parallelization of the calculations.


Options for MF-MD-GC
-------------------------
-nomf
  skip modefollowing
    
-nomd
  skip MD part

-nocross            
    skip genetic crossing part.

-loose              
    decrease used number of selected modes

-vloose             
    decrease used number of selected modes a lot

-tight              
    increase used number of selected modes

-mdlen, -len REAL
    set length of the molecular dynamics simulation to *REAL* ps.
    The ``[DEFAULT]`` is 40 ps.

-shake INT        
    set SHAKE mode for MD. *INT* can be 0(= off), 1(= H-only), 2(= all bonds)
    The ``[DEFAULT]`` is 2.

-quick              
    conduct only one MF/MD (no GC) run to obtain a crude conformer ensemble.


Options for iMTD-GC
-----------------------------
-cross ``[DEFAULT]``
   do the genetic structure crossing (GC) part.

-nocross
   don´t do the GC part.

-mrest INT
    maximum number of MTD restarts in iMTD-GC algorithm. The ``[DEFAULT]`` is 5 cycles.

-shake INT
    set SHAKE mode for MD. *INT* can be 0(= off), 1(= H-only), 2(= all bonds)
    The ``[DEFAULT]`` is 2.

-tstep INT
    set MD time step to *INT* fs. The ``[DEFAULT]`` is 5 fs.

-mdlen, -len REAL
    set length of the meta-dynamics simulations (MTD) to *REAL* ps.
    The ``[DEFAULT]`` is depending on the size and flexibility of the system.

-mddump INT
    set dumpstep in which coordinates are written to the trajectory file to *INT* fs.
    The ``[DEFAULT]`` is 100 fs.

-vbdump REAL 
    set dump frequency in which a new reference structure is taken for :math:`V_{bias}` to *REAL* ps.
    The ``[DEFAULT]`` is 1.0 ps.
                     
-tnmd REAL
    set temperature for the additional normal MDs on the lowest conformers after the MTD step.
    The ``[DEFAULT]`` is 400 K.

-norotmd           
    don´t do the additional  MDs on the lowest conformers after the MTD step.

-quick
    perform a search with reduced settings for a crude conformer ensemble.

-squick, -superquick
    perform an even more crude conformational search than with ``-quick``.

-origin ``[DEFAULT]``           
    track the step of generation for each conformer/rotamer.

-keepdir
    keep sub-directories of the conformer production run.

-nci
    specialized NCI mode that can be used to find aggregates of NCI complexes.
    The option generates an ellipsoide potential around the input structure and adds it to the MTD simulation.
    Also, settings for :math:`k` and :math:`\alpha` are adjusted and some settings are reduced,
    in order to achieve lower computation times.

-wscal REAL
    scale the ellipsoide potential axes in the NCI mode  by factor *REAL*.



Options for CREGEN
------------------

.. note:: The CREGEN routine is also used to sort in between the steps of the conformational searches.
          Therefore the following options also affect the performance of the two conformer algorithms.

-rthr REAL
     set RMSD threshold in Ångström. The ``[DEFAULT]`` is 0.125 Å.

-ethr REAL
     set energy threshold between conformer pairs in kcal/mol. The ``[DEFAULT]`` is 0.10 kcal/mol.

-bthr REAL
     set Rotational constant threshold to *REAL*. The ``[DEFAULT]`` is 0.02.

-athr REAL
     similarity threshold to determine internal rotation equal atoms for NMR.
     The ``[DEFAULT]`` is 0.04.

-pthr REAL
     Boltzmann population threshold. The ``[DEFAULT]`` is 0.05 (= 5%).


-temp REAL
     set temperature for the calculation of Boltzmann weights. The ``[DEFAULT]`` is 298.15 K.

-nmr, -eqv               
      activate determination and printout of NMR-equivalencies. Writes the files
      ``anmr_rotamer`` and ``anmr_nucinfo``, which are required by the ``ENSO`` python script.

-metac 
      automatic methyl group rotamer equivalence correction. 

-esort
      sort only based on energy (i.e., no RMSD and rotational constant comparison)

-nowr 
      don´t write new ensemble files (crest_rotamers_`*`.xyz, crest_conformers.xyz)

-rot
      use only rotational constant for checks (and no RMSD)


Options for other modes
-----------------------
-compare <FILE1> <FILE2>
     compare two ensembles *<FILE1>* and *<FILE2>*. Both ensembles must have the same
     order of atoms of the molecule and should contain rotamers.

-maxcomp INT
     Selcect the lowest *INT* conformers out of each ensemble to be compared with ``-compare``.
     The ``[DEFAULT]`` is the 10 lowest conformers.

-iter INT
     Number of Protonation/Deprotonation Iterations for ``-tautomerize`` mode. The ``[DEFAULT]``
     is 2 iterations.
     
-swel STR
     Change H\ :math:`^+` in the protonation tool to some other ion *STR*, e.g. Na



