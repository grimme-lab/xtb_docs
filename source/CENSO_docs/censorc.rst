.. _censorc:

===========================
Censorc keyword definitions
===========================

.. contents::

General Settings
----------------


.. list-table:: general
    :widths: 30 100 30 30
    :header-rows: 1
    
    * - keyword
      - description
      - default
      - allowed options
    * - maxcores
      - how many cores should be utilized by CENSO at most.
      - 4
      - [4, 256]
    * - omp
      - how many cores should be used for every subprocess launched by CENSO.
      - 4
      - [4, 32]
    * - imagthr
      - value of the imagthr keyword will be passed to the xtb keyword of the same name (relevant for mRRHO).
      - -100.0
      - [-300.0, 0.0]
    * - sthr
      - value of the sthr keyword will be passed to the xtb keyword of the same name (relevant for mRRHO).
      - 0.0
      - [0.0, 100.0]
    * - scale
      - value of the scale keyword will be passed to the xtb keyword of the same name (relevant for mRRHO).
      - 1.0
      - [0.0, 1.0]
    * - temperature
      - temperature in Kelvin; when calculating Gtot, CENSO will use the G values for this temperature.
      - 298.15
      - [1e-5, 2000.0]
    * - solvent
      - CENSO will try to use this solvent with the set solvation models (for more details see documentation on solvation).
      - h2o
      - 
    * - sm_rrho
      - solvation model that should be used for all xtb-only calculations.
      - alpb
      - alpb, gbsa
    * - multitemp
      - wether GmRRHO should be calculated for a range of temperatures (defined in trange) or not.
      - True
      - True, False
    * - evaluate_rrho
      - wether to calculate GmRRHO or not.
      - True
      - True, False
    * - consider_sym
      - wether to determine and use the symmetry of the conformers for xtb-only calculations or not.
      - True
      - True, False
    * - bhess
      - wether to run the mRRHO calculation on unoptimized geometries (True) or geometries preoptimized by xtb (False), which would result in calling xtb with --ohess.
      - True
      - True, False
    * - rmsdbias
      - wether to use a RMSD bias (should be defined in a file called rmsdpot.xyz in the working directory).
      - False
      - True, False
    * - balance
      - wether to use the built-in static load balancing strategy (tries to utilize all the cores as much as possible). If set to False, CENSO will use the number of cores per task assigned with the omp setting.
      - True
      - True, False
    * - gas-phase
      - wether to turn off all solvation modelling (True) or use solvation (False).
      - False
      - True, False
    * - copy_mo
      - wether to copy MO-files of previous calculations of a conformer within a run (True) or not (False). **This is highly recommended** to use, since it is likely to reduce the number of SCF cycles per single-point significantly.
      - True
      - True, False
    * - retry_failed
      - wether to try to recover failed jobs by applying flags hardcoded in CENSO. This is recommended to use if you know that SCFs of your system might be tricky.
      - True
      - True, False
    * - trange
      - specifies the range of temperatures for which GmRRHO will be calculated ([start, end, stepsize]).
      - [273.15, 373.15, 5]
      - 


Part0 - Prescreening - Settings
-------------------------------------

.. list-table:: prescreening
    :widths: 30 100 30 30
    :header-rows: 1

    * - keyword
      - description
      - default
      - allowed options
    * - threshold
      - the threshold (kcal/mol) for δG to the lowest conformer beyond which conformers will be removed from the ensemble.
      - 4.0
      - [1.0, 10.0]
    * - func
      - the functional/dispersion correction combination used for this step.
      - pbe-d4
      - 
    * - basis 
      - the basis set used for this step.
      - def2-SV(P)
      -
    * - prog 
      - program that should be used for this step
      - ORCA
      - 
    * - gfnv
      - Variant of GFN that should be used for xtb calculations in this step.
      - gfn2
      - gfnff, gfn1, gfn2
    * - grid
      - grid preset and SCF threshold that should be used for this step.
      - low 
      - low, low+, high, high+
    * - run
      - when using the command line interface, it tells CENSO wether to run this part or not.
      - True
      - True, False
    * - gcp
      - wether to use the geometric counter-poise correction by Grimme et al. for this step.
      - True
      - True, False
    * - template
      - wether to use a user defined template for this step.
      - False
      - True, False


Part1 - Screening - Settings
-------------------------------

.. list-table:: screening
    :widths: 30 100 30 30
    :header-rows: 1

    * - keyword
      - description
      - default
      - allowed options
    * - threshold
      - the threshold (kcal/mol) for δG to the lowest conformer beyond which conformers will be removed from the ensemble.
      - 3.5
      - [0.75, 7.5]
    * - func
      - the functional/dispersion correction combination used for this step.
      - r2scan-3c
      - 
    * - basis 
      - the basis set used for this step.
      - def2-TZVP
      -
    * - prog 
      - program that should be used for this step
      - ORCA
      - 
    * - sm 
      - solvation model used for this step.
      - smd
      -
    * - gfnv
      - Variant of GFN that should be used for xtb calculations in this step.
      - gfn2
      - gfnff, gfn1, gfn2
    * - grid
      - grid preset and SCF threshold that should be used for this step.
      - low+
      - low, low+, high, high+
    * - run
      - when using the command line interface, it tells CENSO wether to run this part or not.
      - True
      - True, False
    * - gcp
      - wether to use the geometric counter-poise correction by Grimme et al. for this step.
      - True
      - True, False
    * - template
      - wether to use a user defined template for this step.
      - False
      - True, False
    * - implicit
      - wether to calculate the solvation contribution to Gtot implicitely (True) or not (False). If set to True, only one single-point needs to be calculated in this step.
      - True
      - True, False


