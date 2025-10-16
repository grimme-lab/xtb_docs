.. _censorc:

===========================
Censorc keyword definitions
===========================

.. contents::

The settings should be given in the form of an rcfile (either located in ``$HOME`` and named ``.censo2rc``)
or passed to CENSO via the ``--inprc`` argument, or read using the ``configure`` function when working from 
a custom script. If a setting is missing from the rcfile, CENSO will use the default value for that setting.
The settings will be type checked when the rcfile is parsed, so make sure that the appropiate data type 
(e.g. int, float, str) can be parsed from the value you've defined.

.. hint::

    In order to write a new rcfile for configuration you can run ``censo --new-config`` or use the ``write_rcfile``
    function from the ``censo.configuration`` module.


General remarks on avairable programs/functionals/solvation models/xtb variants:
- ORCA is available for all ensemble optimization steps, NMR, and UV/Vis.
- TURBOMOLE is available for all ensemble optimization steps, NMR and OR.
- xtb supports the ALPB and GBSA solvation models.
- COSMO-RS is not available for property calculations and geometry optimizations.
- available xtb variants are GFN1, GFN2, GFNFF.

For geometry optimizations, the following convergence levels are available:
      - crude, sloppy, loose, lax, normal, tight, vtight, extreme

General Settings
----------------

General settings are settings that are used throughout all parts of CENSO. This includes but is not limited to 
the maximum number of cores CENSO should use, single-point Hessian settings, solvent choice, and temperature.

.. list-table:: General Settings
    :widths: 30 100 30
    :header-rows: 1
    
    * - keyword
      - description
      - default
    * - imagthr
      - value of the imagthr keyword will be passed to the xtb keyword of the same name (relevant for mRRHO).
      - -100.0
    * - sthr
      - value of the sthr keyword will be passed to the xtb keyword of the same name (relevant for mRRHO).
      - 50.0
    * - scale
      - value of the scale keyword will be passed to the xtb keyword of the same name (relevant for mRRHO).
      - 1.0
    * - temperature
      - temperature in Kelvin; when calculating Gtot, CENSO will use the G values for this temperature.
      - 298.15
    * - solvent
      - CENSO will try to use this solvent with the set solvation models (for more details see :ref:`censo_solv`).
      - h2o
    * - sm_rrho
      - solvation model that should be used for all xtb-only calculations.
      - alpb
    * - evaluate_rrho
      - whether to calculate GmRRHO or not.
      - True
    * - consider_sym
      - whether to determine and use the symmetry of the conformers for xtb-only calculations or not.
      - True
    * - bhess
      - whether to run the mRRHO calculation on unoptimized geometries (True) or geometries preoptimized by xtb (False), which would result in calling xtb with --ohess.
      - True
    * - balance
      - whether to use the built-in static load balancing strategy (tries to utilize all the cores as much as possible). If set to False, CENSO will use the number of cores per task assigned with the omp setting. Note that this feature is not supported for TURBOMOLE.
      - True
    * - gas-phase
      - whether to turn off all solvation modelling (True) or use solvation (False).
      - False
    * - copy_mo
      - whether to copy MO-files of previous calculations of a conformer within a run (True) or not (False). **This is highly recommended** to use, since it is likely to reduce the number of SCF cycles per single-point significantly.
      - True


Prescreening
------------

.. list-table:: Prescreening Settings
    :widths: 30 100 30
    :header-rows: 1

    * - keyword
      - description
      - default
    * - threshold
      - the threshold (kcal/mol) for ΔG to the lowest conformer beyond which conformers will be removed from the ensemble.
      - 4.0
    * - func
      - the functional/dispersion correction combination used for this step (:ref:`censo_funcs`).
      - pbe-d4
    * - basis 
      - the basis set used for this step (:ref:`censo_bs`).
      - def2-SV(P)
    * - prog 
      - program that should be used for this step
      - tm
    * - gfnv
      - Variant of GFN that should be used for xtb calculations in this step.
      - gfn2
    * - template
      - whether to use a user defined template for this step.
      - False


Screening
---------

.. list-table:: Screening Settings
    :widths: 30 100 30
    :header-rows: 1

    * - keyword
      - description
      - default
    * - threshold
      - the threshold (kcal/mol) for ΔG to the lowest conformer beyond which conformers will be removed from the ensemble.
      - 3.5
    * - func
      - the functional/dispersion correction combination used for this step (:ref:`censo_funcs`).
      - r2scan-3c
    * - basis 
      - the basis set used for this step (:ref:`censo_bs`).
      - def2-TZVP
    * - prog 
      - program that should be used for this step
      - tm
    * - sm 
      - solvation model used for this step.
      - smd
    * - gfnv
      - Variant of GFN that should be used for xtb calculations in this step.
      - gfn2
    * - template
      - whether to use a user defined template for this step.
      - False
    * - gsolv_included
      - whether to calculate the solvation contribution to Gtot explicitly (False) or not (True). If set to True, only one single-point needs to be calculated in this step.
      - True


Optimization
------------

