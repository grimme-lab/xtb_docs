.. _crestcmd:

-------------------------------
 CREST command line arguments
-------------------------------

.. contents::


``CREST`` is usually invoked via commandline, and requires only a coordinate input file.
The program supports the ``TURBOMOLE`` coordinates (coord, Bohr), Xmol (`*`.xyz, Angstroem)
or 3d SDF (V2000,V3000) formats and can be called via

.. code:: bash

   > crest [INPUT] [OPTIONS]
   
If no file is given as ``[INPUT]``, then ``CREST`` automatically searches for a file called *coord*
in the ``TURBOMOLE`` format. Either must be present. The different ``[OPTIONS]`` are listed below and refer
to ``Version 2.11`` of the ``CREST`` code. ``[DEFAULT]`` options do not need to be specified explicitly.


General and techincal options
-----------------------------
   :flag: ``-h,-help``
   :description:
     Print a overview of most avialable options (i.e., this site)

   :flag: ``--version``
   :description:
     Print only the program header and disclaimer.

   :flag: ``--cite``
   :description:
     Print the most relevant citations.

   :flag: ``--xnam BIN``
   :description:
     Specify the name (and path) of the ``xtb`` binary that
     sould be used as *BIN*. The ``[DEFAULT]`` is *xtb*.

   :flag: ``--niceprint``
   :description:
     In-line progress bar printout for optimizations.

   :flag: ``--scratch <DIR>``
   :description:
     Performs the entire calculation in the specified ``<DIR>``. If ``<DIR>`` is not existing it will be created.

   :flag: ``--T <INT>``
   :description:
     Specify the number of CPU threads ``<INT>`` that shall be used.
     ``CREST`` automatically adjusts the number of processes according to this variable
     in each step, in order to achieve optimal parallelization of the calculations.

   :flag: ``--dry``
   :description:
     Perfrom a "dry" run, i.e., nothing is actually done but instead an overview of the 
     settings that would be applied in the calculation is given.


Runtypes
========

Several different applications are availabile within the ``CREST`` program.
The most important usage are the two different conformational search algorithms MF-MD-GC and iMTD-GC,
but there are also some smaller utility tools that can be used, such as an CRE sorting function (CREGEN),
or a standalone z-martix sorting function (ZSORT).
The different runtypes are:

MF-MD-GC algorithm (outdated)
   :flag: ``--v1``
   :description:
     First generation of the GFN\ *n*-xTB driven conformational search algorithm, consisting
     out of mode following, molecular dynamics sampling and genetic structure crossing.

MTD-GC algorithm (outdated)
   :flag: ``--v2``
   :description:
     Second generation of the GFN\ *n*-xTB driven conformational search algorithm, consisting
     out of a meta-dynamics approach and genetic structure crossing.

iMTD-GC algorithm ``[DEFAULT]``
   :flag: ``--v2i, --v3``
   :description:
     Iterative version of the MTD-GC workflow, which is the default runtype of ``CREST``.

iMTD-sMTD algorithm 
   :flag: ``--v4``
   :description:
     Iterative workflow, making use of static metadynmics simulations.

Conformational entropy algorithm 
   :flag: ``--entropy``
   :description:
     Specialized version of the iMTD-sMTD workflow, specialized in the calculation
     of conformational entropy. 


Ensemble sorting
================

