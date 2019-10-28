.. _crestxmpl:

--------------------------------------------------
Example applications
--------------------------------------------------

.. contents::

iMTD-GC conformational search
=============================

The default application of ``CREST`` is the iMTD-GC conformational search as described in section :ref:`crest`.
In the following a standard production run with this workflow is shown for the alanineglycine molecule.
The structure is given as:

.. code:: bash

    > cat struc.xyz
      20
                                         
    C     2.081440     0.615100    -0.508430
    C     2.742230     1.824030    -1.200820
    N     4.117790     1.799870    -1.190410
    C     4.943570     2.827040    -1.822060
    C     6.440080     2.569360    -1.637600
    O     7.351600     3.252270    -2.069090
    N     0.610100     0.695090    -0.538780
    O     2.095560     2.724940    -1.739670
    O     6.705220     1.463410    -0.897460
    H     0.303080     1.426060     0.103770
    H     0.338420     1.050680    -1.460480
    C     2.488753    -0.593400    -1.198448
    H     2.416500     0.557400     0.532050
    H     4.614100     1.081980    -0.670550
    H     4.699850     3.794460    -1.373720
    H     4.722890     2.844690    -2.894180
    H     7.687400     1.448620    -0.860340
    H     2.029201    -1.457008    -0.719999
    H     2.170233    -0.542411    -2.238576
    H     3.572730    -0.688405    -1.154998

.. figure:: ../figures/alagly.png
   :scale: 35 %
   :alt: alagly
   
   Input structure of the alanineglycine molecule.

Let's assume that we are interested in the conformations of Ala-Gly at the GFN2-xTB level with GBSA implicit solvation
for water, and that we are using 4 CPUs. 
Then the command to invoke the conformational search would be:

.. code:: bash

    > crest struc.xyz -gfn2 -g h2o -T 4


.. tip:: It usually is wise to preoptimze your input structure with ``xtb`` at the same level on which
         the conformational search shall be conducted. Since the input structure is taken as a reference
         for several sanity checks within the *CREGEN* routine, such as unchanging coordination numbers
         of the atoms, providing a structure on the same level of theory is recommended.

The program call first creates a ``coord`` file from the given input structure and sorts the z-matrix (*ZSORT*).
Then the length of the MTD simulation is determined and the algorithm is started.
The following output can be expected (some printout was discarded for the documentation):