.. list-table:: Optimization Settings
    :widths: 30 100 30
    :header-rows: 1

    * - keyword
      - description
      - default
    * - optcycles
      - number of microcycles per macrocycles if using macrocycle optimization.
      - 8
    * - maxcyc
      - maximum number of optimization cycles (in the case of macrocycle optimization the maximum number of cumulative microcycles).
      - 200 
    * - threshold
      - the threshold (kcal/mol) for ΔG to the lowest conformer beyond which conformers can be removed from the ensemble if the gradient is too small.
      - 3.0
    * - gradthr
      - threshold for the gradient below which the normal energy threshold condition will be applied.
      - 0.01
    * - hlow
      - value of the hlow keyword will be passed to the xtb keyword of the same name.
      - 0.01
    * - func
      - the functional/dispersion correction combination used for this step (:ref:`censo_funcs`).
      - r2scan-3c
    * - basis 
      - the basis set used for this step (:ref:`censo_bs`).
      - def2-TZVP
    * - prog 
      - program that should be used for this step.
      - tm
    * - sm 
      - solvation model used for this step.
      - smd
    * - gfnv
      - Variant of GFN that should be used for xtb calculations in this step.
      - gfn2
    * - optlevel
      - geometry optimization thresholds passed to xtb.
      - normal
    * - template
      - whether to use a user defined template for this step.
      - False
    * - macrocycles
      - whether to use macrocycle optimization (True) or not.
      - True
    * - xtb_opt
      - whether to use ANCOPT as driver for the geometry optimization or to use the native optimizer (only available for ORCA).
      - True


Refinement
----------

.. list-table:: Refinement Settings
    :widths: 30 100 30
    :header-rows: 1

    * - keyword
      - description
      - default
    * - threshold
      - the threshold for the additive Boltzmann population of the ensemble beyond which conformers will be removed from the ensemble.
      - 0.95
    * - func
      - the functional/dispersion correction combination used for this step (:ref:`censo_funcs`).
      - wb97x-d3
    * - basis 
      - the basis set used for this step (:ref:`censo_bs`).
      - def2-TZVP
    * - prog 
      - program that should be used for this step
      - tm
    * - sm 
      - solvation model used for this step.
      - smd
    * - gfnv
      - Variant of GFN that should be used for xtb calculations in this step.
      - gfn2
    * - template
      - whether to use a user defined template for this step.
      - False
    * - gsolv_included
      - whether to calculate the solvation contribution to Gtot implicitely (True) or not (False). If set to True, only one single-point needs to be calculated in this step.
      - True


NMR
---

.. list-table:: NMR Settings
    :widths: 30 100 30
    :header-rows: 1

    * - keyword
      - description
      - default
    * - resonance_frequency
      - carrier frequency of the microwave radiation in the simulated NMR experiment
      - 300.0
    * - ss_cutoff
      - cutoff radius for the calculation of spin-spin couplings. Pairs with a larger distance than ss_cutoff will be neglected (only for ORCA).
      - 8.0
    * - prog
      - program that should be used to calculate the shielding/coupling single-points.
      - orca
    * - func
      - the functional/dispersion correction combination used for this step (:ref:`censo_funcs`).
      - pbe0-d4
    * - basis
      - the basis set used for this step (:ref:`censo_bs`).
      - def2-TZVP
    * - sm
      - solvation model used for this step.
      - smd
    * - active_nuclei
      - active nuclei for NMR calculations as comma separated list, e.g. ``h,c,f``.
    * - template
      - whether to use a user defined template for this step.
      - False
    * - couplings
      - whether to compute the coupling constants.
      - True
    * - shieldings
      - whether to compute the shieldings.
      - True
    * - fc_only
      - whether to calculate only the Fermi-Contact term for spin-spin couplings.
      - True 

Rotatory Disperson
------------------

.. list-table:: OR Settings
    :widths: 30 100 30
    :header-rows: 1

    * - keyword
      - description
      - default
    * - prog
      - program that should be used to calculate the rotatory dispersion single-points.
      - tm
    * - func
      - the functional/dispersion correction combination used for this step (:ref:`censo_funcs`).
      - pbe-d4
    * - basis
      - the basis set used for this step (:ref:`censo_bs`).
      - def2-SVPD
    * - template
      - whether to use a user defined template for this step.
      - False
    * - freq
      - the wavelengths of the rotational dispersion experiment in nm.
      - [589, 633]

UV/Vis
------

      
.. list-table:: UV/Vis Settings
    :widths: 30 100 30
    :header-rows: 1

    * - keyword
      - description
      - default
    * - nroots
      - number of roots sought for TD-DFT.
      - 20
    * - prog
      - program that should be used to calculate the shielding/coupling single-points.
      - orca
    * - func
      - the functional/dispersion correction combination used for TD-DFT.
      - wb97x-v
    * - basis
      - basis set used for TD-DFT. This will be ignored if the chosen functional is a composite functional.
      - def2-TZVP
    * - sm
      - solvation model used for TD-DFT.
      - smd
    * - template
      - whether to use a user defined template for this step.
      - False


Paths Settings 
--------------

.. list-table:: Paths Settings 
   :widths: 30 100 
   :header-rows: 1 

   * - setting 
     - description 
   * - orca
     - absolute path to the ``orca`` binary. 
   * - xtb
     - absolute path to the ``xtb`` binary.
   * - tm
     - absolute path to the directory containing the TURBOMOLE binaries.
   * - cosmothermpath
     - absolute path to the ``cosmotherm`` binary (COSMOthermX).
   * - cosmorssetup
     - the name of the parameterization file to use for COSMO-RS runs, e.g. ``BP_TZVP_C30_1601.ctd``.


All remaining entries are unused for now. CENSO tries to determine the paths of the binaries 
automatically when creating a new rcfile.
