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


General Settings
----------------

General settings are settings that are used throughout all parts of CENSO. This includes but is not limited to 
the maximum number of cores CENSO should use, single-point Hessian settings, solvent choice, and temperature.

.. list-table:: General Settings
    :widths: 30 100 30 30
    :header-rows: 1
    
    * - keyword
      - description
      - default
      - allowed options
    * - imagthr
      - value of the imagthr keyword will be passed to the xtb keyword of the same name (relevant for mRRHO).
      - -100.0
      - 
    * - sthr
      - value of the sthr keyword will be passed to the xtb keyword of the same name (relevant for mRRHO).
      - 0.0
      - 
    * - scale
      - value of the scale keyword will be passed to the xtb keyword of the same name (relevant for mRRHO).
      - 1.0
      - 
    * - temperature
      - temperature in Kelvin; when calculating Gtot, CENSO will use the G values for this temperature.
      - 298.15
      - 
    * - solvent
      - CENSO will try to use this solvent with the set solvation models (for more details see documentation on solvation).
      - h2o
      - :ref:`censo_solv`
    * - sm_rrho
      - solvation model that should be used for all xtb-only calculations.
      - alpb
      - alpb, gbsa
    * - multitemp
      - whether GmRRHO should be calculated for a range of temperatures (defined in trange) or not.
      - True
      - True, False
    * - evaluate_rrho
      - whether to calculate GmRRHO or not.
      - True
      - True, False
    * - consider_sym
      - whether to determine and use the symmetry of the conformers for xtb-only calculations or not.
      - True
      - True, False
    * - bhess
      - whether to run the mRRHO calculation on unoptimized geometries (True) or geometries preoptimized by xtb (False), which would result in calling xtb with --ohess.
      - True
      - True, False
    * - rmsdbias
      - whether to use a RMSD bias (should be defined in a file called rmsdpot.xyz in the working directory).
      - False
      - True, False
    * - balance
      - whether to use the built-in static load balancing strategy (tries to utilize all the cores as much as possible). If set to False, CENSO will use the number of cores per task assigned with the omp setting. Note that this feature is not supported for TURBOMOLE.
      - True
      - True, False
    * - gas-phase
      - whether to turn off all solvation modelling (True) or use solvation (False).
      - False
      - True, False
    * - copy_mo
      - whether to copy MO-files of previous calculations of a conformer within a run (True) or not (False). **This is highly recommended** to use, since it is likely to reduce the number of SCF cycles per single-point significantly.
      - True
      - True, False
    * - retry_failed
      - whether to try to recover failed jobs by applying flags hardcoded in CENSO. This is recommended to use if you know that SCFs of your system might be tricky.
      - True
      - True, False
    * - trange
      - specifies the range of temperatures for which GmRRHO will be calculated ([start, end, stepsize]).
      - [273.15, 373.15, 5]
      - 


Prescreening
------------

.. list-table:: Prescreening Settings
    :widths: 30 100 30 30
    :header-rows: 1

    * - keyword
      - description
      - default
      - allowed options
    * - threshold
      - the threshold (kcal/mol) for ΔG to the lowest conformer beyond which conformers will be removed from the ensemble.
      - 4.0
      - 
    * - func
      - the functional/dispersion correction combination used for this step.
      - pbe-d4
      - :ref:`censo_funcs`
    * - basis 
      - the basis set used for this step. This will be ignored if the chosen functional is a composite functional..
      - def2-SV(P)
      - :ref:`censo_bs`
    * - prog 
      - program that should be used for this step
      - tm
      - orca, tm
    * - gfnv
      - Variant of GFN that should be used for xtb calculations in this step.
      - gfn2
      - gfnff, gfn1, gfn2
    * - run
      - when using the command line interface, it tells CENSO whether to run this part or not.
      - True
      - True, False
    * - template
      - whether to use a user defined template for this step.
      - False
      - True, False


Screening
---------

