.. _censorc:

===========================
Censorc keyword definitions
===========================

.. contents::

General Settings
----------------


.. list-table:: general settings
    :widths: 30 100
    :header-rows: 1
    
    * - keyword
      - definition
    * - nconf
      - how many conformers should be considered. Either a number or the flag *all*.
    * - charge
      - molecular charge of the molecule under investigation.
    * - unpaired
      - number of unpaired electrons in the molecule under investigation.
    * - solvent
      - Solvent if the molecule is in solution phase, else *gas*.
    * - prog_rrho
      - QM-code used for the calculation of thermostatistical contributions.
        This is only feasible with xtb, since normally a large number of hessian
        calculations have to be performed.
    * - temperature
      - Temperature (in Kelvin) used for the Boltzmann evaluations.
    * - trange
      - temperature range which is used to calculate free energies at different 
        temperatures (considered in G\_mRRHO and δG\_solv[only COSMO-RS]). 
        The temperature range will only be evaluated if multitemp is set to *on*.
    * - multitemp
      - Evaluate free energies at different temperatures defined in trange.
    * - evaluate_rrho
      - Option to consider /not consider thermostatistical contributions.
    * - consider_sym
      - Option to consider symmetry in the thermostatistical contribution (only xtb)
    * - bhess
      - Calculate single point hessians (SPH) on the geometry, instead of 
        "ohess" (optimization + hessian calculation)
    * - imagthr
      - threshold for inverting imaginary frequencies for thermostatistical 
        contributions (in cm\ :sup:`-1` \). Internal defaults are applied if set to *automatic*.
    * - sthr
      - rotor cut-off (in cm\ :sup:`-1` \) used for the thermostatical contributions. 
        Internal defaults are applied if set to *automatic*.
    * - scale
      - scaling factor for frequencies in vibrational partition function. 
        Internal defaults are applied if set to *automatic*.
    * - rmsdbias
      - gESC related, using rmsdpot.xyz to be consistent to CREST.
    * - sm\_rrho
      - solvent model applied in the GFN\ *n*\-xTB thermostatistical contribution calculation.
    * - check
      - Terminate the CENSO run if too many calculations crash.
    * - prog
      - QM code used for part0, part1 and part2, this can be TURBOMOLE or ORCA.
    * - func
      - functional used in part1 (prescreening) and part2 (optimization)
    * - basis
      - basis set used in combination with func in part1 (prescreening) and 
        part2 (optimization). If basis is set to *automatic* the basis set is 
        chosen internally.
    * - maxthreads
      - Used for parallel calculation. Maxthreads determines the number of independent 
        calculations running in parallel. E.g. resulting in 4 independent 
        single-point /optimization calculations.
    * - omp
      - Used for parallel calculation. Omp determines the number of cores each 
        independent calculation can use. Eg. maxthreads = 4 and omp = 5 resulting 
        in 4 independent calculations and each independent calculation uses 5 cores.
    * - cosmorsparam
      - Flag for choosing COSMO-RS parameterizations. If set to *automatic*
        the input from the COSMO-RS input line is chosen.

Part0 - Cheap-Prescreening - Settings
-------------------------------------

.. list-table:: part0
    :widths: 30 100
    :header-rows: 1

    * - keyword
      - definition
    * - part0
      - Option to turn the "cheap prescreening part" on or off.
    * - func0
      - Functional used in part0.
    * - basis0
      - Basis set used in combination with func0. If basis0 is set to *automatic*
        the basis set is chosen internally.
    * - part0_gfnv
      - GFN version employed in the thermostatistical contribution in part0.
    * - part0\_threshold
      - Threshold/Energy-window (kcal/mol) within which all conformers are considered.


Part1 - Prescreening - Settings
-------------------------------

.. list-table:: part1
    :widths: 30 100
    :header-rows: 1

    * - keyword
      - definition
    * - part1
      - Option to turn the "prescreening part" on or off.
    * - smgsolv1
      - Additive solvation contribution employed in part1.
    * - part1_gfnv
      - GFN version employed in the thermostatistical contribution in part1.
    * - part1_threshold
      - Threshold/Energy-window (kcal/mol) within which all conformers are 
        considered further.


Part2 - Optimization - Settings
-------------------------------

.. list-table:: part2
    :widths: 30 100
    :header-rows: 1

    * - keyword
      - definition
    * - part2
      - Option to turn the "optimization" part on or off.
    * - opt_limit
      - Threshold/Energy-window (kcal/mol) within which all conformers are fully
        optimized.
    * - sm2
      - Implicit solvation model used in the optimization (for the implicit
        effect on the geometry).
    * - smgsolv2
      - Additive solvation model used for calculation of δG_solv in part2
        (used to calculate contribution to free energy).
    * - part2_gfnv
      - GFN version employed in the thermostatistical (G_mRRHO) contribution in
        part2.
    * - ancopt
      - Using ANCoptimizer implemented in xTB for geometry optimization.
    * - hlow
      - Lowest force constant in ANC generation, used with ancopt.
    * - opt_spearman
      - Using the new *ensemble-optimizer*, employing batch-wise metacycles.
    * - part2_threshold
      - Boltzmann threshold in % within which all conformers are considered further.
        E.g. 90 %; all conformers up to a sum of 90 % are considered.
    * - optlevel2
      - Optimization threshold in the geometry optimization. If set to *automatic*
        internal defaults will be used.
    * - optcycles
      - Number of optimization iterations performed within one cycle in the ensemble
        optimizer.
    * - spearmanthr
      - Spearman rank correlation coeff. used to determine if PES during geometry
        optimization can be assumed parallel.
    * - radsize
      - Setting of the radial grid size for func used in part2.
    * - crestcheck
      - Automatically sort out conformers which might have become identical or
        rotamers during DFT geometry optimization. Check is performed using CREST
        (this is threshold based, so use with care).


Part3 - Refinement - Settings
-----------------------------

.. list-table:: part3
    :widths: 30 100
    :header-rows: 1

    * - keyword
      - definition
    * - part3
      - Option to turn the "refinement" part *on* or *off*.
    * - prog3
      - QM code used for part3 this can be TURBOMOLE or ORCA.
    * - func3
      - functional used in part3 (refinement)
    * - basis3
      - basis set employed in combination with func3. If basis3 is set to 
        *automatic* the basis set is chosen internally (mainly for composite methods).
    * - smgsolv3
      - Additive solvation model used for calculation of δG_solv in part3.
    * - part3_gfnv
      - GFN version employed in the thermostatistical contribution in part3.
    * - part3_threshold
      - Boltzmann threshold in % within which all conformers are considered further.
        E.g. 90 %, all conformers up to a sum of 90 % are considered.


Part4 - NMR- Settings
---------------------

.. list-table:: part4
    :widths: 30 100
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
