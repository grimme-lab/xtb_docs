.. _censorc:

===========================
Censorc keyword definitions
===========================

.. contents::

The settings should be given in the form of an rcfile (either located in ``$HOME`` and named ``.censo2rc``)
or passed to CENSO via the ``-inprc`` argument, or read using the ``configure`` function when working from 
a custom script). If a setting is missing from the rcfile, CENSO will use the default value for that setting.
The settings will be type checked when the rcfile is parsed, so make sure that the appropiate data type 
(e.g. int, float, str) can be parsed from the value you defined.

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


Prescreening
------------

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
      - the basis set used for this step. This will be ignored if the chosen functional is a composite functional..
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
      - wether to use the geometric counter-poise correction by Grimme et al. for this step. This will be ignored if the chosen functional is a composite functional.
      - True
      - True, False
    * - template
      - wether to use a user defined template for this step.
      - False
      - True, False


Screening
---------

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
      - the basis set used for this step. This will be ignored if the chosen functional is a composite functional.
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
      - wether to use the geometric counter-poise correction by Grimme et al. for this step. This will be ignored if the chosen functional is a composite functional.
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


Optimization
------------

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
      - the basis set used for this step. This will be ignored if the chosen functional is a composite functional.
      - def2-TZVP
      -
    * - prog 
      - program that should be used for this step.
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
      - wether to use the geometric counter-poise correction by Grimme et al. for this step. This will be ignored if the chosen functional is a composite functional.
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
    * - constrain
      - wether to use constraints for the geometry optimization or not. The constraints should be provided as a file `constraints.xtb` in the working directory.
      - False
      - True, False


Refinement
---------

.. list-table:: refinement
    :widths: 30 100 30 30
    :header-rows: 1

    * - keyword
      - description
      - default
      - allowed options
    * - threshold
      - the threshold (%) for the additive Boltzmann population of the ensemble beyond which conformers will be removed from the ensemble.
      - 0.95
      - [0.01, 0.99]
    * - func
      - the functional/dispersion correction combination used for this step.
      - wb97x-d3
      - 
    * - basis 
      - the basis set used for this step. This will be ignored if the chosen functional is a composite functional.
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
      - wether to use the geometric counter-poise correction by Grimme et al. for this step. This will be ignored if the chosen functional is a composite functional.
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


NMR
---

.. list-table:: nmr
    :widths: 30 100 30 30
    :header-rows: 1

    * - keyword
      - description
      - default
      - allowed options
    * - resonance_frequency
      - carrier frequency of the microwave radiation in the simulated NMR experiment
      - 300.0
      - [150.0, 1000.0]
    * - threshold_bmw
      - cumulative Boltzmann population threshold up to which conformers should be considered.
      - 0.95
      - [0.01, 0.99]
    * - prog
      - program that should be used to calculate the shielding/coupling single-points.
      - orca
      - 
    * - func_j
      - the functional/dispersion correction combination used in calculating the couplings.
      - pbe0-d4
      - 
    * - basis_j
      - basis set used in calculating the couplings. This will be ignored if the chosen functional is a composite functional.
      - def2-TZVP
      - 
    * - sm_j
      - solvation model used in the calculation of the couplings.
      - smd
      - smd, cpcm
    * - func_s
      - the functional/dispersion correction combination used in calculating the shieldings.
      - pbe0-d4
      - 
    * - basis_s
      - basis set used in calculating the shieldings. This will be ignored if the chosen functional is a composite functional.
      - def2-TZVP
      - 
    * - sm_s
      - solvation model used in the calculation of the shieldings.
      - smd
      - smd, cpcm
    * - h_ref
      - 
      - TMS
      - 
    * - c_ref
      - 
      - TMS
      - 
    * - f_ref
      - 
      - CFCl3
      - 
    * - si_ref
      - 
      - TMS
      - 
    * - p_ref
      - 
      - TMP
      - 
    * - grid
      - grid preset and SCF threshold that should be used for this step.
      - high+
      - low, low+, high, high+
    * - run
      - when using the command line interface, it tells CENSO wether to run this part or not.
      - False
      - True, False
    * - template
      - wether to use a user defined template for this step.
      - False
      - True, False
    * - gcp
      - wether to use the geometric counter-poise correction by Grimme et al. for this step. This will be ignored if the chosen functional is a composite functional.
      - True
      - True, False
    * - couplings
      - wether to compute the coupling constants.
      - True
      - True, False
    * - shieldings
      - wether to compute the shieldings.
      - True
      - True, False.
    * - h_active
      - wether to calculate NMR parameters for Protium.
      - True
      - True, False
    * - c_active
      - wether to calculate NMR parameters for 13C.
      - True
      - True, False
    * - f_active
      - wether to calculate NMR parameters for 19F.
      - False
      - True, False
    * - si_active
      - wether to calculate NMR parameters for 29Si.
      - False
      - True, False
    * - p_active
      - wether to calculate NMR parameters for 31P.
      - False
      - True, False

Optical Rotation Property
-------------------------

.. list-table:: part5
    :widths: 30 100
    :header-rows: 1

    * - keyword
      - description
      - default
      - allowed options
    * - optical_rotation
      - Option to turn the "OR property part" *on* or *off*.
    * - funcOR
      - Functional employed to calculate the optical rotatory (OR) dispersion.
    * - funcOR_SCF
      - Functional to generate converged MOs.
    * - basisOR
      - Basis set employed for the OR calculation.
    * - frequency_optical_rot
      - List of frequencies in nm to evaluate OR at e.g. [589.0].