.. list-table:: Screening Settings
    :widths: 30 100 30 30
    :header-rows: 1

    * - keyword
      - description
      - default
      - allowed options
    * - threshold
      - the threshold (kcal/mol) for ΔG to the lowest conformer beyond which conformers will be removed from the ensemble.
      - 3.5
      - 
    * - func
      - the functional/dispersion correction combination used for this step.
      - r2scan-3c
      - :ref:`censo_funcs`
    * - basis 
      - the basis set used for this step. This will be ignored if the chosen functional is a composite functional.
      - def2-TZVP
      - :ref:`censo_bs`
    * - prog 
      - program that should be used for this step
      - tm
      - orca, tm
    * - sm 
      - solvation model used for this step.
      - smd
      - smd, cpcm, cosmo, dcosmors, cosmors, cosmors-fine
    * - gfnv
      - Variant of GFN that should be used for xtb calculations in this step.
      - gfn2
      - gfnff, gfn1, gfn2
    * - run
      - when using the command line interface, it tells CENSO whether to run this part or not.
      - True
      - True, False
    * - template
      - whether to use a user defined template for this step.
      - False
      - True, False
    * - implicit
      - whether to calculate the solvation contribution to Gtot implicitely (True) or not (False). If set to True, only one single-point needs to be calculated in this step.
      - True
      - True, False


Optimization
------------

.. list-table:: Optimization Settings
    :widths: 30 100 30 30
    :header-rows: 1

    * - keyword
      - description
      - default
      - allowed options
    * - optcycles
      - number of microcycles per macrocycles if using macrocycle optimization.
      - 8
      - 
    * - maxcyc
      - maximum number of optimization cycles (in the case of macrocycle optimization the maximum number of cumulative microcycles).
      - 200 
      - 
    * - threshold
      - the **minimum** threshold (kcal/mol) for ΔG to the lowest conformer beyond which conformers will be removed from the ensemble.
      - 1.5
      - 
    * - gradthr
      - threshold for the gradient below which the normal energy threshold condition will be applied.
      - 0.01
      - 
    * - hlow
      - value of the hlow keyword will be passed to the xtb keyword of the same name.
      - 0.01
      - 
    * - func
      - the functional/dispersion correction combination used for this step.
      - r2scan-3c
      - :ref:`censo_funcs`
    * - basis 
      - the basis set used for this step. This will be ignored if the chosen functional is a composite functional.
      - def2-TZVP
      - :ref:`censo_bs`
    * - prog 
      - program that should be used for this step.
      - tm
      - orca, tm
    * - sm 
      - solvation model used for this step.
      - smd
      - smd, cpcm, cosmo, dcosmors
    * - gfnv
      - Variant of GFN that should be used for xtb calculations in this step.
      - gfn2
      - gfnff, gfn1, gfn2
    * - optlevel
      - geometry optimization thresholds passed to xtb.
      - normal
      - crude, sloppy, loose, lax, normal, tight, vtight, extreme
    * - run
      - when using the command line interface, it tells CENSO whether to run this part or not.
      - True
      - True, False
    * - template
      - whether to use a user defined template for this step.
      - False
      - True, False
    * - macrocycles
      - whether to use macrocycle optimization (True) or not.
      - True
      - True, False
    * - crestcheck
      - whether to use CREST every macrocycle to check the ensemble for rotamers or not.
      - False
      - True, False
    * - constrain
      - whether to use ``xtb`` constraints for the geometry optimization or not. The constraints should be provided as a file ``constraints.xtb`` in the working directory.
      - False
      - True, False


Refinement
----------

.. list-table:: Refinement Settings
    :widths: 30 100 30 30
    :header-rows: 1

    * - keyword
      - description
      - default
      - allowed options
    * - threshold
      - the threshold for the additive Boltzmann population of the ensemble beyond which conformers will be removed from the ensemble.
      - 0.95
      - 
    * - func
      - the functional/dispersion correction combination used for this step.
      - wb97x-d3
      - :ref:`censo_funcs`
    * - basis 
      - the basis set used for this step. This will be ignored if the chosen functional is a composite functional.
      - def2-TZVP
      - :ref:`censo_bs`
    * - prog 
      - program that should be used for this step
      - tm
      - orca, tm
    * - sm 
      - solvation model used for this step.
      - smd
      - smd, cpcm, cosmo, dcosmors, cosmors, cosmors-fine
    * - gfnv
      - Variant of GFN that should be used for xtb calculations in this step.
      - gfn2
      - gfnff, gfn1, gfn2
    * - run
      - when using the command line interface, it tells CENSO whether to run this part or not.
      - True
      - True, False
    * - template
      - whether to use a user defined template for this step.
      - False
      - True, False
    * - implicit
      - whether to calculate the solvation contribution to Gtot implicitely (True) or not (False). If set to True, only one single-point needs to be calculated in this step.
      - True
      - True, False


