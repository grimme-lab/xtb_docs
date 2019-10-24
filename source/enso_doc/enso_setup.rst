===============
Setting up ENSO
===============

.. contents::


Setup
=====


Make sure that python3 is installed on your system. Place the program *enso.py* in your /home/$USER/bin/ folder and add this folder to your PATH. Make the python program executable:

.. code:: sh
   
   > chmod u+x /home/$USER/bin/enso.py

To setup ``ENSO`` place your global configuration file *.ensorc* in your /home/$USER folder. If you should not have a global configuration file, you can create it by running:

.. code-block:: sh

    > enso.py -write_ensorc 

This creates the file *ensorc_new* which you then have to move to your home directory and adjust to your needs:

.. code-block:: sh

    > mv ensorc_new ~/.ensorc

It is mandatory for ``ENSO`` to find the correct paths to your programs within your  global configuration file *.ensorc*:

.. code-block:: sh
  
    > cat ~/.ensorc
    .ENSORC
  
    ORCA: /tmp1/orca_4_1_0_linux_x86-64_openmpi215 # here the binary must not be included
    ORCA version: 4.1
    GFN-xTB: /home/path-to/bin/xtb              #here the binary has to be included into the path
    CREST: /home/path-to/bin/crest              #here the binary has to be included into the path
    mpshift: /home/path-to/bin/xmpshift         #here the binary has to be included into the path
    escf: /home/path-to/bin/xescf               #here the binary has to be included into the path
  
    #COSMO-RS
    ctd = BP_TZVP_C30_1601.ctd cdir = "/software/cluster/COSMOthermX16/COSMOtherm/  CTDATA-FILES"   ldir = "/software/cluster/COSMOthermX16/COSMOtherm/CTDATA-FILES"
    cosmothermversion: 16    # 16, 17, 18
  
    #NMR data
    reference for 1H: TMS           # TMS, DSS
    reference for 13C: TMS          # TMS, DSS
    reference for 19F: default      # default
    reference for 31P: default      # default
    1H active: on                   # on, off
    13C active: off                 # on, off
    19F active: off                 # on, off
    31P active: off                 # on, off
    resonance frequency: 500        # integer
  
    #FLAGS
    nconf: all                      # all or integer between 0 and total number of  conformers
    charge: 0                       # integer
    unpaired: 0                     # integer
    solvent: gas                    # acetone, chcl3, ch2cl2, dmso, h2o, methanol, thf, toluene, gas
    prog: None                      # tm, orca
    ancopt: on                      # on, off
    prog_rrho: xtb                  # xtb, prog
    gfn_version: gfn2               # 1, 2, 3
    prog4: prog                     # prog, tm, orca
    part1: on                       # on, off
    part2: on                       # on, off
    part3: on                       # on, off
    part4: on                       # on, off
    boltzmann: off                  # on, off
    backup: off                     # on, off
    func:  pbeh-3c                  # pbeh-3c, b97-3c, tpss
    func3:  pw6b95                  # pw6b95, wb97x, dsd-blyp
    basis3: def2-TZVPP              
    funcJ:  pbe0                    # tpss, pbe0
    basisJ: default                 
    funcS:  pbe0                    # tpss, pbe0
    basisS: default                 
    couplings: on                   # on, off
    shieldings: on                  # on, off
    part1_threshold: 4.0            # integer or real number
    part2_threshold: 2.0            # integer or real number
    sm: dcosmors                    # cosmo, dcosmors, cpcm, smd
    sm3: dcosmors                   # cosmors, smd
    sm4: cosmo                      # cosmo, cpcm, smd
    check: on                       # on, off
    crestcheck: off                 # on, off
    maxthreads: 1                   # integer larger than 0
    omp: 4                          # integer larger than 0
    smgsolv2: sm                    # sm, cosmors
    end



Requirements
============

``ENSO`` requires:

* Python3

External programs which are required:

* ``xTB`` program  version 6.2 or above
* ``CREST`` version 2.6.2 or above
* in case of COSMO-RS:

  - cefine
  - the TURBOMOLE program package
  - cosmotherm

* in case of TURBOMOLE

  - cefine
  - the TURBOMOLE program package

* in case of ORCA

  - ORCA4.1 or above

For the final spectrum generation:

* anmr version 3.5 or above
* nmrplot.py  (needs python3, numpy, matplotlib)
* or any other plotting tool (e.g. GNUPLOT)


Detailed information
====================

.. figure:: ../../figures/enso/enso-detailed.png
   :scale: 30 %
   :align: center
   :alt: detailed ENSO description

   ``ENSO`` program detailed flowchart.


Files written by ``ENSO``:
==========================

========================  ===========
Files/Folders             Information
========================  ===========
flags.dat                 | The ENSO-run is started from the settings written in this file 
enso.json                 | All information on the conformers is stored in this file 
                          | (e.g. energy, boltzmann weight ...)
trj-part3.xyz             | File containing the geometries of all conformers
                          | remaining in the 
                          | refined ensemble
