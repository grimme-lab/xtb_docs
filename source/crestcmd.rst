.. _crestcmd:

-------------------------------
 CREST command line arguments
-------------------------------

.. contents::


``CREST`` is usually invoked via command line, and requires only a coordinate input file.
The program supports the ``TURBOMOLE`` coordinates (coord, Bohr), Xmol (`*`.xyz, Ångström)
or 3d SDF (V2000,V3000) formats and can be called via

.. code:: bash

   > crest [INPUT] [OPTIONS]
   
If no file is given as ``[INPUT]``, then ``CREST`` automatically searches for a file called *coord*
in the ``TURBOMOLE`` format. Either must be present. The different ``[OPTIONS]`` are listed below and refer
to ``Version 2.11`` of the ``CREST`` code. ``[DEFAULT]`` options do not need to be specified explicitly.


General and technical options
-----------------------------

.. list-table:: 
    :widths: 30 100
    :header-rows: 1
    
    * - Flag
      - Description
    * - ``-h,-help``
      - Print an overview of most available options (i.e., this site)
    * - ``--version``
      - Print only the program header and disclaimer.
    * - ``--cite``
      - Print the most relevant citations.
    * - ``--xnam BIN``
      - Specify the name (and path) of the ``xtb`` binary that should be used as *BIN*. 
        The ``[DEFAULT]`` is *xtb*.
    * - ``--niceprint``
      - In-line progress bar printout for optimizations.
    * - ``--scratch <DIR>``
      - Performs the entire calculation in the specified ``<DIR>``. If ``<DIR>`` is not existing it will be created.
    * - ``--T <INT>``
      - Specify the number of CPU threads ``<INT>`` that shall be used. ``CREST`` automatically adjusts the number of processes according to this variable in each step, in order to achieve optimal parallelization of the calculations.
    * - ``--dry``
      - Perform a "dry" run, i.e., nothing is actually done but instead an overview of the settings that would be applied in the calculation is given.

Runtypes
========

Several different applications are available within the ``CREST`` program.
The most important usage are the two different conformational search algorithms MF-MD-GC and iMTD-GC,
but there are also some smaller utility tools that can be used, such as an CRE sorting function (CREGEN),
or a standalone z-matrix sorting function (ZSORT).
The different runtypes are:

.. list-table:: 
    :widths: 40 20 100
    :header-rows: 1
    
    * - Algorithm
      - Flag
      - Description
    * - MF-MD-GC algorithm (outdated)
      - ``--v1``
      - First generation of the GFN\ *n*-xTB driven conformational search algorithm, consisting out of mode following, molecular dynamics sampling and genetic structure crossing.
    * - MTD-GC algorithm (outdated)
      - ``--v2``
      - Second generation of the GFN\ *n*-xTB driven conformational search algorithm, consisting out of a meta-dynamics approach and genetic structure crossing.
    * - iMTD-GC algorithm ``[DEFAULT]``
      - ``--v2i, --v3``
      - Iterative version of the MTD-GC workflow, which is the default runtype of ``CREST``.
    * - iMTD-sMTD algorithm 
      - ``--v4``
      - Iterative workflow, making use of static metadynmics simulations.
    * - Conformational entropy algorithm 
      - ``--entropy``
      - Specialized version of the iMTD-sMTD workflow, specialized in the calculation of conformational entropy. 


Ensemble sorting
================

**CREGEN ensemble sorting tool for standalone use**