CREGEN ensemble sorting tool for standalone use
   :flag: ``--cregen <FILE>``
   :description:
     Tool to sort a given ensemble ``<FILE>`` according to energy, atomic RMSD and
     rotational constant. A reference structure (e.g. *coord*) has to be provided.

   :flag: ``--ewin <REAL>``
   :description:
     Set the energy threshold to *REAL* kcal/mol. This affects several runtypes and
     the ``[DEFAULT]`` is depending on the application (6 kcal/mol conformational searches,
     30 kcal/mol screening tools).

   :flag: ``--prsc``
   :description:
     Create a scoord.`*` file for each conformer in the ``TURBOMOLE`` format.

   :flag: ``--rthr <REAL>``
   :description: 
     Set RMSD threshold in Ångström. The ``[DEFAULT]`` is 0.125 Å.

   :flag: ``--ethr <REAL>``
   :description:  
     Set energy threshold between conformer pairs in kcal/mol. 
     The ``[DEFAULT]`` is 0.05 kcal/mol.

   :flag: ``--bthr <REAL>``
   :description:
     Set lower bound for the rotational constant threshold to *REAL*. 
     The ``[DEFAULT]`` is 0.01 (= 1%). The thresold is dynamically
     adjusted between this value and 2.5%, based on the anisotropy
     of the rotational constants.

   :flag: ``--nmr, --eqv, --entropy``
   :description:               
      Activate determination and printout of NMR-equivalencies. Writes the files
      ``anmr_rotamer`` and ``anmr_nucinfo``, which are required by the ``ENSO`` python script.

   :flag: ``--athr <REAL>``
   :description:
     Similarity threshold to determine internal rotation equal atoms for NMR.
     The ``[DEFAULT]`` is 0.04.

   :flag: ``-temp <REAL>``
   :description:
     Set temperature for the calculation of Boltzmann weights. The ``[DEFAULT]`` is 298.15 K.

   :flag: ``--esort``
   :description:
     Sort only based on energy (i.e., no RMSD and rotational constant comparison)

   :flag: ``--nowr``
   :description: 
     Don´t write new ensemble files (crest_rotamers_`*`.xyz, crest_conformers.xyz)

   :flag: ``--subrmsd``
   :description:
     Compare only those parts of the structure that were also included in the metadynamics bias potential.
     Can be important for constrained conformational searches. 

   :flag: ``--notopo``
   :description:
     Turn off the initial topology check of the structures in the ensemble.

An extension to the CREGEN sorting is an automatic principle component analysis (PCA) and
k-Means sorting clustering algorithm. It can be invoked with the ``--cluster`` command.
   :flag: ``--cluster <INT>``
   :description:
     Perform a clustering on the final CREGEN ensemble to identify *INT* most representative
     structures, based on dihedral angles. Note that this algorithm currently does not
     work well for non-covalent complexes or molecular clusters and should only be applied
     to singular molecules.


Options
=======

Calculation settings passed to ``xtb``
--------------------------------------
Method selection
    :flag: ``--gfn2, --gfn1, --gfn0, --gfnff``
    :description:
      Use any of the respective GFN\ *n* methods. The default is GFN2-xTB,
      but GFN-FF is strongly recommended for faster sampling.

Charge and multiplicity
    :flag: ``--chrg <INT>``
    :description:
      Specify molecular charge as *INT*, overrides ``.CHRG`` file.
    :flag: ``--uhf <INT>``
    :description:
      Specify :math:`N_{\alpha}-N_{\beta}` as *INT*, overrides ``.UHF`` file

Implicit solvation
   :flag: ``--g, --gbsa <SOLVENT>``
   :description: 
     Generalized born (GB) model with solvent accessable surface (SASA) model,
     for available *SOLVENT* options se ref:`gbsa``.
     The solvent input is not case-sensitive.
   :flag: ``--alpb <SOLVENT>``
   :description:
     New ALPB implicit solvation model, for available *SOLVENT* options se ref:`gbsa``.
     The solvent input is not case-sensitive.

Geometry optimization thresholds
    :flag: ``--opt <LEVEL>``
    :description:
      Set the optimization accuracy for final GFN\ *n*--xTB optimizations.
      See :ref:`geometry optimization` for valid *LEVEL* arguments.
      The ``[DEFAULT]`` is *vtight*.



