.. _CENSO_setup:

Setting up CENSO
================

The easiest approach is to download a compiled CENSO binary from the release page: 
`https://github.com/grimme-lab/CENSO/releases <https://github.com/grimme-lab/CENSO/releases>`_. 
There are two versions available ``censo`` and ``censo_w_cefine`` which contains the latest 
``cefine`` version (for TURBOMOLE users). The compiled binaries are created with 
pyinstaller and are linked against GLIBC version 2.19 and will work for GLIBC version 
2.19 and above.

Next make CENSO executable, create your global configuration file .censorc and 
adjust settings to your needs.

.. code:: sh

    $ chmod u+x censo_w_cefine
    $ mv censo_w_cefine ~/bin/censo

    $ cd ~/path_to_project/
    $ censo -newconfig
    $ mv censorc_new ~/.censorc

.. tabbed:: -->

    .. code:: sh

        $ cat ~/.censorc
        # shown in next tab

.. tabbed:: Showing file .censorc

    .. code:: sh

        $CENSO global configuration file: .censorc
        $VERSION:1.0.1 

        ORCA: /path/excluding/binary/
        ORCA version: 4.2.1
        GFN-xTB: /path/including/binary/xtb-binary
        CREST: /path/including/binary/crest-binary
        mpshift: /path/including/binary/mpshift-binary
        escf: /path/including/binary/escf-binary

        #COSMO-RS
        ctd = BP_TZVP_C30_1601.ctd cdir = "/software/cluster/COSMOthermX16/COSMOtherm/CTDATA-FILES" ldir = "/software/cluster/COSMOthermX16/COSMOtherm/CTDATA-FILES"
        $ENDPROGRAMS

        $CRE SORTING SETTINGS:
        $GENERAL SETTINGS:
        nconf: all                       # ['all', 'number e.g. 10 up to all conformers'] 
        charge: 0                        # ['number e.g. 0'] 
        unpaired: 0                      # ['number e.g. 0'] 
        solvent: gas                     # ['gas', 'acetone', 'acetonitrile', 'aniline', 'benzaldehyde', 'benzene', 'ccl4', '...'] 
        prog_rrho: xtb                   # ['xtb', 'prog'] 
        temperature: 298.15              # ['temperature in K e.g. 298.15'] 
        trange: [273.15, 378.15, 5]      # ['temperature range [start, end, step]'] 
        multitemp: on                    # ['on', 'off'] 
        evaluate_rrho: on                # ['on', 'off'] 
        consider_sym: off                # ['on', 'off'] 
        bhess: on                        # ['on', 'off'] 
        imagthr: automatic               # ['automatic or e.g., -100    # in cm-1'] 
        sthr: automatic                  # ['automatic or e.g., 50     # in cm-1'] 
        scale: automatic                 # ['automatic or e.g., 1.0 '] 
        rmsdbias: off                    # ['on', 'off'] 
        sm_rrho: alpb                    # ['alpb', 'gbsa'] 
        check: on                        # ['on', 'off'] 
        prog: tm                         # ['tm', 'orca'] 
        func: r2scan-3c                  # ['b97-3c', 'b97-d', 'b97-d3', 'pbe', 'pbeh-3c', 'r2scan-3c', 'tpss'] 
        basis: automatic                 # ['automatic', 'def2-TZVP', 'def2-mSVP', 'def2-mTZVP', 'def2-mTZVP', '...'] 
        maxthreads: 1                    # ['number of threads e.g. 2'] 
        omp: 1                           # ['number cores per thread e.g. 4'] 
        cosmorsparam: automatic          # ['automatic', '12-fine', '12-normal', '13-fine', '13-normal', '14-fine', '...'] 

        $PART0 - CHEAP-PRESCREENING - SETTINGS:
        part0: on                        # ['on', 'off'] 
        func0: b97-d                     # ['b97-3c', 'b97-d', 'b97-d3', 'pbe', 'pbeh-3c', 'r2scan-3c', 'tpss'] 
        basis0: def2-SV(P)               # ['automatic', 'def2-TZVP', 'def2-mSVP', 'def2-mTZVP', 'def2-mTZVP', '...'] 
        part0_gfnv: gfn2                 # ['gfn1', 'gfn2', 'gfnff'] 
        part0_threshold: 4.0             # ['number e.g. 4.0'] 

        $PART1 - PRESCREENING - SETTINGS:
        # func and basis is set under GENERAL SETTINGS
        part1: on                        # ['on', 'off'] 
        smgsolv1: cosmors                # ['alpb_gsolv', 'cosmo', 'cosmors', 'cosmors-fine', 'cpcm', 'dcosmors', '...'] 
        part1_gfnv: gfn2                 # ['gfn1', 'gfn2', 'gfnff'] 
        part1_threshold: 3.5             # ['number e.g. 5.0'] 

        $PART2 - OPTIMIZATION - SETTINGS:
        # func and basis is set under GENERAL SETTINGS
        part2: on                        # ['on', 'off'] 
        opt_limit: 2.5                   # ['number e.g. 4.0'] 
        sm2: default                     # ['cosmo', 'cpcm', 'dcosmors', 'default', 'smd'] 
        smgsolv2: cosmors                # ['alpb_gsolv', 'cosmo', 'cosmors', 'cosmors-fine', 'cpcm', 'dcosmors', '...'] 
        part2_gfnv: gfn2                 # ['gfn1', 'gfn2', 'gfnff'] 
        ancopt: on                       # ['on'] 
        hlow: 0.01                       # ['lowest force constant in ANC generation, e.g. 0.01'] 
        opt_spearman: on                 # ['on', 'off'] 
        part2_threshold: 99              # ['Boltzmann sum threshold in %. e.g. 95 (between 1 and 100)'] 
        optlevel2: automatic             # ['crude', 'sloppy', 'loose', 'lax', 'normal', 'tight', 'vtight', 'extreme', '...'] 
        optcycles: 8                     # ['number e.g. 5 or 10'] 
        spearmanthr: -4.0                # ['value between -1 and 1, if outside set automatically'] 
        radsize: 10                      # ['number e.g. 8 or 10'] 
        crestcheck: off                  # ['on', 'off'] 

        $PART3 - REFINEMENT - SETTINGS:
        part3: off                       # ['on', 'off'] 
        prog3: prog                      # ['tm', 'orca', 'prog'] 
        func3: pw6b95                    # ['b97-d3', 'dsd-blyp', 'pbe0', 'pw6b95', 'r2scan-3c', 'wb97x'] 
        basis3: def2-TZVPD               # ['DZ', 'QZV', 'QZVP', 'QZVPP', 'SV(P)', 'SVP', 'TZVP', 'TZVPP', 'aug-cc-pV5Z', '...'] 
        smgsolv3: cosmors                # ['alpb_gsolv', 'cosmo', 'cosmors', 'cosmors-fine', 'cpcm', 'dcosmors', '...'] 
        part3_gfnv: gfn2                 # ['gfn1', 'gfn2', 'gfnff'] 
        part3_threshold: 99              # ['Boltzmann sum threshold in %. e.g. 95 (between 1 and 100)'] 

        $NMR PROPERTY SETTINGS:
        $PART4 SETTINGS:
        part4: off                       # ['on', 'off'] 
        couplings: on                    # ['on', 'off'] 
        progJ: prog                      # ['tm', 'orca', 'adf', 'prog'] 
        funcJ: pbe0                      # ['pbe0', 'pbeh-3c', 'r2scan-3c', 'tpss'] 
        basisJ: def2-TZVP                # ['DZ', 'QZV', 'QZVP', 'QZVPP', 'SV(P)', 'SVP', 'TZVP', 'TZVPP', 'aug-cc-pV5Z', '...'] 
        sm4J: default                    # ['cosmo', 'cpcm', 'dcosmors', 'smd'] 
        shieldings: on                   # ['on', 'off'] 
        progS: prog                      # ['tm', 'orca', 'adf', 'prog'] 
        funcS: pbe0                      # ['b97-3c', 'dsd-blyp', 'kt1', 'kt2', 'pbe0', 'pbeh-3c', 'r2scan-3c', 'tpss', '...'] 
        basisS: def2-TZVP                # ['DZ', 'QZV', 'QZVP', 'QZVPP', 'SV(P)', 'SVP', 'TZVP', 'TZVPP', 'aug-cc-pV5Z', '...'] 
        sm4S: default                    # ['cosmo', 'cpcm', 'dcosmors', 'smd'] 
        reference_1H: TMS                # ['TMS'] 
        reference_13C: TMS               # ['TMS'] 
        reference_19F: CFCl3             # ['CFCl3'] 
        reference_29Si: TMS              # ['TMS'] 
        reference_31P: TMP               # ['TMP', 'PH3'] 
        1H_active: on                    # ['on', 'off'] 
        13C_active: on                   # ['on', 'off'] 
        19F_active: off                  # ['on', 'off'] 
        29Si_active: off                 # ['on', 'off'] 
        31P_active: off                  # ['on', 'off'] 
        resonance_frequency: 300.0       # ['MHz number of your experimental spectrometer setup'] 

        $OPTICAL ROTATION PROPERTY SETTINGS:
        $PART5 SETTINGS:
        optical_rotation: off            # ['on', 'off'] 
        funcOR: pbe                      # ['functional for opt_rot e.g. pbe'] 
        funcOR_SCF: r2scan-3c            # ['functional for SCF in opt_rot e.g. r2scan-3c'] 
        basisOR: def2-SVPD               # ['basis set for opt_rot e.g. def2-SVPD'] 
        frequency_optical_rot: [589.0]   # ['list of freq in nm to evaluate opt rot at e.g. [589, 700]'] 
        $END CENSORC