.. code-block:: text

        ==============================================
        |                                            |
        |                 C R E S T                  |
        |                                            |
        |  Conformer-Rotamer Ensemble Sampling Tool  |
        |        based on the GFN-xTB method         |
        |             S.Grimme, P.Pracht             |
        |          Universitaet Bonn, MCTC           |
        ==============================================
        Version 2.7.0, Mon 24. Jun 11:41:02 CEST 2019
        Using the GFN-xTB code.
        Compatible with XTB version 6.1 and later.
 
 -------------------------
 Starting z-matrix sorting
 -------------------------
  total number of atoms :          20
  total number of frags :           1
  terminated normally
 
 ------------------------------------------------
 Generating MTD length from a flexibility measure
 ------------------------------------------------
  Calculating WBOs... done.
  flexibility measure :   0.821
 
 -------------------------------------
 Starting a trial MTD to test settings
 -------------------------------------
  Success!
  Estimated runtime for one MTD (5.0 ps) on a single thread: 16 sec
  Estimated runtime for a batch of 14 MTDs on 4 threads: 1 min 4 sec

 *******************************************************************************************
 **                        N E W    I T E R A T I O N    C Y C L E                        **
 *******************************************************************************************
 
 ========================================
             MTD Iteration  1
 ========================================
 
      ========================================
      |         Meta-MD (MTD) Sampling       |
      ========================================
 <.......>
 <.......>

 -----------------------
 Multilevel Optimization
 -----------------------
 
  -------------------------
  1. crude pre-optimization
  -------------------------
  writing TMPCONF* Dirs from file "crest_rotamers_0.xyz" ... done.
  Starting optimization of generated structures
 <.......>
  353 structures remain within    12.00 kcal/mol window
 
  -------------------------------------
  2. optimization with tight thresholds
  -------------------------------------
  writing TMPCONF* Dirs from file "crest_rotamers_1.xyz" ... done.
  Starting optimization of generated structures
 <.......>
  90 structures remain within     6.00 kcal/mol window

 ========================================
             MTD Iteration  2
 ========================================
 <.......>
 <.......>

 ========================================
             MTD Iterations done
 ========================================
  Collecting ensmbles.
  running RMSDs... done.
  E lowest :   -33.88024
  132 structures remain within     6.00 kcal/mol window

 -----------------------------------------------
 Additional regular MDs on lowest 4 conformer(s)
 -----------------------------------------------
 <.......>
 Appending file crest_rotamers_1.xyz with new structures
 
  -------------------------------------------
  Ensemble optimization with tight thresholds
  -------------------------------------------
  writing TMPCONF* Dirs from file "crest_rotamers_1.xyz" ... done.
  Starting optimization of generated structures
 <.......>
  136 structures remain within     6.00 kcal/mol window

      ========================================
      |        Structure Crossing (GC)       |
      ========================================
  input  file name : crest_rotamers_3.xyz
 number of atoms                :    20
 number of points on xyz files  :   136
 conformer energy window  /kcal :    6.00
 CN per atom difference cut-off :  0.3000
 RMSD threshold                 :  0.2500
 max. # of generated structures :   250
  reading xyz file ...
  # in E window                136
  generating pairs ...        9315
   91.2 % done
  generated pairs           :        7838
  number of clash discarded :        1342
  average rmsd w.r.t input  : 2.82902
  sd of ensemble            : 0.63747
  number of new structures      :         116
  removed identical structures  :         384
 <.......>
 <.......>

    ================================================
    |           Final Geometry Optimization        |
    ================================================
  ---------------------
  Ensemble optimization
  ---------------------
  writing TMPCONF* Dirs from file "crest_rotamers_4.xyz" ... done.
  Starting optimization of generated structures
  126 structures remain within     6.00 kcal/mol window

 -------------------------------------
 CREGEN - CONFORMER SYMMETRY ANALYSIS
 -------------------------------------
  input  file name : crest_rotamers_5.xyz
  output file name : crest_rotamers_6.xyz
  number of atoms                :    20
  number of points on xyz files  :   159
  RMSD threshold                 :   0.1250
  Bconst threshold               :   0.0200
  population threshold           :   0.0500
  conformer energy window  /kcal :   6.0000
  # fragment in coord            :     1
  number of reliable points      :   159
  reference state Etot :  -33.8802301686000
  number of doubles removed by rot/RMSD         :          33
  total number unique points considered further :         126
    Erel/kcal    Etot      weight/tot conformer  set degen    origin
     1   0.000  -33.88023    0.04725    0.28280    1    6     mtd10
     2   0.000  -33.88023    0.04725                          md1
     3   0.000  -33.88023    0.04724                          mtd1
     4   0.001  -33.88023    0.04718                          gc
     5   0.003  -33.88022    0.04698                          md3
     6   0.005  -33.88022    0.04689                          gc
     7   0.043  -33.88016    0.04392    0.17556    2    4     md5
     8   0.043  -33.88016    0.04391                          mtd10
     9   0.044  -33.88016    0.04391                          mtd9
    10   0.045  -33.88016    0.04383                          mtd2
    11   0.477  -33.87947    0.02116    0.06323    3    3     mtd5
    12   0.478  -33.87947    0.02112                          md6
    13   0.482  -33.87946    0.02096                          mtd9
    14 .....
    15 .....
 .......
 .......
 CREST terminated normally.


The production run yields 126 structures of Ala-Gly, distributed over 51 different conformers within 6 kcal/mol above the 
lowest conformer that was found at the GFN2-xTB level.

.. figure:: ../figures/alaglyconfs.png
   :scale: 25 %
   :alt: alaglyconf
   
   Three lowest conformers of alanineglycine generated by CREST at the GFN2-xTB level.

The final ensemble of all the found conformers is written to an ensemble file in the Xmol format called ``crest_conformers.xyz``.
The corresponding CRE, i.e., the ensemble containing also the rotamers is written to the file ``crest_rotamers_X.xyz``, where *X* denotes
the highest number of the present files (usually ``crest_rotamers_6.xyz``).