Part2 - Optimization - Settings
-------------------------------

.. list-table:: optimization
    :widths: 30 100 30 30
    :header-rows: 1

    * - keyword
      - description
      - default
      - allowed options
    * - optcycles
      - number of microcycles per macrocycles if using macrocycle optimization.
      - 8
      - [1, 10]
    * - maxcyc
      - maximum number of optimization cycles (in the case of macrocycle optimization the maximum number of cumulative microcycles).
      - 200 
      - [10, 1000]
    * - threshold
      - the **minimum** threshold (kcal/mol) for δG to the lowest conformer beyond which conformers will be removed from the ensemble.
      - 1.5
      - [0.5, 5.0]
    * - gradthr
      - threshold for the gradient below which the normal energy threshold condition will be applied.
      - 0.01
      - [0.001, 0.1]
    * - hlow
      - value of the hlow keyword will be passed to the xtb keyword of the same name.
      - 0.01
      - [0.001, 0.1]
    * - func
      - the functional/dispersion correction combination used for this step.
      - r2scan-3c
      - 
    * - basis 
      - the basis set used for this step.
      - def2-TZVP
      -
    * - prog 
      - program that should be used for this step
      - ORCA
      - 
    * - sm 
      - solvation model used for this step.
      - smd
      -
    * - gfnv
      - Variant of GFN that should be used for xtb calculations in this step.
      - gfn2
      - gfnff, gfn1, gfn2
    * - grid
      - grid preset and SCF threshold that should be used for this step.
      - high
      - low, low+, high, high+
    * - optlevel
      - geometry optimization thresholds passed to xtb.
      - normal
      - crude, sloppy, loose, lax, normal, tight, vtight, extreme
    * - run
      - when using the command line interface, it tells CENSO wether to run this part or not.
      - True
      - True, False
    * - gcp
      - wether to use the geometric counter-poise correction by Grimme et al. for this step.
      - True
      - True, False
    * - template
      - wether to use a user defined template for this step.
      - False
      - True, False
    * - macrocycles
      - wether to use macrocycle optimization (True) or not.
      - True
      - True, False
    * - crestcheck
      - wether to use CREST every macrocycle to check the ensemble for rotamers or not.
      - False
      - True, False



NMR - Settings
---------------------

.. list-table:: nmr
    :widths: 30 100 30 30
    :header-rows: 1

    * - keyword
      - definition
    * - part4
      - Option to turn the "NMR property part" *on* or *off*.
    * - couplings
      - Perform coupling constant calculations [options are *on* or *off*].
    * - prog4J
      - QM code (TM, ORCA) used for coupling constant calculations.
    * - funcJ
      - Density functional employed for the coupling constant calculation.
    * - basisJ
      - basis set employed with the DFA (funcJ) for coupling constant calculations.
    * - sm4J
      - implicit solvent model employed in the coupling constant calculation.
    * - shieldings
      - Perform shielding constant calculations [options are *on* or *off*].
    * - prog4S
      - QM code (TM, ORCA) used for shielding constant calculations.
    * - funcS
      - Density functional employed for the shielding constant calculation.
    * - basisS
      - basis set employed with the DFA \(funcS\) for shielding constant calculations.
    * - sm4S
      - implicit solvent model employed in the shielding constant calculation.
    * - reference_1H
      - Reference molecule to convert 1H shielding constants to shifts e.g. TMS.
    * - reference_13C
      - Reference molecule to convert 13C shielding constants to shifts e.g. TMS.
    * - reference_19F
      - Reference molecule to convert 19F shielding constants to shifts e.g. CFCl3.
    * - reference_29Si
      - Reference molecule to convert 29Si shielding constants to shifts e.g. TMS.
    * - reference_31P
      - Reference molecule to convert 31P shielding constants to shifts e.g. TMP.
    * - 1H_active
      - Calculate 1H NMR properties [options are *on* or *off*].
    * - 13C_active
      - Calculate 13C NMR properties [options are *on* or *off*].
    * - 19F_active
      - Calculate 19F NMR properties [options are *on* or *off*].
    * - 29Si_active
      - Calculate 29Si NMR properties [options are *on* or *off*].
    * - 31P_active
      - Calculate 31P NMR properties [options are *on* or *off*].
    * - resonance_frequency
      - Resonance frequency of the experimental spectrometer (in Hz).

Optical Rotation Property Settings
----------------------------------

.. list-table:: part5
    :widths: 30 100
    :header-rows: 1

    * - keyword
      - definition
    * - optical\_rotation
      - Option to turn the "OR property part" *on* or *off*.
    * - funcOR
      - Functional employed to calculate the optical rotatory (OR) dispersion.
    * - funcOR_SCF
      - Functional to generate converged MOs.
    * - basisOR
      - Basis set employed for the OR calculation.
    * - frequency_optical_rot
      - List of frequencies in nm to evaluate OR at e.g. [589.0].