Options related to molecular dynamics and metadynamics simulations
------------------------------------------------------------------

    :flag: ``--mdlen, --len <REAL>``
    :description:
      The length of the metadynamics simulations (MTD) in CREST is usually determined
      automatically, but with this flag can be set to *REAL* (in ps).
      It is alos possible to set a multiple of the automatically determined length
      by using ``x<REAL>`` instead, where *REAL* then is a multiplicative factor
      (e.g. *x0.5* for half the default simulation length).

    :flag: ``--shake <INT>``
    :description:
      Set SHAKE mode for MD. *INT* can be 0(= off), 1(= H-only), or 2(= all bonds)
      The ``[DEFAULT]`` is 2.

    :flag: ``--tstep <INT>``
    :description:
      Set MD time step to *INT* fs. The ``[DEFAULT]`` is 5 fs for GFNn-xTB calculations
      (requires SHAKE), and 1.5 fs for GFN-FF. The timestep is also automatically checked
      with a trial simulation at the beginning of the conformational search.

    :flag: ``--mddump <INT>``
    :description:
      Set dumpstep in which coordinates are written to the trajectory file to *INT* fs.
      The ``[DEFAULT]`` is 100 fs.

    :flag: ``--vbdump <REAL>``
    :description:
      Set dump frequency in which a new reference structure is taken for :math:`V_{bias}` to *REAL* ps.
      The ``[DEFAULT]`` is 1.0 ps.



Options for conformational search algorithms
--------------------------------------------

Z-matrix sorting (see also ``--zsort`` above)
     :flag: ``--zs``
     :description:
       Perform z-matrix sorting (i.e. ZSORT) for the input coordinate file.
     :flag: ``--nozs``
     :description: ``[DEFAULT]`` Do not perform z-matrix sorting of the input file.

Genetic Z-matrix crossing
     :flag: ``--cross``
     :description: ``[DEFAULT]`` Perform Z-matrix strucutre crossing (GC) in the algorithm.
     :flag: ``--nocross``                                                                        
     :description:  Skip Z-matrix strucutre crossing.

Additional MD sampling after MTD
      :flag: ``--norotmd``           
      :description: Turn off the additional  MDs on the lowest conformers after the MTD step.
      :flag: ``--tnmd <REAL>``
      :description: 
        Set temperature for the additional normal MDs on the lowest conformers after the MTD step.
        The ``[DEFAULT]`` is 400 K.

Adjsuting iterative behavior of iMTD-GC
      :flag: ``--mrest <INT>``
      :description:
        Maximum number of MTD restarts in iMTD-GC algorithm. The ``[DEFAULT]`` is 5 cycles.

Special settings for the iMTD-GC workflow
      :flag: ``--quick``
      :description: Perform a search with reduced settings for a crude conformer ensemble.
      :flag: ``--squick, --superquick``
      :description: Perform an even more crude conformational search than with ``-quick``.
      :flag: ``--mquick``
      :description: Perform an even more crude conformational search than with ``-quick`` or ``-squick``.
      :flag: ``--nci``
      :description:
        Specialized NCI mode that can be used to find aggregates of NCI complexes.
        The option generates an ellipsoide potential around the input structure and adds it to the MTD simulation.
        Also, settings for :math:`k` and :math:`\alpha` are adjusted and some settings are reduced,
        in order to achieve lower computation times.
      :flag: ``---wscal <REAL>``
      :description:
        Scale the ellipsoide potential axes in the ``--nci`` mode by factor *REAL*.


Technical iMTD-GC settings
      :flag: ``--keepdir``
      :description: Keep sub-directories of the conformer production run.


Property mode appendix
       :flag: ``--prop <STR>``
       :description:
         This initializes the usage of the "property" mode as an appendix to the regular conformational search.
        *STR* defines what shall be done with the ensemble.
        Valid options for *STR* are currently (case sensitive!):
       :option: ``hess``  - performs a hessian calculation for all conformers and re-weights the ensemble on free energies
       :option: ``reopt`` - reoptimization of the ensemble with vtight thresholds (usefull for "quick" runs)
       :option: ``autoIR`` - calculate vib. modes for all conformers and average them (weighted by Boltzmann populations) in a single "crest.vibspectum" file.
      
       :flag: ``--for,--forall <FILE>``
       :description:
         Instead of starting the property calculation on the final conformer ensemble file after iMTD-GC
         the property mode can directly be started for a given input ensemble <FILE> in the Xmol (`*`.xyz) format.