MF-MD-GC conformational search
==============================

To use the old MF-MD-GC algorithm (which was implementet in a small tool called ``confscript``) the flag ``-v1`` can be used.
In the following example we conduct this conformational search, again for alanineglycine, using GFN1-xTB and GBSA implicit solvation
for CHCl\ :math:`_3`. The command is:

.. code:: bash

    > crest struc.xyz -v1 -gfn1 -g chcl3 -T 4

The written files are the same as with the iMTD-GC conformational search.

.. note:: The MTD-GC workflow was designed to find low lying conformers more efficiently and more safely than the older MF-MD-GC algorithm.
          Hence it is not recommended to use this search mode.

Sorting an ensemble
===================

The *CREGEN* routine that is used within the conformational search can also be used as an standalone tool.
To use this you can simply call the routine by:

.. code:: bash
   
    > crest struc.xyz -cregen ensemble.xyz

Here ``ensemble.xyz`` is the ensemble file that contains all the structures in the Xmol format.

.. note:: It is required to present a single reference structure (``struc.xyz`` in the example above) of the molecule to check for
          CN clashes. Also, all structurues in the ensemble must have the same atom order.


Comparing two ensemble
======================

Two ensembles generated on different levels of theory can be compared with the ``-compare`` option.
Let's assume that there are two ensembles ``v1.xyz``, generated with the MF-MD-GC procedure and ``v2.xyz``,
generated with the default iMTD-GC workflow.
To compare the 5 lowest conformers of each ensemble simply call:

.. code:: bash
  
    > crest struc.xyz -compare v1.xyz v2.xyz -maxcomp 5

Which produces the output:

.. code-block:: text

        ==============================================
        |                                            |
        |                 C R E S T                  |
        |                                            |
        |  Conformer-Rotamer Ensemble Sampling Tool  |
        |        based on the GFN-xTB method         |
        |             S.Grimme, P.Pracht             |
        |          Universitaet Bonn, MCTC           |
        ==============================================
        Version 2.7, Thu 27. Jun 13:41:37 CEST 2019
        Using the GFN-xTB code.
        Compatible with XTB version 6.1 and later.
  
  ---------------------
  Sorting file <v1.xyz>
  ---------------------
  running RMSDs... done.
   File <v1.xyz> contains 240 conformers.
   The 5 lowest conformers will be taken for the comparison:
   conformer  #rotamers
         1          1
         2          5
         3          3
         4          1
         5          2
  
  ---------------------
  Sorting file <v2.xyz>
  ---------------------
  running RMSDs... done.
   File <v2.xyz> contains 51 conformers.
   The 5 lowest conformers will be taken for the comparison:
   conformer  #rotamers
         1          6
         2          4
         3          3
         4          6
         5          4
  
  -----------------------
  Comparing the Ensembles
  -----------------------
  Calculating RMSDs between conformers... done.
  RMSD threshold:  0.1250 Å
  
  RMSD matrix:
   conformer          1          2          3          4          5 
      1         0.01727    1.44147    1.56327    0.81845    0.83933 
      2         0.00791    1.43084    1.56995    0.79512    0.83992 
      3         1.43350    0.01254    0.80724    1.58138    1.59243 
      4         0.12794    1.40597    1.54663    0.89315    0.83634 
      5         0.14626    1.51398    1.56167    0.68473    0.88006 
  
  --------------------------------
  Correlation between Conformers :
  --------------------------------
     #     Ensemble A             #    Ensemble B
                                  5     -33.87887
                                  4     -33.87937
                                  3     -33.87947
     5      -33.88008
     4      -33.88011
     3      -33.88017   <---->    2     -33.88016
     2      -33.88023   <---->    1     -33.88023
     1      -33.88023
  
  -----------------
  Wall Time Summary
  -----------------
 --------------------
 Overall wall time  : 0h : 0m : 0s
  
  CREST terminated normally.

From  the output it can be seen that there is a correlation between the lowest conformers,
i.e., the lowest conformers were found by both workflows.
As the display options in the terminal are limited, an addtional file called ``rmsdmatch.dat`` is written,
from which the exact correlation between the conformers of the two ensembles can be read.
If, for example, two different levels of theory are used and the energies of the molecules in both ensembles
are too different, then the output will not be of much use and one must refer to the ``rmsdmatch.dat`` file.