.. list-table:: 
    :widths: 30 100
    :header-rows: 1
    
    * - Flag
      - Description
    * - ``--cregen <FILE>``
      - Tool to sort a given ensemble ``<FILE>`` according to energy, atomic RMSD and rotational constant. A reference structure (e.g. *coord*) has to be provided.
    * - ``--ewin <REAL>``
      - Set the energy threshold to *REAL* kcal/mol. This affects several runtypes and the ``[DEFAULT]`` is depending on the application (6 kcal/mol conformational searches, 30 kcal/mol screening tools).
    * - ``--prsc``
      - Create a scoord.`*` file for each conformer in the ``TURBOMOLE`` format.
    * - ``--rthr <REAL>``
      - Set RMSD threshold in Ångström. The ``[DEFAULT]`` is 0.125 Å.
    * - ``--ethr <REAL>``
      - Set energy threshold between conformer pairs in kcal/mol. The ``[DEFAULT]`` is 0.05 kcal/mol.
    * - ``--bthr <REAL>``
      - Set lower bound for the rotational constant threshold to *REAL*. The ``[DEFAULT]`` is 0.01 (= 1%). The threshold is dynamically adjusted between this value and 2.5%, based on the anisotropy of the rotational constants.
    * - ``--pthr <REAL>``
      - Boltzmann population threshold. The ``[DEFAULT]`` is 0.05 (= 5%)
    * - ``--nmr, --eqv, --entropy``
      - Activate determination and printout of NMR-equivalencies. Writes the files ``anmr_rotamer`` and ``anmr_nucinfo``, which are required by the ``ENSO`` python script.
    * - ``--athr <REAL>``
      - Similarity threshold to determine internal rotation of equal atoms for NMR. The ``[DEFAULT]`` is 0.04.
    * - ``-temp <REAL>``
      - Set temperature for the calculation of Boltzmann weights. The ``[DEFAULT]`` is 298.15 K.
    * - ``--esort``
      - Sort only based on energy (i.e., no RMSD and rotational constant comparison)
    * - ``--nowr``
      - Don't write new ensemble files (crest_rotamers_`*`.xyz, crest_conformers.xyz)
    * - ``--subrmsd``
      - Compare only those parts of the structure that were also included in the metadynamics bias potential. Can be important for constrained conformational searches. 
    * - ``--notopo``
      - Turn off the initial topology check of the structures in the ensemble.

An extension to the CREGEN sorting is an automatic principle component analysis (PCA) and
k-Means sorting clustering algorithm. It can be invoked with the ``--cluster`` command.

.. list-table:: 
    :widths: 30 100
    :header-rows: 1
    
    * - Flag
      - Description
    * - ``--cluster <INT>``
      - Perform a clustering on the final CREGEN ensemble to identify *INT* most representative structures, based on dihedral angles. Note that this algorithm currently does not work well for non-covalent complexes or molecular clusters and should only be applied to singular molecules.


Options
-------

Calculation settings passed to ``xtb``
======================================

**Method selection**

.. list-table:: 
    :widths: 30 100
    :header-rows: 1
    
    * - Flag
      - Description
    * - ``--gfn1``
      - Use GFN1-xTB
    * - ``--gfn2``
      - Use GFN2-xTB ``[DEFAULT]``
    * - ``--gff, -gfnff``
      - Use GFN-FF (recommended for faster sampling)
    * - ``--gfn2//gfnff``
      - Use GFN2-xTB//GFN-FF composite method
      
**Charge and multiplicity**

.. list-table:: 
    :widths: 30 100
    :header-rows: 1
    
    * - Flag
      - Description
    * - ``--chrg <INT>``
      - Specify molecular charge as *INT*, overrides ``.CHRG`` file.
    * - ``--uhf <INT>``
      - Specify :math:`N_{\alpha}-N_{\beta}` as *INT*, overrides ``.UHF`` file

**Implicit solvation**

.. list-table:: 
    :widths: 30 100
    :header-rows: 1
    
    * - Flag
      - Description
    * - ``--g, --gbsa <SOLVENT>``
      - Generalized born (GB) model with solvent accessible surface (SASA) model, for available *SOLVENT* options see :ref:`gbsa`. The solvent input is not case-sensitive.
    * - ``--alpb <SOLVENT>``
      - New ALPB implicit solvation model, for available *SOLVENT* options see :ref:`gbsa`. The solvent input is not case-sensitive.