.. note:: The different quick, NCI and property settings are incompatible with ``--entropy``!


Entropy mode settings
        :flag: ``--scthr,--entropy_cthr <REAL>``
        :description:
          Specifiy the ensemble growth threshold (% new conformers) for ``--entropy`` and ``--v4`` convergence.
          The default is 0.02 (=2%) for the entropy mode and  0.05 (=5%) for ``--v4``.

        :flag: ``--ssthr,--entropy_sthr <REAL>``
        :description:
          Specifiy the entropy growth threshold (% growth entropy) for ``--entropy`` and ``--v4`` convergence.
          The default is 0.005 (=0.5%) for the entropy mode and 0.01 (=1%) for ``--v4``. 

        :flag: ``--trange <from> <to> <step>``
        :description:
          Entropies from the ``--entropy`` mode are always printed for a range of temperatures.
          The respective temperatures can be specified with this option.
         
        :flag: ``--ptot <REAL>``
        :description:
           For the rovibrational average :math:`\overline{S}_{msRRHO}` requires frequency calculations
           at GFN level. To reduce computational cost, only the specified *REAL* fraction of structures
           are calculated, and the rest is averaged. The default is 0.9 (=90%).   
        
        :flag: ``--fscal <REAL>``
        :description: 
          Scale frequencies read for :math:`\overline{S}_{msRRHO}` by a given factor.
          Also works together with the ``--thermo`` option (see below, section Other Tools)

        :flag: ``--rotorcut,--sthr``
        :description:
          Specify the rotor cutoff for the ro/vib entropy interpolation (:math:`\tau`).
          Also works together with the ``--thermo`` option (see below, section Other Tools)




Options related to constrainment
--------------------------------
   
    :flag: ``--cinp <FILE>``
    :description:
      Specify a ``<FILE>`` with additional constraints in the xTB syntax.

    :flag: ``--constrain <atom list>``
    :description:
      Set up an example file in which the atoms in ``<atom list>`` shall be constrained.
      The file will be called ``.xcontrol.sample``. No calculations will be performed
      and the run is aborted after this sample is wirtten. The written file can be 
      read with the ``--cinp`` option.

    :flag: ``--cbonds [REAL]``
    :description:
      Set up a constraint on all bonds (as detected in the input coordinates topology),
      where ``[REAL]`` optionally can be used to set the force constant (default value 0.02 Eh)
 
    :flag: ``--cmetal [REAL]``
    :description:
      Set up a constraint on all M-X bonds (as detected in the input coordinates, M = transition metal atom),
      where ``[REAL]`` optionally can be used to set the force constant (default value 0.02 Eh)
    
    :flag: ``--cheavy [REAL]``
    :description:
      Set up a constraint on all heavy atom bonds (i.e., X-H bonds will be not constrained),
      where ``[REAL]`` optionally can be used to set the force constant (default value 0.02 Eh)   

    :flag: ``--clight [REAL]``
    :description:
      Set up a constraint on all X-H bonds (as detected in the input coordinates),
      where ``[REAL]`` optionally can be used to set the force constant (default value 0.02 Eh)   

    :flag: ``--fc <REAL``
    :description:
      Specifiy a force constant for the applied constaints (default value 0.02 Eh).
      Note: Only one force constant is applied for all constaints!



Other tools
===========

ZSORT z-matrix sorting tool
   :flag: ``--zsort``
   :description:
     The atom order of the given input file is sorted in order to yield a more consistent z-matrix,
     i.e., atoms are grouped together according to the molecular structure (e.g. methyl groups).