Upon the first usage of CENSO a folder *~/.censo_assets/* will be created. It 
contains a file  *~/.censo_assets/censo_solvents.json* with information on all 
available solvents and solvent models. If a solvent is not available with a 
certain solvent model, the user can then choose a replacement solvent, e.g. 
if benzene is not available choose toluene. This file is directly used in censo 
and typos will cause the calculation with the repective solvent to crash. 
For further information see section :ref:`censo_solvation`.  

.. tabbed:: -->

    .. code:: sh

        $ cat ~/.censo_assets/censo_solvents.json
        # shown in next tab

.. tabbed:: Showing file .censo\_solvents.json

    .. code:: sh

        # CENSO solvents:
        # example:
        #{
        #    "solvent_name_used_in_censo":{ 
        #        "solvation_model": ["solvent_name_in_solvation_model", "solvent_name_in_solvation_model_which_is_applied"],
        #        "solvation_model2": [null _if_solvent_is_not_available, "replacement_solvent_in_solvation_model2"],
        #        "DC": 20.7 # dielectric constant used for COSMO + DCOSMO-RS
        #    }
        #}
        # end example

        {
            "acetone":{
                "cosmors": ["propanone_c0", "propanone_c0"],
                "dcosmors": ["propanone", "propanone"],
                "xtb": ["acetone", "acetone"],
                "cpcm": ["acetone", "acetone"],
                "smd": ["ACETONE", "ACETONE"],
                "DC": 20.7
            },
            "chcl3":{
                "cosmors": ["chcl3_c0", "chcl3_c0"],
                "dcosmors": ["chcl3", "chcl3"],
                "xtb": ["chcl3", "chcl3"],
                "cpcm": ["chloroform","chloroform"],
                "smd": ["CHLOROFORM", "CHLOROFORM"],
                "DC": 4.8
            },
            "acetonitrile":{
                "cosmors": ["acetonitrile_c0", "acetonitrile_c0"],
                "dcosmors": ["acetonitrile", "acetonitrile"],
                "xtb": ["acetonitrile", "acetonitrile"],
                "cpcm": ["acetonitrile", "acetonitrile"],
                "smd": ["ACETONITRILE", "ACETONITRILE"],
                "DC": 36.6
            },
            "ch2cl2":{
                "cosmors": ["ch2cl2_c0", "ch2cl2_c0"],
                "dcosmors": [null, "chcl3"],
                "xtb": ["ch2cl2", "ch2cl2"],
                "cpcm": ["CH2Cl2", "CH2Cl2"],
                "smd": ["DICHLOROMETHANE", "DICHLOROMETHANE"],
                "DC": 9.1
            },
            "dmso":{
                "cosmors": ["dimethylsulfoxide_c0", "dimethylsulfoxide_c0"],
                "dcosmors": ["dimethylsulfoxide", "dimethylsulfoxide"],
                "xtb": ["dmso", "dmso"],
                "cpcm": ["DMSO", "DMSO"],
                "smd": ["DIMETHYLSULFOXIDE", "DIMETHYLSULFOXIDE"],
                "DC": 47.2
            },
            "h2o":{
                "cosmors": ["h2o_c0", "h2o_c0"],
                "dcosmors": ["h2o", "h2o"],
                "xtb": ["h2o", "h2o"],
                "cpcm": ["Water", "Water"],
                "smd": ["WATER", "WATER"],
                "DC": 80.1
            },
            "methanol":{
                "cosmors": ["methanol_c0", "methanol_c0"],
                "dcosmors": ["methanol", "methanol"],
                "xtb": ["methanol", "methanol"],
                "cpcm": ["Methanol", "Methanol"],
                "smd": ["METHANOL", "METHANOL"],
                "DC": 32.7
            },
            "thf":{
                "cosmors": ["thf_c0", "thf_c0"],
                "dcosmors": ["thf", "thf"],
                "xtb": ["thf", "thf"],
                "cpcm": ["THF", "THF"],
                "smd": ["TETRAHYDROFURAN", "TETRAHYDROFURAN"],
                "DC": 7.6
            },
            "toluene":{
                "cosmors": ["toluene_c0", "toluene_c0"],
                "dcosmors": ["toluene", "toluene"],
                "xtb": ["toluene", "toluene"],
                "cpcm": ["Toluene", "Toluene"],
                "smd": ["TOLUENE", "TOLUENE"],
                "DC": 2.4
            },
            "octanol":{
                "cosmors": ["1-octanol_c0", "1-octanol_c0"],
                "dcosmors": ["octanol", "octanol"],
                "xtb": ["octanol", "octanol"],
                "cpcm": ["Octanol", "Octanol"],
                "smd": ["1-OCTANOL", "1-OCTANOL"],
                "DC": 9.9
            },
            "woctanol":{
                "cosmors": [null, "woctanol"],
                "dcosmors": ["wet-otcanol", "wet-octanol"],
                "xtb": ["woctanol", "woctanol"],
                "cpcm": [null, "Octanol"],
                "smd": [null, "1-OCTANOL"],
                "DC": 8.1
            },
            "hexadecane":{
                "cosmors": ["n-hexadecane_c0", "n-hexadecane_c0"],
                "dcosmors": ["hexadecane", "hexadecane"],
                "xtb": ["hexadecane", "hexadecane"],
                "cpcm": [null, "Hexane"],
                "smd": ["N-HEXADECANE", "N-HEXADECANE"],
                "DC": 2.1
            },
            "dmf":{
                "cosmors": ["dimethylformamide_c0","dimethylformamide_c0"],
                "dcosmors": [null, "dimethylsulfoxide"],
                "xtb": ["dmf", "dmf"],
                "cpcm": ["DMF", "DMF"],
                "smd": ["N,N-DIMETHYLFORMAMIDE", "N,N-DIMETHYLFORMAMIDE"],
                "DC": 38.3
            },
            "aniline":{
                "cosmors": ["aniline_c0", "aniline_c0"],
                "dcosmors": ["aniline", "aniline"],
                "xtb": ["aniline", "aniline"],
                "cpcm": [null,"Pyridine"],
                "smd": ["ANILINE", "ANILINE"],
                "DC": 6.9
            },
            "cyclohexane":{
                "cosmors": ["cyclohexane_c0", "cyclohexane_c0"],
                "dcosmors": ["cyclohexane", "cyclohexane"],
                "xtb": [null, "hexane"],
                "cpcm": ["Cyclohexane", "Cyclohexane"],
                "smd": ["CYCLOHEXANE", "CYCLOHEXANE"],
                "DC": 2.0
            },
            "ccl4":{
                "cosmors": ["ccl4_c0", "ccl4_c0"],
                "dcosmors": ["ccl4", "ccl4"],
                "xtb": ["ccl4", "ccl4"],
                "cpcm": ["CCl4", "CCl4"],
                "smd": ["CARBON TETRACHLORIDE", "CARBON TETRACHLORIDE"],
                "DC": 2.2
            },
            "diethylether":{
                "cosmors": ["diethylether_c0", "diethylether_c0"],
                "dcosmors": ["diethylether", "diethylether"],
                "xtb": ["ether", "ether"],
                "cpcm": [null, "THF"],
                "smd": ["DIETHYL ETHER", "DIETHYL ETHER"],
                "DC": 4.4
            },
            "ethanol":{
                "cosmors": ["ethanol_c0", "ethanol_c0"],
                "dcosmors": ["ethanol", "ethanol"],
                "xtb": ["ethanol", "ethanol"],
                "cpcm": [null, "Methanol"],
                "smd": ["ETHANOL", "ETHANOL"],
                "DC": 24.6
            },
            "hexane":{
                "cosmors": ["hexane_c0", "hexane_c0"],
                "dcosmors": ["hexane", "hexane"],
                "xtb": ["hexane", "hexane"],
                "cpcm": ["Hexane", "Hexane"],
                "smd": ["N-HEXANE", "N-HEXANE"],
                "DC": 1.9
            },
            "nitromethane":{
                "cosmors": ["nitromethane_c0", "nitromethane_c0"],
                "dcosmors": ["nitromethane", "nitromethane"],
                "xtb": ["nitromethane", "nitromethane"],
                "cpcm": [null, "methanol"],
                "smd": "",
                "DC": 38.2
            },
            "benzaldehyde":{
                "cosmors": ["benzaldehyde_c0", "benzaldehyde_c0"],
                "dcosmors": [null, "propanone"],
                "xtb": ["benzaldehyde", "benzaldehyde"],
                "cpcm": [null, "Pyridine"],
                "smd": ["BENZALDEHYDE", "BENZALDEHYDE"],
                "DC": 18.2
            },
            "benzene":{
                "cosmors": ["benzene_c0", "benzene_c0"],
                "dcosmors": [null, "toluene"],
                "xtb": ["benzene", "benzene"],
                "cpcm": ["Benzene", "Benzene"],
                "smd": ["BENZENE", "BENZENE"],
                "DC": 2.3
            },
            "cs2":{
                "cosmors": ["cs2_c0", "cs2_c0"],
                "dcosmors": [null, "ccl4"],
                "xtb": ["cs2", "cs2"],
                "cpcm": [null, "CCl4"],
                "smd": ["CARBON DISULFIDE", "CARBON DISULFIDE"],
                "DC": 2.6
            },
            "dioxane":{
                "cosmors": ["dioxane_c0", "dioxane_c0"],
                "dcosmors": [null, "diethylether"],
                "xtb": ["dioxane", "dioxane"],
                "cpcm": [null, "Cyclohexane"],
                "smd": ["1,4-DIOXANE", "1,4-DIOXANE"],
                "DC": 2.2
            },
            "ethylacetate":{
                "cosmors": ["ethylacetate_c0", "ethylacetate_c0"],
                "dcosmors": [null, "diethylether"],
                "xtb": ["ethylacetate", "ethylacetate"],
                "cpcm": [null, "THF"],
                "smd": ["ETHYL ETHANOATE", "ETHYL ETHANOATE"],
                "DC": 5.9
            },
            "furan":{
                "cosmors": ["furane_c0", "furane_c0"],
                "dcosmors": [null, "diethylether"],
                "xtb": ["furane", "furane"],
                "cpcm": [null, "THF"],
                "smd": [null, "THF"],
                "DC": 3.0
            },
            "phenol":{
                "cosmors": ["phenol_c0", "phenol_c0"],
                "dcosmors": [null, "thf"],
                "xtb": ["phenol", "phenol"],
                "cpcm": [null, "THF"],
                "smd": [null, "THIOPHENOL"],
                "DC": 8.0
            }
        }


Get additional Information:
---------------------------

Some information is already contained in ``censo`` and can be accessed by running:

.. tabbed:: -->

    .. code:: sh

        $ censo --help
        # explaination of all possible command line arguments
        # shown in next tab

.. tabbed:: command line arguments

    .. code:: sh
    
                 ______________________________________________________________
                |                                                              |
                |                                                              |
                |                   CENSO - Commandline ENSO                   |
                |                           v 1.0.1                            |
                |    energetic sorting of CREST Conformer Rotamer Ensembles    |
                |                    University of Bonn, MCTC                  |
                |                           Feb 2021                           |
                |                 based on ENSO version 2.0.1                  |
                |                     F. Bohle and S. Grimme                   |
                |                                                              |
                |______________________________________________________________|

                This program is distributed in the hope that it will be useful,
                but WITHOUT ANY WARRANTY; without even the implied warranty of
                MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

        optional arguments:
          -h, --help            show this help message and exit

        GENERAL SETTINGS:
          -inp , --input        Input name of ensemble file: e.g. crest_conformers.xyz
          -nc , --nconf         Number of conformers which are going to be considered
                                (max number of conformers are all conformers from the
                                input file).
          -chrg , --charge      Charge of the investigated molecule.
          -u , --unpaired       Integer number of unpaired electrons of the
                                investigated molecule.
          -T , --temperature    Temperature in Kelvin for thermostatistical
                                evaluation.
          -multitemp , --multitemp 
                                Needs to be turned on if a temperature range should be
                                evaluated (flag trange). Options for multitemp are:
                                ['on' or 'off'].
          -trange start end step, --trange start end step
                                specify a temperature range [start, end, step] e.g.:
                                250.0 300.0 10.0 resulting in [250.0, 260.0, 270.0,
                                280.0, 290.0].
          -bhess , --bhess      Applies structure constraint to input/DFT geometry for
                                mRRHO calcuation.Options are: ['on' or 'off'].
          -consider_sym , ---consider_sym 
                                Consider symmetry in mRRHO calcuation (based on desy
                                xtb threshold).Options are: ['on' or 'off'].
          -rmsdbias , --rmsdbias 
                                Applies constraint to rmsdpot.xyz to be consistent to
                                CREST.Options are: ['on' or 'off'].
          -sm_rrho , --sm_rrho 
                                Solvation model used in xTB GmRRHO calculation.
                                Applied if not in gas-phase. Options are 'gbsa' or
                                'alpb'.
          -evaluate_rrho , --evaluate_rrho 
                                Evaluate mRRHO contribution. Options: on or off.
          -func , --functional 
                                Functional for geometry optimization (used in part2)
                                and single-points in part1
          -basis , --basis      Basis set employed together with the functional (func)
                                for the low level single point in part1 und
                                optimization in part2.
          -checkinput, --checkinput
                                Option to check if all necessary information for the
                                ENSO calculation are provided and check if certain
                                setting combinations make sence. Option to choose from
                                : ['on' or 'off']
          -solvent , --solvent 
                                Solvent the molecule is solvated in, available
                                solvents are: ['gas', 'acetone', 'acetonitrile',
                                'aniline', 'benzaldehyde', 'benzene', 'ccl4',
                                'ch2cl2', 'chcl3', 'cs2', 'cyclohexane',
                                'diethylether', 'dioxane', 'dmf', 'dmso', 'ethanol',
                                'ethylacetate', 'furan', 'h2o', 'hexadecane',
                                'hexane', 'methanol', 'nitromethane', 'octanol',
                                'phenol', 'thf', 'toluene', 'woctanol']. They can be
                                extended in the file
                                ~/.censo_assets/censo_solvents.json .
          -prog , --prog        QM-program used in part1 and part2 either 'orca' or
                                'tm'.
          -prog_rrho , --prog_rrho 
                                QM-program for mRRHO contribution in part1 2 and 3,
                                either 'xtb' or 'prog'.
          -crestcheck , --crestcheck 
                                Option to sort out conformers after DFT optimization
                                which CREST identifies as identical or rotamers of
                                each other. The identification/analysis is always
                                performed, but the removal of conformers has to be the
                                choice of the user. Options are: ['on' or 'off']
          -check {on,off}, --check {on,off}
                                Option to terminate the ENSO-run if too many
                                calculations/preparation steps fail. Options are:
                                ['on' or 'off'].
          -version, --version   Print CENSO version and exit.
          -part3only, --part3only
                                Option to turn off part1 and part2
          -cosmorsparam , --cosmorsparam 
                                Choose a COSMO-RS parametrization for possible COSMO-
                                RS G_solv calculations: e.g. 19-normal for
                                'BP_TZVP_19.ctd' or 16-fine for
                                'BP_TZVPD_FINE_C30_1601.ctd'.

        SPECIAL RUN MODES:
          -logK, --logK         Automatically set required settings for logK
                                calculation. Of course charge, solvent etc. has to be
                                set by the user.

        CRE CHEAP-PRESCREENING - PART0:
          -part0 , --part0      Option to turn the CHEAP prescreening evaluation
                                (part0) which improves description of ΔE 'on' or
                                'off'.
          -func0 , --func0      Functional for fast single-point (used in part0)
          -basis0 , --basis0    Basis set employed together with the functional
                                (func0) for the fast single point calculation in
                                part0.
          -part0_gfnv , --part0_gfnv 
                                GFNn-xTB version employed for calculating the gas
                                phase GFNn-xTB single point in part0. Allowed values
                                are [gfn1, gfn2, gfnff]
          -part0_threshold , -thrpart0 , --thresholdpart0 
                                Threshold in kcal/mol. All conformers in part0 (cheap
                                single-point) with a relativ energy below the
                                threshold are considered for part1.

        CRE PRESCREENING - PART1:
          -part1 , --part1      Option to turn the prescreening evaluation (part1)
                                'on' or 'off'.
          -smgsolv1 , --smgsolv1 
                                Solvent model for the Gsolv evaluation in part1. This
                                can either be an implicit solvation or an additive
                                solvation model. Allowed values are [alpb_gsolv,
                                cosmo, cosmors, cosmors-fine, cpcm, dcosmors,
                                gbsa_gsolv, sm2, smd, smd_gsolv]
          -part1_gfnv , --part1_gfnv 
                                GFNn-xTB version employed for calculating the mRRHO
                                contribution in part1. Allowed values are [gfn1, gfn2,
                                gfnff]
          -part1_threshold , -thrpart1 , --thresholdpart1 
                                Threshold in kcal/mol. All conformers in part1
                                (lax_single-point) with a relativ energy below the
                                threshold are considered for part2.

        CRE OPTIMIZATION - PART2:
          -part2 , --part2      Option to turn the full optimization (part2) 'on' or
                                'off'.
          -sm2 , --solventmodel2 
                                Solvent model employed during the geometry
                                optimization part2.The solvent model sm2 is not used
                                for Gsolv evaluation, but for the implicit effect on a
                                property (e.g. the optimization).
          -smgsolv2 , --smgsolv2 
                                Solvent model for the Gsolv calculation in part2.
                                Either the solvent model of the optimization (sm) or
                                an additive solvation model. Allowed values are
                                [alpb_gsolv, cosmo, cosmors, cosmors-fine, cpcm,
                                dcosmors, gbsa_gsolv, sm2, smd, smd_gsolv]
          -part2_gfnv , --part2_gfnv 
                                GFNn-xTB version employed for calculating the mRRHO
                                contribution in part2. Allowed values are [gfn1, gfn2,
                                gfnff]
          -ancopt               Option to use xtb as driver for the xTB-optimizer in
                                part2.
          -opt_spearman         Option to use an optimizer which checks if the
                                hypersurface of DFT andxTB is parallel and optimizes
                                mainly low lying conformers
          -optlevel2 , --optlevel2 
                                Option to set the optlevel in part2, only if
                                optimizing with the xTB-optimizer!Allowed values are
                                crude, sloppy, loose, lax, normal, tight, vtight,
                                extreme, automatic
          -optcycles , --optcycles 
                                number of cycles in ensemble optimizer.
          -hlow , --hlow        Lowest force constant in ANC generation (real), used
                                by xTB-optimizer.
          -spearmanthr , --spearmanthr 
                                Value between -1 and 1 for the spearman correlation
                                coeffient threshold
          -opt_limit , --opt_limit 
                                Lower limit Threshold in kcal/mol. If the GFNn and DFT
                                hypersurfaces areassumed parallel, the conformers
                                above the threshold are removed and not optimized
                                further.The conformers in part2 with a relativ free
                                energy below the threshold are fully optimized.
          -thrpart2 , --thresholdpart2 , -part2_threshold 
                                Boltzmann population sum threshold for part2 in %. The
                                conformers with the highest Boltzmann weigths are
                                summed up until the threshold is reached.E.g. all
                                conformers up to a Boltzmann population of 90 % are
                                considered.Example usage: "-thrpart2 99" --> considers
                                a population of 99 %
          -radsize , --radsize 
                                Radsize used in optimization and only for r2scan-3c!

        CRE REFINEMENT - PART3:
          -part3 , --part3      Option to turn the high level free energy evaluation
                                (part3) 'on' or 'off'.
          -prog3 , --prog3      QM-program used in part3 either 'orca' or 'tm'.
          -func3 , --functionalpart3 
                                Functional for the COSMO-RS calculation, use
                                functional names as recognized by cefine.
          -basis3 , --basis3    Basis set employed together with the functional
                                (func3) for the high level single point in part3.
          -smgsolv3 , --smgsolv3 
                                Solvent model for the Gsolv calculation in part3.
                                Either the solvent model of the optimization (sm2) or
                                an additive solvation model.
          -part3_gfnv , --part3_gfnv 
                                GFNn-xTB version employed for calculating the mRRHO
                                contribution in part3. Allowed values are [gfn1, gfn2,
                                gfnff]
          -thrpart3 , --thresholdpart3 
                                Boltzmann population sum threshold for part3 in %. The
                                conformers with the highest Boltzmann weigths are
                                summed up until the threshold is reached.E.g. all
                                conformers up to a Boltzmann population of 90 % are
                                consideredExample usage: "-thrpart3 99" --> considers
                                a population of 99 %

        NMR Mode:
          -part4 , --part4      Option to turn the NMR property calculation mode
                                (part4) 'on' or 'off'.
          -couplings , --couplings 
                                Option to run coupling constant calculations. Options
                                are 'on' or 'off'.
          -prog4J , --prog4J    QM-program for the calculation of coupling constants.
          -funcJ , --funcJ      Functional for the coupling constant calculation.
          -basisJ , --basisJ    Basis set for the calculation of coupling constants.
          -sm4_j , --sm4_j      Solvation model used in the coupling constant
                                calculation.
          -shieldings , --shieldings 
                                Option to run shielding constant calculations. Options
                                are 'on' or 'off'.
          -prog4S , --prog4S    QM-program for the calculation of shielding constants.
          -funcS , --funcS      Functional for shielding constant calculation.
          -basisS , --basisS    Basis set for the calculation of shielding constants.
          -sm4_s , --sm4_s      Solvation model used in the shielding constant
                                calculation.
          -hactive , --hactive 
                                Investigates hydrogen nuclei in coupling and shielding
                                calculations.choices=['on', 'off']
          -cactive , --cactive 
                                Investigates carbon nuclei in coupling and shielding
                                calculations.choices=['on', 'off']
          -factive , --factive 
                                Investigates fluorine nuclei in coupling and shielding
                                calculations.choices=['on', 'off']
          -siactive , --siactive 
                                Investigates silicon nuclei in coupling and shielding
                                calculations.choices=['on', 'off']
          -pactive , --pactive 
                                Investigates phosophorus nuclei in coupling and
                                shielding calculations.choices=['on', 'off']

        OPTICAL ROTATION MODE:
          -OR , --OR , -part5   Do optical rotation calculation.
          -funcOR , --funcOR    Functional for optical rotation calculation.
          -funcOR_SCF , --funcOR_SCF 
                                Functional used in SCF for optical rotation
                                calculation.
          -basisOR , --basisOR 
                                Basis set for optical rotation calculation.
          -freqOR [ [ ...]], --freqOR [ [ ...]]
                                Frequencies to evaluate specific rotation at in nm.
                                E.g. 589 Or 589 700 to evaluate at 598 nm and 700 nm.

        OPTIONS FOR PARALLEL CALCULATIONS:
          -O , --omp            Number of cores each thread can use. E.g. (maxthreads)
                                5 threads with each (omp) 4 cores --> 20 cores need to
                                be available on the machine.
          -P , --maxthreads     Number of independent calculations during the ENSO
                                calculation. E.g. (maxthreads) 5 independent
                                calculation- threads with each (omp) 4 cores --> 20
                                cores need to be available on the machine.

        Concerning overall mRRHO calculations:
          -imagthr , --imagthr 
                                threshold for inverting imaginary frequencies for
                                thermo in cm-1. (e.g. -30.0)
          -sthr , --sthr        Rotor cut-off for thermo in cm-1. (e.g. 50.0)
          -scale , --scale      scaling factor for frequencies (e.g. 1.0)

        CREATION/DELETION OF FILES:
          --cleanup, -cleanup   Delete unneeded files from current working directory.
          --cleanup_all, -cleanup_all
                                Delete all unneeded files from current working
                                directory. Stronger than -cleanup !
          -newconfig, -write_censorc, --write_censorc
                                Write new configuration file, which is placed into the
                                current directory.
          -inprc INPRCPATH, --inprc INPRCPATH
                                Path to the destination of the configuration file
                                .censorc
          -tutorial, --tutorial
                                Start interactive CENSO documentation.


.. tabbed:: --> 
    :new-group:

    .. code:: sh

        $ censo -tutorial
        # general explainations
        # shown in next tab

.. tabbed:: Interactive Documentation:

        .. code:: sh

                     ______________________________________________________________
                    |                                                              |
                    |                                                              |
                    |                   CENSO - Commandline ENSO                   |
                    |                           v 1.0.1                            |
                    |    energetic sorting of CREST Conformer Rotamer Ensembles    |
                    |                    University of Bonn, MCTC                  |
                    |                           Feb 2021                           |
                    |                 based on ENSO version 2.0.1                  |
                    |                     F. Bohle and S. Grimme                   |
                    |                                                              |
                    |______________________________________________________________|

                    This program is distributed in the hope that it will be useful,
                    but WITHOUT ANY WARRANTY; without even the implied warranty of
                    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

            This is the CENSO tutorial / interactive documentation:

            Topic options are:
                general
                censorc
                setup
                thresholds
                solvation
                examples
                files
                jobscript
                everything

            To exit please type one of the following: exit or q

            Please input your information request:

            ....

Requirements
------------

.. note:: 

    CENSO interfaces to other codes (ORCA, TURBOMOLE, COSMOtherm). They are not part of CENSO!


CENSO needs other programs in certain versions and will not work properly without them:

* xTB in version 6.4.0 or above
* TM in version 7.5.x or above (when using r2scan-3c)
* ORCA in version 4.x or above
* cefine in the newest version, when using TURBOMOLE (or use `censo_w_cefine`)

Run CENSO on a cluster
----------------------

When submitting a calculation on a cluster architecture the following points 
have to be considered:

* Are the program paths in your .censorc file correct (ORCA, xTB, CREST, COSMO-RS)
* Is the correct TURBOMOLE version sourced in your job-submission file and are 
  the correct environment variables for parallelization set?
* provide the correct number of available cores to CENSO (P, maxthreads) * 
  (O,omp) = number of cores
* CENSO will generate a lot of data for each conformer. This data is stored in the 
  CONFX (X=number) folders. **If you restart and resubmit a calculation to the cluster,
  you have to tell your submission script to copy these folders.**

.. hint:: 

    CENSO can not be parallelized over several nodes!

.. dropdown:: Example job-submission script

    .. code:: sh

        #!/bin/bash
        # PBS Job
        #PBS -V
        #PBS -N JOB_NAME
        #PBS -m ae
        #PBS -q batch
        #PBS -l nodes=1:ppn=28
        # 
        cd $PBS_O_WORKDIR

        ### setup programs
        ## XTB
        export OMP_NUM_THREADS=1
        export MKL_NUM_THREADS=1
        ulimit -s unlimited
        export OMP_STACKSIZE=1000m

        ## TM
        export PARA_ARCH=SMP
        source /home/$USER/bin/.turbo751
        export PARNODES=4  ## omp 
        export TM_PAR_FORK=1

        ### ORCA4.2.1
        ORCAPATH="/tmp1/orca_4_2_1_linux_x86-64_openmpi216";
        MPIPATH="/software/openmpi-2.1.5/bin";
        MPILIB="/software/openmpi-2.1.5/lib64";
        PATH=${ORCAPATH}:${MPIPATH}:$PATH 
        LD_LIBRARY_PATH=${ORCAPATH}:${MPILIB}:$LD_LIBRARY_PATH
        LD_LIBRARY_PATH=/software/intel/parallel_studio_xe_2017.1/parallel_studio_xe_2017.4.056/compilers_and_libraries_2017/linux/compiler/lib/intel64_lin:$LD_LIBRARY_PATH
        LD_LIBRARY_PATH=/software/intel/parallel_studio_xe_2017/mkl/lib/intel64:$LD_LIBRARY_PATH
        export LD_LIBRARY_PATH

        ## PATH
        PATH=/home/$USER/bin:$PATH
        export PATH
        ### end programs + PATH

        export HOSTS_FILE=$PBS_NODEFILE
        cat $HOSTS_FILE>hosts_file

        TMP_DIR=/tmp1/$USER
        DIR1=$PWD

        mkdir -p $TMP_DIR/$PBS_JOBID

        #check file system access
        if [ ! -d $TMP_DIR/$PBS_JOBID ]; then
         echo "Unable to create $TMP_DIR/$PBS_JOBID  on $HOSTNAME. Must stop."
         exit
        fi

        #check current location
        if [ "$PWD" == "$HOME" ]; then
         echo "Cowardly refusing to copy the whole home directory"
         exit
        fi

        #copy everything to node (will NOT copy directories for safety reasons.
        #Add an 'r' only if absolutely sure what you are doing)
        #bwlimit limits bandwidth to 5000 kbytes/sec

         rsync -q --bwlimit=5000 $DIR1/* $TMP_DIR/$PBS_JOBID/
         rsync -rq --ignore-missing-args --bwlimit=5000 $DIR1/CONF* $TMP_DIR/$PBS_JOBID/
         rsync -q --bwlimit=5000 $DIR1/.* $TMP_DIR/$PBS_JOBID/
         cd $TMP_DIR/$PBS_JOBID

        ####################################################################################
        #Gettimings
        start=$(date +%s)
        #####################################################################################
        #jobs start here (if you have no idea what this script does, only edit this part...)

        echo "Calculation from $(date)" >> RUNTIME
        export PYTHONUNBUFFERED=1

        censo -inp inputfile.xyz -P 7 -O 4 > censo.out

        #end of job      (....and stop editing here.)
        #####################################################################################
        #Print timings to file
        end=$(date +%s)
        secs=$(expr $end - $start)
        printf '%dh:%dm:%02ds\n' $(($secs/3600)) $(($secs%3600/60)) $(($secs%60)) >> RUNTIME
        #####################################################################################
        #copy everything back that is smaller than 5 gbytes

         rsync -rq --bwlimit=5000 --max-size=5G $TMP_DIR/$PBS_JOBID/* $DIR1/
         rsync -q --bwlimit=5000 --max-size=5G $TMP_DIR/$PBS_JOBID/.* $DIR1/

        #to be safe, get mos alpha and beta seperately. 
        #Note that the rsync syntax is strange; you need to first include everything, 
        #then exclude the rest ("*" includes subdirectories)

         rsync -rq --bwlimit=5000 --include="*/" --include="mos" --include="alpha" --include="beta" --exclude=* $TMP_DIR/$PBS_JOBID/* $DIR1/

        #if you want the large files as well, comment in the following

        #rsync -r --bwlimit=1000 --min-size=5G $TMP_DIR/$PBS_JOBID/* $DIR1/

         cd $DIR1
         rm -r $TMP_DIR/$PBS_JOBID
    
    .. hint:: The program paths in your job-submission script have to be adjusted to your local environment