NMR
---

.. list-table:: NMR Settings
    :widths: 30 100 30 30
    :header-rows: 1

    * - keyword
      - description
      - default
      - allowed options
    * - resonance_frequency
      - carrier frequency of the microwave radiation in the simulated NMR experiment
      - 300.0
      - 
    * - ss_cutoff
      - cutoff radius for the calculation of spin-spin couplings. Pairs with a larger distance than ss_cutoff will be neglected (only for ORCA).
      - 8.0
      - 
    * - prog
      - program that should be used to calculate the shielding/coupling single-points.
      - orca
      - orca, tm
    * - func_j
      - the functional/dispersion correction combination used in calculating the couplings.
      - pbe0-d4
      - :ref:`censo_funcs`
    * - basis_j
      - basis set used in calculating the couplings. This will be ignored if the chosen functional is a composite functional.
      - def2-TZVP
      - :ref:`censo_bs`
    * - sm_j
      - solvation model used in the calculation of the couplings.
      - smd
      - smd, cpcm, cosmo, dcosmors
    * - func_s
      - the functional/dispersion correction combination used in calculating the shieldings.
      - pbe0-d4
      - :ref:`censo_funcs`
    * - basis_s
      - basis set used in calculating the shieldings. This will be ignored if the chosen functional is a composite functional.
      - def2-TZVP
      - :ref:`censo_bs`
    * - sm_s
      - solvation model used in the calculation of the shieldings.
      - smd
      - smd, cpcm, cosmo, dcosmors
    * - run
      - when using the command line interface, it tells CENSO whether to run this part or not.
      - False
      - True, False
    * - template
      - whether to use a user defined template for this step.
      - False
      - True, False
    * - couplings
      - whether to compute the coupling constants.
      - True
      - True, False
    * - shieldings
      - whether to compute the shieldings.
      - True
      - True, False
    * - fc_only
      - whether to calculate only the Fermi-Contact term for spin-spin couplings.
      - True 
      - True, False
    * - h_active
      - whether to calculate NMR parameters for Protium.
      - True
      - True, False
    * - c_active
      - whether to calculate NMR parameters for 13C.
      - True
      - True, False
    * - f_active
      - whether to calculate NMR parameters for 19F.
      - False
      - True, False
    * - si_active
      - whether to calculate NMR parameters for 29Si.
      - False
      - True, False
    * - p_active
      - whether to calculate NMR parameters for 31P.
      - False
      - True, False

UV/Vis
------

      
.. list-table:: UV/Vis Settings
    :widths: 30 100 30 30
    :header-rows: 1

    * - keyword
      - description
      - default
      - allowed options
    * - nroots
      - number of roots sought for TD-DFT.
      - 20
      - 
    * - prog
      - program that should be used to calculate the shielding/coupling single-points.
      - orca
      - orca
    * - func
      - the functional/dispersion correction combination used for TD-DFT.
      - wb97x-d4
      - :ref:`censo_funcs`
    * - basis
      - basis set used for TD-DFT. This will be ignored if the chosen functional is a composite functional.
      - def2-TZVP
      - :ref:`censo_bs`
    * - sm
      - solvation model used for TD-DFT.
      - smd
      - smd, cpcm
    * - run
      - when using the command line interface, it tells CENSO whether to run this part or not.
      - False
      - True, False
    * - template
      - whether to use a user defined template for this step.
      - False
      - True, False


Paths Settings 
--------------

.. list-table:: Paths Settings 
   :widths: 30 100 
   :header-rows: 1 

   * - setting 
     - description 
   * - orcapath 
     - absolute path to the ``orca`` binary. 
   * - orcaversion 
     - version of ORCA you're using, e.g. 5.0.4.
   * - xtbpath 
     - absolute path to the ``xtb`` binary.
   * - mpshiftpath
     - absolute path to the ``mpshift`` binary (TURBOMOLE).
   * - escfpath
     - absolute path to the ``escf`` binary (TURBOMOLE).
   * - cefinepath
     - absolute path to the ``cefine`` binary.
   * - cosmothermpath
     - absolute path to the ``cosmotherm`` binary (COSMOthermX).
   * - cosmorssetup
     - the name of the parameterization file to use for COSMO-RS runs, e.g. ``BP_TZVP_C30_1601.ctd``.


All remaining entries are unused for now. CENSO tries to determine the paths of the binaries 
automatically when creating a new rcfile.