MDOPT parallel ensemble optimization
   :flag: ``--mdopt <FILE>``
   :description:
     Optimize each point on a given trajectory or ensemble file ``<FILE>`` with GFN\ *n*--xTB.

SCREEN ensemble screening tool
   :flag: ``--screen <FILE>``
   :description:
     Optimize each point on a given trajectory or ensemble file ``<FILE>`` with GFN\ *n*--xTB
     in a multilevel approach and sort the resulting ensemble (CREGEN).

Automated protonation site screening
    :flag: ``--protonate``
    :description:
      A tool that can be used to find protonation sites, i.e., the protomers of the input structure.
      In the approach first localized molecular orbitals (LMOs) are calculated and LP- and π-centers
      are identified. Then, a proton is added to each of these centers and the resulting structures are
      optimized and sorted.
    :modifier: ``--swel <STR>``
    :description:
      Change H\ :math:`^+` in the protonation tool to some other ion specified by *STR*.
      *STR* has to contain the element symbol AND charge, e.g. ``Na+`` or ``Ca2+``

Automated deprotonation site screening
    :flag: ``--deprotonate``
    :description:
      A tool to find deprotomers of the input structure. Each H atom is removed and the resulting 
      structures are optimized and sorted.

Automated tautomerization screening
    :flag: ``--tautomerize``
    :description:
      A tool that combines the ``-protonate`` and ``-deprotonate`` options to find (prototropic)
      tautomers of the input structure.
    :modifier: ``--trev`` 
    :description: Reverse order of operation for tautomerization mode, i.e., first deprotonate and then protonate.
    :modifier: ``--iter <INT>``
    :description: Number of Protonation/Deprotonation Iterations for ``-tautomerize`` mode. The ``[DEFAULT]`` is 2 iterations.


Ensemble comparison
     :flag: ``--compare <FILE1> <FILE2>``
     :description:
       Compare two ensembles *<FILE1>* and *<FILE2>*. Both ensembles must have the same
       order of atoms of the molecule and should contain rotamers (e.g. ``crest_rotamers.xyz``).
       Furthermore the structures should have energies in the same magnitude, or relative energies
       as the comment in the ensemble file.
     :modifier: ``--maxcomp <INT>``
     :description:
       Selcect the lowest *INT* conformers out of each ensemble to be compared with ``-compare``.
       The ``[DEFAULT]`` is the 10 lowest conformers.


Rovibrational entropy average
      :flag: ``--rrhoav <FILE>``
      :description:
        Calculate the :math:`\overline{S}_{msRRHO}` term for a specified ensemble.
        The ``--rrhoav`` option can be modified with the same settings as the entropy mode (e.g. ``--trange``).
      :modifier: ``--printpop``
      :description: 
        Write files with free energy Boltzmann populations for each ``--trange`` temperature into a 
        sperate directory called ``populations``.
 
RMSD comparison
      :flag: ``--rmsd,--rmsdheavy <FILE1> <FILE2>``
      :description: 
        Calculate the RMSD or heavy atom RMSD between two given structures.
        Input format of the two structures can be any of the formats that can be read by CREST, 
        output will always be the RMSD in Angstroem.

Topology check
       :flag: ``--testtopo <FILE>``
       :description: 
         Calculate the topology (neighbour lists) for a given input structure and print to info to screen.

Thermostatistical calculations from frequencies
       :flag: ``--thermo <FILE>``
       :description:
         Calculate thermo data for given structure. Also requires vibrational
         frequencies in the Turbomole format, saved as file called ``vibspectrum``.
       :modifiers: ``--fscal``, ``--rotorcut,--sthr`` (see above)
         
Splitting an Ensemble into seperate files
        :flag: ``--splitfile <FILE> [from] [to]``
        :description: 
          Split an ensemble from ``<FILE>`` into seperate directories for each structure. 
          ``[from]`` and ``[to]`` can be used to select specific structures from the file or
          a range of structures. The new directories are collected in the ``SPLIT`` directory.