.. code:: bash

    > cat rmsdmatch.dat
           1     1
           2     1
           3     2


Each line in this file consists of only two values *a* and *b* which denote that conformer *a* from ensemble *A* matches
conformer *b* from ensemble *B*.
In the example case shown above, the MF-MD-GC produced the lowest conformer twice, which both naturally match conformer 1 from
the iMTD-GC procedure. The second conformer also is the same in both ensembles.

.. note:: In order for the comparison to work, both ensembles **must** have the same number of atoms with the same
          atom order in each structure. Furthermore the ensembles should be full CREs, i.e., rotamers should be present.



Constrained conformational sampling
===================================

.. warning:: The following application is still under development and should be considered
          an experimental feature.

It is possible to include additional constraints to all ``xtb`` calculations 
that are conducted by ``CREST``. To do this one has to create a file called
``.constrains`` (or ``.xcontrol``, both is valid) in the working directory, which contains the constraints
in the exact same syntax as used by the ``xtb`` (see section :ref:`detailed-input`)
Constraints that are included via the ``.constrains`` file will be included in *ALL* calculations
of the conformer search run.
To circumvent name conventions a constrainement file under arbitrary name can directly be provided
by the ``-cinp <FILE>`` option.
Since this can overwrite settings created by ``CREST`` it should only be used very cautiously!

The main application for the additional constraints is the constrainment (fixing) of atoms,
which could for example be used to sample only conformations for parts of a molecule.
Another use could be the sampling of conformers for the transition state of an reaction.

To fix atoms it is also recommended to use an reference input file additionally to the 
normal structure input file, which is done with the argument ``reference=FILE`` in the ``.xcontrol`` file.
Furthermore, fixed atoms should not be included in the RMSD of the MTD collective variables.

The content of the ``.xcontrol`` file for fixing atoms should look like the following example:

.. code:: bash

    > cat .xcontrol
    $constrain
      atoms: 4,8,10,12            # atoms 4, 8, 10 and 12 of some example molecule shall be constrained
      force constant=0.5
      reference=coord.original    # name of the reference file (just a copy of the input coord-file)
    $metadyn
      atoms: 1-3,5-7,9,11         # atoms *included* to RMSD in the MTD (typically NOT the constrained atoms)
    $end

This should ensure correct constrainment (as far as possible) in the MTD, as well as in the GFN\ *n*-xTB geometry
optimization within a ``CREST`` run.

It is also possible to let ``CREST`` generate such a file automatically.
To do this the list of atoms has to be provided with the flag ``--constrain <atom list>``, i.e.,

.. code:: bash

    > crest coord --constrain <atom list>

which will **not** start any calculation but instead write a file ``.xcontrol.sample`` that could subsequentially be used.
Furthermore the file ``coord.ref`` will be created. (e.g. for a molecule with 65 atoms):

.. code:: text

    > crest coord --constrain 1,2,3,26-30
     
           ==============================================
           |                                            |
           |                 C R E S T                  |
           |                                            |
           |  Conformer-Rotamer Ensemble Sampling Tool  |
           |        based on the GFN-xTB method         |
           |             P.Pracht, S.Grimme             |
           |          Universitaet Bonn, MCTC           |
           ==============================================
           Version 2.8, Fri 25. Oct 12:04:52 CEST 2019
           Using the GFN-xTB code.
           Compatible with XTB version 6.1 and later.
    
     Command line input:
     > crest --constrain 1,2,3,26-30
    
     Input list of atoms: 1,2,3,26-30
     8 of 65 atoms will be constrained.
     A reference coord file coord.ref was created.
     The following will be written to <.xcontrol.sample>:
    
     > $constrain
     >   atoms: 1-3,26-30
     >   force constant=0.5
     >   reference=coord.ref
     > $metadyn
     >   atoms: 4-25,31-65
     > $end
     
    <.xcontrol.sample> written. exit.

.. note:: Important: <atom list> must not contain any blanks and atoms must be seperated by comma. Ranges (e.g. 26-30) are allowed.


Sampling of noncovalent complexes and aggregates (NCI mode)
===========================================================