conformer_rotamer_check/  | Folder in which the optimized ensemble (part2) is checked for rotamers or 
                          | identical conformers
anmr_enso                 | File needed by ANMR, containing the Boltzmann weight, free energy
                          | contribution, conformer-folder information
.anmrrc                   | File needed by ANMR, constaining the reference shieldings, which 
                          | nuclei are active, which program package was used for the 
                          | NMR property
                          | calculations
.ensorc                   | Global configuration file where the user can adjust
                          | default settings for all ENSO-runs.
                          | and all absolute PATHS to the external programs are stored.
========================  ===========

.. _flags_settings:

Settings in .ensorc and flags.dat
=================================

=================== ================
flags               explaination
=================== ================
nconf               | number of conformers considered in this ENSO-run, 
                    | taken from the *crest_conformers.xyz* file 
                    | [e.g. 10 or all]
charge              | molecular charge 
unpaired            number of unpaired electrons
solvent             | solvents that are available: gas (if no solvent is required),
                    | [options: gas, acetone, chcl3, ch2cl2, dmso, h2o, methanol, thf, toluene]
prog                | program- used for calculating parts 1,2 and 3
                    | [options: tm or orca]
ancopt              | if choosen the ANCOPT (Aproximate normal coordinates optimizer) 
                    | implemented in ``xtb`` is employed as driver for *prog* (ORCA, TURBOMOLE)
                    | [options: on, off]
prog_rrho           | chooses which program is employed for the hessian calculation: either 
                    | GFNx-xTB or the program package choosen by *prog*, our recommendation is 
                    | to use GFNx-xTB! [options: prog, xtb]
gfn_version         | If prog_rrho is set to ``xtb`` then you can choose which GFNx-xTB version
                    | should be used. We recommend using *GFN2-xTB* with the keyword *gfn2*
                    | [options: gfn1, gfn2]
prog3               | The program package for calculating high-level free energies can 
                    | be choosen independent of `prog` [options: prog, tm, orca]
prog4               | The program package for calculating NMR properties can be choosen 
                    | independent of
                    | the program package chosen for part 1-3. [options: prog, tm, orca]
part1               | Turn the crude optimization (Part1) on or off, [options: on, off]
part2               | Turn the full optimization and low level free energy calculation (Part2) on or off,
                    | [options: on, off]
part3               | Turn the high level free energy calculation on or off (Part3) [options: on, off]
part4               | Turn the NMR property calculation (Part4) on or off, [options: on, off]
boltzmann           | Option to recalculate the boltzmann weight from the data written in *enso.json*.
backup              | Option to include conformers which were sorted out either in Part1 or Part2, because
                    | were above the threshold, but still within (treshold + 2 kcal/mol). These conformers can
                    | be taken into account if backup is set to *on*. This is of course only possible after 
                    | a previous run. [options: on, off]
func                | density functional employed in *Part1* and *Part2* (crude and full optimization),
                    | [options: pbeh-3c, b97-3c, tpss (*tpss* is only the keyword used is then:
                    | TPSS-D3/def2-TZVP)]