**Geometry optimization thresholds**

.. list-table:: 
    :widths: 30 100
    :header-rows: 1
    
    * - Flag
      - Description
    * - ``--opt <LEVEL>``
      - Set the optimization accuracy for final GFN\ *n*--xTB optimizations. See :ref:`geometry optimization` for valid *LEVEL* arguments. The ``[DEFAULT]`` is *vtight*.
      
Options related to molecular dynamics and metadynamics simulations
==================================================================

.. list-table:: 
    :widths: 30 100
    :header-rows: 1
    
    * - Flag
      - Description
    * - ``--mdlen, --len <REAL>``
      - The length of the metadynamics simulations (MTD) in CREST is usually determined automatically, but with this flag it can be set to *REAL* (in ps). It is also possible to set a multiple of the automatically determined length by using ``x<REAL>`` instead, where *REAL* then is a multiplicative factor (e.g. *x0.5* for half the default simulation length).
    * - ``--shake <INT>``
      - Set SHAKE mode for MD. *INT* can be 0(= off), 1(= H-only), or 2(= all bonds). The ``[DEFAULT]`` is 2.
    * - ``--tstep <INT>``
      - Set MD time step to *INT* fs. The ``[DEFAULT]`` is 5 fs for GFNn-xTB calculations (requires SHAKE), and 1.5 fs for GFN-FF. The timestep is also automatically checked with a trial simulation at the beginning of the conformational search.
    * - ``--mddump <INT>``
      - Set dumpstep in which coordinates are written to the trajectory file to *INT* fs. The ``[DEFAULT]`` is 100 fs.
    * - ``--vbdump <REAL>``
      - Set dump frequency in which a new reference structure is taken for :math:`V_{bias}` to *REAL* ps. The ``[DEFAULT]`` is 1.0 ps.

Options for conformational search algorithms
============================================

**Z-matrix sorting (see also** ``--zsort`` **above)**

.. list-table:: 
    :widths: 30 100
    :header-rows: 1
    
    * - Flag
      - Description
    * - ``--zs``
      - Perform z-matrix sorting (i.e. ZSORT) for the input coordinate file.
    * - ``--nozs``
      - ``[DEFAULT]`` Do not perform z-matrix sorting of the input file.

**Genetic Z-matrix crossing**

.. list-table:: 
    :widths: 30 100
    :header-rows: 1
    
    * - Flag
      - Description
    * - ``--cross``
      - ``[DEFAULT]`` Perform Z-matrix structure crossing (GC) in the algorithm.
    * - ``--nocross``                                                                        
      - Skip Z-matrix structure crossing.

**Additional MD sampling after MTD**

.. list-table:: 
    :widths: 30 100
    :header-rows: 1
    
    * - Flag
      - Description
    * - ``--norotmd``           
      - Turn off the additional  MDs on the lowest conformers after the MTD step.
    * - ``--tnmd <REAL>``
      - Set temperature for the additional normal MDs on the lowest conformers after the MTD step. The ``[DEFAULT]`` is 400 K.

**Adjusting iterative behavior of iMTD-GC**

.. list-table:: 
    :widths: 30 100
    :header-rows: 1
    
    * - Flag
      - Description
    * - ``--mrest <INT>``
      - Maximum number of MTD restarts in iMTD-GC algorithm. The ``[DEFAULT]`` is 5 cycles.

**Special settings for the iMTD-GC workflow**