A specialized application of ``CREST`` is the sampling of aggregates (also refered to as NCI mode).
The idea here is to find different conformations of non-covalently bound complexes in which the 
arrangement of the fragments is of interest.
The application can be called by:

.. code:: bash

    > crest struc.xyz -nci

The procedure and output is essentially the same as a normal iMTD-GC production run, but with reduced settings
(less MTDs, different :math:`k` and :math:`\alpha`), and no genetic structure crossing.
What is different, however, is that first a ellipsoide wall potential is created and added to the meta-dynamics.
A nice example for this application are small molecular clusters, e.g. (H\ :sub:`2`\ O)\ :sub:`6`.
The ellipsoide potential that is automatically determined for the input cluster is visualized in the figure below.

.. figure:: ../figures/wclustpot.png
   :scale: 30 %
   :alt: wclustpot
   
   Visualization of an ellipsoide potential around (H\ :sub:`2`\ O)\ :sub:`6` cluster.

The ellipsoide potential is required in the MTDs to counteract the bias potential, which would simply lead to a
dissociation of the NCI complex after a few pico seconds (due to the maximization of the RMSD).
In the subsequent geometry optimization, however, the surrounding potential must not be present since the bias potential
is also not there and the structure would be artificially compressed by the ellipsoide. Hence it is automatically removed in 
the geometry optimizations

.. note:: The ellipsoide potential can be scaled by the factor *REAL*  with the flag ``-wscal REAL``.

Many new clusters are generated even for small NCI complexes, typically much more than conformers are generated for a single medium sized molecule.
In general, the task of finding new low lying aggregates is much more challenging than finding (only) conformers, since each fragment of
the complex could also have several different low lying conformations.
For the (H\ :sub:`2`\ O)\ :sub:`6` cluster 3 examples are shown in the figure below. Note that all three structures are also part of the
well established WATER27 benchmark set, but were generated automatically by ``CREST`` from a single input structure. In total 69 different clusters were
found of which only 3 are shown.

.. figure:: ../figures/wclust1.png
   :scale: 30 %
   :alt: wclust1
   
   Three automatically generated structures for a (H\ :sub:`2`\ O)\ :sub:`6` cluster.


Molecular prototropy screening
==============================

Protonation site screening
--------------------------
The screening for possible protonation sites, i.e., for the different protomers of an molecule is possible
by using a localized molecular orbital LMO approach. Herein, first the :math:`\pi`- and LP-centers are determined by a GFNn-xTB
calculation, and then all possible input structures are generated where a proton is placed at one of these centers.
This procedure was first described in *J. Comput. Chem.*, **2017**, *38*, 2618–2631.

The example calculation is performed for alanineglycine, in the gas phase, with the command

.. code:: bash

    > crest struc.xyz -protonate

Which returns the following output:

.. code-block:: text

        ==============================================
        |                                            |
        |                 C R E S T                  |
        |                                            |
        |  Conformer-Rotamer Ensemble Sampling Tool  |
        |        based on the GFN-xTB method         |
        |             S.Grimme, P.Pracht             |
        |          Universitaet Bonn, MCTC           |
        ==============================================
        Version 2.7.0, Mon 24. Jun 11:41:02 CEST 2019
        Using the GFN-xTB code.
        Compatible with XTB version 6.1 and later.
 
         __________________________________________
        |                                          |
        |       automated protonation script       |
        |__________________________________________|
  
  LMO calculation ... done.
  
 -----------------------
 Multilevel Optimization
 -----------------------
  -------------------------
  1. crude pre-optimization
  -------------------------
  writing TMPCONF* Dirs from file "protonate_0.xyz" ... done.
  Starting optimization of generated structures
 <.......>
  Now appending opt.xyz file with new structures
  12 structures remain within    90.00 kcal/mol window
  
  ---------------------
  2. loose optimization
  ---------------------
  writing TMPCONF* Dirs from file "protonate_1.xyz" ... done.
  Starting optimization of generated structures
 <.......>
  Now appending opt.xyz file with new structures
  Structures sorted out due to dissociation:    1
  11 structures remain within    60.00 kcal/mol window
  
  --------------------------------------------
  3. optimization with user-defined thresholds
  --------------------------------------------
  writing TMPCONF* Dirs from file "protonate_2.xyz" ... done.
  Starting optimization of generated structures
 <.......>
  Now appending opt.xyz file with new structures
  9 structures remain within    30.00 kcal/mol window
  
  ===================================================
  Identifying topologically equivalent structures:
  Equivalent to 1. structure: 2 structure(s).
  Equivalent to 3. structure: 5 structure(s).
  Equivalent to 5. structure: 2 structure(s).
  Done.
  Appending file <protonated.xyz> with structures.
  
  Initial 9 structures from file protonate_3.xyz have
  been reduced to 3 topologically unique structures.
  
 ===================================================
 ============= ordered structure list ==============
 ===================================================
  written to file <protonated.xyz>
  
  structure    ΔE(kcal/mol)   Etot(Eh)
     1            0.00        -33.964453
     2            3.51        -33.958853
     3            5.75        -33.955296
  
  
  -----------------
  Wall Time Summary
  -----------------
            LMO calc. wall time :         0h : 0m : 0s
       multilevel OPT wall time :         0h : 0m : 3s
 --------------------
 Overall wall time  : 0h : 0m : 4s
  
  CREST terminated normally.