func3               | density functional for calculating the high level single-point for the high level free
                    | energy evaluation in *Part3*, [options: pw6b95, wb97x, dsd-blyp] (Not all functionals
                    | are available in each program package (ORCA, TURBOMOLE)!
basis3              | Basis set used for calculating the high level single-point in Part3. (Be sure that the
                    | basis set exists (typos can lead to crashing single-point calculations).
                    | There are more possibilities than mentioned in options, but they can not be checked!
                    | [options: SVP, SV(P), TZVP, TZVPP, QZVP, QZVPP, def2-SV(P), def2-SVP, def2-TZVP, 
                    | def2-TZVPP, def-SVP, def-SV(P), def2-QZVP, DZ, QZV, cc-pVDZ, cc-pVTZ, cc-pVQZ,
                    | cc-pV5Z, aug-cc-pVDZ, aug-cc-pVTZ, aug-cc-pVQZ, aug-cc-pV5Z, def2-QZVPP]
couplings           | Option to calculate couplings *J* in Part4 or not (e.g. if you would want to 
                    | calculate only [options: on, off]
                    | shieldings in Part4)
funcJ               | density functional used to calculate the couplings in the NMR property calculation = 
                    | *Part4*  [options: pbe0, tpss]
basisJ              | basis set employed in the calculations of the couplings *J* in *Part4*. 
                    | [options: ???]
shieldings          | Options to calculate shieldings *S* in *Part4* or not (e.g. if 
                    | you would want
                    | to calculate only couplings) [options: on, off]
funcS               | density functional uset to calculate the shieldings in the NMR property calculation = 
                    | *Part4* [options: pbe0, tpss]
basisS              | basis set employed in the calculations of the shieldings *S* in *Part4*.
                    | [options: ???]
part1_threshold     | All conformers below this threshold (in kcal/mol) are considered for the full
                    | optimization in Part2. Conformers within threshold > Econf < (threshold + 2 kcal/mol)  
                    | are noted as backup conformers (which can be recalculated if the refined ensemble is 
                    | missing some conformers). In *Part1* all conformers above (threshold + 2 kcal/mol) 
                    | are dismissed. Our recommendation is to set this threshold to 4.0 kcal/mol. 
part2_threshold     | All conformers below this threshold (in kcal/mol) are considered for the high level 
                    | free energy calculation in *Part3*. Conformers within threshold > Econf < 
                    | (threshold + 2 kcal/mol) are noted as backup conformers In *Part2* all conformers above
                    | (threshold + 2 kcal/mol) are dismissed. Our recommendation is to set this threshold to 
                    | 2.0 kcal/mol.
sm                  | Solvation model employed for the optimization in *Part1* und *Part2*. Not all solvation
                    | models are available in each program package (ORCA,TURBOMOLE). In order to use the 
                    | solvation model a *solvent* has to be specified! [options: cosmo, dcosmors, cpcm, smd]
smgsolv2            | In *Part2* first the full optimization is performed using the solvent model specified
                    | in *sm*. Then the low level free energy calculation is performed (still *Part2*)
                    | and to calculate the solvation contribution to free energy (:math:`G_{solv}`) another
                    | solvation model can be choosen. This makes sence, if this solvation model is then 
                    | used in the high level free energy calculation *Part3* too. [options: sm, cosmors]
sm3                 | solvation model employed in the high level free energy calculation *Part3*. 
                    | we recommend to use the same solvation model as in *smgsolv2*. [options: cosmors,
                    | dcosmors, smd].
sm4                 | solvation model employed in the NMR property calculation in *Part4*. 
                    | [options: cosmo, cpcm, smd]
check               | Option to terminate the ENSO-run if too many calculations/preparation steps fail.
                    | [options: on, off]
crestcheck          | The conformers  could become identical or rotamers of each other during the full 
                    | DFT optimization (*Part2*). Therefore we use ``CREST`` to identify identical 
                    | conformers or rotamers. The *crestcheck* option (on) can automatically remove 
                    | identical conformers and rotamers. If it is set to off, the check is still run
                    | but the user is only informed and has to remove the conformers manually after
                    | inspection. Our recommendation is to sort out conformers manually (option: off)
                    | since the sorting alogrithm is threshold based. [options: on, off]
maxthreads          | Number of threads the ENSO program can use. (maxthreads * omp = number of cores)
                    | e.g. the maximal number of calculations that can run in parallel.
                    | Make sure that the number of cores does not exceed your machine
omp                 | specification. Number of cores each thread (set with *maxthreads*)
                    | has available. e.g. maxthreads = 2 and omp = 3 would use two threads 
                    | using each three cores, the total number of cores in use would be six. 
reference for 1H    | Reference for calculating the shifts of your 1H spectrum.
                    | This is written to the file .anmrrc. [options: TMS, DSS]
reference for 13C   | Reference for calculating the 13C shifts of your spectrum.
                    | This information is written to the file .anmrrc. [options: TMS, DSS]
1H active           | Calculate NMR properties for the 1H nuclei. [options: on, off]
13C active          | Calculate NMR propgerties for the 13C nuclei [options: on, off]
19F active          | Calculate NMR propgerties for the 19F nuclei [options: on, off]
31P active          | Calculate NMR propgerties for the 31P nuclei [options: on, off]
resonance frequency | Frequency of your simulated NMR spectrometer
                    | (the spectrometer you are comparing against. 
temperature         | Temperature in Kelvin for thermostatistical and Boltzmann weight
                    | evaluation.
=================== ================

NMR-property calculations
=========================


Information about the basis sets employed (default) for NMR property calculations:

* Jensen (ORCA)
* def2-TZVP (TURBOMOLE)

For user convenience shielding values of the reference molecules (TMS, DSS ...) were precalculated
and stored within the ``ENSO`` program. The reference shielding values are used in the ``ANMR``
program to calculate the shifts and the reference values are written to the file *.anmrrc*.

To be consistent with your calculation, the reference shielding values were calculated on the
reference molecules with all possible geometry-optimization-settings eg. {TURBOMOLE/ORCA, PBEh-3c /
TPSS-D3/def2-TZVP / B97-3c, (gas phase or solvent)}. The shieldings were then calculated either with
TPSS or PBE0 and depending on ORCA (gas or CPCM and pcSseg-2 Jensen basis set) or TURBOMOLE (gas or
COSMO with the def2-TZVP basis set). At the end of part4 the file *.anmrrc* is written into the
calculation folder and stores the reference shielding values of your settings for the subsequent
ANMR calculation.

.. note:: The ``ENSO`` program only writes the reference shielding values to the file .anmrrc but 
      does not do anything with it. Hence, no results of the ``ENSO`` program are influenced 
      by a non-matching reference value. If you want to change the reference shielding values, 
      you can simply modify the file .anmrrc before calling the ``ANMR`` program. 