.. list-table:: 
    :widths: 30 100
    :header-rows: 1
    
    * - Flag
      - Description
    * - ``--hflip, --noflip``
      - Turn a small enhancement routine on/off to rotate OH groups after MTD. The ``[DEFAULT]`` is OFF.
    * - ``--maxflip <INT>```
      - Maximum number of new structures by the above mentioned enhancement routine. The ``[DEFAULT]`` is OFF.
    * - ``--quick``
      - Perform a search with reduced settings for a crude conformer ensemble.
    * - ``--squick, --superquick``
      - Perform an even more crude conformational search than with ``-quick``.
    * - ``--mquick``
      - Perform an even more crude conformational search than with ``-quick`` or ``-squick``.
    * - ``--origin``
      - Track the step of generation for each conformer/rotamer. ``[DEFAULT]`` 
    * - ``--nci``
      - Specialized NCI mode that can be used to find aggregates of NCI complexes. The option generates an ellipsoid potential around the input structure and adds it to the MTD simulation. Also, settings for :math:`k` and :math:`\alpha` are adjusted and some settings are reduced, in order to achieve lower computation times.
    * - ``--wscal <REAL>``
      - Scale the ellipsoid potential axes in the ``--nci`` mode by factor *REAL*.

**Technical iMTD-GC settings**

.. list-table:: 
    :widths: 30 100
    :header-rows: 1
    
    * - Flag
      - Description
    * - ``--keepdir``
      - Keep sub-directories of the conformer production run.


**Property mode appendix**

.. list-table:: 
    :widths: 30 100
    :header-rows: 1

    * - Flag
      - Description
    * - ``--prop <STR>``
      - This initializes the usage of the "property" mode as an appendix to the regular conformational search. *STR* defines what shall be done with the ensemble.
    * - ``--for,--forall <FILE>``
      - Instead of starting the property calculation on the final conformer ensemble file after iMTD-GC the property mode can directly be started for a given input ensemble <FILE> in the Xmol (`*`.xyz) format.
      
Valid options for *STR* are currently (case sensitive!):

.. list-table:: 
    :widths: 30 100
    :header-rows: 1

    * - Option
      - Description    
    * - ``hess``  
      - performs a hessian calculation for all conformers and re-weights the ensemble on free energies
    * - ``reopt`` 
      - reoptimization of the ensemble with vtight thresholds (useful for "quick" runs)
    * - ``autoIR`` 
      - calculate vib. modes for all conformers and average them (weighted by Boltzmann populations) in a single "crest.vibspectrum" file.
      
.. note:: The different quick, NCI and property settings are incompatible with ``--entropy``!

.. _entropymodesettings:

**Entropy mode settings**

.. list-table:: 
    :widths: 30 100
    :header-rows: 1
    
    * - Flag
      - Description
    * - ``--scthr,--entropy_cthr <REAL>``
      - Specify the ensemble growth threshold (% new conformers) for ``--entropy`` and ``--v4`` convergence. The default is 0.02 (=2%) for the entropy mode and  0.05 (=5%) for ``--v4``.
    * - ``--ssthr,--entropy_sthr <REAL>``
      - Specify the entropy growth threshold (% growth entropy) for ``--entropy`` and ``--v4`` convergence. The default is 0.005 (=0.5%) for the entropy mode and 0.01 (=1%) for ``--v4``. 
    * - ``--trange <from> <to> <step>``
      - Entropies from the ``--entropy`` mode are always printed for a range of temperatures. The respective temperatures can be specified with this option.
    * - ``--ptot <REAL>``
      - For the rovibrational average :math:`\overline{S}_{msRRHO}` requires frequency calculations at GFN level. To reduce computational cost, only the specified *REAL* fraction of structures are calculated, and the rest is averaged. The default is 0.9 (=90%).   
    * - ``--fscal <FLOAT>``
      - Scale frequencies read for :math:`\overline{S}_{msRRHO}` by a given factor. Also works together with the ``--thermo`` option (see below, section :ref:`othertools`). The ``[DEFAULT]`` is 1.0.
    * - ``--rotorcut, --sthr <FLOAT>``
      - Specify the rotor cutoff for the ro/vib entropy interpolation (:math:`\tau`). Also works together with the ``--thermo`` option (see below, section :ref:`othertools`). The ``[DEFAULT]`` is 25.0 cm-1.
    * - ``--ptot <FLOAT>``
      - Sum of population for structures considered in msRRHO average. The ``[DEFAULT]`` is 0.9 (=90%). (?)

Options related to constrainment
================================

.. list-table:: 
    :widths: 30 100
    :header-rows: 1
    
    * - Flag
      - Description   
    * - ``--cinp <FILE>``
      - Specify a ``<FILE>`` with additional constraints in the xTB syntax.
    * -  ``--constrain <atom list>``
      - Set up an example file in which the atoms in ``<atom list>`` shall be constrained. The file will be called ``.xcontrol.sample``. No calculations will be performed and the run is aborted after this sample is written. The written file can be read with the ``--cinp`` option.
    * -  ``--cbonds [REAL]``
      - Set up a constraint on all bonds (as detected in the input coordinates topology), where ``[REAL]`` optionally can be used to set the force constant (default value 0.02 Eh)
    * -  ``--cbonds [REAL]``
      - Turn off ``-cbonds`` (mainly for GFN-FF)
    * -  ``--cmetal [REAL]``
      - Set up a constraint on all M-X bonds (as detected in the input coordinates, M = transition metal atom), where ``[REAL]`` optionally can be used to set the force constant (default value 0.02 Eh)
    * -  ``--cheavy [REAL]``
      - Set up a constraint on all heavy atom bonds (i.e., X-H bonds will be not constrained), where ``[REAL]`` optionally can be used to set the force constant (default value 0.02 Eh)   
    * -  ``--clight [REAL]``
      - Set up a constraint on all X-H bonds (as detected in the input coordinates), where ``[REAL]`` optionally can be used to set the force constant (default value 0.02 Eh)   
    * -  ``--fc <REAL``
      - Specify a force constant for the applied constraints (default value 0.02 Eh). Note: Only one force constant is applied for all constraints!

.. _othertools:

Other tools
-----------

ZSORT z-matrix sorting tool

.. list-table:: 
    :widths: 30 100
    :header-rows: 1
    
    * - Flag
      - Description  
    * -  ``--zsort``
      - The atom order of the given input file is sorted in order to yield a more consistent z-matrix, i.e., atoms are grouped together according to the molecular structure (e.g. methyl groups).

MDOPT parallel ensemble optimization

.. list-table:: 
    :widths: 30 100
    :header-rows: 1
    
    * - Flag
      - Description  
    * -  ``--mdopt <FILE>``
      - Optimize each point on a given trajectory or ensemble file ``<FILE>`` with GFN\ *n*--xTB.

SCREEN ensemble screening tool

.. list-table:: 
    :widths: 30 100
    :header-rows: 1
    
    * - Flag
      - Description  
    * -  ``--screen <FILE>``
      - Optimize each point on a given trajectory or ensemble file ``<FILE>`` with GFN\ *n*--xTB in a multilevel approach and sort the resulting ensemble (CREGEN).

Automated protonation site screening

.. list-table:: 
    :widths: 30 100
    :header-rows: 1
    
    * - Flag
      - Description  
    * -  ``--protonate``
      - A tool that can be used to find protonation sites, i.e., the protomers of the input structure. In the approach first localized molecular orbitals (LMOs) are calculated and LP- and π-centers are identified. Then, a proton is added to each of these centers and the resulting structures are optimized and sorted.

.. list-table:: 
    :widths: 30 100
    :header-rows: 1
    
    * - Modifier
      - Description        
    * - ``--swel <STR>``
      - Change H\ :math:`^+` in the protonation tool to some other ion specified by *STR*. *STR* has to contain the element symbol AND charge, e.g. ``Na+`` or ``Ca2+``

Automated deprotonation site screening

.. list-table:: 
    :widths: 30 100
    :header-rows: 1
    
    * - Flag
      - Description  
    * -  ``--deprotonate``
      - A tool to find deprotomers of the input structure. Each H atom is removed and the resulting structures are optimized and sorted.

Automated tautomerization screening

.. list-table:: 
    :widths: 30 100
    :header-rows: 1
    
    * - Flag
      - Description  
    * -  ``--tautomerize``
      - A tool that combines the ``-protonate`` and ``-deprotonate`` options to find (prototropic) tautomers of the input structure.
      
.. list-table:: 
    :widths: 30 100
    :header-rows: 1
    
    * - Modifier
      - Description  
    * - ``--trev`` 
      - Reverse order of operation for tautomerization mode, i.e., first deprotonate and then protonate.
    * - ``--iter <INT>``
      - Number of Protonation/Deprotonation Iterations for ``-tautomerize`` mode. The ``[DEFAULT]`` is 2 iterations.


Ensemble comparison

.. list-table:: 
    :widths: 30 100
    :header-rows: 1
    
    * - Flag
      - Description  
    * -  ``--compare <FILE1> <FILE2>``
      - Compare two ensembles *<FILE1>* and *<FILE2>*. Both ensembles must have the same order of atoms of the molecule and should contain rotamers (e.g. ``crest_rotamers.xyz``). Furthermore, the structures should have energies in the same magnitude, or relative energies as the comment in the ensemble file.
      
.. list-table:: 
    :widths: 30 100
    :header-rows: 1
    
    * - Modifier
      - Description        
    * - ``--maxcomp <INT>``
      - Select the lowest *INT* conformers out of each ensemble to be compared with ``-compare``. The ``[DEFAULT]`` is the 10 lowest conformers.


Rovibrational entropy average

.. list-table:: 
    :widths: 30 100
    :header-rows: 1
    
    * - Flag
      - Description  
    * -  ``--rrhoav <FILE>``
      - Calculate the :math:`\overline{S}_{msRRHO}` term for a specified ensemble. The ``--rrhoav`` option can be modified with the same settings as the entropy mode (e.g. ``--trange``).

.. list-table:: 
    :widths: 30 100
    :header-rows: 1
    
    * - Modifier
      - Description  
    * - ``--printpop``
      - Write files with free energy Boltzmann populations for each ``--trange`` temperature into a  separate directory called ``populations``.
 
RMSD comparison

.. list-table:: 
    :widths: 30 100
    :header-rows: 1
    
    * - Flag
      - Description  
    * -  ``--rmsd,--rmsdheavy <FILE1> <FILE2>``
      - Calculate the RMSD or heavy atom RMSD between two given structures. Input format of the two structures can be any of the formats that can be read by CREST, output will always be the RMSD in Ångström.

Topology check

.. list-table:: 
    :widths: 30 100
    :header-rows: 1
    
    * - Flag
      - Description  
    * -  ``--testtopo <FILE>``
      - Calculate the topology (neighbour lists) for a given input structure and print to info to screen.

Thermostatistical calculations from frequencies

.. list-table:: 
    :widths: 30 100
    :header-rows: 1
    
    * - Flag
      - Description  
    * -  ``--thermo <FILE>``
      - Calculate thermo data for given structure. Also requires vibrational frequencies in the Turbomole format, saved as file called ``vibspectrum``.

.. list-table:: 
    :widths: 30 100
    :header-rows: 1
    
    * - Modifier
      - Description  
    * - ``--fscal``, ``--rotorcut,--sthr`` 
      - see :ref:`Entropy mode settings<entropymodesettings>`
         
Splitting an Ensemble into separate files

.. list-table:: 
    :widths: 30 100
    :header-rows: 1
    
    * - Flag
      - Description  
    * -  ``--splitfile <FILE> [from] [to]``
      - Split an ensemble from ``<FILE>`` into separate directories for each structure.  ``[from]`` and ``[to]`` can be used to select specific structures from the file or a range of structures. The new directories are collected in the ``SPLIT`` directory.