As one can see from the output, three possible protomers of alanineglycine were found at the GFN2-xTB level (within the default
30 kcal/mol energy window around the most stable protomer). This ensemble of structures is written to a file called
``protomers.xyz``.
The first (lowest) protomer created by ``CREST`` for this molecule includes a ring-closure, apparently caused by the addition of the proton.
This nicely demonstrates the ability of our approach to form and break new bonds.
The three protomers are shown in the figure below.

.. figure:: ../figures/alaglyprot.png
   :scale: 20 %
   :alt: alaglyprot
   
   Three lowest protomers of alanineglycine generated by CREST at the GFN2-xTB level.


Deprotonation site screening
----------------------------

The general approach to find deprotonation sites at a GFN level is much more simple than finding protonation sites.
For each hydrogen atom in the structure a new (deprotonated) reference structure is created and optimized in a multilevel
approach.
The commandline argument to invoke this search is:

.. code:: bash

    > crest struc.xyz -deprotonate

For the example of alanineglycine, again three structures are obtained and written to a file called ``deprotonated.xyz``:

.. code-block:: text
  
 <.......>
 <.......>
 
 ===================================================
 ============= ordered structure list ==============
 ===================================================
  written to file <deprotonated.xyz>
  
  structure    ΔE(kcal/mol)   Etot(Eh)
     1            0.00        -33.593702
     2           21.83        -33.558913
     3           25.12        -33.553669
 
 <.......>
 <.......>

However, two of the three structures have much higher energies and therefore mainly the lowest deprotomer should be considered.


.. figure:: ../figures/alaglydep.png
   :scale: 25 %
   :alt: alaglydeprot
   
   Lowest deprotomer of alanineglycine at the GFN2-xTB level. The deprotonation happens at the carboxyl group.


Tautomerization screening
-------------------------

The last application of the different prototropy screening protocols is an automatized tautomerization tool, which utilizes
both the protonation and deprotonation procedures presented in the previous two subsections.
By first protonating a molecule and then deprotonation of the resulting protomers at all postions, prototropic tautomers
relative to the initial input structure can be found.
A single cycle of this protonation/deprotonation in principle yields all tautomers with a single hydrogen permutation relative to the input.
If a higher number of hydrogen permutations is required, the procedure can simply be repeated with the created tautomers, i.e., tautomers with
two or more hydrogen atom permutations are generated.
From experience, however, it is generally sufficient to repeat this protonation/deprotonation cycle twice (which is the default in ``CREST``),
in order to get the relevant *low energy* tautomers.
The approach was first described in *J. Comput.-Aided Mol. Des.*, **2018**, *32*, 1139-1149. 
The tautomerization search can be conducted by the command

.. code:: bash
   
    > crest struc.xyz -tautomerize

.. tip:: The number of protonation/deprotonation cycles can be adjustet with the flag ``-iter INT``, where *INT* is the number of cycles.

For alanineglycine the following output is generated:

.. code-block:: text
  
        ==============================================
        |                                            |
        |                 C R E S T                  |
        |                                            |
        |  Conformer-Rotamer Ensemble Sampling Tool  |
        |        based on the GFN-xTB method         |
        |             S.Grimme, P.Pracht             |
        |          Universitaet Bonn, MCTC           |
        ==============================================
        Version 2.7.0, Mon 24. Jun 11:41:02 CEST 2019
        Using the GFN-xTB code.
        Compatible with XTB version 6.1 and later.
 
         __________________________________________
        |                                          |
        |     automated tautomerization script     |
        |__________________________________________|
  
 *******************************************************************************************
 **                   P R O T O N A T I O N   C Y C L E     1 of 2                        **
 *******************************************************************************************
  
  LMO calculation ... done.
 -----------------------
 Multilevel Optimization
 -----------------------
 <.......> 
  ===================================================
  Identifying topologically equivalent structures:
 <.......>
  Appending file <protonated.xyz> with structures.
  
  Initial 9 structures from file protonate_2.xyz have
  been reduced to 3 topologically unique structures.
  ===================================================
  ============= ordered structure list ==============
  ===================================================
  written to file <protonated.xyz>
 
  structure    ΔE(kcal/mol)   Etot(Eh)
     1            0.00        -33.964400
     2            3.60        -33.958659
     3            5.78        -33.955188
  
 *******************************************************************************************
 **                 D E P R O T O N A T I O N   C Y C L E     1 of 2                      **
 *******************************************************************************************
 -----------------------
 Multilevel Optimization
 -----------------------
 <.......>
  ===================================================
  Identifying topologically equivalent structures:
 <.......>
  Appending file <deprotonated.xyz> with structures.
  
  Initial 24 structures from file deprotonate_2.xyz have
  been reduced to 8 topologically unique structures.
  ===================================================
  ============= ordered structure list ==============
  ===================================================
  written to file <deprotonated.xyz>
 
  structure    ΔE(kcal/mol)   Etot(Eh)
 <.......>
  
 *******************************************************************************************
 **                   P R O T O N A T I O N   C Y C L E     2 of 2                        **
 *******************************************************************************************
 Calculating LMOs for all structures in file <tautomerize_1.xyz>
 <.......>        
 Collecting generated protomers ... done.
  
 -----------------------
 Multilevel Optimization
 -----------------------
 <.......>
  ===================================================
  Identifying topologically equivalent structures:
 <.......>
  Appending file <protonated.xyz> with structures.
  
  Initial 51 structures from file protonate_1.xyz have
  been reduced to 17 topologically unique structures.
  ===================================================
  ============= ordered structure list ==============
  ===================================================
  written to file <protonated.xyz>
 
  structure    ΔE(kcal/mol)   Etot(Eh)
 <.......>
  
 *******************************************************************************************
 **                 D E P R O T O N A T I O N   C Y C L E     2 of 2                      **
 *******************************************************************************************
 -----------------------
 Multilevel Optimization
 -----------------------
 <.......>
  ===================================================
  Identifying topologically equivalent structures:
 <.......>
  Appending file <deprotonated.xyz> with structures.
  
  Initial 95 structures from file deprotonate_2.xyz have
  been reduced to 19 topologically unique structures.
  ===================================================
  ============= ordered structure list ==============
  ===================================================
  written to file <deprotonated.xyz>
 
  structure    ΔE(kcal/mol)   Etot(Eh)
 <.......>
  
 *******************************************************************************************
 **                               T A U T O M E R I Z E                                   **
 *******************************************************************************************
  ---------------------------
  Final Geometry Optimization
  ---------------------------
 <.......>
  ===================================================
  Identifying topologically equivalent structures:
  Done.
  Appending file <tautomers.xyz> with structures.
  
  All initial 19 structures from file tautomerize_4.xyz are unique.
  
 ===================================================
 ============= ordered structure list ==============
 ===================================================
  written to file <tautomers.xyz>
  
  structure    ΔE(kcal/mol)   Etot(Eh)
     1            0.00        -33.867777
     2            1.99        -33.864606
     3            3.84        -33.861657
     4            3.84        -33.861656
     5            4.42        -33.860731
     6            4.68        -33.860314
     7           10.63        -33.850839
     8           10.79        -33.850575
     9           10.92        -33.850381
    10           10.95        -33.850329
    11           12.18        -33.848371
    12           12.18        -33.848371
    13           13.45        -33.846343
    14           19.21        -33.837164
    15           19.21        -33.837164
    16           20.24        -33.835520
    17           24.97        -33.827984
    18           25.58        -33.827014
    19           29.53        -33.820725
  
  
  -----------------
  Wall Time Summary
  -----------------
            LMO calc. wall time :         0h : 0m : 0s
       multilevel OPT wall time :         0h : 0m :31s
 --------------------
 Overall wall time  : 0h : 0m :32s
  
  CREST terminated normally.

As can be seen from the output, the entire procedure is constructed from the protonation and deprotonation site screening routines.
The first protonation step yields the same three protomers that are also obtained by the standalone application, which are then
automatically deprotonated. Two protonation/deprotonation cycles are performed.
The final tautomer ensemble consists of 19 structures (within 30 kcal/mol) and is written to the file ``tautomers.xyz``.


Property calculations on final ensemble
=======================================

It is possible to (automatically) perform further calculations on the final conformer ensemble
by the usage of the ``-prop`` option:

.. code:: bash

    > crest [input] [options] -prop [property option]

Currently there are only some few options available but we plan to implement more.

A useful type of this mode, e.g. is the the reoptimization of the conformer ensemble with
very tight convergence thresholds. In combination with crude conformational search settings
such as ``-qucik``, ``-squick`` or ``-mquick`` this helps to ensure the ensemble convergence,
i.e., the minimization of artificial structural differences for the same conformer due to
too loose geometry optimizations.
This reoptimization can be requested by

.. code:: bash
    
    > crest coord -mquick -prop reopt

Updated geometries will generally be written to a new ensemble file called ``crest_property.xyz``.

Another useful runtype of this mode is the calculation of frequencies and reweighting
of the conformers on the resulting free energies. E.g.:

.. code:: bash

    > crest coord -prop hess

The property mode can also directly be applied to a given ensemble:

.. code:: bash

    > crest -forall <ensemble>.xyz -prop [property option]


Dry run to check settings prior to calculations
===============================================

A dry run can be performed by ``CREST`` to verify the settings that would be applied in the
calculation. To do this, simply add the ``-dry`` flag to the cmd-input line.

.. code:: bash

    > crest [input] [options] -dry

Whit this option nothing will be actually be calculated but instead the settings are printed.
E.g. for some random setting:

.. code:: text

    > crest coord -ewin 3.2 -temp 999 -gfn1 -nozs -chrg 1 -cinp .xcontrol.sample -dry

    <....>
    <....>
    *******************************************************************************************
    **                                  D R Y    R U N                                       **
    *******************************************************************************************
     Dry run was requested.
     Running CREST with the chosen cmd arguments will result in the following settings:
    
     Input file : coord
    
     Job type :
      1.  Conformational search via the iMTD-GC algo
    
     Job settings
      sort Z-matrix        :      F
    
     CRE settings
      energy window         (-ewin) :    3.2000
      RMSD threshold        (-rthr) :    0.1250
      energy threshold      (-ethr) :    0.1000
      rot. const. threshold (-bthr) :    0.0200
      T (for boltz. weight) (-temp) :    999.00
    
     General MD/MTD settings
      simulation length [ps]    (-len) : <system dependent>
      time step [fs]          (-tstep) :       5.0
      shake mode              (-shake) :         2
      MTD temperature [K]    (-mdtemp) :    300.00
      trj dump step  [fs]    (-mddump) :       100
      MTD Vbias dump [ps]    (-vbdump) :       1.0
    
     Constrainment info
      applying constraints?  :       T
      constraining file      : .xcontrol.sample
      file content :
      > $constrain
      >   atoms: 1-3,26-30
      >   force constant=0.5
      >   reference=coord.ref
      > $metadyn
      >   atoms: 4-25,31-65
      > $end
    
     XTB settings
      binary name        (-xnam) : xtb
      GFN method         (-gfn)  : --gfn1
      (final) opt level  (-opt)  : 2
      Molecular charge   (-chrg) : 1
    
     Technical settings
      working directory : /home/philipp/calculations/cresttest
      CPUs (threads)     (-T) : 4
    
    
    normal dry run termination.

