.. _usage_examples:

Usage examples
==============

CENSO can be used for several applications / target quantities. Some are listed below:

.. hint::

    CENSO has sorting "parts" which can be turned *on* and *off*. The parts are run 
    in sequence and the sorting-results are influenced by the choice of 
    sorting-parts employed. 
    If for example the optimization part (part2) is not performed, then all subsequent 
    parts will calculate free energies or properties on the input SQM/FF geometries 
    (not DFT optimized geometries)!
    Each part contains thresholds (see :ref:`Threshold <censo_thresholds>`) and choosing 
    to low (free) energy windows (in the early sorting parts) will affect your 
    final ensemble / averaged free energy / property. 

.. note::

    For the demonstration purpose it is assumed that all parts are turned *off* in 
    the global configuration file of the user!


Calculate fast DFT\(B97-D3(0)/def2-SV(P)+gcp) single-point energies on GFN\ *n*\ -xTB input geometries
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

.. hint::
    Useful in case of large structure ensembles (SE). Very efficient (fast) 
    improvement on the electronic energy description compared to the initial 
    SQM/FF energies. High lying conformers are quickly sorted out.

.. tabbed:: input

    .. code:: sh

        $ censo -inp ensemble.xyz -part0 on -chrg 1 -solvent h2o > censo.out &

.. tabbed:: output

    .. code:: sh

                     ______________________________________________________________
                    |                                                              |
                    |                                                              |
                    |                   CENSO - Commandline ENSO                   |
                    |                           v 1.0.3                            |
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


            ----------------------------------------------------------------------------------------------------
                                                         PARAMETERS                                             
            ----------------------------------------------------------------------------------------------------

            The configuration file .censorc is read from /home/bohle/1projects/from_tmp1/CENSO/documentation-calcs/1-part0/.censorc.
            Reading conformer rotamer ensemble from: /home/bohle/1projects/from_tmp1/CENSO/documentation-calcs/1-part0/ensemble.xyz.
            Reading file: censo_solvents.json


            --------------------------------------------------
                           CRE SORTING SETTINGS               
            --------------------------------------------------

            number of atoms in system:                                     25
            number of considered conformers:                               22
            number of all conformers from input:                           22
            charge:                                                        1
            unpaired:                                                      0
            solvent:                                                       h2o
            temperature:                                                   298.15
            evaluate at different temperatures:                            on
            temperature range:                                             273.15, 278.15, 283.15, 288.15, ...
            calculate mRRHO contribution:                                  on
            consider symmetry for mRRHO contribution:                      off
            cautious checking for error and failed calculations:           on
            checking the DFT-ensemble using CREST:                         off
            maxthreads:                                                    2
            omp:                                                           2

            --------------------------------------------------
                      CRE CHEAP-PRESCREENING - PART0          
            --------------------------------------------------
            part0:                                                         on
            starting number of considered conformers:                      22
            program for part0:                                             tm
            functional for fast single-point:                              b97-d
            basis set for fast single-point:                               def2-SV(P)
            threshold g_thr(0) for sorting in part0:                       4.0
            Solvent model used with xTB:                                   alpb

            short-notation:
            b97-d-D3/def2-SV(P) // GFNn-xTB (Input geometry)
            END of parameters


            ------------------------------------------------------------
                           PATHS of external QM programs                
            ------------------------------------------------------------

            The following program paths are used:
                xTB:          /home/abt-grimme/AK-bin/xtb
                TURBOMOLE:    /home/abt-grimme/TURBOMOLE.7.5.1/bin/em64t-unknown-linux-gnu_smp

                Using cefine from /home/bohle/bin/cefine
                PARNODES for TM or COSMO-RS calculation was set to 2

            ----------------------------------------------------------------------------------------------------
                                        Processing data from previous run (enso.json)                           
            ----------------------------------------------------------------------------------------------------

            INFORMATION: No restart information exists and is created during this run!


            ----------------------------------------------------------------------------------------------------
                                               CRE CHEAP-PRESCREENING - PART0                                   
            ----------------------------------------------------------------------------------------------------

            program:                                                       tm
            functional for part0:                                          b97-d
            basis set for part0:                                           def2-SV(P)
            threshold g_thr(0):                                            4.0
            starting number of considered conformers:                      22

            Calculating efficient gas-phase single-point energies:
            The efficient gas-phase single-point is calculated for:
             CONF1,  CONF2,  CONF3,  CONF4,  CONF5,  CONF6,  CONF7,  CONF8,  CONF9, CONF10, CONF11
            CONF12, CONF13, CONF14, CONF15, CONF16, CONF17, CONF18, CONF19, CONF20, CONF21, CONF22
            Constructed folders!

            Starting 22 ALPB-Gsolv calculations
            Running single-point in CONF1/part0_sp    
            Running single-point in CONF2/part0_sp    
            Running ALPB_GSOLV calculation in 1-part0/CONF1/part0_sp
            Running ALPB_GSOLV calculation in 1-part0/CONF2/part0_sp
            Running single-point in CONF3/part0_sp    
            Running single-point in CONF4/part0_sp    
            Running ALPB_GSOLV calculation in 1-part0/CONF3/part0_sp
            Running ALPB_GSOLV calculation in 1-part0/CONF4/part0_sp
            Running single-point in CONF5/part0_sp    
            Running single-point in CONF6/part0_sp    
            Running ALPB_GSOLV calculation in 1-part0/CONF5/part0_sp
            Running ALPB_GSOLV calculation in 1-part0/CONF6/part0_sp
            Running single-point in CONF7/part0_sp    
            Running single-point in CONF8/part0_sp    
            Running ALPB_GSOLV calculation in 1-part0/CONF7/part0_sp
            Running ALPB_GSOLV calculation in 1-part0/CONF8/part0_sp
            Running single-point in CONF9/part0_sp    
            Running single-point in CONF10/part0_sp   
            Running ALPB_GSOLV calculation in 1-part0/CONF9/part0_sp
            Running ALPB_GSOLV calculation in 1-part0/CONF10/part0_sp
            Running single-point in CONF11/part0_sp   
            Running single-point in CONF12/part0_sp   
            Running ALPB_GSOLV calculation in 1-part0/CONF11/part0_sp
            Running ALPB_GSOLV calculation in 1-part0/CONF12/part0_sp
            Running single-point in CONF13/part0_sp   
            Running single-point in CONF14/part0_sp   
            Running ALPB_GSOLV calculation in 1-part0/CONF13/part0_sp
            Running ALPB_GSOLV calculation in 1-part0/CONF14/part0_sp
            Running single-point in CONF15/part0_sp   
            Running single-point in CONF16/part0_sp   
            Running ALPB_GSOLV calculation in 1-part0/CONF15/part0_sp
            Running ALPB_GSOLV calculation in 1-part0/CONF16/part0_sp
            Running single-point in CONF17/part0_sp   
            Running single-point in CONF18/part0_sp   
            Running ALPB_GSOLV calculation in 1-part0/CONF17/part0_sp
            Running ALPB_GSOLV calculation in 1-part0/CONF18/part0_sp
            Running single-point in CONF19/part0_sp   
            Running single-point in CONF20/part0_sp   
            Running ALPB_GSOLV calculation in 1-part0/CONF19/part0_sp
            Running ALPB_GSOLV calculation in 1-part0/CONF20/part0_sp
            Running single-point in CONF21/part0_sp   
            Running single-point in CONF22/part0_sp   
            Running ALPB_GSOLV calculation in 1-part0/CONF21/part0_sp
            Running ALPB_GSOLV calculation in 1-part0/CONF22/part0_sp
            Tasks completed!

            The efficient gas-phase single-point was successful for  CONF1/part0_sp: E(DFT) = -496.55110270 Gsolv = -0.12279245
            The efficient gas-phase single-point was successful for  CONF2/part0_sp: E(DFT) = -496.55473079 Gsolv = -0.12241044
            The efficient gas-phase single-point was successful for  CONF3/part0_sp: E(DFT) = -496.55551554 Gsolv = -0.12099969
            The efficient gas-phase single-point was successful for  CONF4/part0_sp: E(DFT) = -496.55384665 Gsolv = -0.12156811
            The efficient gas-phase single-point was successful for  CONF5/part0_sp: E(DFT) = -496.55341065 Gsolv = -0.12251050
            The efficient gas-phase single-point was successful for  CONF6/part0_sp: E(DFT) = -496.55368828 Gsolv = -0.12175678
            The efficient gas-phase single-point was successful for  CONF7/part0_sp: E(DFT) = -496.54759208 Gsolv = -0.12365913
            The efficient gas-phase single-point was successful for  CONF8/part0_sp: E(DFT) = -496.55118920 Gsolv = -0.12200372
            The efficient gas-phase single-point was successful for  CONF9/part0_sp: E(DFT) = -496.54970422 Gsolv = -0.12167549
            The efficient gas-phase single-point was successful for CONF10/part0_sp: E(DFT) = -496.55157272 Gsolv = -0.12001541
            The efficient gas-phase single-point was successful for CONF11/part0_sp: E(DFT) = -496.54991969 Gsolv = -0.12296934
            The efficient gas-phase single-point was successful for CONF12/part0_sp: E(DFT) = -496.55212770 Gsolv = -0.11751864
            The efficient gas-phase single-point was successful for CONF13/part0_sp: E(DFT) = -496.55077718 Gsolv = -0.11854291
            The efficient gas-phase single-point was successful for CONF14/part0_sp: E(DFT) = -496.55028792 Gsolv = -0.12090071
            The efficient gas-phase single-point was successful for CONF15/part0_sp: E(DFT) = -496.54987374 Gsolv = -0.12311773
            The efficient gas-phase single-point was successful for CONF16/part0_sp: E(DFT) = -496.55360018 Gsolv = -0.11274054
            The efficient gas-phase single-point was successful for CONF17/part0_sp: E(DFT) = -496.55289531 Gsolv = -0.11378052
            The efficient gas-phase single-point was successful for CONF18/part0_sp: E(DFT) = -496.50423172 Gsolv = -0.16468285
            The efficient gas-phase single-point was successful for CONF19/part0_sp: E(DFT) = -496.52384365 Gsolv = -0.14390846
            The efficient gas-phase single-point was successful for CONF20/part0_sp: E(DFT) = -496.50481476 Gsolv = -0.16519450
            The efficient gas-phase single-point was successful for CONF21/part0_sp: E(DFT) = -496.53860751 Gsolv = -0.13212346
            The efficient gas-phase single-point was successful for CONF22/part0_sp: E(DFT) = -496.50938650 Gsolv = -0.15826546

            ----------------------------------------------------------------------------------------------------
                               Removing high lying conformers by improved energy description                    
            ----------------------------------------------------------------------------------------------------

             CONF#       G [Eh] ΔG [kcal/mol]                 E [Eh]   Gsolv [Eh]         Gtot    ΔE(DFT)     ΔGsolv      ΔGtot
                       GFN2-xTB      GFN2-xTB b97-d-D3(0)/def2-SV(P)         alpb         [Eh] [kcal/mol] [kcal/mol] [kcal/mol]
                         [ALPB]        [ALPB]                              [gfn2]                                              
            CONF1   -34.1746819          0.00           -496.5511027   -0.1227924 -496.6738951       2.28      -0.24       2.04
            CONF2   -34.1743917          0.18           -496.5547308   -0.1224104 -496.6771412       0.00       0.00       0.00     <------
            CONF3   -34.1742792          0.25           -496.5555155   -0.1209997 -496.6765152      -0.49       0.89       0.39
            CONF4   -34.1739821          0.44           -496.5538467   -0.1215681 -496.6754148       0.55       0.53       1.08
            CONF5   -34.1740224          0.41           -496.5534106   -0.1225105 -496.6759212       0.83      -0.06       0.77
            CONF6   -34.1736309          0.66           -496.5536883   -0.1217568 -496.6754451       0.65       0.41       1.06
            CONF7   -34.1725555          1.33           -496.5475921   -0.1236591 -496.6712512       4.48      -0.78       3.70
            CONF8   -34.1721876          1.57           -496.5511892   -0.1220037 -496.6731929       2.22       0.26       2.48
            CONF9   -34.1719439          1.72           -496.5497042   -0.1216755 -496.6713797       3.15       0.46       3.62
            CONF10  -34.1714765          2.01           -496.5515727   -0.1200154 -496.6715881       1.98       1.50       3.48
            CONF11  -34.1712638          2.14           -496.5499197   -0.1229693 -496.6728890       3.02      -0.35       2.67
            CONF12  -34.1704841          2.63           -496.5521277   -0.1175186 -496.6696463       1.63       3.07       4.70
            CONF13  -34.1703687          2.71           -496.5507772   -0.1185429 -496.6693201       2.48       2.43       4.91
            CONF14  -34.1694191          3.30           -496.5502879   -0.1209007 -496.6711886       2.79       0.95       3.74
            CONF15  -34.1691431          3.48           -496.5498737   -0.1231177 -496.6729915       3.05      -0.44       2.60
            CONF16  -34.1664418          5.17           -496.5536002   -0.1127405 -496.6663407       0.71       6.07       6.78
            CONF17  -34.1661652          5.34           -496.5528953   -0.1137805 -496.6666758       1.15       5.42       6.57
            CONF18  -34.1660003          5.45           -496.5042317   -0.1646828 -496.6689146      31.69     -26.53       5.16
            CONF19  -34.1664801          5.15           -496.5238437   -0.1439085 -496.6677521      19.38     -13.49       5.89
            CONF20  -34.1658387          5.55           -496.5048148   -0.1651945 -496.6700093      31.32     -26.85       4.48
            CONF21  -34.1670164          4.81           -496.5386075   -0.1321235 -496.6707310      10.12      -6.10       4.02
            CONF22  -34.1656180          5.69           -496.5093865   -0.1582655 -496.6676520      28.45     -22.50       5.95
            ----------------------------------------------------------------------------------------------------

            --------------------------------------------------
                      Conformers considered further           
            --------------------------------------------------

            These conformers are below the 4.000 kcal/mol g_thr(0) threshold.

             CONF1,  CONF2,  CONF3,  CONF4,  CONF5,  CONF6,  CONF7,  CONF8,  CONF9, CONF10, CONF11
            CONF14, CONF15


            Calculating Boltzmann averaged (free) energy of ensemble on input geometries (not DFT optimized)!

            temperature /K:   avE(T) /a.u.   avG(T) /a.u. 
            ----------------------------------------------------------------------------------------------------
                298.15        -496.5544580    -496.6764445     <<==part0==
            ----------------------------------------------------------------------------------------------------


            >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>END of Part0<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<
            Ran part0 in 32.8727 seconds


            Part                : #conf    time
            --------------------------------------------------
            Input               :    22    -
            Part0_all           :    13    32.87s
            --------------------------------------------------
            All parts           :          32.87s

            CENSO all done!


.. tabbed:: .censorc

    .. code:: sh

            $CENSO global configuration file: .censorc
            $VERSION:1.0.3 

            ORCA: /home/USER/orca_4_2_1_linux_x86-64_openmpi216
            ORCA version: 4.2.1
            GFN-xTB: xtb
            CREST: crest
            mpshift: mpshift
            escf: escf

            #COSMO-RS
            ctd = BP_TZVPD_FINE_C30_1601.ctd  cdir = "/home/USER/COSMOlogic/COSMOthermX19/COSMOtherm/CTDATA-FILES" ldir = "/home/USER/COSMOlogic/COSMOthermX19/COSMOtherm/CTDATA-FILES"
            cosmothermversion: 19
            $ENDPROGRAMS

            $CRE SORTING SETTINGS:
            $GENERAL SETTINGS:
            nconf: all                       # ['all', 'number e.g. 10 up to all conformers'] 
            charge: 0                        # ['number e.g. 0'] 
            unpaired: 0                      # ['number e.g. 0'] 
            solvent: gas                     # ['gas', 'acetone', 'chcl3', 'acetonitrile', 'ch2cl2', 'dmso', 'h2o', 'methanol', 'thf', '...'] 
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
            func: r2scan-3c                  # ['pbe', 'b97-d', 'pbeh-3c', 'tpss', 'b97-d3', 'r2scan-3c', 'b97-3c'] 
            basis: automatic                 # ['automatic', 'def2-mSVP', 'def2-mTZVP', 'def2-mTZVP', 'def2-TZVP', '...'] 
            maxthreads: 2                    # ['number of threads e.g. 2'] 
            omp: 2                           # ['number cores per thread e.g. 4'] 
            cosmorsparam: automatic          # ['automatic', '12-fine', '12-normal', '13-fine', '13-normal', '14-fine', '...']

            $PART0 - CHEAP-PRESCREENING - SETTINGS:
            part0: off                        # ['on', 'off'] 
            func0: b97-d                     # ['pbeh-3c', 'b97-3c', 'b97-d3', 'pbe', 'r2scan-3c', 'tpss', 'b97-d'] 
            basis0: def2-SV(P)               # ['automatic', 'def2-mSVP', 'def2-mTZVP', 'def2-mTZVP', 'def2-TZVP', '...'] 
            part0_gfnv: gfn2                 # ['gfn1', 'gfn2', 'gfnff'] 
            part0_threshold: 4.0             # ['number e.g. 4.0'] 

            $PART1 - PRESCREENING - SETTINGS:
            # func and basis is set under GENERAL SETTINGS
            part1: off                        # ['on', 'off'] 
            smgsolv1: cosmors                # ['alpb_gsolv', 'dcosmors', 'cosmo', 'smd', 'cosmors-fine', 'cpcm', 'gbsa_gsolv', '...'] 
            part1_gfnv: gfn2                 # ['gfn1', 'gfn2', 'gfnff'] 
            part1_threshold: 3.5             # ['number e.g. 5.0'] 

            $PART2 - OPTIMIZATION - SETTINGS:
            # func and basis is set under GENERAL SETTINGS
            part2: off                        # ['on', 'off'] 
            opt_limit: 2.5                   # ['number e.g. 4.0'] 
            sm2: dcosmors                    # ['cosmo', 'cpcm', 'default', 'smd', 'dcosmors'] 
            smgsolv2: cosmors                # ['cosmors-fine', 'gbsa_gsolv', 'cosmo', 'cpcm', 'smd_gsolv', 'alpb_gsolv', 'smd', '...'] 
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
            func3: pw6b95                    # ['dsd-blyp', 'pw6b95', 'b97-d3', 'r2scan-3c', 'pbe0', 'wb97x'] 
            basis3: def2-TZVPD               # ['SVP', 'SV(P)', 'TZVP', 'TZVPP', 'QZVP', 'QZVPP', 'def2-SV(P)', 'def2-mSVP', '...'] 
            smgsolv3: cosmors                # ['alpb_gsolv', 'dcosmors', 'cosmo', 'smd', 'cosmors-fine', 'cpcm', 'gbsa_gsolv', '...'] 
            part3_gfnv: gfn2                 # ['gfn1', 'gfn2', 'gfnff'] 
            part3_threshold: 99              # ['Boltzmann sum threshold in %. e.g. 95 (between 1 and 100)'] 

            $NMR PROPERTY SETTINGS:
            $PART4 SETTINGS:
            part4: off                       # ['on', 'off'] 
            couplings: on                    # ['on', 'off'] 
            progJ: prog                      # ['tm', 'orca', 'adf', 'prog'] 
            funcJ: pbe0                      # ['tpss', 'pbe0', 'pbeh-3c'] 
            basisJ: def2-TZVP                # ['SVP', 'SV(P)', 'TZVP', 'TZVPP', 'QZVP', 'QZVPP', 'def2-SV(P)', 'def2-mSVP', '...'] 
            sm4J: default                    # ['dcosmors', 'smd', 'cosmo', 'cpcm'] 
            shieldings: on                   # ['on', 'off'] 
            progS: prog                      # ['tm', 'orca', 'adf', 'prog'] 
            funcS: pbe0                      # ['dsd-blyp', 'pbeh-3c', 'tpss', 'pbe0', 'kt2'] 
            basisS: def2-TZVP                # ['SVP', 'SV(P)', 'TZVP', 'TZVPP', 'QZVP', 'QZVPP', 'def2-SV(P)', 'def2-mSVP', '...'] 
            sm4S: default                    # ['dcosmors', 'smd', 'cosmo', 'cpcm'] 
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

.. tabbed:: input-ensemble.xyz

    .. code:: sh

              25
                    -34.15995484
             O         -2.7404108553       -0.9210263756        0.0505546155
             O         -0.6532148483       -1.7303331963        0.1381884568
             N         -2.1811530948        1.4662136641       -0.3280604715
             N          1.9999042791       -1.6115147219       -0.1737452561
             C          1.2832974514        1.7494376339        0.2478643443
             C          0.0382242755        1.0547713107        0.8044626435
             C          2.1972182535        0.8461961224       -0.5852172042
             C         -0.9624505804        0.6276788621       -0.2713633850
             C          2.7304045776       -0.3664138808        0.1827513585
             C         -1.5037598584       -0.8390454546        0.0048901917
             H          1.8561477225        2.1402514306        1.0892838382
             H          0.9852377848        2.6009114549       -0.3642611073
             H         -0.4518270956        1.7282098745        1.5089809935
             H          0.3322545004        0.1775252924        1.3825123935
             H          1.6820235078        0.5059218506       -1.4832357051
             H          3.0380251642        1.4546200515       -0.9165284484
             H         -0.4868586610        0.6089411942       -1.2530945560
             H          2.6388482788       -0.2021893439        1.2562535001
             H          3.7853986884       -0.5188774904       -0.0473087348
             H         -2.2380899515        2.1426216511        0.4407329422
             H         -2.3218532286        1.9466721688       -1.2205963738
             H          2.3394829642       -2.3992859458        0.3882351555
             H          0.9350292280       -1.5547588441       -0.0165809300
             H          2.1475890354       -1.8368760327       -1.1636223077
             H         -2.9328940343        0.6971914121       -0.1921053926
              25
                    -34.15944839
             O         -2.5910945174       -0.7526707388        0.4737473250
             O         -0.5258609320       -1.6055473558        0.4259946366
             N         -2.1883047577        1.2429283468       -0.9276187078
             N          2.0221224751       -1.5482527990       -0.3635876013
             C          1.0959612422        0.9269941773        1.0928268792
             C         -0.1118183900        1.5287127427        0.3646185610
             C          2.3883955950        0.8232150730        0.2771806877
             C         -0.9137052232        0.5859776609       -0.5447048684
             C          2.3984857923       -0.1999516224       -0.8542391926
             C         -1.3796771114       -0.7342464594        0.2052799197
             H          0.8202702309       -0.0346561349        1.5230052762
             H          1.3150554091        1.5881707649        1.9329336393
             H          0.2252898792        2.3837647705       -0.2251981715
             H         -0.7970460540        1.8894152433        1.1356879771
             H          2.6174364896        1.7989347868       -0.1517789136
             H          3.1942064704        0.5825472961        0.9724285437
             H         -0.3499369029        0.3059081761       -1.4335569624
             H          3.4048136146       -0.2402683769       -1.2749539435
             H          1.7122153210        0.0852818535       -1.6492945642
             H         -2.2438155060        2.2226460139       -0.6308218074
             H         -2.4094139539        1.1700691932       -1.9248068681
             H          0.9927667520       -1.5675551657       -0.0428313977
             H          2.1377830502       -2.2542373686       -1.0973459319
             H          2.6054467969       -1.8083924261        0.4393088723
             H         -2.8796747565        0.6310313638       -0.3580585669
              25
                    -34.15925304
             O         -2.6152400314       -1.0465052003        0.2378304729
             O         -0.5362082855       -1.4663149505       -0.4593671015
             N         -2.4557715395        1.4039918868        0.4362491058
             N          2.1447268012       -1.5911295291       -0.0908488427
             C          1.1849792140        1.1291603967       -0.7123125066
             C          0.0549239500        1.3190958463        0.3109988160
             C          2.5516538580        0.8614163999       -0.0796419105
             C         -1.2770304521        0.8481909298       -0.2699298296
             C          2.6278492228       -0.4408272994        0.7143702153
             C         -1.4798499370       -0.7275247362       -0.1594413730
             H          1.2638776944        2.0288103522       -1.3231621409
             H          0.9216680773        0.3153070715       -1.3864974308
             H         -0.0132519646        2.3752873127        0.5749134480
             H          0.2604586805        0.7576942571        1.2223482952
             H          3.2969117377        0.8423750312       -0.8755852689
             H          2.8115908685        1.6810094994        0.5898815386
             H         -1.3353089850        1.1014685130       -1.3310936133
             H          2.0240292821       -0.3842292575        1.6193674755
             H          3.6638250279       -0.6160286136        1.0070988283
             H         -2.2160156909        1.7709567167        1.3631094761
             H         -2.9723151833        2.1102297821       -0.0929227069
             H          2.6253531272       -1.6216026098       -0.9974699479
             H          2.3064470721       -2.4761725884        0.4009650983
             H          1.0998172314       -1.5111491960       -0.2664030290
             H         -3.0342449423        0.4895941432        0.5473101358
              25
                    -34.15909539
             O         -2.5859531813       -0.8098009525        0.4685129327
             O         -0.4816245052       -1.5436026390        0.3102868940
             N         -2.3339104715        1.3007756781       -0.7836622820
             N          2.0492564794       -1.5871472251       -0.5900906398
             C          1.1052196056        0.9870949301        0.9271737021
             C         -0.2140772115        1.5925038037        0.4451868362
             C          2.1667497593        0.8464686753       -0.1771749747
             C         -1.0154302176        0.6861215046       -0.4984780600
             C          2.9073785151       -0.4823039670       -0.0859386105
             C         -1.3840762559       -0.7076318190        0.1707141950
             H          0.8920163207        0.0201435075        1.3815317896
             H          1.4930774855        1.6249540613        1.7213050551
             H         -0.0067258320        2.5368714102       -0.0630175124
             H         -0.8282372023        1.7972548662        1.3249938991
             H          1.7135886317        0.9350726379       -1.1636100949
             H          2.8915967677        1.6540129472       -0.0882838850
             H         -0.4677225963        0.5007855037       -1.4227063242
             H          3.1688824777       -0.6847294678        0.9524589517
             H          3.8237876908       -0.4492687854       -0.6762385697
             H         -2.5870605138        1.3036833133       -1.7755142934
             H         -2.9689761577        0.6014426661       -0.2449843511
             H          1.9723283573       -1.5417442554       -1.6118847971
             H          2.4360322309       -2.5007606995       -0.3327999527
             H          1.0548817542       -1.5294780724       -0.1881461707
             H         -2.4296444696        2.2452869585       -0.3976639893
              25
                    -34.15892503
             O         -2.4565204045       -0.8423454324        0.7070928712
             O         -0.5980888346       -1.6084799899       -0.2741310513
             N         -2.2081439307        1.5499049911        0.0797136762
             N          2.0665657616       -1.5738488309        0.1665385721
             C          0.8489992162        1.1021214800        0.5754910780
             C          0.1319038004        1.1643519245       -0.7806607655
             C          2.3611287197        0.8895018711        0.4504901307
             C         -1.3084663819        0.6750096988       -0.7114456668
             C          2.7727042231       -0.3622083435       -0.3226992137
             C         -1.4583757981       -0.7537800183       -0.0274928276
             H          0.4155006230        0.3036523983        1.1797441237
             H          0.7020515096        2.0340597672        1.1228295970
             H          0.6304477123        0.5210588124       -1.5026223849
             H          0.1602968624        2.1778026423       -1.1819014190
             H          2.8058653217        1.7473421346       -0.0537220547
             H          2.7810957945        0.8489865537        1.4562868383
             H         -1.7106345304        0.5638398621       -1.7216356404
             H          3.8478441803       -0.5048945341       -0.2050768697
             H          2.5630334022       -0.2516058916       -1.3860041489
             H         -2.8269415029        2.1396743466       -0.4823076953
             H         -2.7642911303        0.7978509993        0.6164252281
             H          2.4375926491       -2.4160896797       -0.2859521821
             H          2.1793786728       -1.6693140839        1.1823811296
             H          1.0245787819       -1.5392035408       -0.0507409777
             H         -1.6858363886        2.1297513428        0.7452520673
              25
                    -34.15874648
             O         -2.3605981473       -0.8116042758        0.8659409274
             O         -0.5843274484       -1.6230194998       -0.2244865945
             N         -2.2109877661        1.5364887021        0.0742080589
             N          2.0757147084       -1.4672125485       -0.4830865528
             C          0.8597549995        1.2262238823        0.4517476395
             C          0.0874032105        1.1513231513       -0.8771132949
             C          2.3512317789        0.8985689618        0.2982329522
             C         -1.3361657088        0.6347191585       -0.7132778341
             C          2.6824356905       -0.5712975375        0.5355813092
             C         -1.4234260659       -0.7566236862        0.0528412632
             H          0.4221004807        0.5356789162        1.1753502177
             H          0.7733707657        2.2309740830        0.8650565898
             H          0.5827203837        0.4802075195       -1.5756071724
             H          0.0631682297        2.1335262804       -1.3514203271
             H          2.7003072358        1.2129278451       -0.6855204577
             H          2.9172351560        1.4667500260        1.0360745366
             H         -1.7833398578        0.4616073570       -1.6955611361
             H          2.3142140534       -0.8770131145        1.5154931358
             H          3.7656889000       -0.7005750162        0.5151167425
             H         -1.6671444199        2.1735125324        0.6661186569
             H         -2.8827343086        2.0695764162       -0.4835506438
             H          2.3643871815       -1.1879653946       -1.4273853498
             H          2.3725094498       -2.4360036549       -0.3237631449
             H          1.0071911744       -1.4659435853       -0.4227446705
             H         -2.7090441201        0.8070062032        0.6949537917
              25
                    -34.15780648
             O         -0.6998712600       -1.7595821198        0.3315266042
             O         -2.7564980797       -0.8922468771        0.1127087169
             N         -2.0912345729        1.3897515697       -0.6207661035
             N          1.9218668343       -1.5538051846       -0.0911139226
             C          1.2724611881        1.7665596691        0.2614262584
             C         -0.0126245071        1.0963347719        0.7686474620
             C          2.5091659413        0.8642624497        0.2746224048
             C         -0.9098130242        0.5407794733       -0.3415020783
             C          2.4929985644       -0.3196472927       -0.6935656161
             C         -1.5185682699       -0.8616584773        0.0830057999
             H          1.4961397665        2.6156227165        0.9073501428
             H          1.1219744652        2.1603919352       -0.7434483203
             H         -0.5894557289        1.8123867185        1.3552290309
             H          0.2456564204        0.2915541326        1.4587568868
             H          3.3629575544        1.4905499194        0.0148013745
             H          2.6790096911        0.5030846620        1.2896168914
             H         -0.3423105707        0.3842564461       -1.2596402044
             H          3.5203242755       -0.5424656780       -0.9864282797
             H          1.9321769951       -0.0751719518       -1.5947809502
             H         -2.8797605313        0.6907420298       -0.3760235139
             H         -2.1503924665        2.2120232522       -0.0104200923
             H          2.3803263882       -1.7503210865        0.8055549093
             H          0.8561607870       -1.5268128372        0.0892049642
             H          2.0800598725       -2.3543999415       -0.7123089418
             H         -2.1774862389        1.6805852373       -1.5986092278
              25
                    -34.15712409
             O         -0.5169415866       -1.4677953372       -0.4155166026
             O         -2.6718105717       -1.1029442546        0.0532698902
             N         -2.5041882719        1.3222450456        0.5356135683
             N          2.0547500815       -1.4660199090        0.3952674841
             C          1.1460672076        1.2350620947       -0.5905524477
             C          0.0199991306        1.2471137062        0.4571994904
             C          2.5324119546        0.9449216441        0.0010319703
             C         -1.3028765754        0.8219664596       -0.1753086903
             C          2.9759584940       -0.4928036507       -0.2449354813
             C         -1.5027299259       -0.7570468322       -0.1889883845
             H          1.1567284127        2.2014747982       -1.0932584224
             H          0.9206483903        0.4869950226       -1.3509226784
             H         -0.0730555237        2.2557886152        0.8619704582
             H          0.2503077610        0.5750867956        1.2847618985
             H          3.2724883861        1.5930094341       -0.4660504111
             H          2.5411017268        1.1667575642        1.0680525607
             H         -1.3439398378        1.1490834108       -1.2170635960
             H          3.9809942022       -0.6416689206        0.1522940223
             H          2.9906906878       -0.6917352586       -1.3167852841
             H         -3.1147358709        0.4302326474        0.4978236161
             H         -2.3055244586        1.5552316017        1.5147727778
             H          1.0589542040       -1.3722826387        0.0237179616
             H          2.3537713222       -2.4274392985        0.2004063016
             H          2.0384435533       -1.3281897829        1.4119124615
             H         -2.9671283162        2.1123945703        0.0792841186
              25
                    -34.15703719
             O         -0.4840204654       -1.5908390855       -0.1977054306
             O         -2.6424172363       -1.0172969494       -0.2313294170
             N         -2.3824391815        1.4118161237        0.1129756179
             N          2.0591606020       -1.4080559447        0.6444993748
             C          1.3255949204        1.5932768744        0.1316443482
             C          0.0292286137        1.1405631347        0.8180158808
             C          1.9611296764        0.5914767293       -0.8382657525
             C         -1.0774245534        0.7702791240       -0.1744053472
             C          2.8498636045       -0.4645206094       -0.1922799795
             C         -1.4218007565       -0.7850501416       -0.2021503198
             H          2.0511714191        1.8626404605        0.8996124027
             H          1.1017826165        2.5033885185       -0.4265728164
             H         -0.3083479528        1.9763847128        1.4332069953
             H          0.2033356421        0.3027453082        1.4918974312
             H          1.1939462785        0.0852292002       -1.4240400771
             H          2.5871303840        1.1474547523       -1.5362113415
             H         -0.7775416723        1.0312499156       -1.1919311777
             H          3.6205966803        0.0055444316        0.4194317100
             H          3.3361661233       -1.0318279746       -0.9856062704
             H         -2.6284628403        2.1753990573       -0.5216811303
             H         -3.0325627338        0.5540981187       -0.0284710758
             H          2.4910941867       -2.3367192092        0.6680136974
             H          1.9781398838       -1.0663385144        1.6071566991
             H          1.0702227114       -1.5059322479        0.2455456256
             H         -2.4592842029        1.7302943098        1.0848423546
              25
                    -34.15632171
             O         -0.6377330316       -1.5602972801       -0.5925407457
             O         -1.9438226066       -0.8962122015        1.0968963302
             N         -2.0691310397        1.5115608178        0.5520085904
             N          2.0161394685       -1.6864701960       -0.6808935380
             C          1.0245208700        1.5678424724        0.3114580637
             C          0.0268814843        1.3822638313       -0.8376423667
             C          1.7125418505        0.2918072015        0.8062089597
             C         -1.3255436976        0.7602094261       -0.4861917907
             C          2.5798867778       -0.3735189736       -0.2699515146
             C         -1.2821941821       -0.7269243894        0.0579815663
             H          0.5483025277        2.0449086772        1.1692286093
             H          1.7966540418        2.2561425086       -0.0359536522
             H          0.4649670843        0.7632830938       -1.6196837604
             H         -0.1661732017        2.3619672870       -1.2796881790
             H          2.3322565126        0.5621906956        1.6605054657
             H          0.9655697921       -0.4140620317        1.1690705974
             H         -1.9201614836        0.7061105980       -1.4020292431
             H          3.5936748701       -0.5312821922        0.0979019662
             H          2.6369135707        0.2686143194       -1.1490685384
             H         -2.9086496435        1.9840429736        0.2094100180
             H         -2.3313717130        0.6888337551        1.2084854943
             H          2.1820919942       -2.3913736082        0.0460188498
             H          0.9581287874       -1.6201065231       -0.7926007692
             H          2.4297604389       -2.0163864988       -1.5591512145
             H         -1.4715716162        2.1835379113        1.0453989384
              25
                    -34.15592507
             O         -2.1033969149       -0.8344078612        0.9487088877
             O         -0.5699612374       -1.6680274471       -0.4517081377
             N         -1.9569275917        1.5465619838        0.2627686808
             N          2.0837337252       -1.3633283870       -0.5773762363
             C          1.1075416509        1.5419562581       -0.0103918263
             C          0.1209945172        1.1686244794       -1.1251953409
             C          1.4352918321        0.4570859531        1.0214669735
             C         -1.2503428799        0.6557309102       -0.6867527508
             C          2.5057372966       -0.5407868185        0.5902823997
             C         -1.2948162683       -0.7745608298        0.0075959523
             H          0.7274518351        2.4141375453        0.5251162579
             H          2.0330446007        1.8691474761       -0.4864359338
             H          0.5312965142        0.4086384701       -1.7869917742
             H         -0.0324587227        2.0621064981       -1.7347668750
             H          1.8169519144        0.9463147999        1.9183948838
             H          0.5367641743       -0.0848292604        1.3149274400
             H         -1.8634330878        0.5270669166       -1.5839659846
             H          2.7015999487       -1.2099014501        1.4282318085
             H          3.4296803314       -0.0167769980        0.3414844052
             H         -1.2992225514        2.1272618717        0.7958603647
             H         -2.6755557698        2.1370116562       -0.1630903688
             H          2.2813676133       -0.8782194673       -1.4586367594
             H          2.5735263109       -2.2635096262       -0.5875520766
             H          1.0325521121       -1.5556317222       -0.5353874345
             H         -2.3935764356        0.8063761382        0.9145561672
              25
                    -34.15547499
             O         -2.3288681540       -0.9689367039        0.5974852723
             O         -0.4008216845       -1.3559960891       -0.4559450313
             N         -2.6333104330        1.3901145964        0.0148791472
             N          2.2688734235       -1.7836122490       -0.3135245436
             C          1.2045970266        1.2147830910       -0.3207104722
             C         -0.1766849998        1.7049883679        0.1388725454
             C          1.9421007389        0.4104322018        0.7578355455
             C         -1.3241367595        0.8966054975       -0.4749078790
             C          2.9601024995       -0.5430248114        0.1410496733
             C         -1.3285933693       -0.6410895869       -0.0712604222
             H          1.8194085679        2.0702167267       -0.5988277282
             H          1.0710671602        0.6065579215       -1.2146344993
             H         -0.3076575365        2.7461839050       -0.1612866699
             H         -0.2378627475        1.6551531995        1.2275331138
             H          2.4468113264        1.0960018700        1.4370167059
             H          1.2297127637       -0.1683684162        1.3468389979
             H         -1.2704988015        0.9491259299       -1.5640455987
             H          3.7310656000       -0.8072491298        0.8640696447
             H          3.4371256948       -0.0632845904       -0.7133472266
             H         -2.5432643621        2.1520849835        0.6930123940
             H         -3.2834926874        1.6599628672       -0.7262042831
             H          2.7178100064       -2.1982719631       -1.1361624210
             H          2.2482990815       -2.4828183187        0.4368503414
             H          1.2567750610       -1.5723871751       -0.5319861416
             H         -2.9757946227        0.4721100866        0.5082732629
              25
                    -34.15545846
             O         -2.3777023451       -0.9177733836        0.6489622803
             O         -0.4009151018       -1.4460423055       -0.2363506290
             N         -2.5928675119        1.3702602408       -0.1958788709
             N          2.2705863538       -1.5552728060       -0.6959671399
             C          1.2626408325        1.2650767695       -0.1999508619
             C         -0.1676900608        1.6997857440        0.1433859727
             C          1.8576282855        0.3222355626        0.8537184709
             C         -1.2484356318        0.8255651567       -0.5054674146
             C          2.9084111559       -0.6168744122        0.2764230940
             C         -1.3258577703       -0.6732783629        0.0271901706
             H          1.8935551201        2.1501893106       -0.2772016053
             H          1.2458750287        0.7908735642       -1.1798122422
             H         -0.3171466901        2.7186794233       -0.2201486160
             H         -0.2967658760        1.7007241110        1.2272150872
             H          2.3089550370        0.9081814099        1.6535209776
             H          1.0686425282       -0.2818531316        1.3016188748
             H         -1.0876822336        0.7817119785       -1.5847014431
             H          3.3524799930       -1.1924945383        1.0873745985
             H          3.6963304583       -0.0519426265       -0.2212122945
             H         -2.9913275573        0.5162412164        0.3615908966
             H         -2.5589278358        2.2025641690        0.3999047288
             H          2.6187881911       -2.5117770389       -0.5815509942
             H          1.2217357961       -1.5619680721       -0.5321789754
             H          2.4345460024       -1.2660811775       -1.6650051770
             H         -3.1696789754        1.5558016135       -1.0193997348
              25
                    -34.15372845
             O         -0.8759513206       -1.0507714945        0.7395130487
             O         -1.0471783147       -1.2210634731       -1.4857197061
             N         -2.0100713682        1.1638582833        0.9733772433
             N          1.7020599065       -1.7447966827        0.8365171560
             C          0.9224701988        1.3977552196        0.1299066020
             C         -0.3034955554        1.7474745141       -0.7290850754
             C          1.8313865205        0.3371413309       -0.5152183962
             C         -1.5397536849        0.8954837973       -0.4124258872
             C          2.5286316072       -0.5462459399        0.5204181162
             C         -1.1337894558       -0.6241211220       -0.4224393440
             H          0.5999134506        1.0447379113        1.1088504940
             H          1.5075448530        2.3032957331        0.2891927248
             H         -0.0603736240        1.6138572758       -1.7832639404
             H         -0.5689030950        2.7975685720       -0.5933476217
             H          1.2520636483       -0.2949117808       -1.1892484363
             H          2.5821478388        0.8362393886       -1.1261103610
             H         -2.3194229349        1.0797505602       -1.1497937252
             H          2.6951475316        0.0179430508        1.4383444706
             H          3.4942793529       -0.8824584649        0.1428332329
             H         -1.6366862386        2.0406897131        1.3566364027
             H         -3.0319635647        1.1643591957        1.0502533535
             H          2.0123953855       -2.1956055335        1.7033301803
             H          0.6617699349       -1.4988916853        0.9180060433
             H          1.7620357216       -2.4244612968        0.0700935319
             H         -1.6155014744        0.3110635636        1.4848864268
              25
                    -34.15296366
             O         -0.9806508876       -0.9847570656        0.9016095316
             O         -1.0342961735       -1.4975369515       -1.2790158618
             N         -2.0600699488        1.3003330160        0.6598555012
             N          1.6483681534       -1.4770759046        1.0193012553
             C          0.8930197778        1.4409262402        0.3162871289
             C         -0.1018287155        1.4892580056       -0.8622608316
             C          2.2127299908        0.7309357778       -0.0043863659
             C         -1.4370888955        0.7837090011       -0.5889439421
             C          2.1074033787       -0.7768274559       -0.2134156979
             C         -1.1447588094       -0.7374998177       -0.3265126418
             H          0.4424872625        0.9591479756        1.1822965353
             H          1.1476108940        2.4604379775        0.6056686014
             H          0.3344753521        1.0203527521       -1.7446629435
             H         -0.3104542151        2.5259312846       -1.1308040482
             H          2.6400205950        1.1617275571       -0.9101356051
             H          2.9157320587        0.9267783500        0.8074716570
             H         -2.0965188879        0.8923814409       -1.4486238017
             H          3.0903725751       -1.1592546293       -0.4905246456
             H          1.4081882505       -1.0149227036       -1.0156071158
             H         -3.0831759004        1.3363322990        0.6000761811
             H         -1.7780292676        0.5566802205        1.3629478428
             H          0.5935972201       -1.3552723672        1.1310943683
             H          1.8230458958       -2.4842696303        0.9418471118
             H          2.1352867113       -1.1124232113        1.8467861942
             H         -1.6994055067        2.2241075607        0.9291898687
              25
                    -34.15141381
             O         -1.3943785432       -1.0307567325        0.9025777408
             O         -0.4225106153       -1.2766835982       -1.0942963862
             N         -2.1382332497        1.3072100696        0.6820859684
             N          1.8303685962       -1.9710495392        0.0707587666
             C          0.9607624662        1.6168605590        0.1707894371
             C         -0.1959862014        1.7114505222       -0.8393306352
             C          1.9799050629        0.5013843169       -0.1348429740
             C         -1.4247901716        0.8500833461       -0.5368218294
             C          1.9371661639       -0.6960029627        0.8273160062
             C         -1.0467465663       -0.6453837331       -0.2300826075
             H          0.5821582777        1.4936196592        1.1855414824
             H          1.4853467761        2.5717069600        0.1437592453
             H          0.1684852019        1.4328271659       -1.8279890567
             H         -0.5295558410        2.7487716363       -0.9068101296
             H          1.7990108513        0.1423863706       -1.1479579769
             H          2.9805578856        0.9303497506       -0.1217137710
             H         -2.0885910031        0.8601550715       -1.4024758281
             H          1.0694995988       -0.6377417602        1.4853144468
             H          2.8334350246       -0.7245407799        1.4470296049
             H         -1.6564311817        2.0816802979        1.1506799283
             H         -3.1156981590        1.5603768219        0.5197984898
             H          2.6238854975       -2.0888513426       -0.5678832194
             H          1.7803639034       -2.7755595157        0.7040003308
             H          0.9218328585       -1.9204346441       -0.4990801476
             H         -2.0745975566        0.3957317695        1.2728068192
              25
                    -34.15122546
             O         -1.4776298354       -0.9779162840        0.9689911790
             O         -0.4925234496       -1.3628551244       -1.0036507954
             N         -2.1639329473        1.3683833598        0.5828976748
             N          1.8796097534       -1.9018199765       -0.0324662357
             C          0.9212842989        1.5787487906        0.2022882964
             C         -0.1575522111        1.5949891498       -0.8983466380
             C          2.0807266384        0.5903317386       -0.0312863938
             C         -1.4336452672        0.8124987799       -0.5833640723
             C          2.0372506913       -0.6914975753        0.8153963432
             C         -1.1077721831       -0.6712596326       -0.1784565029
             H          0.4772061571        1.3762504059        1.1769048440
             H          1.3491352329        2.5796760629        0.2516191005
             H          0.2519128515        1.1866703660       -1.8221067218
             H         -0.4443786804        2.6264492546       -1.1098247151
             H          2.1069094344        0.3214704229       -1.0870338715
             H          3.0133169149        1.1098047495        0.1839278312
             H         -2.0690635873        0.7917157945       -1.4696202320
             H          1.1994207751       -0.6693189818        1.5132782369
             H          2.9569334194       -0.7921592267        1.3922309558
             H         -1.6800036187        2.1679532791        1.0053931771
             H         -3.1334683512        1.6233161070        0.3791665015
             H          2.6232513605       -1.9579134554       -0.7362257317
             H          1.8912989959       -2.7525757838        0.5392212719
             H          0.9273933706       -1.8444066710       -0.5257136187
             H         -2.1333692549        0.5050567581        1.2388793063
              25
                    -34.15102491
             O         -2.7007099152       -1.6852260154       -0.4665848832
             O         -3.5578446821        0.0350414384        0.6950996250
             N         -1.7249631955        1.6316108345        0.4298941943
             N          4.8503432460       -0.2765578553        0.0049566510
             C          1.1047501969        0.4501828237       -0.3379055216
             C         -0.1445140391       -0.2886936630        0.1459845288
             C          2.3741940866       -0.2828979224        0.0965187068
             C         -1.4339016427        0.3793165613       -0.3028003047
             C          3.6160850376        0.4539254738       -0.3951671902
             C         -2.6934727541       -0.5488189533       -0.0039847520
             H          1.1202182595        1.4641232269        0.0631636060
             H          1.0744507790        0.5160807326       -1.4261342120
             H         -0.1466884742       -0.3764861277        1.2347858649
             H         -0.1614173698       -1.3033571375       -0.2567289346
             H          2.3826839006       -0.3550960319        1.1849100754
             H          2.3458274825       -1.2937888279       -0.3120785959
             H         -1.4193517541        0.5529473168       -1.3796978179
             H          3.6476836700        1.4575725004        0.0303995802
             H          3.5908364898        0.5356651831       -1.4823627548
             H         -1.7888195298        2.4639789876       -0.1581990321
             H         -2.7206446817        1.3385249836        0.8116492773
             H          5.6915565940        0.2174183391       -0.3259194480
             H          4.8962264202       -0.3605243899        1.0314916029
             H          4.8443782262       -1.2261178281       -0.3963638456
             H         -1.0847775954        1.7904079765        1.2125175138
              25
                    -34.15096274
             O         -3.0664396656       -0.6811514259        0.4732552569
             O         -1.1830540829       -1.6357953168       -0.2743493731
             N         -2.5677177347        1.6455787239       -0.0357930901
             N          3.4161836207       -1.4947231742        0.0043983586
             C          1.0526738400        0.4198619079       -0.3271341828
             C         -0.1392962419        1.1756334823        0.2601668795
             C          2.3301787116        0.6870764296        0.4694516416
             C         -1.4374925629        0.7826136622       -0.4432938226
             C          3.5433351448       -0.0155439223       -0.1263696890
             C         -1.9232732343       -0.6849210780       -0.0466890927
             H          1.1914397265        0.7321338619       -1.3635085567
             H          0.7858921189       -0.6373779302       -0.3204311762
             H          0.0218209785        2.2505182304        0.1468292069
             H         -0.2266241551        0.9455046686        1.3247226359
             H          2.5278796928        1.7590277621        0.4727119199
             H          2.1882724928        0.3782271446        1.5058219746
             H         -1.3025118280        0.8032883597       -1.5259809861
             H          4.4479425383        0.3027435209        0.3921583620
             H          3.6346463266        0.2349775607       -1.1836334244
             H         -3.0211573542        2.1410074345       -0.8045922540
             H         -3.2218629847        0.8371482423        0.3587042034
             H          4.2253281871       -1.9726822174       -0.4155872948
             H          3.3489651634       -1.7581308116        0.9983748528
             H          2.5570336567       -1.8144640232       -0.4679619280
             H         -2.3203269015        2.3008181314        0.7098687755
              25
                    -34.15085064
             O         -3.5353488917        0.0322108098        0.4084906022
             O         -2.4254579214       -1.9084093375        0.6284254091
             N         -1.7744403653        1.3652775888       -0.6438663864
             N          4.1843308757       -0.1227935311       -1.0149931633
             C          1.2054477125        0.3338345853       -0.2573250215
             C         -0.0723683835       -0.0922253390        0.4701395235
             C          2.4067626747        0.2476116899        0.6871054133
             C         -1.3095998224       -0.0201188789       -0.4106994862
             C          3.6989393857        0.7735207317        0.0732627710
             C         -2.5481677099       -0.7347193516        0.2919952398
             H          1.1039179513        1.3593179418       -0.6150772671
             H          1.3305507433       -0.3269971917       -1.1156830038
             H         -0.2323641779        0.5179203592        1.3619095074
             H          0.0148258366       -1.1272987239        0.8068454593
             H          2.1939049448        0.8432977651        1.5748900270
             H          2.5356727482       -0.7824695024        1.0211816116
             H         -1.1430786561       -0.5391980852       -1.3555500184
             H          4.4681954117        0.8303339300        0.8434799998
             H          3.5410323285        1.7707403176       -0.3387001734
             H         -2.7959421798        1.2370421526       -0.2418926774
             H         -1.2663143192        2.0543977636       -0.0827359543
             H          4.3101055621       -1.0795234870       -0.6518667702
             H          3.4997443822       -0.1538144186       -1.7841531054
             H          5.0842467659        0.2120203831       -1.3873716549
             H         -1.8037773550        1.6476905491       -1.6249441216
              25
                    -34.15048917
             O         -1.9774584219       -0.8822136242       -1.3447844876
             O         -0.7353222350       -1.3040874260        0.4712779300
             N         -1.2018711229        0.8204001109        1.6759883927
             N          1.9464051556       -1.6923491374        0.3496868798
             C          0.8058411616        1.0462579377       -0.9468506072
             C         -0.4754178942        1.7785998054       -0.5275051890
             C          1.8226970853        0.7789686777        0.1752315710
             C         -1.5019161161        0.9155320919        0.2220611780
             C          2.6576744598       -0.4659344426       -0.1177787426
             C         -1.4255171488       -0.5796126398       -0.2987743324
             H          1.2927227489        1.6467822241       -1.7161034319
             H          0.5115436171        0.1136959367       -1.4299162738
             H         -0.9626535770        2.1244807715       -1.4395462399
             H         -0.2313854006        2.6631384020        0.0636669169
             H          2.4836824282        1.6401220787        0.2631946203
             H          1.3379895832        0.6415415675        1.1385387246
             H         -2.5044279955        1.3042479639        0.0511624497
             H          3.6220420553       -0.4105315490        0.3870533340
             H          2.8346807725       -0.5412209492       -1.1903941633
             H         -0.5537202797        1.5469435382        2.0029686770
             H         -2.0510266978        0.8448716653        2.2497725116
             H          0.8939724574       -1.6086997720        0.2334441992
             H          2.2630899289       -2.5235198565       -0.1606712473
             H          2.1155048181       -1.8450780227        1.3513380594
             H         -0.7798223195       -0.1709536166        1.7101046448
              25
                    -34.15043307
             O         -3.3078583007       -0.3518099727        0.5480566241
             O         -1.8441149941       -2.0073969491        0.1397595703
             N         -1.9672328473        1.5403727742       -0.2372000800
             N          3.6044925126       -0.8358440764       -0.6659647320
             C          1.1684565146        1.0446332415       -0.4943940738
             C          0.1082467096        0.2177955398        0.2395309885
             C          2.4785715426        1.1657908669        0.2896724500
             C         -1.2458520269        0.2676289576       -0.4513144135
             C          3.1696429470       -0.1592315605        0.5890169973
             C         -2.2263428078       -0.8414226742        0.1422529421
             H          0.7932826990        2.0569151989       -0.6482792542
             H          1.3354698371        0.6191303209       -1.4843783659
             H         -0.0031461344        0.5661490514        1.2686433794
             H          0.3791631733       -0.8378892566        0.2841197320
             H          3.1579098277        1.8188328559       -0.2597110183
             H          2.2713923792        1.6478155935        1.2450536588
             H         -1.1432114953        0.0569418065       -1.5170262136
             H          2.5004236326       -0.8240708177        1.1341470919
             H          4.0493855213        0.0280822082        1.2049984895
             H         -1.5070190112        2.1488324010        0.4454597884
             H         -2.1837208522        2.0584218679       -1.0901715209
             H          4.2094106392       -0.2068158824       -1.2153183037
             H          4.1256865653       -1.6973771969       -0.4514692989
             H          2.7834959501       -1.0854874671       -1.2358687369
             H         -2.8845837684        1.1083956166        0.2016053653


Calculate free energies in solution phase \(CHCl\ :sub:`3`\) on GFN\ *n*\-xTB geometries
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

.. note::

    There are two options available:
    
    * Using part1 (prescreening) or 
    * only using part3 and do not calculate part2 (optimization)
    
    The difference between the two approaches is that Part3 applies tighter 
    thresholds in the SCF.


.. tabbed:: input

    .. code:: sh

        $ censo -inp ensemble.xyz -part1 on -chrg 1 -solvent chcl3 -smgsolv1 cosmors -func r2scan-3c -basis automatic -P 4 -O 2  > censo.out &

.. tabbed:: output

    .. code:: sh

                     ______________________________________________________________
                    |                                                              |
                    |                                                              |
                    |                   CENSO - Commandline ENSO                   |
                    |                           v 1.0.3                            |
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


            ----------------------------------------------------------------------------------------------------
                                                         PARAMETERS                                             
            ----------------------------------------------------------------------------------------------------

            The configuration file .censorc is read from /home/bohle/1projects/from_tmp1/CENSO/documentation-calcs/2-part1/.censorc.
            Reading conformer rotamer ensemble from: /home/bohle/1projects/from_tmp1/CENSO/documentation-calcs/2-part1/ensemble.xyz.
            Reading file: censo_solvents.json


            --------------------------------------------------
                           CRE SORTING SETTINGS               
            --------------------------------------------------

            number of atoms in system:                                     25
            number of considered conformers:                               22
            number of all conformers from input:                           22
            charge:                                                        1
            unpaired:                                                      0
            solvent:                                                       chcl3
            temperature:                                                   298.15
            evaluate at different temperatures:                            on
            temperature range:                                             273.15, 278.15, 283.15, 288.15, ...
            calculate mRRHO contribution:                                  on
            consider symmetry for mRRHO contribution:                      off
            cautious checking for error and failed calculations:           on
            checking the DFT-ensemble using CREST:                         off
            maxthreads:                                                    4
            omp:                                                           2

            --------------------------------------------------
                         CRE PRESCREENING - PART1             
            --------------------------------------------------
            part1:                                                         on
            starting number of considered conformers:                      22
            program for part1:                                             tm
            functional for initial evaluation:                             r2scan-3c
            basis set for initial evaluation:                              def2-mTZVPP
            calculate mRRHO contribution:                                  on
            program for mRRHO contribution:                                xtb
            GFN version for mRRHO and/or GBSA_Gsolv:                       gfn2
            Apply constraint to input geometry during mRRHO calculation:   on
            solvent model applied with xTB:                                alpb
            evaluate at different temperatures:                            off
            threshold g_thr(1) and G_thr(1) for sorting in part1:          3.5
            solvent model for Gsolv contribution of part1:                 cosmors

            short-notation:
            r2scan-3c + COSMORS[chcl3] + GmRRHO(GFN2[alpb]-bhess) // GFNn-xTB (Input geometry)
            END of parameters


            ------------------------------------------------------------
                           PATHS of external QM programs                
            ------------------------------------------------------------

            The following program paths are used:
                xTB:          /home/abt-grimme/AK-bin/xtb
                TURBOMOLE:    /home/abt-grimme/TURBOMOLE.7.5.1/bin/em64t-unknown-linux-gnu_smp
                Setup of COSMO-RS:
                    ctd = BP_TZVPD_FINE_C30_1601.ctd
                    cdir = "/home/bohle/COSMOlogic/COSMOthermX19/COSMOtherm/CTDATA-FILES"
                    ldir = "/home/bohle/COSMOlogic/COSMOthermX19/COSMOtherm/CTDATA-FILES"
                Using /home/bohle/COSMOlogic/COSMOthermX19/COSMOtherm/DATABASE-COSMO/BP-TZVP-COSMO
                as path to the COSMO-RS NORMAL DATABASE.

                Using cefine from /home/bohle/bin/cefine
                PARNODES for TM or COSMO-RS calculation was set to 2
                Using COSMOtherm from /home/bohle/COSMOlogic/COSMOthermX19/COSMOtherm/BIN-LINUX/cosmotherm

            ----------------------------------------------------------------------------------------------------
                                        Processing data from previous run (enso.json)                           
            ----------------------------------------------------------------------------------------------------

            INFORMATION: No restart information exists and is created during this run!


            ----------------------------------------------------------------------------------------------------
                                                  CRE PRESCREENING - PART1                                      
            ----------------------------------------------------------------------------------------------------

            program:                                                       tm
            functional for part1 and 2:                                    r2scan-3c
            basis set for part1 and 2:                                     def2-mTZVPP
            Solvent:                                                       chcl3
            solvent model for Gsolv contribution:                          cosmors
            threshold g_thr(1) and G_thr(1):                               3.5
            starting number of considered conformers:                      22
            calculate mRRHO contribution:                                  on
            program for mRRHO contribution:                                xtb
            GFN version for mRRHO and/or GBSA_Gsolv:                       gfn2
            Apply constraint to input geometry during mRRHO calculation:   on
            temperature:                                                   298.15

            Calculating single-point energies and solvation contribution (G_solv):
            The prescreening COSMO-RS is calculated for:
             CONF1,  CONF2,  CONF3,  CONF4,  CONF5,  CONF6,  CONF7,  CONF8,  CONF9, CONF10, CONF11
            CONF12, CONF13, CONF14, CONF15, CONF16, CONF17, CONF18, CONF19, CONF20, CONF21, CONF22
            Constructed folders!
            Constructed folders!

            Starting 22 COSMO-RS-Gsolv calculations.
            Running COSMO-RS calculation in CONF1/r2scan-3c/COSMO
            Running COSMO-RS calculation in CONF2/r2scan-3c/COSMO
            Running COSMO-RS calculation in CONF3/r2scan-3c/COSMO
            Running COSMO-RS calculation in CONF4/r2scan-3c/COSMO
            Running COSMO-RS calculation in CONF5/r2scan-3c/COSMO
            Running COSMO-RS calculation in CONF6/r2scan-3c/COSMO
            Running COSMO-RS calculation in CONF7/r2scan-3c/COSMO
            Running COSMO-RS calculation in CONF8/r2scan-3c/COSMO
            Running COSMO-RS calculation in CONF9/r2scan-3c/COSMO
            Running COSMO-RS calculation in CONF10/r2scan-3c/COSMO
            Running COSMO-RS calculation in CONF11/r2scan-3c/COSMO
            Running COSMO-RS calculation in CONF12/r2scan-3c/COSMO
            Running COSMO-RS calculation in CONF13/r2scan-3c/COSMO
            Running COSMO-RS calculation in CONF14/r2scan-3c/COSMO
            Running COSMO-RS calculation in CONF15/r2scan-3c/COSMO
            Running COSMO-RS calculation in CONF16/r2scan-3c/COSMO
            Running COSMO-RS calculation in CONF17/r2scan-3c/COSMO
            Running COSMO-RS calculation in CONF18/r2scan-3c/COSMO
            Running COSMO-RS calculation in CONF19/r2scan-3c/COSMO
            Running COSMO-RS calculation in CONF20/r2scan-3c/COSMO
            Running COSMO-RS calculation in CONF21/r2scan-3c/COSMO
            Running COSMO-RS calculation in CONF22/r2scan-3c/COSMO
            Tasks completed!

            prescreening COSMO-RS calculation was successful for CONF1/r2scan-3c/COSMO: -0.09391084
            prescreening COSMO-RS calculation was successful for CONF2/r2scan-3c/COSMO: -0.09344716
            prescreening COSMO-RS calculation was successful for CONF3/r2scan-3c/COSMO: -0.09189166
            prescreening COSMO-RS calculation was successful for CONF4/r2scan-3c/COSMO: -0.09269361
            prescreening COSMO-RS calculation was successful for CONF5/r2scan-3c/COSMO: -0.09328873
            prescreening COSMO-RS calculation was successful for CONF6/r2scan-3c/COSMO: -0.09278491
            prescreening COSMO-RS calculation was successful for CONF7/r2scan-3c/COSMO: -0.09457155
            prescreening COSMO-RS calculation was successful for CONF8/r2scan-3c/COSMO: -0.09275254
            prescreening COSMO-RS calculation was successful for CONF9/r2scan-3c/COSMO: -0.09316162
            prescreening COSMO-RS calculation was successful for CONF10/r2scan-3c/COSMO: -0.09116989
            prescreening COSMO-RS calculation was successful for CONF11/r2scan-3c/COSMO: -0.09348580
            prescreening COSMO-RS calculation was successful for CONF12/r2scan-3c/COSMO: -0.08900263
            prescreening COSMO-RS calculation was successful for CONF13/r2scan-3c/COSMO: -0.08998048
            prescreening COSMO-RS calculation was successful for CONF14/r2scan-3c/COSMO: -0.09122770
            prescreening COSMO-RS calculation was successful for CONF15/r2scan-3c/COSMO: -0.09272288
            prescreening COSMO-RS calculation was successful for CONF16/r2scan-3c/COSMO: -0.08509833
            prescreening COSMO-RS calculation was successful for CONF17/r2scan-3c/COSMO: -0.08598277
            prescreening COSMO-RS calculation was successful for CONF18/r2scan-3c/COSMO: -0.13224220
            prescreening COSMO-RS calculation was successful for CONF19/r2scan-3c/COSMO: -0.11057302
            prescreening COSMO-RS calculation was successful for CONF20/r2scan-3c/COSMO: -0.13223689
            prescreening COSMO-RS calculation was successful for CONF21/r2scan-3c/COSMO: -0.09915029
            prescreening COSMO-RS calculation was successful for CONF22/r2scan-3c/COSMO: -0.12536341

            --------------------------------------------------
                      Removing high lying conformers          
            --------------------------------------------------

             CONF#  E(GFNn-xTB) ΔE(GFNn-xTB)       E [Eh]      Gsolv [Eh]         Gtot      ΔGtot
                         [a.u.]   [kcal/mol]    r2scan-3c COSMO-RS-normal         [Eh] [kcal/mol]
                                                              [r2scan-3c]                        
            CONF1   -34.1599548         0.00 -497.3021467      -0.0939108 -497.3960575       1.82
            CONF2   -34.1594484         0.32 -497.3049956      -0.0934472 -497.3984428       0.33
            CONF3   -34.1592530         0.44 -497.3064930      -0.0918917 -497.3983847       0.36
            CONF4   -34.1590954         0.54 -497.3049058      -0.0926936 -497.3975994       0.86
            CONF5   -34.1589250         0.65 -497.3056747      -0.0932887 -497.3989634       0.00     <------
            CONF6   -34.1587465         0.76 -497.3049670      -0.0927849 -497.3977519       0.76
            CONF7   -34.1578065         1.35 -497.2983644      -0.0945716 -497.3929360       3.78
            CONF8   -34.1571241         1.78 -497.3029570      -0.0927525 -497.3957096       2.04
            CONF9   -34.1570372         1.83 -497.3008248      -0.0931616 -497.3939864       3.12
            CONF10  -34.1563217         2.28 -497.3038986      -0.0911699 -497.3950685       2.44
            CONF11  -34.1559251         2.53 -497.3008250      -0.0934858 -497.3943108       2.92
            CONF12  -34.1554750         2.81 -497.3045828      -0.0890026 -497.3935854       3.37
            CONF13  -34.1554585         2.82 -497.3036749      -0.0899805 -497.3936554       3.33
            CONF14  -34.1537285         3.91 -497.3022176      -0.0912277 -497.3934453       3.46
            CONF15  -34.1529637         4.39 -497.3009381      -0.0927229 -497.3936610       3.33
            CONF16  -34.1514138         5.36 -497.3063638      -0.0850983 -497.3914621       4.71
            CONF17  -34.1512255         5.48 -497.3047147      -0.0859828 -497.3906975       5.19
            CONF18  -34.1510249         5.60 -497.2564102      -0.1322422 -497.3886524       6.47
            CONF19  -34.1509627         5.64 -497.2746319      -0.1105730 -497.3852050       8.63
            CONF20  -34.1508506         5.71 -497.2550494      -0.1322369 -497.3872863       7.33
            CONF21  -34.1504892         5.94 -497.2896633      -0.0991503 -497.3888135       6.37
            CONF22  -34.1504331         5.98 -497.2596139      -0.1253634 -497.3849773       8.78

            --------------------------------------------------
                      Conformers considered further           
            --------------------------------------------------

            Below the g_thr(1) threshold of 3.5 kcal/mol.

             CONF1,  CONF2,  CONF3,  CONF4,  CONF5,  CONF6,  CONF8,  CONF9, CONF10, CONF11, CONF12
            CONF13, CONF14, CONF15

            --------------------------------------------------

            Calculating prescreening G_mRRHO with implicit solvation!
            The prescreening G_mRRHO calculation is now performed for:
             CONF1,  CONF2,  CONF3,  CONF4,  CONF5,  CONF6,  CONF8,  CONF9, CONF10, CONF11, CONF12
            CONF13, CONF14, CONF15

            Constructed folders!

            Starting 14 G_RRHO calculations.
            Running GFN2-xTB mRRHO in CONF1/rrho_part1
            Running GFN2-xTB mRRHO in CONF2/rrho_part1
            Running GFN2-xTB mRRHO in CONF3/rrho_part1
            Running GFN2-xTB mRRHO in CONF4/rrho_part1
            Running GFN2-xTB mRRHO in CONF5/rrho_part1
            Running GFN2-xTB mRRHO in CONF6/rrho_part1
            Running GFN2-xTB mRRHO in CONF8/rrho_part1
            Running GFN2-xTB mRRHO in CONF9/rrho_part1
            Running GFN2-xTB mRRHO in CONF10/rrho_part1
            Running GFN2-xTB mRRHO in CONF11/rrho_part1
            Running GFN2-xTB mRRHO in CONF12/rrho_part1
            Running GFN2-xTB mRRHO in CONF13/rrho_part1
            Running GFN2-xTB mRRHO in CONF14/rrho_part1
            Running GFN2-xTB mRRHO in CONF15/rrho_part1
            Tasks completed!

            The prescreening G_mRRHO calculation @ c1 was successful for  CONF1/rrho_part1: 0.18236493
            The prescreening G_mRRHO calculation @ c1 was successful for  CONF2/rrho_part1: 0.18246714
            The prescreening G_mRRHO calculation @ c1 was successful for  CONF3/rrho_part1: 0.18235782
            The prescreening G_mRRHO calculation @ c1 was successful for  CONF4/rrho_part1: 0.18239049
            The prescreening G_mRRHO calculation @ c1 was successful for  CONF5/rrho_part1: 0.18241907
            The prescreening G_mRRHO calculation @ c1 was successful for  CONF6/rrho_part1: 0.18225430
            The prescreening G_mRRHO calculation @ c1 was successful for  CONF8/rrho_part1: 0.18193879
            The prescreening G_mRRHO calculation @ c1 was successful for  CONF9/rrho_part1: 0.18214342
            The prescreening G_mRRHO calculation @ c1 was successful for CONF10/rrho_part1: 0.18217367
            The prescreening G_mRRHO calculation @ c1 was successful for CONF11/rrho_part1: 0.18267361
            The prescreening G_mRRHO calculation @ c1 was successful for CONF12/rrho_part1: 0.18167483
            The prescreening G_mRRHO calculation @ c1 was successful for CONF13/rrho_part1: 0.18145764
            The prescreening G_mRRHO calculation @ c1 was successful for CONF14/rrho_part1: 0.18268845
            The prescreening G_mRRHO calculation @ c1 was successful for CONF15/rrho_part1: 0.18320604

            --------------------------------------------------
                     * Gibbs free energies of part1 *         
            --------------------------------------------------

             CONF#  G(GFNn-xTB) ΔG(GFNn-xTB)       E [Eh]      Gsolv [Eh]  GmRRHO [Eh]         Gtot      ΔGtot
                         [a.u.]   [kcal/mol]    r2scan-3c COSMO-RS-normal         GFN2         [Eh] [kcal/mol]
                                                              [r2scan-3c] [alpb]-bhess                        
            CONF1   -33.9775899         0.00 -497.3021467      -0.0939108    0.1823649 -497.2136926       1.79
            CONF2   -33.9769813         0.38 -497.3049956      -0.0934472    0.1824671 -497.2159757       0.36
            CONF3   -33.9768952         0.44 -497.3064930      -0.0918917    0.1823578 -497.2160268       0.32
            CONF4   -33.9767049         0.56 -497.3049058      -0.0926936    0.1823905 -497.2152089       0.84
            CONF5   -33.9765060         0.68 -497.3056747      -0.0932887    0.1824191 -497.2165444       0.00     <------
            CONF6   -33.9764922         0.69 -497.3049670      -0.0927849    0.1822543 -497.2154976       0.66
            CONF8   -33.9751853         1.51 -497.3029570      -0.0927525    0.1819388 -497.2137708       1.74
            CONF9   -33.9748938         1.69 -497.3008248      -0.0931616    0.1821434 -497.2118430       2.95
            CONF10  -33.9741480         2.16 -497.3038986      -0.0911699    0.1821737 -497.2128948       2.29
            CONF11  -33.9732515         2.72 -497.3008250      -0.0934858    0.1826736 -497.2116372       3.08
            CONF12  -33.9738002         2.38 -497.3045828      -0.0890026    0.1816748 -497.2119106       2.91
            CONF13  -33.9740008         2.25 -497.3036749      -0.0899805    0.1814576 -497.2121977       2.73
            CONF14  -33.9710400         4.11 -497.3022176      -0.0912277    0.1826885 -497.2107568       3.63
            CONF15  -33.9697576         4.91 -497.3009381      -0.0927229    0.1832060 -497.2104549       3.82

            Additional global 'fuzzy-threshold' based on the standard deviation of (G_mRRHO):
            Std_dev(G_mRRHO) = 0.272 kcal/mol
            Fuzzythreshold   = 0.309 kcal/mol
            Final sorting threshold G_thr(1) = 3.500 + 0.309 = 3.809 kcal/mol
            Spearman correlation coefficient between (E + Solv) and (E + Solv + mRRHO) = 0.908

            --------------------------------------------------
                      Conformers considered further           
            --------------------------------------------------

            Considered CONF14 because of increased fuzzythr.
            These conformers are below the 3.809 kcal/mol G_thr(1) threshold.

             CONF1,  CONF2,  CONF3,  CONF4,  CONF5,  CONF6,  CONF8,  CONF9, CONF10, CONF11, CONF12
            CONF13, CONF14


            Calculating Boltzmann averaged free energy of ensemble on input geometries (not DFT optimized)!

            temperature /K:   avE(T) /a.u. avGmRRHO(T) /a.u. avGsolv(T) /a.u.   avG(T) /a.u.
            ----------------------------------------------------------------------------------------------------
                298.15        -497.3054054        0.1823775       -0.0928881   -497.2159160      <<==part1==
            ----------------------------------------------------------------------------------------------------


            Calculating unbiased GFNn-xTB energy
            Constructed folders!

            Starting 13 xTB - single-point calculations.
            gfn2-xTB energy for CONF1/GFN_unbiased = -34.1515395
            gfn2-xTB energy for CONF2/GFN_unbiased = -34.1509941
            gfn2-xTB energy for CONF3/GFN_unbiased = -34.1515326
            gfn2-xTB energy for CONF4/GFN_unbiased = -34.1508491
            gfn2-xTB energy for CONF5/GFN_unbiased = -34.1514366
            gfn2-xTB energy for CONF6/GFN_unbiased = -34.1512893
            gfn2-xTB energy for CONF8/GFN_unbiased = -34.1493072
            gfn2-xTB energy for CONF9/GFN_unbiased = -34.1491025
            gfn2-xTB energy for CONF10/GFN_unbiased = -34.1491972
            gfn2-xTB energy for CONF11/GFN_unbiased = -34.1487085
            gfn2-xTB energy for CONF12/GFN_unbiased = -34.1484725
            gfn2-xTB energy for CONF13/GFN_unbiased = -34.1480733
            gfn2-xTB energy for CONF14/GFN_unbiased = -34.1461266
            Tasks completed!


            >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>END of Part1<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<
            Ran part1 in 277.0194 seconds


            Part                : #conf    time
            --------------------------------------------------
            Input               :    22    -
            Part1_initial_sort  :    14    -
            Part1_all           :    14    277.02s
            --------------------------------------------------
            All parts           :          277.02s

            CENSO all done!


.. tabbed:: example censorc

    .. code:: sh

        $CENSO global configuration file: .censorc
        $VERSION:1.0.3 

        ORCA: /home/USER/orca_4_2_1_linux_x86-64_openmpi216
        ORCA version: 4.2.1
        GFN-xTB: xtb
        CREST: crest
        mpshift: mpshift
        escf: escf

        #COSMO-RS
        ctd = BP_TZVPD_FINE_C30_1601.ctd  cdir = "/home/USER/COSMOlogic/COSMOthermX19/COSMOtherm/CTDATA-FILES" ldir = "/home/USER/COSMOlogic/COSMOthermX19/COSMOtherm/CTDATA-FILES"
        cosmothermversion: 19
        $ENDPROGRAMS

        $CRE SORTING SETTINGS:
        $GENERAL SETTINGS:
        nconf: all                       # ['all', 'number e.g. 10 up to all conformers'] 
        charge: 0                        # ['number e.g. 0'] 
        unpaired: 0                      # ['number e.g. 0'] 
        solvent: gas                     # ['gas', 'acetone', 'chcl3', 'acetonitrile', 'ch2cl2', 'dmso', 'h2o', 'methanol', 'thf', '...'] 
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
        func: r2scan-3c                  # ['pbe', 'b97-d', 'pbeh-3c', 'tpss', 'b97-d3', 'r2scan-3c', 'b97-3c'] 
        basis: automatic                 # ['automatic', 'def2-mSVP', 'def2-mTZVP', 'def2-mTZVP', 'def2-TZVP', '...'] 
        maxthreads: 2                    # ['number of threads e.g. 2'] 
        omp: 2                           # ['number cores per thread e.g. 4'] 
        cosmorsparam: automatic          # ['automatic', '12-fine', '12-normal', '13-fine', '13-normal', '14-fine', '...']

        $PART0 - CHEAP-PRESCREENING - SETTINGS:
        part0: off                        # ['on', 'off'] 
        func0: b97-d                     # ['pbeh-3c', 'b97-3c', 'b97-d3', 'pbe', 'r2scan-3c', 'tpss', 'b97-d'] 
        basis0: def2-SV(P)               # ['automatic', 'def2-mSVP', 'def2-mTZVP', 'def2-mTZVP', 'def2-TZVP', '...'] 
        part0_gfnv: gfn2                 # ['gfn1', 'gfn2', 'gfnff'] 
        part0_threshold: 4.0             # ['number e.g. 4.0'] 

        $PART1 - PRESCREENING - SETTINGS:
        # func and basis is set under GENERAL SETTINGS
        part1: off                        # ['on', 'off'] 
        smgsolv1: cosmors                # ['alpb_gsolv', 'dcosmors', 'cosmo', 'smd', 'cosmors-fine', 'cpcm', 'gbsa_gsolv', '...'] 
        part1_gfnv: gfn2                 # ['gfn1', 'gfn2', 'gfnff'] 
        part1_threshold: 3.5             # ['number e.g. 5.0'] 

        $PART2 - OPTIMIZATION - SETTINGS:
        # func and basis is set under GENERAL SETTINGS
        part2: off                        # ['on', 'off'] 
        opt_limit: 2.5                   # ['number e.g. 4.0'] 
        sm2: dcosmors                    # ['cosmo', 'cpcm', 'default', 'smd', 'dcosmors'] 
        smgsolv2: cosmors                # ['cosmors-fine', 'gbsa_gsolv', 'cosmo', 'cpcm', 'smd_gsolv', 'alpb_gsolv', 'smd', '...'] 
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
        func3: pw6b95                    # ['dsd-blyp', 'pw6b95', 'b97-d3', 'r2scan-3c', 'pbe0', 'wb97x'] 
        basis3: def2-TZVPD               # ['SVP', 'SV(P)', 'TZVP', 'TZVPP', 'QZVP', 'QZVPP', 'def2-SV(P)', 'def2-mSVP', '...'] 
        smgsolv3: cosmors                # ['alpb_gsolv', 'dcosmors', 'cosmo', 'smd', 'cosmors-fine', 'cpcm', 'gbsa_gsolv', '...'] 
        part3_gfnv: gfn2                 # ['gfn1', 'gfn2', 'gfnff'] 
        part3_threshold: 99              # ['Boltzmann sum threshold in %. e.g. 95 (between 1 and 100)'] 

        $NMR PROPERTY SETTINGS:
        $PART4 SETTINGS:
        part4: off                       # ['on', 'off'] 
        couplings: on                    # ['on', 'off'] 
        progJ: prog                      # ['tm', 'orca', 'adf', 'prog'] 
        funcJ: pbe0                      # ['tpss', 'pbe0', 'pbeh-3c'] 
        basisJ: def2-TZVP                # ['SVP', 'SV(P)', 'TZVP', 'TZVPP', 'QZVP', 'QZVPP', 'def2-SV(P)', 'def2-mSVP', '...'] 
        sm4J: default                    # ['dcosmors', 'smd', 'cosmo', 'cpcm'] 
        shieldings: on                   # ['on', 'off'] 
        progS: prog                      # ['tm', 'orca', 'adf', 'prog'] 
        funcS: pbe0                      # ['dsd-blyp', 'pbeh-3c', 'tpss', 'pbe0', 'kt2'] 
        basisS: def2-TZVP                # ['SVP', 'SV(P)', 'TZVP', 'TZVPP', 'QZVP', 'QZVPP', 'def2-SV(P)', 'def2-mSVP', '...'] 
        sm4S: default                    # ['dcosmors', 'smd', 'cosmo', 'cpcm'] 
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

.. tabbed:: input-ensemble.xyz

    .. code:: sh

          25
                -34.15995484
         O         -2.7404108553       -0.9210263756        0.0505546155
         O         -0.6532148483       -1.7303331963        0.1381884568
         N         -2.1811530948        1.4662136641       -0.3280604715
         N          1.9999042791       -1.6115147219       -0.1737452561
         C          1.2832974514        1.7494376339        0.2478643443
         C          0.0382242755        1.0547713107        0.8044626435
         C          2.1972182535        0.8461961224       -0.5852172042
         C         -0.9624505804        0.6276788621       -0.2713633850
         C          2.7304045776       -0.3664138808        0.1827513585
         C         -1.5037598584       -0.8390454546        0.0048901917
         H          1.8561477225        2.1402514306        1.0892838382
         H          0.9852377848        2.6009114549       -0.3642611073
         H         -0.4518270956        1.7282098745        1.5089809935
         H          0.3322545004        0.1775252924        1.3825123935
         H          1.6820235078        0.5059218506       -1.4832357051
         H          3.0380251642        1.4546200515       -0.9165284484
         H         -0.4868586610        0.6089411942       -1.2530945560
         H          2.6388482788       -0.2021893439        1.2562535001
         H          3.7853986884       -0.5188774904       -0.0473087348
         H         -2.2380899515        2.1426216511        0.4407329422
         H         -2.3218532286        1.9466721688       -1.2205963738
         H          2.3394829642       -2.3992859458        0.3882351555
         H          0.9350292280       -1.5547588441       -0.0165809300
         H          2.1475890354       -1.8368760327       -1.1636223077
         H         -2.9328940343        0.6971914121       -0.1921053926
          25
                -34.15944839
         O         -2.5910945174       -0.7526707388        0.4737473250
         O         -0.5258609320       -1.6055473558        0.4259946366
         N         -2.1883047577        1.2429283468       -0.9276187078
         N          2.0221224751       -1.5482527990       -0.3635876013
         C          1.0959612422        0.9269941773        1.0928268792
         C         -0.1118183900        1.5287127427        0.3646185610
         C          2.3883955950        0.8232150730        0.2771806877
         C         -0.9137052232        0.5859776609       -0.5447048684
         C          2.3984857923       -0.1999516224       -0.8542391926
         C         -1.3796771114       -0.7342464594        0.2052799197
         H          0.8202702309       -0.0346561349        1.5230052762
         H          1.3150554091        1.5881707649        1.9329336393
         H          0.2252898792        2.3837647705       -0.2251981715
         H         -0.7970460540        1.8894152433        1.1356879771
         H          2.6174364896        1.7989347868       -0.1517789136
         H          3.1942064704        0.5825472961        0.9724285437
         H         -0.3499369029        0.3059081761       -1.4335569624
         H          3.4048136146       -0.2402683769       -1.2749539435
         H          1.7122153210        0.0852818535       -1.6492945642
         H         -2.2438155060        2.2226460139       -0.6308218074
         H         -2.4094139539        1.1700691932       -1.9248068681
         H          0.9927667520       -1.5675551657       -0.0428313977
         H          2.1377830502       -2.2542373686       -1.0973459319
         H          2.6054467969       -1.8083924261        0.4393088723
         H         -2.8796747565        0.6310313638       -0.3580585669
          25
                -34.15925304
         O         -2.6152400314       -1.0465052003        0.2378304729
         O         -0.5362082855       -1.4663149505       -0.4593671015
         N         -2.4557715395        1.4039918868        0.4362491058
         N          2.1447268012       -1.5911295291       -0.0908488427
         C          1.1849792140        1.1291603967       -0.7123125066
         C          0.0549239500        1.3190958463        0.3109988160
         C          2.5516538580        0.8614163999       -0.0796419105
         C         -1.2770304521        0.8481909298       -0.2699298296
         C          2.6278492228       -0.4408272994        0.7143702153
         C         -1.4798499370       -0.7275247362       -0.1594413730
         H          1.2638776944        2.0288103522       -1.3231621409
         H          0.9216680773        0.3153070715       -1.3864974308
         H         -0.0132519646        2.3752873127        0.5749134480
         H          0.2604586805        0.7576942571        1.2223482952
         H          3.2969117377        0.8423750312       -0.8755852689
         H          2.8115908685        1.6810094994        0.5898815386
         H         -1.3353089850        1.1014685130       -1.3310936133
         H          2.0240292821       -0.3842292575        1.6193674755
         H          3.6638250279       -0.6160286136        1.0070988283
         H         -2.2160156909        1.7709567167        1.3631094761
         H         -2.9723151833        2.1102297821       -0.0929227069
         H          2.6253531272       -1.6216026098       -0.9974699479
         H          2.3064470721       -2.4761725884        0.4009650983
         H          1.0998172314       -1.5111491960       -0.2664030290
         H         -3.0342449423        0.4895941432        0.5473101358
          25
                -34.15909539
         O         -2.5859531813       -0.8098009525        0.4685129327
         O         -0.4816245052       -1.5436026390        0.3102868940
         N         -2.3339104715        1.3007756781       -0.7836622820
         N          2.0492564794       -1.5871472251       -0.5900906398
         C          1.1052196056        0.9870949301        0.9271737021
         C         -0.2140772115        1.5925038037        0.4451868362
         C          2.1667497593        0.8464686753       -0.1771749747
         C         -1.0154302176        0.6861215046       -0.4984780600
         C          2.9073785151       -0.4823039670       -0.0859386105
         C         -1.3840762559       -0.7076318190        0.1707141950
         H          0.8920163207        0.0201435075        1.3815317896
         H          1.4930774855        1.6249540613        1.7213050551
         H         -0.0067258320        2.5368714102       -0.0630175124
         H         -0.8282372023        1.7972548662        1.3249938991
         H          1.7135886317        0.9350726379       -1.1636100949
         H          2.8915967677        1.6540129472       -0.0882838850
         H         -0.4677225963        0.5007855037       -1.4227063242
         H          3.1688824777       -0.6847294678        0.9524589517
         H          3.8237876908       -0.4492687854       -0.6762385697
         H         -2.5870605138        1.3036833133       -1.7755142934
         H         -2.9689761577        0.6014426661       -0.2449843511
         H          1.9723283573       -1.5417442554       -1.6118847971
         H          2.4360322309       -2.5007606995       -0.3327999527
         H          1.0548817542       -1.5294780724       -0.1881461707
         H         -2.4296444696        2.2452869585       -0.3976639893
          25
                -34.15892503
         O         -2.4565204045       -0.8423454324        0.7070928712
         O         -0.5980888346       -1.6084799899       -0.2741310513
         N         -2.2081439307        1.5499049911        0.0797136762
         N          2.0665657616       -1.5738488309        0.1665385721
         C          0.8489992162        1.1021214800        0.5754910780
         C          0.1319038004        1.1643519245       -0.7806607655
         C          2.3611287197        0.8895018711        0.4504901307
         C         -1.3084663819        0.6750096988       -0.7114456668
         C          2.7727042231       -0.3622083435       -0.3226992137
         C         -1.4583757981       -0.7537800183       -0.0274928276
         H          0.4155006230        0.3036523983        1.1797441237
         H          0.7020515096        2.0340597672        1.1228295970
         H          0.6304477123        0.5210588124       -1.5026223849
         H          0.1602968624        2.1778026423       -1.1819014190
         H          2.8058653217        1.7473421346       -0.0537220547
         H          2.7810957945        0.8489865537        1.4562868383
         H         -1.7106345304        0.5638398621       -1.7216356404
         H          3.8478441803       -0.5048945341       -0.2050768697
         H          2.5630334022       -0.2516058916       -1.3860041489
         H         -2.8269415029        2.1396743466       -0.4823076953
         H         -2.7642911303        0.7978509993        0.6164252281
         H          2.4375926491       -2.4160896797       -0.2859521821
         H          2.1793786728       -1.6693140839        1.1823811296
         H          1.0245787819       -1.5392035408       -0.0507409777
         H         -1.6858363886        2.1297513428        0.7452520673
          25
                -34.15874648
         O         -2.3605981473       -0.8116042758        0.8659409274
         O         -0.5843274484       -1.6230194998       -0.2244865945
         N         -2.2109877661        1.5364887021        0.0742080589
         N          2.0757147084       -1.4672125485       -0.4830865528
         C          0.8597549995        1.2262238823        0.4517476395
         C          0.0874032105        1.1513231513       -0.8771132949
         C          2.3512317789        0.8985689618        0.2982329522
         C         -1.3361657088        0.6347191585       -0.7132778341
         C          2.6824356905       -0.5712975375        0.5355813092
         C         -1.4234260659       -0.7566236862        0.0528412632
         H          0.4221004807        0.5356789162        1.1753502177
         H          0.7733707657        2.2309740830        0.8650565898
         H          0.5827203837        0.4802075195       -1.5756071724
         H          0.0631682297        2.1335262804       -1.3514203271
         H          2.7003072358        1.2129278451       -0.6855204577
         H          2.9172351560        1.4667500260        1.0360745366
         H         -1.7833398578        0.4616073570       -1.6955611361
         H          2.3142140534       -0.8770131145        1.5154931358
         H          3.7656889000       -0.7005750162        0.5151167425
         H         -1.6671444199        2.1735125324        0.6661186569
         H         -2.8827343086        2.0695764162       -0.4835506438
         H          2.3643871815       -1.1879653946       -1.4273853498
         H          2.3725094498       -2.4360036549       -0.3237631449
         H          1.0071911744       -1.4659435853       -0.4227446705
         H         -2.7090441201        0.8070062032        0.6949537917
          25
                -34.15780648
         O         -0.6998712600       -1.7595821198        0.3315266042
         O         -2.7564980797       -0.8922468771        0.1127087169
         N         -2.0912345729        1.3897515697       -0.6207661035
         N          1.9218668343       -1.5538051846       -0.0911139226
         C          1.2724611881        1.7665596691        0.2614262584
         C         -0.0126245071        1.0963347719        0.7686474620
         C          2.5091659413        0.8642624497        0.2746224048
         C         -0.9098130242        0.5407794733       -0.3415020783
         C          2.4929985644       -0.3196472927       -0.6935656161
         C         -1.5185682699       -0.8616584773        0.0830057999
         H          1.4961397665        2.6156227165        0.9073501428
         H          1.1219744652        2.1603919352       -0.7434483203
         H         -0.5894557289        1.8123867185        1.3552290309
         H          0.2456564204        0.2915541326        1.4587568868
         H          3.3629575544        1.4905499194        0.0148013745
         H          2.6790096911        0.5030846620        1.2896168914
         H         -0.3423105707        0.3842564461       -1.2596402044
         H          3.5203242755       -0.5424656780       -0.9864282797
         H          1.9321769951       -0.0751719518       -1.5947809502
         H         -2.8797605313        0.6907420298       -0.3760235139
         H         -2.1503924665        2.2120232522       -0.0104200923
         H          2.3803263882       -1.7503210865        0.8055549093
         H          0.8561607870       -1.5268128372        0.0892049642
         H          2.0800598725       -2.3543999415       -0.7123089418
         H         -2.1774862389        1.6805852373       -1.5986092278
          25
                -34.15712409
         O         -0.5169415866       -1.4677953372       -0.4155166026
         O         -2.6718105717       -1.1029442546        0.0532698902
         N         -2.5041882719        1.3222450456        0.5356135683
         N          2.0547500815       -1.4660199090        0.3952674841
         C          1.1460672076        1.2350620947       -0.5905524477
         C          0.0199991306        1.2471137062        0.4571994904
         C          2.5324119546        0.9449216441        0.0010319703
         C         -1.3028765754        0.8219664596       -0.1753086903
         C          2.9759584940       -0.4928036507       -0.2449354813
         C         -1.5027299259       -0.7570468322       -0.1889883845
         H          1.1567284127        2.2014747982       -1.0932584224
         H          0.9206483903        0.4869950226       -1.3509226784
         H         -0.0730555237        2.2557886152        0.8619704582
         H          0.2503077610        0.5750867956        1.2847618985
         H          3.2724883861        1.5930094341       -0.4660504111
         H          2.5411017268        1.1667575642        1.0680525607
         H         -1.3439398378        1.1490834108       -1.2170635960
         H          3.9809942022       -0.6416689206        0.1522940223
         H          2.9906906878       -0.6917352586       -1.3167852841
         H         -3.1147358709        0.4302326474        0.4978236161
         H         -2.3055244586        1.5552316017        1.5147727778
         H          1.0589542040       -1.3722826387        0.0237179616
         H          2.3537713222       -2.4274392985        0.2004063016
         H          2.0384435533       -1.3281897829        1.4119124615
         H         -2.9671283162        2.1123945703        0.0792841186
          25
                -34.15703719
         O         -0.4840204654       -1.5908390855       -0.1977054306
         O         -2.6424172363       -1.0172969494       -0.2313294170
         N         -2.3824391815        1.4118161237        0.1129756179
         N          2.0591606020       -1.4080559447        0.6444993748
         C          1.3255949204        1.5932768744        0.1316443482
         C          0.0292286137        1.1405631347        0.8180158808
         C          1.9611296764        0.5914767293       -0.8382657525
         C         -1.0774245534        0.7702791240       -0.1744053472
         C          2.8498636045       -0.4645206094       -0.1922799795
         C         -1.4218007565       -0.7850501416       -0.2021503198
         H          2.0511714191        1.8626404605        0.8996124027
         H          1.1017826165        2.5033885185       -0.4265728164
         H         -0.3083479528        1.9763847128        1.4332069953
         H          0.2033356421        0.3027453082        1.4918974312
         H          1.1939462785        0.0852292002       -1.4240400771
         H          2.5871303840        1.1474547523       -1.5362113415
         H         -0.7775416723        1.0312499156       -1.1919311777
         H          3.6205966803        0.0055444316        0.4194317100
         H          3.3361661233       -1.0318279746       -0.9856062704
         H         -2.6284628403        2.1753990573       -0.5216811303
         H         -3.0325627338        0.5540981187       -0.0284710758
         H          2.4910941867       -2.3367192092        0.6680136974
         H          1.9781398838       -1.0663385144        1.6071566991
         H          1.0702227114       -1.5059322479        0.2455456256
         H         -2.4592842029        1.7302943098        1.0848423546
          25
                -34.15632171
         O         -0.6377330316       -1.5602972801       -0.5925407457
         O         -1.9438226066       -0.8962122015        1.0968963302
         N         -2.0691310397        1.5115608178        0.5520085904
         N          2.0161394685       -1.6864701960       -0.6808935380
         C          1.0245208700        1.5678424724        0.3114580637
         C          0.0268814843        1.3822638313       -0.8376423667
         C          1.7125418505        0.2918072015        0.8062089597
         C         -1.3255436976        0.7602094261       -0.4861917907
         C          2.5798867778       -0.3735189736       -0.2699515146
         C         -1.2821941821       -0.7269243894        0.0579815663
         H          0.5483025277        2.0449086772        1.1692286093
         H          1.7966540418        2.2561425086       -0.0359536522
         H          0.4649670843        0.7632830938       -1.6196837604
         H         -0.1661732017        2.3619672870       -1.2796881790
         H          2.3322565126        0.5621906956        1.6605054657
         H          0.9655697921       -0.4140620317        1.1690705974
         H         -1.9201614836        0.7061105980       -1.4020292431
         H          3.5936748701       -0.5312821922        0.0979019662
         H          2.6369135707        0.2686143194       -1.1490685384
         H         -2.9086496435        1.9840429736        0.2094100180
         H         -2.3313717130        0.6888337551        1.2084854943
         H          2.1820919942       -2.3913736082        0.0460188498
         H          0.9581287874       -1.6201065231       -0.7926007692
         H          2.4297604389       -2.0163864988       -1.5591512145
         H         -1.4715716162        2.1835379113        1.0453989384
          25
                -34.15592507
         O         -2.1033969149       -0.8344078612        0.9487088877
         O         -0.5699612374       -1.6680274471       -0.4517081377
         N         -1.9569275917        1.5465619838        0.2627686808
         N          2.0837337252       -1.3633283870       -0.5773762363
         C          1.1075416509        1.5419562581       -0.0103918263
         C          0.1209945172        1.1686244794       -1.1251953409
         C          1.4352918321        0.4570859531        1.0214669735
         C         -1.2503428799        0.6557309102       -0.6867527508
         C          2.5057372966       -0.5407868185        0.5902823997
         C         -1.2948162683       -0.7745608298        0.0075959523
         H          0.7274518351        2.4141375453        0.5251162579
         H          2.0330446007        1.8691474761       -0.4864359338
         H          0.5312965142        0.4086384701       -1.7869917742
         H         -0.0324587227        2.0621064981       -1.7347668750
         H          1.8169519144        0.9463147999        1.9183948838
         H          0.5367641743       -0.0848292604        1.3149274400
         H         -1.8634330878        0.5270669166       -1.5839659846
         H          2.7015999487       -1.2099014501        1.4282318085
         H          3.4296803314       -0.0167769980        0.3414844052
         H         -1.2992225514        2.1272618717        0.7958603647
         H         -2.6755557698        2.1370116562       -0.1630903688
         H          2.2813676133       -0.8782194673       -1.4586367594
         H          2.5735263109       -2.2635096262       -0.5875520766
         H          1.0325521121       -1.5556317222       -0.5353874345
         H         -2.3935764356        0.8063761382        0.9145561672
          25
                -34.15547499
         O         -2.3288681540       -0.9689367039        0.5974852723
         O         -0.4008216845       -1.3559960891       -0.4559450313
         N         -2.6333104330        1.3901145964        0.0148791472
         N          2.2688734235       -1.7836122490       -0.3135245436
         C          1.2045970266        1.2147830910       -0.3207104722
         C         -0.1766849998        1.7049883679        0.1388725454
         C          1.9421007389        0.4104322018        0.7578355455
         C         -1.3241367595        0.8966054975       -0.4749078790
         C          2.9601024995       -0.5430248114        0.1410496733
         C         -1.3285933693       -0.6410895869       -0.0712604222
         H          1.8194085679        2.0702167267       -0.5988277282
         H          1.0710671602        0.6065579215       -1.2146344993
         H         -0.3076575365        2.7461839050       -0.1612866699
         H         -0.2378627475        1.6551531995        1.2275331138
         H          2.4468113264        1.0960018700        1.4370167059
         H          1.2297127637       -0.1683684162        1.3468389979
         H         -1.2704988015        0.9491259299       -1.5640455987
         H          3.7310656000       -0.8072491298        0.8640696447
         H          3.4371256948       -0.0632845904       -0.7133472266
         H         -2.5432643621        2.1520849835        0.6930123940
         H         -3.2834926874        1.6599628672       -0.7262042831
         H          2.7178100064       -2.1982719631       -1.1361624210
         H          2.2482990815       -2.4828183187        0.4368503414
         H          1.2567750610       -1.5723871751       -0.5319861416
         H         -2.9757946227        0.4721100866        0.5082732629
          25
                -34.15545846
         O         -2.3777023451       -0.9177733836        0.6489622803
         O         -0.4009151018       -1.4460423055       -0.2363506290
         N         -2.5928675119        1.3702602408       -0.1958788709
         N          2.2705863538       -1.5552728060       -0.6959671399
         C          1.2626408325        1.2650767695       -0.1999508619
         C         -0.1676900608        1.6997857440        0.1433859727
         C          1.8576282855        0.3222355626        0.8537184709
         C         -1.2484356318        0.8255651567       -0.5054674146
         C          2.9084111559       -0.6168744122        0.2764230940
         C         -1.3258577703       -0.6732783629        0.0271901706
         H          1.8935551201        2.1501893106       -0.2772016053
         H          1.2458750287        0.7908735642       -1.1798122422
         H         -0.3171466901        2.7186794233       -0.2201486160
         H         -0.2967658760        1.7007241110        1.2272150872
         H          2.3089550370        0.9081814099        1.6535209776
         H          1.0686425282       -0.2818531316        1.3016188748
         H         -1.0876822336        0.7817119785       -1.5847014431
         H          3.3524799930       -1.1924945383        1.0873745985
         H          3.6963304583       -0.0519426265       -0.2212122945
         H         -2.9913275573        0.5162412164        0.3615908966
         H         -2.5589278358        2.2025641690        0.3999047288
         H          2.6187881911       -2.5117770389       -0.5815509942
         H          1.2217357961       -1.5619680721       -0.5321789754
         H          2.4345460024       -1.2660811775       -1.6650051770
         H         -3.1696789754        1.5558016135       -1.0193997348
          25
                -34.15372845
         O         -0.8759513206       -1.0507714945        0.7395130487
         O         -1.0471783147       -1.2210634731       -1.4857197061
         N         -2.0100713682        1.1638582833        0.9733772433
         N          1.7020599065       -1.7447966827        0.8365171560
         C          0.9224701988        1.3977552196        0.1299066020
         C         -0.3034955554        1.7474745141       -0.7290850754
         C          1.8313865205        0.3371413309       -0.5152183962
         C         -1.5397536849        0.8954837973       -0.4124258872
         C          2.5286316072       -0.5462459399        0.5204181162
         C         -1.1337894558       -0.6241211220       -0.4224393440
         H          0.5999134506        1.0447379113        1.1088504940
         H          1.5075448530        2.3032957331        0.2891927248
         H         -0.0603736240        1.6138572758       -1.7832639404
         H         -0.5689030950        2.7975685720       -0.5933476217
         H          1.2520636483       -0.2949117808       -1.1892484363
         H          2.5821478388        0.8362393886       -1.1261103610
         H         -2.3194229349        1.0797505602       -1.1497937252
         H          2.6951475316        0.0179430508        1.4383444706
         H          3.4942793529       -0.8824584649        0.1428332329
         H         -1.6366862386        2.0406897131        1.3566364027
         H         -3.0319635647        1.1643591957        1.0502533535
         H          2.0123953855       -2.1956055335        1.7033301803
         H          0.6617699349       -1.4988916853        0.9180060433
         H          1.7620357216       -2.4244612968        0.0700935319
         H         -1.6155014744        0.3110635636        1.4848864268
          25
                -34.15296366
         O         -0.9806508876       -0.9847570656        0.9016095316
         O         -1.0342961735       -1.4975369515       -1.2790158618
         N         -2.0600699488        1.3003330160        0.6598555012
         N          1.6483681534       -1.4770759046        1.0193012553
         C          0.8930197778        1.4409262402        0.3162871289
         C         -0.1018287155        1.4892580056       -0.8622608316
         C          2.2127299908        0.7309357778       -0.0043863659
         C         -1.4370888955        0.7837090011       -0.5889439421
         C          2.1074033787       -0.7768274559       -0.2134156979
         C         -1.1447588094       -0.7374998177       -0.3265126418
         H          0.4424872625        0.9591479756        1.1822965353
         H          1.1476108940        2.4604379775        0.6056686014
         H          0.3344753521        1.0203527521       -1.7446629435
         H         -0.3104542151        2.5259312846       -1.1308040482
         H          2.6400205950        1.1617275571       -0.9101356051
         H          2.9157320587        0.9267783500        0.8074716570
         H         -2.0965188879        0.8923814409       -1.4486238017
         H          3.0903725751       -1.1592546293       -0.4905246456
         H          1.4081882505       -1.0149227036       -1.0156071158
         H         -3.0831759004        1.3363322990        0.6000761811
         H         -1.7780292676        0.5566802205        1.3629478428
         H          0.5935972201       -1.3552723672        1.1310943683
         H          1.8230458958       -2.4842696303        0.9418471118
         H          2.1352867113       -1.1124232113        1.8467861942
         H         -1.6994055067        2.2241075607        0.9291898687
          25
                -34.15141381
         O         -1.3943785432       -1.0307567325        0.9025777408
         O         -0.4225106153       -1.2766835982       -1.0942963862
         N         -2.1382332497        1.3072100696        0.6820859684
         N          1.8303685962       -1.9710495392        0.0707587666
         C          0.9607624662        1.6168605590        0.1707894371
         C         -0.1959862014        1.7114505222       -0.8393306352
         C          1.9799050629        0.5013843169       -0.1348429740
         C         -1.4247901716        0.8500833461       -0.5368218294
         C          1.9371661639       -0.6960029627        0.8273160062
         C         -1.0467465663       -0.6453837331       -0.2300826075
         H          0.5821582777        1.4936196592        1.1855414824
         H          1.4853467761        2.5717069600        0.1437592453
         H          0.1684852019        1.4328271659       -1.8279890567
         H         -0.5295558410        2.7487716363       -0.9068101296
         H          1.7990108513        0.1423863706       -1.1479579769
         H          2.9805578856        0.9303497506       -0.1217137710
         H         -2.0885910031        0.8601550715       -1.4024758281
         H          1.0694995988       -0.6377417602        1.4853144468
         H          2.8334350246       -0.7245407799        1.4470296049
         H         -1.6564311817        2.0816802979        1.1506799283
         H         -3.1156981590        1.5603768219        0.5197984898
         H          2.6238854975       -2.0888513426       -0.5678832194
         H          1.7803639034       -2.7755595157        0.7040003308
         H          0.9218328585       -1.9204346441       -0.4990801476
         H         -2.0745975566        0.3957317695        1.2728068192
          25
                -34.15122546
         O         -1.4776298354       -0.9779162840        0.9689911790
         O         -0.4925234496       -1.3628551244       -1.0036507954
         N         -2.1639329473        1.3683833598        0.5828976748
         N          1.8796097534       -1.9018199765       -0.0324662357
         C          0.9212842989        1.5787487906        0.2022882964
         C         -0.1575522111        1.5949891498       -0.8983466380
         C          2.0807266384        0.5903317386       -0.0312863938
         C         -1.4336452672        0.8124987799       -0.5833640723
         C          2.0372506913       -0.6914975753        0.8153963432
         C         -1.1077721831       -0.6712596326       -0.1784565029
         H          0.4772061571        1.3762504059        1.1769048440
         H          1.3491352329        2.5796760629        0.2516191005
         H          0.2519128515        1.1866703660       -1.8221067218
         H         -0.4443786804        2.6264492546       -1.1098247151
         H          2.1069094344        0.3214704229       -1.0870338715
         H          3.0133169149        1.1098047495        0.1839278312
         H         -2.0690635873        0.7917157945       -1.4696202320
         H          1.1994207751       -0.6693189818        1.5132782369
         H          2.9569334194       -0.7921592267        1.3922309558
         H         -1.6800036187        2.1679532791        1.0053931771
         H         -3.1334683512        1.6233161070        0.3791665015
         H          2.6232513605       -1.9579134554       -0.7362257317
         H          1.8912989959       -2.7525757838        0.5392212719
         H          0.9273933706       -1.8444066710       -0.5257136187
         H         -2.1333692549        0.5050567581        1.2388793063
          25
                -34.15102491
         O         -2.7007099152       -1.6852260154       -0.4665848832
         O         -3.5578446821        0.0350414384        0.6950996250
         N         -1.7249631955        1.6316108345        0.4298941943
         N          4.8503432460       -0.2765578553        0.0049566510
         C          1.1047501969        0.4501828237       -0.3379055216
         C         -0.1445140391       -0.2886936630        0.1459845288
         C          2.3741940866       -0.2828979224        0.0965187068
         C         -1.4339016427        0.3793165613       -0.3028003047
         C          3.6160850376        0.4539254738       -0.3951671902
         C         -2.6934727541       -0.5488189533       -0.0039847520
         H          1.1202182595        1.4641232269        0.0631636060
         H          1.0744507790        0.5160807326       -1.4261342120
         H         -0.1466884742       -0.3764861277        1.2347858649
         H         -0.1614173698       -1.3033571375       -0.2567289346
         H          2.3826839006       -0.3550960319        1.1849100754
         H          2.3458274825       -1.2937888279       -0.3120785959
         H         -1.4193517541        0.5529473168       -1.3796978179
         H          3.6476836700        1.4575725004        0.0303995802
         H          3.5908364898        0.5356651831       -1.4823627548
         H         -1.7888195298        2.4639789876       -0.1581990321
         H         -2.7206446817        1.3385249836        0.8116492773
         H          5.6915565940        0.2174183391       -0.3259194480
         H          4.8962264202       -0.3605243899        1.0314916029
         H          4.8443782262       -1.2261178281       -0.3963638456
         H         -1.0847775954        1.7904079765        1.2125175138
          25
                -34.15096274
         O         -3.0664396656       -0.6811514259        0.4732552569
         O         -1.1830540829       -1.6357953168       -0.2743493731
         N         -2.5677177347        1.6455787239       -0.0357930901
         N          3.4161836207       -1.4947231742        0.0043983586
         C          1.0526738400        0.4198619079       -0.3271341828
         C         -0.1392962419        1.1756334823        0.2601668795
         C          2.3301787116        0.6870764296        0.4694516416
         C         -1.4374925629        0.7826136622       -0.4432938226
         C          3.5433351448       -0.0155439223       -0.1263696890
         C         -1.9232732343       -0.6849210780       -0.0466890927
         H          1.1914397265        0.7321338619       -1.3635085567
         H          0.7858921189       -0.6373779302       -0.3204311762
         H          0.0218209785        2.2505182304        0.1468292069
         H         -0.2266241551        0.9455046686        1.3247226359
         H          2.5278796928        1.7590277621        0.4727119199
         H          2.1882724928        0.3782271446        1.5058219746
         H         -1.3025118280        0.8032883597       -1.5259809861
         H          4.4479425383        0.3027435209        0.3921583620
         H          3.6346463266        0.2349775607       -1.1836334244
         H         -3.0211573542        2.1410074345       -0.8045922540
         H         -3.2218629847        0.8371482423        0.3587042034
         H          4.2253281871       -1.9726822174       -0.4155872948
         H          3.3489651634       -1.7581308116        0.9983748528
         H          2.5570336567       -1.8144640232       -0.4679619280
         H         -2.3203269015        2.3008181314        0.7098687755
          25
                -34.15085064
         O         -3.5353488917        0.0322108098        0.4084906022
         O         -2.4254579214       -1.9084093375        0.6284254091
         N         -1.7744403653        1.3652775888       -0.6438663864
         N          4.1843308757       -0.1227935311       -1.0149931633
         C          1.2054477125        0.3338345853       -0.2573250215
         C         -0.0723683835       -0.0922253390        0.4701395235
         C          2.4067626747        0.2476116899        0.6871054133
         C         -1.3095998224       -0.0201188789       -0.4106994862
         C          3.6989393857        0.7735207317        0.0732627710
         C         -2.5481677099       -0.7347193516        0.2919952398
         H          1.1039179513        1.3593179418       -0.6150772671
         H          1.3305507433       -0.3269971917       -1.1156830038
         H         -0.2323641779        0.5179203592        1.3619095074
         H          0.0148258366       -1.1272987239        0.8068454593
         H          2.1939049448        0.8432977651        1.5748900270
         H          2.5356727482       -0.7824695024        1.0211816116
         H         -1.1430786561       -0.5391980852       -1.3555500184
         H          4.4681954117        0.8303339300        0.8434799998
         H          3.5410323285        1.7707403176       -0.3387001734
         H         -2.7959421798        1.2370421526       -0.2418926774
         H         -1.2663143192        2.0543977636       -0.0827359543
         H          4.3101055621       -1.0795234870       -0.6518667702
         H          3.4997443822       -0.1538144186       -1.7841531054
         H          5.0842467659        0.2120203831       -1.3873716549
         H         -1.8037773550        1.6476905491       -1.6249441216
          25
                -34.15048917
         O         -1.9774584219       -0.8822136242       -1.3447844876
         O         -0.7353222350       -1.3040874260        0.4712779300
         N         -1.2018711229        0.8204001109        1.6759883927
         N          1.9464051556       -1.6923491374        0.3496868798
         C          0.8058411616        1.0462579377       -0.9468506072
         C         -0.4754178942        1.7785998054       -0.5275051890
         C          1.8226970853        0.7789686777        0.1752315710
         C         -1.5019161161        0.9155320919        0.2220611780
         C          2.6576744598       -0.4659344426       -0.1177787426
         C         -1.4255171488       -0.5796126398       -0.2987743324
         H          1.2927227489        1.6467822241       -1.7161034319
         H          0.5115436171        0.1136959367       -1.4299162738
         H         -0.9626535770        2.1244807715       -1.4395462399
         H         -0.2313854006        2.6631384020        0.0636669169
         H          2.4836824282        1.6401220787        0.2631946203
         H          1.3379895832        0.6415415675        1.1385387246
         H         -2.5044279955        1.3042479639        0.0511624497
         H          3.6220420553       -0.4105315490        0.3870533340
         H          2.8346807725       -0.5412209492       -1.1903941633
         H         -0.5537202797        1.5469435382        2.0029686770
         H         -2.0510266978        0.8448716653        2.2497725116
         H          0.8939724574       -1.6086997720        0.2334441992
         H          2.2630899289       -2.5235198565       -0.1606712473
         H          2.1155048181       -1.8450780227        1.3513380594
         H         -0.7798223195       -0.1709536166        1.7101046448
          25
                -34.15043307
         O         -3.3078583007       -0.3518099727        0.5480566241
         O         -1.8441149941       -2.0073969491        0.1397595703
         N         -1.9672328473        1.5403727742       -0.2372000800
         N          3.6044925126       -0.8358440764       -0.6659647320
         C          1.1684565146        1.0446332415       -0.4943940738
         C          0.1082467096        0.2177955398        0.2395309885
         C          2.4785715426        1.1657908669        0.2896724500
         C         -1.2458520269        0.2676289576       -0.4513144135
         C          3.1696429470       -0.1592315605        0.5890169973
         C         -2.2263428078       -0.8414226742        0.1422529421
         H          0.7932826990        2.0569151989       -0.6482792542
         H          1.3354698371        0.6191303209       -1.4843783659
         H         -0.0031461344        0.5661490514        1.2686433794
         H          0.3791631733       -0.8378892566        0.2841197320
         H          3.1579098277        1.8188328559       -0.2597110183
         H          2.2713923792        1.6478155935        1.2450536588
         H         -1.1432114953        0.0569418065       -1.5170262136
         H          2.5004236326       -0.8240708177        1.1341470919
         H          4.0493855213        0.0280822082        1.2049984895
         H         -1.5070190112        2.1488324010        0.4454597884
         H         -2.1837208522        2.0584218679       -1.0901715209
         H          4.2094106392       -0.2068158824       -1.2153183037
         H          4.1256865653       -1.6973771969       -0.4514692989
         H          2.7834959501       -1.0854874671       -1.2358687369
         H         -2.8845837684        1.1083956166        0.2016053653



Calculate free energies on populated, DFT optimized conformers
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

.. note::

    * Using sorting parts: part0 and part1 in order to reduce the number of 
      computational costly DFT geometry optimizations
    * example settings are read from .censorc file


.. tabbed:: input

    .. code:: sh

        $ censo -inp ensemble.xyz -inprc /path/to/.censorc > censo.out &

.. tabbed:: output

    .. code:: sh

                 ______________________________________________________________
                |                                                              |
                |                                                              |
                |                   CENSO - Commandline ENSO                   |
                |                           v 1.0.3                            |
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


        ----------------------------------------------------------------------------------------------------
                                                     PARAMETERS
        ----------------------------------------------------------------------------------------------------

        The configuration file .censorc is read from /home/bohle/1projects/from_tmp1/CENSO/documentation-calcs/3-part0-2/.censorc.
        Reading conformer rotamer ensemble from: /home/bohle/1projects/from_tmp1/CENSO/documentation-calcs/3-part0-2/ensemble.xyz.
        Reading file: censo_solvents.json

        --------------------------------------------------
                       CRE SORTING SETTINGS               
        --------------------------------------------------

        number of atoms in system:                                     25
        number of considered conformers:                               22
        number of all conformers from input:                           22
        charge:                                                        1
        unpaired:                                                      0
        solvent:                                                       h2o
        temperature:                                                   298.15
        evaluate at different temperatures:                            on
        temperature range:                                             273.15, 278.15, 283.15, 288.15, ...
        calculate mRRHO contribution:                                  on
        consider symmetry for mRRHO contribution:                      off
        cautious checking for error and failed calculations:           on
        checking the DFT-ensemble using CREST:                         off
        maxthreads:                                                    4
        omp:                                                           2

        --------------------------------------------------
                  CRE CHEAP-PRESCREENING - PART0          
        --------------------------------------------------
        part0:                                                         on
        starting number of considered conformers:                      22
        program for part0:                                             tm
        functional for fast single-point:                              b97-d
        basis set for fast single-point:                               def2-SV(P)
        threshold g_thr(0) for sorting in part0:                       4.0
        Solvent model used with xTB:                                   alpb

        short-notation:
        b97-d-D3/def2-SV(P) // GFNn-xTB (Input geometry)

        --------------------------------------------------
                     CRE PRESCREENING - PART1             
        --------------------------------------------------
        part1:                                                         on
        starting number of considered conformers:                      22
        program for part1:                                             tm
        functional for initial evaluation:                             r2scan-3c
        basis set for initial evaluation:                              def2-mTZVPP
        calculate mRRHO contribution:                                  on
        program for mRRHO contribution:                                xtb
        GFN version for mRRHO and/or GBSA_Gsolv:                       gfn2
        Apply constraint to input geometry during mRRHO calculation:   on
        solvent model applied with xTB:                                alpb
        evaluate at different temperatures:                            off
        threshold g_thr(1) and G_thr(1) for sorting in part1:          3.5
        solvent model for Gsolv contribution of part1:                 cosmors

        short-notation:
        r2scan-3c + COSMORS[h2o] + GmRRHO(GFN2[alpb]-bhess) // GFNn-xTB (Input geometry)

        --------------------------------------------------
                     CRE OPTIMIZATION - PART2             
        --------------------------------------------------
        part2:                                                         on
        program:                                                       tm
        functional for part2:                                          r2scan-3c
        basis set for part2:                                           def2-mTZVPP
        using xTB-optimizer for optimization:                          on
        using the new ensemble optimizer:                              on
        optimize all conformers below this G_thr(opt,2) threshold:     2.5
        spearmanthr:                                                   0.935
        optimization level in part2:                                   lax
        solvent model applied in the optimization:                     dcosmors
        solvent model for Gsolv contribution:                          cosmors
        evaluate at different temperatures:                            on
        Boltzmann sum threshold G_thr(2) for sorting in part2:         99.0
        calculate mRRHO contribution:                                  on
        program for mRRHO contribution:                                xtb
        GFN version for mRRHO and/or GBSA_Gsolv:                       gfn2
        Apply constraint to input geometry during mRRHO calculation:   on
        solvent model applied with xTB:                                alpb

        short-notation:
        r2scan-3c + COSMORS[h2o] + GmRRHO(GFN2[alpb]-bhess) // r2scan-3c[DCOSMORS] 
        END of parameters


        ------------------------------------------------------------
                       PATHS of external QM programs                
        ------------------------------------------------------------

        The following program paths are used:
            xTB:          /home/abt-grimme/AK-bin/xtb
            TURBOMOLE:    /home/abt-grimme/TURBOMOLE.7.5.1/bin/em64t-unknown-linux-gnu_smp
            Setup of COSMO-RS:
                ctd = BP_TZVPD_FINE_C30_1601.ctd
                cdir = "/home/bohle/COSMOlogic/COSMOthermX19/COSMOtherm/CTDATA-FILES"
                ldir = "/home/bohle/COSMOlogic/COSMOthermX19/COSMOtherm/CTDATA-FILES"
            Using /home/bohle/COSMOlogic/COSMOthermX19/COSMOtherm/DATABASE-COSMO/BP-TZVP-COSMO
            as path to the COSMO-RS NORMAL DATABASE.

            Using cefine from /home/bohle/bin/cefine
            PARNODES for TM or COSMO-RS calculation was set to 2
            Using COSMOtherm from /home/bohle/COSMOlogic/COSMOthermX19/COSMOtherm/BIN-LINUX/cosmotherm

        ----------------------------------------------------------------------------------------------------
                                    Processing data from previous run (enso.json)                           
        ----------------------------------------------------------------------------------------------------

        INFORMATION: No restart information exists and is created during this run!


        ----------------------------------------------------------------------------------------------------
                                           CRE CHEAP-PRESCREENING - PART0                                   
        ----------------------------------------------------------------------------------------------------

        program:                                                       tm
        functional for part0:                                          b97-d
        basis set for part0:                                           def2-SV(P)
        threshold g_thr(0):                                            4.0
        starting number of considered conformers:                      22

        Calculating efficient gas-phase single-point energies:
        The efficient gas-phase single-point is calculated for:
         CONF1,  CONF2,  CONF3,  CONF4,  CONF5,  CONF6,  CONF7,  CONF8,  CONF9, CONF10, CONF11
        CONF12, CONF13, CONF14, CONF15, CONF16, CONF17, CONF18, CONF19, CONF20, CONF21, CONF22
        Constructed folders!

        Starting 22 ALPB-Gsolv calculations
        Running single-point in CONF1/part0_sp    
        Running single-point in CONF2/part0_sp    
        Running single-point in CONF3/part0_sp    
        Running single-point in CONF4/part0_sp    
        Running ALPB_GSOLV calculation in 3-part0-2/CONF1/part0_sp
        Running ALPB_GSOLV calculation in 3-part0-2/CONF3/part0_sp
        Running ALPB_GSOLV calculation in 3-part0-2/CONF2/part0_sp
        Running ALPB_GSOLV calculation in 3-part0-2/CONF4/part0_sp
        Running single-point in CONF5/part0_sp    
        Running single-point in CONF6/part0_sp    
        Running single-point in CONF7/part0_sp    
        Running single-point in CONF8/part0_sp    
        Running ALPB_GSOLV calculation in 3-part0-2/CONF5/part0_sp
        Running ALPB_GSOLV calculation in 3-part0-2/CONF6/part0_sp
        Running ALPB_GSOLV calculation in 3-part0-2/CONF7/part0_sp
        Running ALPB_GSOLV calculation in 3-part0-2/CONF8/part0_sp
        Running single-point in CONF9/part0_sp    
        Running single-point in CONF11/part0_sp   
        Running single-point in CONF10/part0_sp   
        Running single-point in CONF12/part0_sp   
        Running ALPB_GSOLV calculation in 3-part0-2/CONF9/part0_sp
        Running ALPB_GSOLV calculation in 3-part0-2/CONF10/part0_sp
        Running ALPB_GSOLV calculation in 3-part0-2/CONF11/part0_sp
        Running ALPB_GSOLV calculation in 3-part0-2/CONF12/part0_sp
        Running single-point in CONF13/part0_sp   
        Running single-point in CONF15/part0_sp   
        Running single-point in CONF14/part0_sp   
        Running single-point in CONF16/part0_sp   
        Running ALPB_GSOLV calculation in 3-part0-2/CONF13/part0_sp
        Running ALPB_GSOLV calculation in 3-part0-2/CONF15/part0_sp
        Running ALPB_GSOLV calculation in 3-part0-2/CONF14/part0_sp
        Running single-point in CONF17/part0_sp   
        Running ALPB_GSOLV calculation in 3-part0-2/CONF16/part0_sp
        Running single-point in CONF18/part0_sp   
        Running single-point in CONF19/part0_sp   
        Running ALPB_GSOLV calculation in 3-part0-2/CONF17/part0_sp
        Running single-point in CONF20/part0_sp   
        Running ALPB_GSOLV calculation in 3-part0-2/CONF18/part0_sp
        Running ALPB_GSOLV calculation in 3-part0-2/CONF19/part0_sp
        Running single-point in CONF21/part0_sp   
        Running single-point in CONF22/part0_sp   
        Running ALPB_GSOLV calculation in 3-part0-2/CONF20/part0_sp
        Running ALPB_GSOLV calculation in 3-part0-2/CONF22/part0_sp
        Running ALPB_GSOLV calculation in 3-part0-2/CONF21/part0_sp
        Tasks completed!

        The efficient gas-phase single-point was successful for  CONF1/part0_sp: E(DFT) = -496.55110270 Gsolv = -0.12279245
        The efficient gas-phase single-point was successful for  CONF2/part0_sp: E(DFT) = -496.55473079 Gsolv = -0.12241044
        The efficient gas-phase single-point was successful for  CONF3/part0_sp: E(DFT) = -496.55551554 Gsolv = -0.12099969
        The efficient gas-phase single-point was successful for  CONF4/part0_sp: E(DFT) = -496.55384665 Gsolv = -0.12156811
        The efficient gas-phase single-point was successful for  CONF5/part0_sp: E(DFT) = -496.55341065 Gsolv = -0.12251050
        The efficient gas-phase single-point was successful for  CONF6/part0_sp: E(DFT) = -496.55368828 Gsolv = -0.12175678
        The efficient gas-phase single-point was successful for  CONF7/part0_sp: E(DFT) = -496.54759208 Gsolv = -0.12365913
        The efficient gas-phase single-point was successful for  CONF8/part0_sp: E(DFT) = -496.55118920 Gsolv = -0.12200372
        The efficient gas-phase single-point was successful for  CONF9/part0_sp: E(DFT) = -496.54970422 Gsolv = -0.12167549
        The efficient gas-phase single-point was successful for CONF10/part0_sp: E(DFT) = -496.55157272 Gsolv = -0.12001541
        The efficient gas-phase single-point was successful for CONF11/part0_sp: E(DFT) = -496.54991969 Gsolv = -0.12296934
        The efficient gas-phase single-point was successful for CONF12/part0_sp: E(DFT) = -496.55212770 Gsolv = -0.11751864
        The efficient gas-phase single-point was successful for CONF13/part0_sp: E(DFT) = -496.55077718 Gsolv = -0.11854291
        The efficient gas-phase single-point was successful for CONF14/part0_sp: E(DFT) = -496.55028792 Gsolv = -0.12090071
        The efficient gas-phase single-point was successful for CONF15/part0_sp: E(DFT) = -496.54987374 Gsolv = -0.12311773
        The efficient gas-phase single-point was successful for CONF16/part0_sp: E(DFT) = -496.55360018 Gsolv = -0.11274054
        The efficient gas-phase single-point was successful for CONF17/part0_sp: E(DFT) = -496.55289531 Gsolv = -0.11378052
        The efficient gas-phase single-point was successful for CONF18/part0_sp: E(DFT) = -496.50423172 Gsolv = -0.16468285
        The efficient gas-phase single-point was successful for CONF19/part0_sp: E(DFT) = -496.52384365 Gsolv = -0.14390846
        The efficient gas-phase single-point was successful for CONF20/part0_sp: E(DFT) = -496.50481476 Gsolv = -0.16519450
        The efficient gas-phase single-point was successful for CONF21/part0_sp: E(DFT) = -496.53860751 Gsolv = -0.13212346
        The efficient gas-phase single-point was successful for CONF22/part0_sp: E(DFT) = -496.50938650 Gsolv = -0.15826546

        ----------------------------------------------------------------------------------------------------
                           Removing high lying conformers by improved energy description                    
        ----------------------------------------------------------------------------------------------------

         CONF#       G [Eh] ΔG [kcal/mol]                 E [Eh]   Gsolv [Eh]         Gtot    ΔE(DFT)     ΔGsolv      ΔGtot
                   GFN2-xTB      GFN2-xTB b97-d-D3(0)/def2-SV(P)         alpb         [Eh] [kcal/mol] [kcal/mol] [kcal/mol]
                     [ALPB]        [ALPB]                              [gfn2]                                              
        CONF1   -34.1746819          0.00           -496.5511027   -0.1227924 -496.6738951       2.28      -0.24       2.04
        CONF2   -34.1743917          0.18           -496.5547308   -0.1224104 -496.6771412       0.00       0.00       0.00     <------
        CONF3   -34.1742792          0.25           -496.5555155   -0.1209997 -496.6765152      -0.49       0.89       0.39
        CONF4   -34.1739821          0.44           -496.5538467   -0.1215681 -496.6754148       0.55       0.53       1.08
        CONF5   -34.1740224          0.41           -496.5534106   -0.1225105 -496.6759212       0.83      -0.06       0.77
        CONF6   -34.1736309          0.66           -496.5536883   -0.1217568 -496.6754451       0.65       0.41       1.06
        CONF7   -34.1725555          1.33           -496.5475921   -0.1236591 -496.6712512       4.48      -0.78       3.70
        CONF8   -34.1721876          1.57           -496.5511892   -0.1220037 -496.6731929       2.22       0.26       2.48
        CONF9   -34.1719439          1.72           -496.5497042   -0.1216755 -496.6713797       3.15       0.46       3.62
        CONF10  -34.1714765          2.01           -496.5515727   -0.1200154 -496.6715881       1.98       1.50       3.48
        CONF11  -34.1712638          2.14           -496.5499197   -0.1229693 -496.6728890       3.02      -0.35       2.67
        CONF12  -34.1704841          2.63           -496.5521277   -0.1175186 -496.6696463       1.63       3.07       4.70
        CONF13  -34.1703687          2.71           -496.5507772   -0.1185429 -496.6693201       2.48       2.43       4.91
        CONF14  -34.1694191          3.30           -496.5502879   -0.1209007 -496.6711886       2.79       0.95       3.74
        CONF15  -34.1691431          3.48           -496.5498737   -0.1231177 -496.6729915       3.05      -0.44       2.60
        CONF16  -34.1664418          5.17           -496.5536002   -0.1127405 -496.6663407       0.71       6.07       6.78
        CONF17  -34.1661652          5.34           -496.5528953   -0.1137805 -496.6666758       1.15       5.42       6.57
        CONF18  -34.1660003          5.45           -496.5042317   -0.1646828 -496.6689146      31.69     -26.53       5.16
        CONF19  -34.1664801          5.15           -496.5238437   -0.1439085 -496.6677521      19.38     -13.49       5.89
        CONF20  -34.1658387          5.55           -496.5048148   -0.1651945 -496.6700093      31.32     -26.85       4.48
        CONF21  -34.1670164          4.81           -496.5386075   -0.1321235 -496.6707310      10.12      -6.10       4.02
        CONF22  -34.1656180          5.69           -496.5093865   -0.1582655 -496.6676520      28.45     -22.50       5.95
        ----------------------------------------------------------------------------------------------------

        --------------------------------------------------
                  Conformers considered further           
        --------------------------------------------------

        These conformers are below the 4.000 kcal/mol g_thr(0) threshold.

         CONF1,  CONF2,  CONF3,  CONF4,  CONF5,  CONF6,  CONF7,  CONF8,  CONF9, CONF10, CONF11
        CONF14, CONF15


        Calculating Boltzmann averaged (free) energy of ensemble on input geometries (not DFT optimized)!

        temperature /K:   avE(T) /a.u.   avG(T) /a.u. 
        ----------------------------------------------------------------------------------------------------
            298.15        -496.5544580    -496.6764445     <<==part0==
        ----------------------------------------------------------------------------------------------------


        >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>END of Part0<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<
        Ran part0 in 25.3156 seconds

        ----------------------------------------------------------------------------------------------------
                                              CRE PRESCREENING - PART1                                      
        ----------------------------------------------------------------------------------------------------

        program:                                                       tm
        functional for part1 and 2:                                    r2scan-3c
        basis set for part1 and 2:                                     def2-mTZVPP
        Solvent:                                                       h2o
        solvent model for Gsolv contribution:                          cosmors
        threshold g_thr(1) and G_thr(1):                               3.5
        starting number of considered conformers:                      13
        calculate mRRHO contribution:                                  on
        program for mRRHO contribution:                                xtb
        GFN version for mRRHO and/or GBSA_Gsolv:                       gfn2
        Apply constraint to input geometry during mRRHO calculation:   on
        temperature:                                                   298.15

        Calculating single-point energies and solvation contribution (G_solv):
        The prescreening COSMO-RS is calculated for:
         CONF1,  CONF2,  CONF3,  CONF4,  CONF5,  CONF6,  CONF7,  CONF8,  CONF9, CONF10, CONF11
        CONF14, CONF15

        Constructed folders!
        Constructed folders!

        Starting 13 COSMO-RS-Gsolv calculations.
        Running COSMO-RS calculation in CONF1/r2scan-3c/COSMO
        Running COSMO-RS calculation in CONF2/r2scan-3c/COSMO
        Running COSMO-RS calculation in CONF3/r2scan-3c/COSMO
        Running COSMO-RS calculation in CONF4/r2scan-3c/COSMO
        Running COSMO-RS calculation in CONF5/r2scan-3c/COSMO
        Running COSMO-RS calculation in CONF6/r2scan-3c/COSMO
        Running COSMO-RS calculation in CONF7/r2scan-3c/COSMO
        Running COSMO-RS calculation in CONF8/r2scan-3c/COSMO
        Running COSMO-RS calculation in CONF9/r2scan-3c/COSMO
        Running COSMO-RS calculation in CONF10/r2scan-3c/COSMO
        Running COSMO-RS calculation in CONF11/r2scan-3c/COSMO
        Running COSMO-RS calculation in CONF14/r2scan-3c/COSMO
        Running COSMO-RS calculation in CONF15/r2scan-3c/COSMO
        Tasks completed!

        prescreening COSMO-RS calculation was successful for CONF1/r2scan-3c/COSMO: -0.10999518
        prescreening COSMO-RS calculation was successful for CONF2/r2scan-3c/COSMO: -0.10920229
        prescreening COSMO-RS calculation was successful for CONF3/r2scan-3c/COSMO: -0.10753681
        prescreening COSMO-RS calculation was successful for CONF4/r2scan-3c/COSMO: -0.10829026
        prescreening COSMO-RS calculation was successful for CONF5/r2scan-3c/COSMO: -0.10936797
        prescreening COSMO-RS calculation was successful for CONF6/r2scan-3c/COSMO: -0.10838241
        prescreening COSMO-RS calculation was successful for CONF7/r2scan-3c/COSMO: -0.11058677
        prescreening COSMO-RS calculation was successful for CONF8/r2scan-3c/COSMO: -0.10882956
        prescreening COSMO-RS calculation was successful for CONF9/r2scan-3c/COSMO: -0.10889910
        prescreening COSMO-RS calculation was successful for CONF10/r2scan-3c/COSMO: -0.10665800
        prescreening COSMO-RS calculation was successful for CONF11/r2scan-3c/COSMO: -0.10960927
        prescreening COSMO-RS calculation was successful for CONF14/r2scan-3c/COSMO: -0.10711489
        prescreening COSMO-RS calculation was successful for CONF15/r2scan-3c/COSMO: -0.10994652

        --------------------------------------------------
                  Removing high lying conformers          
        --------------------------------------------------

         CONF#  E(GFNn-xTB) ΔE(GFNn-xTB)       E [Eh]      Gsolv [Eh]         Gtot      ΔGtot
                     [a.u.]   [kcal/mol]    r2scan-3c COSMO-RS-normal         [Eh] [kcal/mol]
                                                          [r2scan-3c]                        
        CONF1   -34.1599548         0.00 -497.3021467      -0.1099952 -497.4121419       1.82
        CONF2   -34.1594484         0.32 -497.3049956      -0.1092023 -497.4141979       0.53
        CONF3   -34.1592530         0.44 -497.3064930      -0.1075368 -497.4140298       0.64
        CONF4   -34.1590954         0.54 -497.3049058      -0.1082903 -497.4131961       1.16
        CONF5   -34.1589250         0.65 -497.3056747      -0.1093680 -497.4150427       0.00     <------
        CONF6   -34.1587465         0.76 -497.3049670      -0.1083824 -497.4133494       1.06
        CONF7   -34.1578065         1.35 -497.2983644      -0.1105868 -497.4089512       3.82
        CONF8   -34.1571241         1.78 -497.3029570      -0.1088296 -497.4117866       2.04
        CONF9   -34.1570372         1.83 -497.3008248      -0.1088991 -497.4097239       3.34
        CONF10  -34.1563217         2.28 -497.3038986      -0.1066580 -497.4105566       2.82
        CONF11  -34.1559251         2.53 -497.3008250      -0.1096093 -497.4104343       2.89
        CONF14  -34.1537285         3.91 -497.3022176      -0.1071149 -497.4093325       3.58
        CONF15  -34.1529637         4.39 -497.3009381      -0.1099465 -497.4108846       2.61

        --------------------------------------------------
                  Conformers considered further           
        --------------------------------------------------

        Below the g_thr(1) threshold of 3.5 kcal/mol.

         CONF1,  CONF2,  CONF3,  CONF4,  CONF5,  CONF6,  CONF8,  CONF9, CONF10, CONF11, CONF15
        --------------------------------------------------

        Calculating prescreening G_mRRHO with implicit solvation!
        The prescreening G_mRRHO calculation is now performed for:
         CONF1,  CONF2,  CONF3,  CONF4,  CONF5,  CONF6,  CONF8,  CONF9, CONF10, CONF11, CONF15
        Constructed folders!

        Starting 11 G_RRHO calculations.
        Running GFN2-xTB mRRHO in CONF1/rrho_part1
        Running GFN2-xTB mRRHO in CONF2/rrho_part1
        Running GFN2-xTB mRRHO in CONF3/rrho_part1
        Running GFN2-xTB mRRHO in CONF4/rrho_part1
        Running GFN2-xTB mRRHO in CONF5/rrho_part1
        Running GFN2-xTB mRRHO in CONF6/rrho_part1
        Running GFN2-xTB mRRHO in CONF8/rrho_part1
        Running GFN2-xTB mRRHO in CONF9/rrho_part1
        Running GFN2-xTB mRRHO in CONF10/rrho_part1
        Running GFN2-xTB mRRHO in CONF11/rrho_part1
        Running GFN2-xTB mRRHO in CONF15/rrho_part1
        Tasks completed!

        The prescreening G_mRRHO calculation @ c1 was successful for  CONF1/rrho_part1: 0.18287159
        The prescreening G_mRRHO calculation @ c1 was successful for  CONF2/rrho_part1: 0.18311981
        The prescreening G_mRRHO calculation @ c1 was successful for  CONF3/rrho_part1: 0.18287068
        The prescreening G_mRRHO calculation @ c1 was successful for  CONF4/rrho_part1: 0.18270975
        The prescreening G_mRRHO calculation @ c1 was successful for  CONF5/rrho_part1: 0.18272101
        The prescreening G_mRRHO calculation @ c1 was successful for  CONF6/rrho_part1: 0.18277331
        The prescreening G_mRRHO calculation @ c1 was successful for  CONF8/rrho_part1: 0.18239977
        The prescreening G_mRRHO calculation @ c1 was successful for  CONF9/rrho_part1: 0.18270475
        The prescreening G_mRRHO calculation @ c1 was successful for CONF10/rrho_part1: 0.18264350
        The prescreening G_mRRHO calculation @ c1 was successful for CONF11/rrho_part1: 0.18284194
        The prescreening G_mRRHO calculation @ c1 was successful for CONF15/rrho_part1: 0.18323466

        --------------------------------------------------
                 * Gibbs free energies of part1 *         
        --------------------------------------------------

         CONF#  G(GFNn-xTB) ΔG(GFNn-xTB)       E [Eh]      Gsolv [Eh]  GmRRHO [Eh]         Gtot      ΔGtot
                     [a.u.]   [kcal/mol]    r2scan-3c COSMO-RS-normal         GFN2         [Eh] [kcal/mol]
                                                          [r2scan-3c] [alpb]-bhess                        
        CONF1   -33.9770832         0.00 -497.3021467      -0.1099952    0.1828716 -497.2292703       1.91
        CONF2   -33.9763286         0.47 -497.3049956      -0.1092023    0.1831198 -497.2310781       0.78
        CONF3   -33.9763824         0.44 -497.3064930      -0.1075368    0.1828707 -497.2311591       0.73
        CONF4   -33.9763856         0.44 -497.3049058      -0.1082903    0.1827098 -497.2304863       1.15
        CONF5   -33.9762040         0.55 -497.3056747      -0.1093680    0.1827210 -497.2323217       0.00     <------
        CONF6   -33.9759732         0.70 -497.3049670      -0.1083824    0.1827733 -497.2305761       1.10
        CONF8   -33.9747243         1.48 -497.3029570      -0.1088296    0.1823998 -497.2293868       1.84
        CONF9   -33.9743324         1.73 -497.3008248      -0.1088991    0.1827047 -497.2270192       3.33
        CONF10  -33.9736782         2.14 -497.3038986      -0.1066580    0.1826435 -497.2279131       2.77
        CONF11  -33.9730831         2.51 -497.3008250      -0.1096093    0.1828419 -497.2275923       2.97
        CONF15  -33.9697290         4.61 -497.3009381      -0.1099465    0.1832347 -497.2276500       2.93

        Additional global 'fuzzy-threshold' based on the standard deviation of (G_mRRHO):
        Std_dev(G_mRRHO) = 0.142 kcal/mol
        Fuzzythreshold   = 0.096 kcal/mol
        Final sorting threshold G_thr(1) = 3.500 + 0.096 = 3.596 kcal/mol
        Spearman correlation coefficient between (E + Solv) and (E + Solv + mRRHO) = 0.973

        All relative (free) energies are below the initial G_thr(1) threshold of 3.5 kcal/mol.
        All conformers are considered further.

        Calculating Boltzmann averaged free energy of ensemble on input geometries (not DFT optimized)!

        temperature /K:   avE(T) /a.u. avGmRRHO(T) /a.u. avGsolv(T) /a.u.   avG(T) /a.u.
        ----------------------------------------------------------------------------------------------------
            298.15        -497.3054081        0.1827983       -0.1089068   -497.2315166      <<==part1==
        ----------------------------------------------------------------------------------------------------


        Calculating unbiased GFNn-xTB energy
        Constructed folders!

        Starting 11 xTB - single-point calculations.
        gfn2-xTB energy for CONF1/GFN_unbiased = -34.1764957
        gfn2-xTB energy for CONF2/GFN_unbiased = -34.1762055
        gfn2-xTB energy for CONF3/GFN_unbiased = -34.1760929
        gfn2-xTB energy for CONF4/GFN_unbiased = -34.1757959
        gfn2-xTB energy for CONF5/GFN_unbiased = -34.1758362
        gfn2-xTB energy for CONF6/GFN_unbiased = -34.1754446
        gfn2-xTB energy for CONF8/GFN_unbiased = -34.1740014
        gfn2-xTB energy for CONF9/GFN_unbiased = -34.1737577
        gfn2-xTB energy for CONF10/GFN_unbiased = -34.1732903
        gfn2-xTB energy for CONF11/GFN_unbiased = -34.1730775
        gfn2-xTB energy for CONF15/GFN_unbiased = -34.1709569
        Tasks completed!


        >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>END of Part1<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<
        Ran part1 in 166.4199 seconds

        ----------------------------------------------------------------------------------------------------
                                              CRE OPTIMIZATION - PART2                                      
        ----------------------------------------------------------------------------------------------------

        program:                                                       tm
        functional for part2:                                          r2scan-3c
        basis set for part2:                                           def2-mTZVPP
        using the xTB-optimizer for optimization:                      on
        using the new ensemble optimizer:                              on
        optimize all conformers below this G_thr(opt,2) threshold:     2.5
        Spearman threshold:                                            0.935
        number of optimization iterations:                             8
        radsize:                                                       10
        optimization level in part2:                                   lax
        solvent:                                                       h2o
        solvent model applied in the optimization:                     dcosmors
        solvent model for Gsolv contribution:                          cosmors
        temperature:                                                   298.15
        evalulate at different temperatures:                           on
        temperature range:                                             273.15, 278.15, 283.15, 288.15, ...
        Boltzmann sum threshold G_thr(2) for sorting in part2:         99.0
        calculate mRRHO contribution:                                  on
        program for mRRHO contribution:                                xtb
        GFN version for mRRHO and/or GBSA_Gsolv:                       gfn2
        Apply constraint to input geometry during mRRHO calculation:   on

        Optimizing geometries at DFT level with implicit solvation!
        The optimization is calculated for:
         CONF1,  CONF2,  CONF3,  CONF4,  CONF5,  CONF6,  CONF8,  CONF9, CONF10, CONF11, CONF15
        Constructed folders!

        Preparing 11 calculations.
        Tasks completed!

        ************************Starting optimizations************************

        Starting threshold is set to 2.5 + 60.0 % = 4.0 kcal/mol

        Lower limit is set to G_thr(opt,2) = 2.5 kcal/mol

        *******************************CYCLE 1********************************

        Starting 11 optimizations.
        Running optimization in CONF1/r2scan-3c   
        Running optimization in CONF2/r2scan-3c   
        Running optimization in CONF3/r2scan-3c   
        Running optimization in CONF4/r2scan-3c   
        Running optimization in CONF5/r2scan-3c   
        Running optimization in CONF6/r2scan-3c   
        Running optimization in CONF8/r2scan-3c   
        Running optimization in CONF9/r2scan-3c   
        Running optimization in CONF10/r2scan-3c  
        Running optimization in CONF11/r2scan-3c  
        Running optimization in CONF15/r2scan-3c  
        Tasks completed!

        Constructed folders!

        Starting 11 G_RRHO calculations.
        Running GFN2-xTB mRRHO in r2scan-3c/rrho_crude
        Running GFN2-xTB mRRHO in r2scan-3c/rrho_crude
        Running GFN2-xTB mRRHO in r2scan-3c/rrho_crude
        Running GFN2-xTB mRRHO in r2scan-3c/rrho_crude
        Running GFN2-xTB mRRHO in r2scan-3c/rrho_crude
        Running GFN2-xTB mRRHO in r2scan-3c/rrho_crude
        Running GFN2-xTB mRRHO in r2scan-3c/rrho_crude
        Running GFN2-xTB mRRHO in r2scan-3c/rrho_crude
        Running GFN2-xTB mRRHO in r2scan-3c/rrho_crude
        Running GFN2-xTB mRRHO in r2scan-3c/rrho_crude
        Running GFN2-xTB mRRHO in r2scan-3c/rrho_crude
        Tasks completed!

        The G_mRRHO calculation on crudely optimized DFT geometry @ c1 was successful for CONF1/r2scan-3c/rrho_crude: 0.18253283
        The G_mRRHO calculation on crudely optimized DFT geometry @ c1 was successful for CONF2/r2scan-3c/rrho_crude: 0.18217211
        The G_mRRHO calculation on crudely optimized DFT geometry @ c1 was successful for CONF3/r2scan-3c/rrho_crude: 0.18223212
        The G_mRRHO calculation on crudely optimized DFT geometry @ c1 was successful for CONF4/r2scan-3c/rrho_crude: 0.18258415
        The G_mRRHO calculation on crudely optimized DFT geometry @ c1 was successful for CONF5/r2scan-3c/rrho_crude: 0.18245612
        The G_mRRHO calculation on crudely optimized DFT geometry @ c1 was successful for CONF6/r2scan-3c/rrho_crude: 0.18170042
        The G_mRRHO calculation on crudely optimized DFT geometry @ c1 was successful for CONF8/r2scan-3c/rrho_crude: 0.18260313
        The G_mRRHO calculation on crudely optimized DFT geometry @ c1 was successful for CONF9/r2scan-3c/rrho_crude: 0.18272834
        The G_mRRHO calculation on crudely optimized DFT geometry @ c1 was successful for CONF10/r2scan-3c/rrho_crude: 0.18229001
        The G_mRRHO calculation on crudely optimized DFT geometry @ c1 was successful for CONF11/r2scan-3c/rrho_crude: 0.18150537
        The G_mRRHO calculation on crudely optimized DFT geometry @ c1 was successful for CONF15/r2scan-3c/rrho_crude: 0.18100199
        Max number of performed iterations: 8
        Spearman rank evaluation is performed in the next cycle.

        CONF1  initial ΔG =  2.13  kcal/mol and current ΔG =  2.59  kcal/mol. (not_converged)
        CONF2  initial ΔG =  0.86  kcal/mol and current ΔG =  0.46  kcal/mol. (not_converged)
        CONF3  initial ΔG =  0.97  kcal/mol and current ΔG =  1.47  kcal/mol. (not_converged)
        CONF4  initial ΔG =  1.62  kcal/mol and current ΔG =  1.40  kcal/mol. (not_converged)
        CONF5  initial ΔG =  0.00  kcal/mol and current ΔG =  0.42  kcal/mol. (not_converged)
        CONF6  initial ΔG =  0.60  kcal/mol and current ΔG =  1.15  kcal/mol. (not_converged)
        CONF8  initial ΔG =  2.26  kcal/mol and current ΔG =  2.31  kcal/mol. (not_converged)
        CONF9  initial ΔG =  3.90  kcal/mol and current ΔG =  4.42  kcal/mol. (not_converged)
        CONF10 initial ΔG =  2.86  kcal/mol and current ΔG =  2.92  kcal/mol. (not_converged)
        CONF11 initial ΔG =  2.26  kcal/mol and current ΔG =  3.43  kcal/mol. (not_converged)
        CONF15 initial ΔG =  0.23  kcal/mol and current ΔG =  0.00  kcal/mol. (not_converged)

        CONF9 is above 4.0 kcal/mol and gradient norm (0.004056375026) is below 0.01.
        CONF9 is above threshold, dont optimize further and remove conformer.

        CYCLE 1 performed in 1310.1062 seconds
        *******************************CYCLE 2********************************

        Starting 10 optimizations.
        Running optimization in CONF1/r2scan-3c   
        Running optimization in CONF2/r2scan-3c   
        Running optimization in CONF3/r2scan-3c   
        Running optimization in CONF4/r2scan-3c   
        Running optimization in CONF5/r2scan-3c   
        Running optimization in CONF6/r2scan-3c   
        Running optimization in CONF8/r2scan-3c   
        Running optimization in CONF10/r2scan-3c  
        Running optimization in CONF11/r2scan-3c  
        Running optimization in CONF15/r2scan-3c  
        Tasks completed!

        Geometry optimization converged for: CONF3 within  12 cycles
        Max number of performed iterations: 16

        CONF1  previous ΔG =  2.59  kcal/mol and current ΔG =  2.54  kcal/mol. (not_converged)
        CONF2  previous ΔG =  0.46  kcal/mol and current ΔG =  0.00  kcal/mol. (not_converged)
        CONF3  previous ΔG =  1.47  kcal/mol and current ΔG =  1.92  kcal/mol. (converged)
        CONF4  previous ΔG =  1.40  kcal/mol and current ΔG =  10.87 kcal/mol. (not_converged)
        CONF5  previous ΔG =  0.42  kcal/mol and current ΔG =  0.99  kcal/mol. (not_converged)
        CONF6  previous ΔG =  1.15  kcal/mol and current ΔG =  1.57  kcal/mol. (not_converged)
        CONF8  previous ΔG =  2.31  kcal/mol and current ΔG =  1.85  kcal/mol. (not_converged)
        CONF10 previous ΔG =  2.92  kcal/mol and current ΔG =  3.68  kcal/mol. (not_converged)
        CONF11 previous ΔG =  3.43  kcal/mol and current ΔG =  3.29  kcal/mol. (not_converged)
        CONF15 previous ΔG =  0.00  kcal/mol and current ΔG =  0.03  kcal/mol. (not_converged)

                   Spearman coeff. from    8 -->   11 =  0.9758
                   Spearman coeff. from    9 -->   12 =  0.9879
                   Spearman coeff. from   10 -->   13 =  0.7455
                   Spearman coeff. from   11 -->   14 =  1.0000
        Evaluating Spearman coeff. from   12 -->   15 =  0.9515
        Evaluating Spearman coeff. from   13 -->   16 =  0.8667
        Final averaged Spearman correlation coefficient: 0.9091
        CONF4 is above 4.0 kcal/mol but gradient norm (0.108293192291) is above 0.01 --> not sorted out!

        CYCLE 2 performed in 980.4814 seconds
        *******************************CYCLE 3********************************

        Starting 9 optimizations.
        Running optimization in CONF1/r2scan-3c   
        Running optimization in CONF2/r2scan-3c   
        Running optimization in CONF4/r2scan-3c   
        Running optimization in CONF5/r2scan-3c   
        Running optimization in CONF6/r2scan-3c   
        Running optimization in CONF8/r2scan-3c   
        Running optimization in CONF10/r2scan-3c  
        Running optimization in CONF11/r2scan-3c  
        Running optimization in CONF15/r2scan-3c  
        Tasks completed!

        Geometry optimization converged for: CONF5 within  23 cycles
        Geometry optimization converged for: CONF11 within  21 cycles
        Max number of performed iterations: 24

        CONF1  previous ΔG =  2.54  kcal/mol and current ΔG =  2.38  kcal/mol. (not_converged)
        CONF2  previous ΔG =  0.00  kcal/mol and current ΔG =  0.00  kcal/mol. (not_converged)
        CONF3  previous ΔG =  1.92  kcal/mol and current ΔG =  2.09  kcal/mol. (converged)
        CONF4  previous ΔG =  10.87 kcal/mol and current ΔG =  0.07  kcal/mol. (not_converged)
        CONF5  previous ΔG =  0.99  kcal/mol and current ΔG =  1.25  kcal/mol. (converged)
        CONF6  previous ΔG =  1.57  kcal/mol and current ΔG =  1.51  kcal/mol. (not_converged)
        CONF8  previous ΔG =  1.85  kcal/mol and current ΔG =  1.89  kcal/mol. (not_converged)
        CONF10 previous ΔG =  3.68  kcal/mol and current ΔG =  3.36  kcal/mol. (not_converged)
        CONF11 previous ΔG =  3.29  kcal/mol and current ΔG =  3.30  kcal/mol. (converged)
        CONF15 previous ΔG =  0.03  kcal/mol and current ΔG =  0.22  kcal/mol. (not_converged)

                   Spearman coeff. from   16 -->   19 =  0.6606
                   Spearman coeff. from   17 -->   20 =  0.6000
                   Spearman coeff. from   18 -->   21 =  0.7455
                   Spearman coeff. from   19 -->   22 =  0.8182
        Evaluating Spearman coeff. from   20 -->   23 =  0.7091
        Evaluating Spearman coeff. from   21 -->   24 =  1.0000
        Final averaged Spearman correlation coefficient: 0.8545

        CYCLE 3 performed in 830.9565 seconds
        *******************************CYCLE 4********************************

        Starting 7 optimizations.
        Running optimization in CONF1/r2scan-3c   
        Running optimization in CONF2/r2scan-3c   
        Running optimization in CONF4/r2scan-3c   
        Running optimization in CONF6/r2scan-3c   
        Running optimization in CONF8/r2scan-3c   
        Running optimization in CONF10/r2scan-3c  
        Running optimization in CONF15/r2scan-3c  
        Tasks completed!

        Geometry optimization converged for: CONF8 within  26 cycles
        Geometry optimization converged for: CONF15 within  25 cycles
        Max number of performed iterations: 32

        CONF1  previous ΔG =  2.38  kcal/mol and current ΔG =  1.84  kcal/mol. (not_converged)
        CONF2  previous ΔG =  0.00  kcal/mol and current ΔG =  0.00  kcal/mol. (not_converged)
        CONF3  previous ΔG =  2.09  kcal/mol and current ΔG =  2.11  kcal/mol. (converged)
        CONF4  previous ΔG =  0.07  kcal/mol and current ΔG =  0.01  kcal/mol. (not_converged)
        CONF5  previous ΔG =  1.25  kcal/mol and current ΔG =  1.26  kcal/mol. (converged)
        CONF6  previous ΔG =  1.51  kcal/mol and current ΔG =  1.57  kcal/mol. (not_converged)
        CONF8  previous ΔG =  1.89  kcal/mol and current ΔG =  1.91  kcal/mol. (converged)
        CONF10 previous ΔG =  3.36  kcal/mol and current ΔG =  6.58  kcal/mol. (not_converged)
        CONF11 previous ΔG =  3.30  kcal/mol and current ΔG =  3.32  kcal/mol. (converged)
        CONF15 previous ΔG =  0.22  kcal/mol and current ΔG =  0.23  kcal/mol. (converged)

                   Spearman coeff. from   24 -->   27 =  0.9636
                   Spearman coeff. from   25 -->   28 =  0.9879
                   Spearman coeff. from   26 -->   29 =  0.9758
                   Spearman coeff. from   27 -->   30 =  0.8424
        Evaluating Spearman coeff. from   28 -->   31 =  0.9515
        Evaluating Spearman coeff. from   29 -->   32 =  0.9636
        Final averaged Spearman correlation coefficient: 0.9576

        PES is assumed to be parallel
        Updated optimization threshold to: 3.50 kcal/mol
        CONF10 is above 3.5 kcal/mol but gradient norm (0.09473608827) is above 0.01 --> not sorted out!

        CYCLE 4 performed in 613.0740 seconds
        *******************************CYCLE 5********************************

        Starting 5 optimizations.
        Running optimization in CONF1/r2scan-3c   
        Running optimization in CONF2/r2scan-3c   
        Running optimization in CONF4/r2scan-3c   
        Running optimization in CONF6/r2scan-3c   
        Running optimization in CONF10/r2scan-3c  
        Tasks completed!

        Geometry optimization converged for: CONF2 within  34 cycles
        Geometry optimization converged for: CONF6 within  39 cycles
        Max number of performed iterations: 40

        CONF1  previous ΔG =  1.84  kcal/mol and current ΔG =  7.79  kcal/mol. (not_converged)
        CONF2  previous ΔG =  0.00  kcal/mol and current ΔG =  0.38  kcal/mol. (converged)
        CONF3  previous ΔG =  2.11  kcal/mol and current ΔG =  2.53  kcal/mol. (converged)
        CONF4  previous ΔG =  0.01  kcal/mol and current ΔG =  0.35  kcal/mol. (not_converged)
        CONF5  previous ΔG =  1.26  kcal/mol and current ΔG =  1.68  kcal/mol. (converged)
        CONF6  previous ΔG =  1.57  kcal/mol and current ΔG =  1.94  kcal/mol. (converged)
        CONF8  previous ΔG =  1.91  kcal/mol and current ΔG =  2.33  kcal/mol. (converged)
        CONF10 previous ΔG =  6.58  kcal/mol and current ΔG =  0.00  kcal/mol. (not_converged)
        CONF11 previous ΔG =  3.32  kcal/mol and current ΔG =  3.74  kcal/mol. (converged)
        CONF15 previous ΔG =  0.23  kcal/mol and current ΔG =  0.65  kcal/mol. (converged)

                   Spearman coeff. from   32 -->   35 =  0.7333
                   Spearman coeff. from   33 -->   36 =  0.9273
                   Spearman coeff. from   34 -->   37 =  0.8182
                   Spearman coeff. from   35 -->   38 =  1.0000
        Evaluating Spearman coeff. from   36 -->   39 =  0.9879
        Evaluating Spearman coeff. from   37 -->   40 =  0.8545
        Final averaged Spearman correlation coefficient: 0.9212
        CONF1 is above 3.5 kcal/mol but gradient norm (0.083474094636) is above 0.01 --> not sorted out!

        CYCLE 5 performed in 403.3400 seconds
        *******************************CYCLE 6********************************

        Starting 3 optimizations.
        Running optimization in CONF1/r2scan-3c   
        Running optimization in CONF4/r2scan-3c   
        Running optimization in CONF10/r2scan-3c  
        Tasks completed!

        Geometry optimization converged for: CONF4 within  44 cycles
        Max number of performed iterations: 48

        CONF1  previous ΔG =  7.79  kcal/mol and current ΔG =  2.37  kcal/mol. (not_converged)
        CONF2  previous ΔG =  0.38  kcal/mol and current ΔG =  1.25  kcal/mol. (converged)
        CONF3  previous ΔG =  2.53  kcal/mol and current ΔG =  3.40  kcal/mol. (converged)
        CONF4  previous ΔG =  0.35  kcal/mol and current ΔG =  1.19  kcal/mol. (converged)
        CONF5  previous ΔG =  1.68  kcal/mol and current ΔG =  2.55  kcal/mol. (converged)
        CONF6  previous ΔG =  1.94  kcal/mol and current ΔG =  2.80  kcal/mol. (converged)
        CONF8  previous ΔG =  2.33  kcal/mol and current ΔG =  3.20  kcal/mol. (converged)
        CONF10 previous ΔG =  0.00  kcal/mol and current ΔG =  0.00  kcal/mol. (not_converged)
        CONF11 previous ΔG =  3.74  kcal/mol and current ΔG =  4.61  kcal/mol. (converged)
        CONF15 previous ΔG =  0.65  kcal/mol and current ΔG =  1.52  kcal/mol. (converged)

                   Spearman coeff. from   40 -->   43 =  0.8788
                   Spearman coeff. from   41 -->   44 =  0.6242
                   Spearman coeff. from   42 -->   45 =  0.9273
                   Spearman coeff. from   43 -->   46 =  1.0000
        Evaluating Spearman coeff. from   44 -->   47 =  0.6485
        Evaluating Spearman coeff. from   45 -->   48 =  0.9879
        Final averaged Spearman correlation coefficient: 0.8182

        CYCLE 6 performed in 296.2324 seconds
        *******************************CYCLE 7********************************

        Starting 2 optimizations.
        Running optimization in CONF1/r2scan-3c   
        Running optimization in CONF10/r2scan-3c  
        Tasks completed!

        Max number of performed iterations: 56

        CONF1  previous ΔG =  2.37  kcal/mol and current ΔG =  2.60  kcal/mol. (not_converged)
        CONF2  previous ΔG =  1.25  kcal/mol and current ΔG =  1.66  kcal/mol. (converged)
        CONF3  previous ΔG =  3.40  kcal/mol and current ΔG =  3.81  kcal/mol. (converged)
        CONF4  previous ΔG =  1.19  kcal/mol and current ΔG =  1.61  kcal/mol. (converged)
        CONF5  previous ΔG =  2.55  kcal/mol and current ΔG =  2.97  kcal/mol. (converged)
        CONF6  previous ΔG =  2.80  kcal/mol and current ΔG =  3.22  kcal/mol. (converged)
        CONF8  previous ΔG =  3.20  kcal/mol and current ΔG =  3.61  kcal/mol. (converged)
        CONF10 previous ΔG =  0.00  kcal/mol and current ΔG =  0.00  kcal/mol. (not_converged)
        CONF11 previous ΔG =  4.61  kcal/mol and current ΔG =  5.02  kcal/mol. (converged)
        CONF15 previous ΔG =  1.52  kcal/mol and current ΔG =  1.94  kcal/mol. (converged)

                   Spearman coeff. from   48 -->   51 =  1.0000
                   Spearman coeff. from   49 -->   52 =  1.0000
                   Spearman coeff. from   50 -->   53 =  1.0000
                   Spearman coeff. from   51 -->   54 =  1.0000
        Evaluating Spearman coeff. from   52 -->   55 =  1.0000
        Evaluating Spearman coeff. from   53 -->   56 =  1.0000
        Final averaged Spearman correlation coefficient: 1.0000

        PES is assumed to be parallel
        Updated optimization threshold to: 3.00 kcal/mol

        CYCLE 7 performed in 252.6446 seconds
        *******************************CYCLE 8********************************

        Starting 2 optimizations.
        Running optimization in CONF1/r2scan-3c   
        Running optimization in CONF10/r2scan-3c  
        Tasks completed!

        Max number of performed iterations: 64

        CONF1  previous ΔG =  2.60  kcal/mol and current ΔG =  2.71  kcal/mol. (not_converged)
        CONF2  previous ΔG =  1.66  kcal/mol and current ΔG =  1.87  kcal/mol. (converged)
        CONF3  previous ΔG =  3.81  kcal/mol and current ΔG =  4.02  kcal/mol. (converged)
        CONF4  previous ΔG =  1.61  kcal/mol and current ΔG =  1.81  kcal/mol. (converged)
        CONF5  previous ΔG =  2.97  kcal/mol and current ΔG =  3.17  kcal/mol. (converged)
        CONF6  previous ΔG =  3.22  kcal/mol and current ΔG =  3.42  kcal/mol. (converged)
        CONF8  previous ΔG =  3.61  kcal/mol and current ΔG =  3.82  kcal/mol. (converged)
        CONF10 previous ΔG =  0.00  kcal/mol and current ΔG =  0.00  kcal/mol. (not_converged)
        CONF11 previous ΔG =  5.02  kcal/mol and current ΔG =  5.23  kcal/mol. (converged)
        CONF15 previous ΔG =  1.94  kcal/mol and current ΔG =  2.14  kcal/mol. (converged)

                   Spearman coeff. from   56 -->   59 =  1.0000
                   Spearman coeff. from   57 -->   60 =  1.0000
                   Spearman coeff. from   58 -->   61 =  1.0000
                   Spearman coeff. from   59 -->   62 =  1.0000
        Evaluating Spearman coeff. from   60 -->   63 =  1.0000
        Evaluating Spearman coeff. from   61 -->   64 =  1.0000
        Final averaged Spearman correlation coefficient: 1.0000

        PES is assumed to be parallel
        Updated optimization threshold to: 2.50 kcal/mol
        CONF1 is above 2.5 kcal/mol and gradient norm (0.001262363132) is below 0.01.
        CONF1 is removed because of the lowered threshold!
        CONF1 is above threshold, dont optimize further and remove conformer.

        CYCLE 8 performed in 251.6678 seconds
        *******************************CYCLE 9********************************

        Starting 1 optimizations.
        Running optimization in CONF10/r2scan-3c  
        Tasks completed!

        Max number of performed iterations: 72

        CONF2  previous ΔG =  1.87  kcal/mol and current ΔG =  1.84  kcal/mol. (converged)
        CONF3  previous ΔG =  4.02  kcal/mol and current ΔG =  3.99  kcal/mol. (converged)
        CONF4  previous ΔG =  1.81  kcal/mol and current ΔG =  1.78  kcal/mol. (converged)
        CONF5  previous ΔG =  3.17  kcal/mol and current ΔG =  3.14  kcal/mol. (converged)
        CONF6  previous ΔG =  3.42  kcal/mol and current ΔG =  3.39  kcal/mol. (converged)
        CONF8  previous ΔG =  3.82  kcal/mol and current ΔG =  3.79  kcal/mol. (converged)
        CONF10 previous ΔG =  0.00  kcal/mol and current ΔG =  0.00  kcal/mol. (not_converged)
        CONF11 previous ΔG =  5.23  kcal/mol and current ΔG =  5.20  kcal/mol. (converged)
        CONF15 previous ΔG =  2.14  kcal/mol and current ΔG =  2.11  kcal/mol. (converged)

                   Spearman coeff. from   64 -->   67 =  1.0000
                   Spearman coeff. from   65 -->   68 =  1.0000
                   Spearman coeff. from   66 -->   69 =  1.0000
                   Spearman coeff. from   67 -->   70 =  1.0000
        Evaluating Spearman coeff. from   68 -->   71 =  1.0000
        Evaluating Spearman coeff. from   69 -->   72 =  1.0000
        Final averaged Spearman correlation coefficient: 1.0000

        PES is assumed to be parallel
        Current optimization threshold: 2.50 kcal/mol

        CYCLE 9 performed in 202.2149 seconds
        *******************************CYCLE 10*******************************

        Starting 1 optimizations.
        Running optimization in CONF10/r2scan-3c  
        Tasks completed!

        Geometry optimization converged for: CONF10 within  80 cycles
        ***********************Finished optimizations!************************
        Timings:
        Cycle:  [s]
           1  1310.11
           2  980.48
           3  830.96
           4  613.07
           5  403.34
           6  296.23
           7  252.64
           8  251.67
           9  202.21
          10  200.34
        sum:  5341.06

        CONVERGED optimizations for the following remaining conformers:
        Converged optimization for CONF2  after  34 cycles: -497.4508410
        Converged optimization for CONF3  after  12 cycles: -497.4474711
        Converged optimization for CONF4  after  44 cycles: -497.4513419
        Converged optimization for CONF5  after  23 cycles: -497.4490457
        Converged optimization for CONF6  after  39 cycles: -497.4478893
        Converged optimization for CONF8  after  26 cycles: -497.4481610
        Converged optimization for CONF10 after  80 cycles: -497.4538409
        Converged optimization for CONF11 after  21 cycles: -497.4448166
        Converged optimization for CONF15 after  25 cycles: -497.4492319

        Calculating single-point energies and solvation contribution (G_solv)!
        The low level gsolv calculation is now calculated for:
        Constructed folders!
         CONF2,  CONF3,  CONF4,  CONF5,  CONF6,  CONF8, CONF10, CONF11, CONF15


        Starting 9 COSMO-RS-Gsolv calculations.
        Running COSMO-RS calculation in CONF2/r2scan-3c/COSMO
        Running COSMO-RS calculation in CONF3/r2scan-3c/COSMO
        Running COSMO-RS calculation in CONF4/r2scan-3c/COSMO
        Running COSMO-RS calculation in CONF5/r2scan-3c/COSMO
        Running COSMO-RS calculation in CONF6/r2scan-3c/COSMO
        Running COSMO-RS calculation in CONF8/r2scan-3c/COSMO
        Running COSMO-RS calculation in CONF10/r2scan-3c/COSMO
        Running COSMO-RS calculation in CONF11/r2scan-3c/COSMO
        Running COSMO-RS calculation in CONF15/r2scan-3c/COSMO
        Tasks completed!

        lowlevel COSMO-RS calculation was successful for  CONF2/r2scan-3c/COSMO: -0.13589836
        lowlevel COSMO-RS calculation was successful for  CONF3/r2scan-3c/COSMO: -0.12865185
        lowlevel COSMO-RS calculation was successful for  CONF4/r2scan-3c/COSMO: -0.13533997
        lowlevel COSMO-RS calculation was successful for  CONF5/r2scan-3c/COSMO: -0.12940253
        lowlevel COSMO-RS calculation was successful for  CONF6/r2scan-3c/COSMO: -0.12980575
        lowlevel COSMO-RS calculation was successful for  CONF8/r2scan-3c/COSMO: -0.12959608
        lowlevel COSMO-RS calculation was successful for CONF10/r2scan-3c/COSMO: -0.15938290
        lowlevel COSMO-RS calculation was successful for CONF11/r2scan-3c/COSMO: -0.13131124
        lowlevel COSMO-RS calculation was successful for CONF15/r2scan-3c/COSMO: -0.13806979

        Calculating lowlevel G_mRRHO with implicit solvation on DFT geometry!
        The lowlevel G_mRRHO calculation is now performed for:
         CONF2,  CONF3,  CONF4,  CONF5,  CONF6,  CONF8, CONF10, CONF11, CONF15

        Constructed folders!

        Starting 9 G_RRHO calculations.
        Running GFN2-xTB mRRHO in CONF2/rrho_part2
        Running GFN2-xTB mRRHO in CONF3/rrho_part2
        Running GFN2-xTB mRRHO in CONF4/rrho_part2
        Running GFN2-xTB mRRHO in CONF5/rrho_part2
        Running GFN2-xTB mRRHO in CONF6/rrho_part2
        Running GFN2-xTB mRRHO in CONF8/rrho_part2
        Running GFN2-xTB mRRHO in CONF10/rrho_part2
        Running GFN2-xTB mRRHO in CONF11/rrho_part2
        Running GFN2-xTB mRRHO in CONF15/rrho_part2
        Tasks completed!

        The lowlevel G_mRRHO calculation @ c1 was successful for  CONF2/rrho_part2: 0.18028469
        The lowlevel G_mRRHO calculation @ c1 was successful for  CONF3/rrho_part2: 0.18207391
        The lowlevel G_mRRHO calculation @ c1 was successful for  CONF4/rrho_part2: 0.18117449
        The lowlevel G_mRRHO calculation @ c1 was successful for  CONF5/rrho_part2: 0.18231888
        The lowlevel G_mRRHO calculation @ c1 was successful for  CONF6/rrho_part2: 0.18172189
        The lowlevel G_mRRHO calculation @ c1 was successful for  CONF8/rrho_part2: 0.18218769
        The lowlevel G_mRRHO calculation @ c1 was successful for CONF10/rrho_part2: 0.17889859
        The lowlevel G_mRRHO calculation @ c1 was successful for CONF11/rrho_part2: 0.18153296
        The lowlevel G_mRRHO calculation @ c1 was successful for CONF15/rrho_part2: 0.18070768

        --------------------------------------------------
                 * Gibbs free energies of part2 *         
        --------------------------------------------------

         CONF#  E(GFNn-xTB) ΔE(GFNn-xTB)       E [Eh]      Gsolv [Eh]  GmRRHO [Eh]         Gtot      ΔGtot Boltzmannweight
                     [a.u.]   [kcal/mol]    r2scan-3c COSMO-RS-normal         GFN2         [Eh] [kcal/mol]   % at 298.15 K
                                                          [r2scan-3c] [alpb]-bhess                                        
        CONF2   -34.1594484         0.32 -497.2837810      -0.1358984    0.1802847 -497.2393947       0.17           21.10
        CONF3   -34.1592530         0.44 -497.2915335      -0.1286518    0.1820739 -497.2381114       0.98            5.42
        CONF4   -34.1590954         0.54 -497.2853431      -0.1353400    0.1811745 -497.2395086       0.10           23.80
        CONF5   -34.1589250         0.65 -497.2917403      -0.1294025    0.1823189 -497.2388239       0.53           11.53
        CONF6   -34.1587465         0.76 -497.2896963      -0.1298058    0.1817219 -497.2377802       1.19            3.82
        CONF8   -34.1571241         1.78 -497.2905928      -0.1295961    0.1821877 -497.2380012       1.05            4.82
        CONF10  -34.1563217         2.28 -497.2591855      -0.1593829    0.1788986 -497.2396698       0.00           28.23     <------
        CONF11  -34.1559251         2.53 -497.2848139      -0.1313112    0.1815330 -497.2345922       3.19            0.13
        CONF15  -34.1529637         4.39 -497.2792896      -0.1380698    0.1807077 -497.2366518       1.89            1.15

        Calculating ALPB_Gsolv values for evaluation of the std. dev. of Gsolv.
        Constructed folders!

        Starting 9 ALPB-Gsolv calculations
        Running ALPB_GSOLV calculation in 3-part0-2/CONF2/alpb_gsolv
        Running ALPB_GSOLV calculation in 3-part0-2/CONF3/alpb_gsolv
        Running ALPB_GSOLV calculation in 3-part0-2/CONF4/alpb_gsolv
        Running ALPB_GSOLV calculation in 3-part0-2/CONF5/alpb_gsolv
        Running ALPB_GSOLV calculation in 3-part0-2/CONF6/alpb_gsolv
        Running ALPB_GSOLV calculation in 3-part0-2/CONF8/alpb_gsolv
        Running ALPB_GSOLV calculation in 3-part0-2/CONF10/alpb_gsolv
        Running ALPB_GSOLV calculation in 3-part0-2/CONF11/alpb_gsolv
        Running ALPB_GSOLV calculation in 3-part0-2/CONF15/alpb_gsolv
        Tasks completed!

        ALPB_GSOLV calculation was successful for  CONF2/alpb_gsolv: -0.14572423
        ALPB_GSOLV calculation was successful for  CONF3/alpb_gsolv: -0.14136373
        ALPB_GSOLV calculation was successful for  CONF4/alpb_gsolv: -0.14534319
        ALPB_GSOLV calculation was successful for  CONF5/alpb_gsolv: -0.14108828
        ALPB_GSOLV calculation was successful for  CONF6/alpb_gsolv: -0.14215086
        ALPB_GSOLV calculation was successful for  CONF8/alpb_gsolv: -0.14150861
        ALPB_GSOLV calculation was successful for CONF10/alpb_gsolv: -0.16262359
        ALPB_GSOLV calculation was successful for CONF11/alpb_gsolv: -0.14230853
        ALPB_GSOLV calculation was successful for CONF15/alpb_gsolv: -0.14881714

        SD of solvation models (all units in kcal/mol):
        CONFX ΔG(COSMO-RS) ΔG(DCOSMO-RS_gsolv) ΔG(ALPB_gsolv)  SD(COSMO-RS 40%, DCOSMO-RS_gsolv 40%, ALPB_gsolv 20%)
        ----------------------------------------------------------------------------------------------------
        CONF2      14.74           17.32             10.60                        3.01                  
        CONF3      19.28           24.30             13.34                        4.97                  
        CONF4      15.09           17.98             10.84                        3.21                  
        CONF5      18.81           23.44             13.51                        4.51                  
        CONF6      18.56           22.88             12.85                        4.53                  
        CONF8      18.69           23.27             13.25                        4.54                  
        CONF10     0.00             0.00             0.00                         0.00                  
        CONF11     17.62           21.74             12.75                        4.08                  
        CONF15     13.37           15.51             8.66                         3.06                  
        ----------------------------------------------------------------------------------------------------

        Calculating Boltzmann averaged free energy of ensemble!

        temperature /K:   avE(T) /a.u. avGmRRHO(T) /a.u. avGsolv(T) /a.u.   avG(T) /a.u.
        ----------------------------------------------------------------------------------------------------
            273.15        -497.2736863        0.1842434       -0.1489372   -497.2383801 
            278.15        -497.2749007        0.1835285       -0.1471130   -497.2384852 
            283.15        -497.2760521        0.1828067       -0.1453716   -497.2386171 
            288.15        -497.2771316        0.1820765       -0.1437255   -497.2387805 
            293.15        -497.2781302        0.1813377       -0.1421818   -497.2389743 
            298.15        -497.2790497        0.1805897       -0.1407374   -497.2391974      <<==part2==
            303.15        -497.2798874        0.1798315       -0.1393937   -497.2394496 
            308.15        -497.2806529        0.1790640       -0.1381406   -497.2397295 
            313.15        -497.2813425        0.1782861       -0.1369800   -497.2400363 
            318.15        -497.2819636        0.1774979       -0.1359037   -497.2403694 
            323.15        -497.2825199        0.1767001       -0.1349073   -497.2407270 
            328.15        -497.2830226        0.1758934       -0.1339792   -497.2411084 
            333.15        -497.2834731        0.1750769       -0.1331172   -497.2415134 
            338.15        -497.2838754        0.1742509       -0.1323164   -497.2419409 
            343.15        -497.2842354        0.1734163       -0.1315708   -497.2423900 
            348.15        -497.2845604        0.1725735       -0.1308732   -497.2428601 
            353.15        -497.2848497        0.1717219       -0.1302228   -497.2433506 
            358.15        -497.2851101        0.1708621       -0.1296129   -497.2438610 
            363.15        -497.2853448        0.1699946       -0.1290401   -497.2443904 
            368.15        -497.2855549        0.1691186       -0.1285027   -497.2449389 
            373.15        -497.2857417        0.1682355       -0.1279985   -497.2455047 
        ----------------------------------------------------------------------------------------------------



        --------------------------------------------------
                  Conformers considered further           
        --------------------------------------------------


        Conformers that are below the Boltzmann threshold G_thr(2) of 99.0%:
        CONF10,  CONF4,  CONF2,  CONF5,  CONF3,  CONF8,  CONF6, CONF15


        >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>END of Part2<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<
        Ran part2 in 5486.1036 seconds


        Part                : #conf    time
        --------------------------------------------------
        Input               :    22    -
        Part0_all           :    13    25.32s
        Part1_initial_sort  :    11    -
        Part1_all           :    11    166.42s
        Part2_opt           :     9    -
        Part2_all           :     8    5486.10s
        --------------------------------------------------
        All parts           :          5677.84s

        CENSO all done!

.. tabbed:: example censorc

    .. code:: sh

        $CENSO global configuration file: .censorc
        $VERSION:1.0.3 

        ORCA: /home/$USER/orca_4_2_1_linux_x86-64_openmpi216
        ORCA version: 4.2.1
        GFN-xTB: /home/$USER/bin/xtb
        CREST: /home/$USER/bin/crest
        mpshift: mpshift
        escf: escf

        #COSMO-RS
        ctd = BP_TZVPD_FINE_C30_1601.ctd  cdir = "/home/$USER/COSMOlogic/COSMOthermX19/COSMOtherm/CTDATA-FILES" ldir = "/home/bohle/COSMOlogic/COSMOthermX19/COSMOtherm/CTDATA-FILES"
        cosmothermversion: 19
        $ENDPROGRAMS

        $CRE SORTING SETTINGS:
        $GENERAL SETTINGS:
        nconf: all                       # ['all', 'number e.g. 10 up to all conformers'] 
        charge: 1                        # ['number e.g. 0'] 
        unpaired: 0                      # ['number e.g. 0'] 
        solvent: h2o                     # ['gas', 'acetone', 'chcl3', 'acetonitrile', 'ch2cl2', 'dmso', 'h2o', 'methanol', 'thf', '...'] 
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
        func: r2scan-3c                  # ['pbe', 'b97-d', 'pbeh-3c', 'tpss', 'b97-d3', 'r2scan-3c', 'b97-3c'] 
        basis: automatic                 # ['automatic', 'def2-mSVP', 'def2-mTZVP', 'def2-mTZVP', 'def2-TZVP', '...'] 
        maxthreads: 4                    # ['number of threads e.g. 2'] 
        omp: 2                           # ['number cores per thread e.g. 4'] 
        cosmorsparam: automatic          # ['automatic', '12-fine', '12-normal', '13-fine', '13-normal', '14-fine', '...']

        $PART0 - CHEAP-PRESCREENING - SETTINGS:
        part0: on                        # ['on', 'off'] 
        func0: b97-d                     # ['pbeh-3c', 'b97-3c', 'b97-d3', 'pbe', 'r2scan-3c', 'tpss', 'b97-d'] 
        basis0: def2-SV(P)               # ['automatic', 'def2-mSVP', 'def2-mTZVP', 'def2-mTZVP', 'def2-TZVP', '...'] 
        part0_gfnv: gfn2                 # ['gfn1', 'gfn2', 'gfnff'] 
        part0_threshold: 4.0             # ['number e.g. 4.0'] 

        $PART1 - PRESCREENING - SETTINGS:
        # func and basis is set under GENERAL SETTINGS
        part1: on                        # ['on', 'off'] 
        smgsolv1: cosmors                # ['alpb_gsolv', 'dcosmors', 'cosmo', 'smd', 'cosmors-fine', 'cpcm', 'gbsa_gsolv', '...'] 
        part1_gfnv: gfn2                 # ['gfn1', 'gfn2', 'gfnff'] 
        part1_threshold: 3.5             # ['number e.g. 5.0'] 

        $PART2 - OPTIMIZATION - SETTINGS:
        # func and basis is set under GENERAL SETTINGS
        part2: on                        # ['on', 'off'] 
        opt_limit: 2.5                   # ['number e.g. 4.0'] 
        sm2: dcosmors                    # ['cosmo', 'cpcm', 'default', 'smd', 'dcosmors'] 
        smgsolv2: cosmors                # ['cosmors-fine', 'gbsa_gsolv', 'cosmo', 'cpcm', 'smd_gsolv', 'alpb_gsolv', 'smd', '...'] 
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
        func3: pw6b95                    # ['dsd-blyp', 'pw6b95', 'b97-d3', 'r2scan-3c', 'pbe0', 'wb97x'] 
        basis3: def2-TZVPD               # ['SVP', 'SV(P)', 'TZVP', 'TZVPP', 'QZVP', 'QZVPP', 'def2-SV(P)', 'def2-mSVP', '...'] 
        smgsolv3: cosmors                # ['alpb_gsolv', 'dcosmors', 'cosmo', 'smd', 'cosmors-fine', 'cpcm', 'gbsa_gsolv', '...'] 
        part3_gfnv: gfn2                 # ['gfn1', 'gfn2', 'gfnff'] 
        part3_threshold: 99              # ['Boltzmann sum threshold in %. e.g. 95 (between 1 and 100)'] 

        $NMR PROPERTY SETTINGS:
        $PART4 SETTINGS:
        part4: off                       # ['on', 'off'] 
        couplings: on                    # ['on', 'off'] 
        progJ: prog                      # ['tm', 'orca', 'adf', 'prog'] 
        funcJ: pbe0                      # ['tpss', 'pbe0', 'pbeh-3c'] 
        basisJ: def2-TZVP                # ['SVP', 'SV(P)', 'TZVP', 'TZVPP', 'QZVP', 'QZVPP', 'def2-SV(P)', 'def2-mSVP', '...'] 
        sm4J: default                    # ['dcosmors', 'smd', 'cosmo', 'cpcm'] 
        shieldings: on                   # ['on', 'off'] 
        progS: prog                      # ['tm', 'orca', 'adf', 'prog'] 
        funcS: pbe0                      # ['dsd-blyp', 'pbeh-3c', 'tpss', 'pbe0', 'kt2'] 
        basisS: def2-TZVP                # ['SVP', 'SV(P)', 'TZVP', 'TZVPP', 'QZVP', 'QZVPP', 'def2-SV(P)', 'def2-mSVP', '...'] 
        sm4S: default                    # ['dcosmors', 'smd', 'cosmo', 'cpcm'] 
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

.. tabbed:: input-ensemble.xyz

    .. code:: sh

          25
                -34.15995484
         O         -2.7404108553       -0.9210263756        0.0505546155
         O         -0.6532148483       -1.7303331963        0.1381884568
         N         -2.1811530948        1.4662136641       -0.3280604715
         N          1.9999042791       -1.6115147219       -0.1737452561
         C          1.2832974514        1.7494376339        0.2478643443
         C          0.0382242755        1.0547713107        0.8044626435
         C          2.1972182535        0.8461961224       -0.5852172042
         C         -0.9624505804        0.6276788621       -0.2713633850
         C          2.7304045776       -0.3664138808        0.1827513585
         C         -1.5037598584       -0.8390454546        0.0048901917
         H          1.8561477225        2.1402514306        1.0892838382
         H          0.9852377848        2.6009114549       -0.3642611073
         H         -0.4518270956        1.7282098745        1.5089809935
         H          0.3322545004        0.1775252924        1.3825123935
         H          1.6820235078        0.5059218506       -1.4832357051
         H          3.0380251642        1.4546200515       -0.9165284484
         H         -0.4868586610        0.6089411942       -1.2530945560
         H          2.6388482788       -0.2021893439        1.2562535001
         H          3.7853986884       -0.5188774904       -0.0473087348
         H         -2.2380899515        2.1426216511        0.4407329422
         H         -2.3218532286        1.9466721688       -1.2205963738
         H          2.3394829642       -2.3992859458        0.3882351555
         H          0.9350292280       -1.5547588441       -0.0165809300
         H          2.1475890354       -1.8368760327       -1.1636223077
         H         -2.9328940343        0.6971914121       -0.1921053926
          25
                -34.15944839
         O         -2.5910945174       -0.7526707388        0.4737473250
         O         -0.5258609320       -1.6055473558        0.4259946366
         N         -2.1883047577        1.2429283468       -0.9276187078
         N          2.0221224751       -1.5482527990       -0.3635876013
         C          1.0959612422        0.9269941773        1.0928268792
         C         -0.1118183900        1.5287127427        0.3646185610
         C          2.3883955950        0.8232150730        0.2771806877
         C         -0.9137052232        0.5859776609       -0.5447048684
         C          2.3984857923       -0.1999516224       -0.8542391926
         C         -1.3796771114       -0.7342464594        0.2052799197
         H          0.8202702309       -0.0346561349        1.5230052762
         H          1.3150554091        1.5881707649        1.9329336393
         H          0.2252898792        2.3837647705       -0.2251981715
         H         -0.7970460540        1.8894152433        1.1356879771
         H          2.6174364896        1.7989347868       -0.1517789136
         H          3.1942064704        0.5825472961        0.9724285437
         H         -0.3499369029        0.3059081761       -1.4335569624
         H          3.4048136146       -0.2402683769       -1.2749539435
         H          1.7122153210        0.0852818535       -1.6492945642
         H         -2.2438155060        2.2226460139       -0.6308218074
         H         -2.4094139539        1.1700691932       -1.9248068681
         H          0.9927667520       -1.5675551657       -0.0428313977
         H          2.1377830502       -2.2542373686       -1.0973459319
         H          2.6054467969       -1.8083924261        0.4393088723
         H         -2.8796747565        0.6310313638       -0.3580585669
          25
                -34.15925304
         O         -2.6152400314       -1.0465052003        0.2378304729
         O         -0.5362082855       -1.4663149505       -0.4593671015
         N         -2.4557715395        1.4039918868        0.4362491058
         N          2.1447268012       -1.5911295291       -0.0908488427
         C          1.1849792140        1.1291603967       -0.7123125066
         C          0.0549239500        1.3190958463        0.3109988160
         C          2.5516538580        0.8614163999       -0.0796419105
         C         -1.2770304521        0.8481909298       -0.2699298296
         C          2.6278492228       -0.4408272994        0.7143702153
         C         -1.4798499370       -0.7275247362       -0.1594413730
         H          1.2638776944        2.0288103522       -1.3231621409
         H          0.9216680773        0.3153070715       -1.3864974308
         H         -0.0132519646        2.3752873127        0.5749134480
         H          0.2604586805        0.7576942571        1.2223482952
         H          3.2969117377        0.8423750312       -0.8755852689
         H          2.8115908685        1.6810094994        0.5898815386
         H         -1.3353089850        1.1014685130       -1.3310936133
         H          2.0240292821       -0.3842292575        1.6193674755
         H          3.6638250279       -0.6160286136        1.0070988283
         H         -2.2160156909        1.7709567167        1.3631094761
         H         -2.9723151833        2.1102297821       -0.0929227069
         H          2.6253531272       -1.6216026098       -0.9974699479
         H          2.3064470721       -2.4761725884        0.4009650983
         H          1.0998172314       -1.5111491960       -0.2664030290
         H         -3.0342449423        0.4895941432        0.5473101358
          25
                -34.15909539
         O         -2.5859531813       -0.8098009525        0.4685129327
         O         -0.4816245052       -1.5436026390        0.3102868940
         N         -2.3339104715        1.3007756781       -0.7836622820
         N          2.0492564794       -1.5871472251       -0.5900906398
         C          1.1052196056        0.9870949301        0.9271737021
         C         -0.2140772115        1.5925038037        0.4451868362
         C          2.1667497593        0.8464686753       -0.1771749747
         C         -1.0154302176        0.6861215046       -0.4984780600
         C          2.9073785151       -0.4823039670       -0.0859386105
         C         -1.3840762559       -0.7076318190        0.1707141950
         H          0.8920163207        0.0201435075        1.3815317896
         H          1.4930774855        1.6249540613        1.7213050551
         H         -0.0067258320        2.5368714102       -0.0630175124
         H         -0.8282372023        1.7972548662        1.3249938991
         H          1.7135886317        0.9350726379       -1.1636100949
         H          2.8915967677        1.6540129472       -0.0882838850
         H         -0.4677225963        0.5007855037       -1.4227063242
         H          3.1688824777       -0.6847294678        0.9524589517
         H          3.8237876908       -0.4492687854       -0.6762385697
         H         -2.5870605138        1.3036833133       -1.7755142934
         H         -2.9689761577        0.6014426661       -0.2449843511
         H          1.9723283573       -1.5417442554       -1.6118847971
         H          2.4360322309       -2.5007606995       -0.3327999527
         H          1.0548817542       -1.5294780724       -0.1881461707
         H         -2.4296444696        2.2452869585       -0.3976639893
          25
                -34.15892503
         O         -2.4565204045       -0.8423454324        0.7070928712
         O         -0.5980888346       -1.6084799899       -0.2741310513
         N         -2.2081439307        1.5499049911        0.0797136762
         N          2.0665657616       -1.5738488309        0.1665385721
         C          0.8489992162        1.1021214800        0.5754910780
         C          0.1319038004        1.1643519245       -0.7806607655
         C          2.3611287197        0.8895018711        0.4504901307
         C         -1.3084663819        0.6750096988       -0.7114456668
         C          2.7727042231       -0.3622083435       -0.3226992137
         C         -1.4583757981       -0.7537800183       -0.0274928276
         H          0.4155006230        0.3036523983        1.1797441237
         H          0.7020515096        2.0340597672        1.1228295970
         H          0.6304477123        0.5210588124       -1.5026223849
         H          0.1602968624        2.1778026423       -1.1819014190
         H          2.8058653217        1.7473421346       -0.0537220547
         H          2.7810957945        0.8489865537        1.4562868383
         H         -1.7106345304        0.5638398621       -1.7216356404
         H          3.8478441803       -0.5048945341       -0.2050768697
         H          2.5630334022       -0.2516058916       -1.3860041489
         H         -2.8269415029        2.1396743466       -0.4823076953
         H         -2.7642911303        0.7978509993        0.6164252281
         H          2.4375926491       -2.4160896797       -0.2859521821
         H          2.1793786728       -1.6693140839        1.1823811296
         H          1.0245787819       -1.5392035408       -0.0507409777
         H         -1.6858363886        2.1297513428        0.7452520673
          25
                -34.15874648
         O         -2.3605981473       -0.8116042758        0.8659409274
         O         -0.5843274484       -1.6230194998       -0.2244865945
         N         -2.2109877661        1.5364887021        0.0742080589
         N          2.0757147084       -1.4672125485       -0.4830865528
         C          0.8597549995        1.2262238823        0.4517476395
         C          0.0874032105        1.1513231513       -0.8771132949
         C          2.3512317789        0.8985689618        0.2982329522
         C         -1.3361657088        0.6347191585       -0.7132778341
         C          2.6824356905       -0.5712975375        0.5355813092
         C         -1.4234260659       -0.7566236862        0.0528412632
         H          0.4221004807        0.5356789162        1.1753502177
         H          0.7733707657        2.2309740830        0.8650565898
         H          0.5827203837        0.4802075195       -1.5756071724
         H          0.0631682297        2.1335262804       -1.3514203271
         H          2.7003072358        1.2129278451       -0.6855204577
         H          2.9172351560        1.4667500260        1.0360745366
         H         -1.7833398578        0.4616073570       -1.6955611361
         H          2.3142140534       -0.8770131145        1.5154931358
         H          3.7656889000       -0.7005750162        0.5151167425
         H         -1.6671444199        2.1735125324        0.6661186569
         H         -2.8827343086        2.0695764162       -0.4835506438
         H          2.3643871815       -1.1879653946       -1.4273853498
         H          2.3725094498       -2.4360036549       -0.3237631449
         H          1.0071911744       -1.4659435853       -0.4227446705
         H         -2.7090441201        0.8070062032        0.6949537917
          25
                -34.15780648
         O         -0.6998712600       -1.7595821198        0.3315266042
         O         -2.7564980797       -0.8922468771        0.1127087169
         N         -2.0912345729        1.3897515697       -0.6207661035
         N          1.9218668343       -1.5538051846       -0.0911139226
         C          1.2724611881        1.7665596691        0.2614262584
         C         -0.0126245071        1.0963347719        0.7686474620
         C          2.5091659413        0.8642624497        0.2746224048
         C         -0.9098130242        0.5407794733       -0.3415020783
         C          2.4929985644       -0.3196472927       -0.6935656161
         C         -1.5185682699       -0.8616584773        0.0830057999
         H          1.4961397665        2.6156227165        0.9073501428
         H          1.1219744652        2.1603919352       -0.7434483203
         H         -0.5894557289        1.8123867185        1.3552290309
         H          0.2456564204        0.2915541326        1.4587568868
         H          3.3629575544        1.4905499194        0.0148013745
         H          2.6790096911        0.5030846620        1.2896168914
         H         -0.3423105707        0.3842564461       -1.2596402044
         H          3.5203242755       -0.5424656780       -0.9864282797
         H          1.9321769951       -0.0751719518       -1.5947809502
         H         -2.8797605313        0.6907420298       -0.3760235139
         H         -2.1503924665        2.2120232522       -0.0104200923
         H          2.3803263882       -1.7503210865        0.8055549093
         H          0.8561607870       -1.5268128372        0.0892049642
         H          2.0800598725       -2.3543999415       -0.7123089418
         H         -2.1774862389        1.6805852373       -1.5986092278
          25
                -34.15712409
         O         -0.5169415866       -1.4677953372       -0.4155166026
         O         -2.6718105717       -1.1029442546        0.0532698902
         N         -2.5041882719        1.3222450456        0.5356135683
         N          2.0547500815       -1.4660199090        0.3952674841
         C          1.1460672076        1.2350620947       -0.5905524477
         C          0.0199991306        1.2471137062        0.4571994904
         C          2.5324119546        0.9449216441        0.0010319703
         C         -1.3028765754        0.8219664596       -0.1753086903
         C          2.9759584940       -0.4928036507       -0.2449354813
         C         -1.5027299259       -0.7570468322       -0.1889883845
         H          1.1567284127        2.2014747982       -1.0932584224
         H          0.9206483903        0.4869950226       -1.3509226784
         H         -0.0730555237        2.2557886152        0.8619704582
         H          0.2503077610        0.5750867956        1.2847618985
         H          3.2724883861        1.5930094341       -0.4660504111
         H          2.5411017268        1.1667575642        1.0680525607
         H         -1.3439398378        1.1490834108       -1.2170635960
         H          3.9809942022       -0.6416689206        0.1522940223
         H          2.9906906878       -0.6917352586       -1.3167852841
         H         -3.1147358709        0.4302326474        0.4978236161
         H         -2.3055244586        1.5552316017        1.5147727778
         H          1.0589542040       -1.3722826387        0.0237179616
         H          2.3537713222       -2.4274392985        0.2004063016
         H          2.0384435533       -1.3281897829        1.4119124615
         H         -2.9671283162        2.1123945703        0.0792841186
          25
                -34.15703719
         O         -0.4840204654       -1.5908390855       -0.1977054306
         O         -2.6424172363       -1.0172969494       -0.2313294170
         N         -2.3824391815        1.4118161237        0.1129756179
         N          2.0591606020       -1.4080559447        0.6444993748
         C          1.3255949204        1.5932768744        0.1316443482
         C          0.0292286137        1.1405631347        0.8180158808
         C          1.9611296764        0.5914767293       -0.8382657525
         C         -1.0774245534        0.7702791240       -0.1744053472
         C          2.8498636045       -0.4645206094       -0.1922799795
         C         -1.4218007565       -0.7850501416       -0.2021503198
         H          2.0511714191        1.8626404605        0.8996124027
         H          1.1017826165        2.5033885185       -0.4265728164
         H         -0.3083479528        1.9763847128        1.4332069953
         H          0.2033356421        0.3027453082        1.4918974312
         H          1.1939462785        0.0852292002       -1.4240400771
         H          2.5871303840        1.1474547523       -1.5362113415
         H         -0.7775416723        1.0312499156       -1.1919311777
         H          3.6205966803        0.0055444316        0.4194317100
         H          3.3361661233       -1.0318279746       -0.9856062704
         H         -2.6284628403        2.1753990573       -0.5216811303
         H         -3.0325627338        0.5540981187       -0.0284710758
         H          2.4910941867       -2.3367192092        0.6680136974
         H          1.9781398838       -1.0663385144        1.6071566991
         H          1.0702227114       -1.5059322479        0.2455456256
         H         -2.4592842029        1.7302943098        1.0848423546
          25
                -34.15632171
         O         -0.6377330316       -1.5602972801       -0.5925407457
         O         -1.9438226066       -0.8962122015        1.0968963302
         N         -2.0691310397        1.5115608178        0.5520085904
         N          2.0161394685       -1.6864701960       -0.6808935380
         C          1.0245208700        1.5678424724        0.3114580637
         C          0.0268814843        1.3822638313       -0.8376423667
         C          1.7125418505        0.2918072015        0.8062089597
         C         -1.3255436976        0.7602094261       -0.4861917907
         C          2.5798867778       -0.3735189736       -0.2699515146
         C         -1.2821941821       -0.7269243894        0.0579815663
         H          0.5483025277        2.0449086772        1.1692286093
         H          1.7966540418        2.2561425086       -0.0359536522
         H          0.4649670843        0.7632830938       -1.6196837604
         H         -0.1661732017        2.3619672870       -1.2796881790
         H          2.3322565126        0.5621906956        1.6605054657
         H          0.9655697921       -0.4140620317        1.1690705974
         H         -1.9201614836        0.7061105980       -1.4020292431
         H          3.5936748701       -0.5312821922        0.0979019662
         H          2.6369135707        0.2686143194       -1.1490685384
         H         -2.9086496435        1.9840429736        0.2094100180
         H         -2.3313717130        0.6888337551        1.2084854943
         H          2.1820919942       -2.3913736082        0.0460188498
         H          0.9581287874       -1.6201065231       -0.7926007692
         H          2.4297604389       -2.0163864988       -1.5591512145
         H         -1.4715716162        2.1835379113        1.0453989384
          25
                -34.15592507
         O         -2.1033969149       -0.8344078612        0.9487088877
         O         -0.5699612374       -1.6680274471       -0.4517081377
         N         -1.9569275917        1.5465619838        0.2627686808
         N          2.0837337252       -1.3633283870       -0.5773762363
         C          1.1075416509        1.5419562581       -0.0103918263
         C          0.1209945172        1.1686244794       -1.1251953409
         C          1.4352918321        0.4570859531        1.0214669735
         C         -1.2503428799        0.6557309102       -0.6867527508
         C          2.5057372966       -0.5407868185        0.5902823997
         C         -1.2948162683       -0.7745608298        0.0075959523
         H          0.7274518351        2.4141375453        0.5251162579
         H          2.0330446007        1.8691474761       -0.4864359338
         H          0.5312965142        0.4086384701       -1.7869917742
         H         -0.0324587227        2.0621064981       -1.7347668750
         H          1.8169519144        0.9463147999        1.9183948838
         H          0.5367641743       -0.0848292604        1.3149274400
         H         -1.8634330878        0.5270669166       -1.5839659846
         H          2.7015999487       -1.2099014501        1.4282318085
         H          3.4296803314       -0.0167769980        0.3414844052
         H         -1.2992225514        2.1272618717        0.7958603647
         H         -2.6755557698        2.1370116562       -0.1630903688
         H          2.2813676133       -0.8782194673       -1.4586367594
         H          2.5735263109       -2.2635096262       -0.5875520766
         H          1.0325521121       -1.5556317222       -0.5353874345
         H         -2.3935764356        0.8063761382        0.9145561672
          25
                -34.15547499
         O         -2.3288681540       -0.9689367039        0.5974852723
         O         -0.4008216845       -1.3559960891       -0.4559450313
         N         -2.6333104330        1.3901145964        0.0148791472
         N          2.2688734235       -1.7836122490       -0.3135245436
         C          1.2045970266        1.2147830910       -0.3207104722
         C         -0.1766849998        1.7049883679        0.1388725454
         C          1.9421007389        0.4104322018        0.7578355455
         C         -1.3241367595        0.8966054975       -0.4749078790
         C          2.9601024995       -0.5430248114        0.1410496733
         C         -1.3285933693       -0.6410895869       -0.0712604222
         H          1.8194085679        2.0702167267       -0.5988277282
         H          1.0710671602        0.6065579215       -1.2146344993
         H         -0.3076575365        2.7461839050       -0.1612866699
         H         -0.2378627475        1.6551531995        1.2275331138
         H          2.4468113264        1.0960018700        1.4370167059
         H          1.2297127637       -0.1683684162        1.3468389979
         H         -1.2704988015        0.9491259299       -1.5640455987
         H          3.7310656000       -0.8072491298        0.8640696447
         H          3.4371256948       -0.0632845904       -0.7133472266
         H         -2.5432643621        2.1520849835        0.6930123940
         H         -3.2834926874        1.6599628672       -0.7262042831
         H          2.7178100064       -2.1982719631       -1.1361624210
         H          2.2482990815       -2.4828183187        0.4368503414
         H          1.2567750610       -1.5723871751       -0.5319861416
         H         -2.9757946227        0.4721100866        0.5082732629
          25
                -34.15545846
         O         -2.3777023451       -0.9177733836        0.6489622803
         O         -0.4009151018       -1.4460423055       -0.2363506290
         N         -2.5928675119        1.3702602408       -0.1958788709
         N          2.2705863538       -1.5552728060       -0.6959671399
         C          1.2626408325        1.2650767695       -0.1999508619
         C         -0.1676900608        1.6997857440        0.1433859727
         C          1.8576282855        0.3222355626        0.8537184709
         C         -1.2484356318        0.8255651567       -0.5054674146
         C          2.9084111559       -0.6168744122        0.2764230940
         C         -1.3258577703       -0.6732783629        0.0271901706
         H          1.8935551201        2.1501893106       -0.2772016053
         H          1.2458750287        0.7908735642       -1.1798122422
         H         -0.3171466901        2.7186794233       -0.2201486160
         H         -0.2967658760        1.7007241110        1.2272150872
         H          2.3089550370        0.9081814099        1.6535209776
         H          1.0686425282       -0.2818531316        1.3016188748
         H         -1.0876822336        0.7817119785       -1.5847014431
         H          3.3524799930       -1.1924945383        1.0873745985
         H          3.6963304583       -0.0519426265       -0.2212122945
         H         -2.9913275573        0.5162412164        0.3615908966
         H         -2.5589278358        2.2025641690        0.3999047288
         H          2.6187881911       -2.5117770389       -0.5815509942
         H          1.2217357961       -1.5619680721       -0.5321789754
         H          2.4345460024       -1.2660811775       -1.6650051770
         H         -3.1696789754        1.5558016135       -1.0193997348
          25
                -34.15372845
         O         -0.8759513206       -1.0507714945        0.7395130487
         O         -1.0471783147       -1.2210634731       -1.4857197061
         N         -2.0100713682        1.1638582833        0.9733772433
         N          1.7020599065       -1.7447966827        0.8365171560
         C          0.9224701988        1.3977552196        0.1299066020
         C         -0.3034955554        1.7474745141       -0.7290850754
         C          1.8313865205        0.3371413309       -0.5152183962
         C         -1.5397536849        0.8954837973       -0.4124258872
         C          2.5286316072       -0.5462459399        0.5204181162
         C         -1.1337894558       -0.6241211220       -0.4224393440
         H          0.5999134506        1.0447379113        1.1088504940
         H          1.5075448530        2.3032957331        0.2891927248
         H         -0.0603736240        1.6138572758       -1.7832639404
         H         -0.5689030950        2.7975685720       -0.5933476217
         H          1.2520636483       -0.2949117808       -1.1892484363
         H          2.5821478388        0.8362393886       -1.1261103610
         H         -2.3194229349        1.0797505602       -1.1497937252
         H          2.6951475316        0.0179430508        1.4383444706
         H          3.4942793529       -0.8824584649        0.1428332329
         H         -1.6366862386        2.0406897131        1.3566364027
         H         -3.0319635647        1.1643591957        1.0502533535
         H          2.0123953855       -2.1956055335        1.7033301803
         H          0.6617699349       -1.4988916853        0.9180060433
         H          1.7620357216       -2.4244612968        0.0700935319
         H         -1.6155014744        0.3110635636        1.4848864268
          25
                -34.15296366
         O         -0.9806508876       -0.9847570656        0.9016095316
         O         -1.0342961735       -1.4975369515       -1.2790158618
         N         -2.0600699488        1.3003330160        0.6598555012
         N          1.6483681534       -1.4770759046        1.0193012553
         C          0.8930197778        1.4409262402        0.3162871289
         C         -0.1018287155        1.4892580056       -0.8622608316
         C          2.2127299908        0.7309357778       -0.0043863659
         C         -1.4370888955        0.7837090011       -0.5889439421
         C          2.1074033787       -0.7768274559       -0.2134156979
         C         -1.1447588094       -0.7374998177       -0.3265126418
         H          0.4424872625        0.9591479756        1.1822965353
         H          1.1476108940        2.4604379775        0.6056686014
         H          0.3344753521        1.0203527521       -1.7446629435
         H         -0.3104542151        2.5259312846       -1.1308040482
         H          2.6400205950        1.1617275571       -0.9101356051
         H          2.9157320587        0.9267783500        0.8074716570
         H         -2.0965188879        0.8923814409       -1.4486238017
         H          3.0903725751       -1.1592546293       -0.4905246456
         H          1.4081882505       -1.0149227036       -1.0156071158
         H         -3.0831759004        1.3363322990        0.6000761811
         H         -1.7780292676        0.5566802205        1.3629478428
         H          0.5935972201       -1.3552723672        1.1310943683
         H          1.8230458958       -2.4842696303        0.9418471118
         H          2.1352867113       -1.1124232113        1.8467861942
         H         -1.6994055067        2.2241075607        0.9291898687
          25
                -34.15141381
         O         -1.3943785432       -1.0307567325        0.9025777408
         O         -0.4225106153       -1.2766835982       -1.0942963862
         N         -2.1382332497        1.3072100696        0.6820859684
         N          1.8303685962       -1.9710495392        0.0707587666
         C          0.9607624662        1.6168605590        0.1707894371
         C         -0.1959862014        1.7114505222       -0.8393306352
         C          1.9799050629        0.5013843169       -0.1348429740
         C         -1.4247901716        0.8500833461       -0.5368218294
         C          1.9371661639       -0.6960029627        0.8273160062
         C         -1.0467465663       -0.6453837331       -0.2300826075
         H          0.5821582777        1.4936196592        1.1855414824
         H          1.4853467761        2.5717069600        0.1437592453
         H          0.1684852019        1.4328271659       -1.8279890567
         H         -0.5295558410        2.7487716363       -0.9068101296
         H          1.7990108513        0.1423863706       -1.1479579769
         H          2.9805578856        0.9303497506       -0.1217137710
         H         -2.0885910031        0.8601550715       -1.4024758281
         H          1.0694995988       -0.6377417602        1.4853144468
         H          2.8334350246       -0.7245407799        1.4470296049
         H         -1.6564311817        2.0816802979        1.1506799283
         H         -3.1156981590        1.5603768219        0.5197984898
         H          2.6238854975       -2.0888513426       -0.5678832194
         H          1.7803639034       -2.7755595157        0.7040003308
         H          0.9218328585       -1.9204346441       -0.4990801476
         H         -2.0745975566        0.3957317695        1.2728068192
          25
                -34.15122546
         O         -1.4776298354       -0.9779162840        0.9689911790
         O         -0.4925234496       -1.3628551244       -1.0036507954
         N         -2.1639329473        1.3683833598        0.5828976748
         N          1.8796097534       -1.9018199765       -0.0324662357
         C          0.9212842989        1.5787487906        0.2022882964
         C         -0.1575522111        1.5949891498       -0.8983466380
         C          2.0807266384        0.5903317386       -0.0312863938
         C         -1.4336452672        0.8124987799       -0.5833640723
         C          2.0372506913       -0.6914975753        0.8153963432
         C         -1.1077721831       -0.6712596326       -0.1784565029
         H          0.4772061571        1.3762504059        1.1769048440
         H          1.3491352329        2.5796760629        0.2516191005
         H          0.2519128515        1.1866703660       -1.8221067218
         H         -0.4443786804        2.6264492546       -1.1098247151
         H          2.1069094344        0.3214704229       -1.0870338715
         H          3.0133169149        1.1098047495        0.1839278312
         H         -2.0690635873        0.7917157945       -1.4696202320
         H          1.1994207751       -0.6693189818        1.5132782369
         H          2.9569334194       -0.7921592267        1.3922309558
         H         -1.6800036187        2.1679532791        1.0053931771
         H         -3.1334683512        1.6233161070        0.3791665015
         H          2.6232513605       -1.9579134554       -0.7362257317
         H          1.8912989959       -2.7525757838        0.5392212719
         H          0.9273933706       -1.8444066710       -0.5257136187
         H         -2.1333692549        0.5050567581        1.2388793063
          25
                -34.15102491
         O         -2.7007099152       -1.6852260154       -0.4665848832
         O         -3.5578446821        0.0350414384        0.6950996250
         N         -1.7249631955        1.6316108345        0.4298941943
         N          4.8503432460       -0.2765578553        0.0049566510
         C          1.1047501969        0.4501828237       -0.3379055216
         C         -0.1445140391       -0.2886936630        0.1459845288
         C          2.3741940866       -0.2828979224        0.0965187068
         C         -1.4339016427        0.3793165613       -0.3028003047
         C          3.6160850376        0.4539254738       -0.3951671902
         C         -2.6934727541       -0.5488189533       -0.0039847520
         H          1.1202182595        1.4641232269        0.0631636060
         H          1.0744507790        0.5160807326       -1.4261342120
         H         -0.1466884742       -0.3764861277        1.2347858649
         H         -0.1614173698       -1.3033571375       -0.2567289346
         H          2.3826839006       -0.3550960319        1.1849100754
         H          2.3458274825       -1.2937888279       -0.3120785959
         H         -1.4193517541        0.5529473168       -1.3796978179
         H          3.6476836700        1.4575725004        0.0303995802
         H          3.5908364898        0.5356651831       -1.4823627548
         H         -1.7888195298        2.4639789876       -0.1581990321
         H         -2.7206446817        1.3385249836        0.8116492773
         H          5.6915565940        0.2174183391       -0.3259194480
         H          4.8962264202       -0.3605243899        1.0314916029
         H          4.8443782262       -1.2261178281       -0.3963638456
         H         -1.0847775954        1.7904079765        1.2125175138
          25
                -34.15096274
         O         -3.0664396656       -0.6811514259        0.4732552569
         O         -1.1830540829       -1.6357953168       -0.2743493731
         N         -2.5677177347        1.6455787239       -0.0357930901
         N          3.4161836207       -1.4947231742        0.0043983586
         C          1.0526738400        0.4198619079       -0.3271341828
         C         -0.1392962419        1.1756334823        0.2601668795
         C          2.3301787116        0.6870764296        0.4694516416
         C         -1.4374925629        0.7826136622       -0.4432938226
         C          3.5433351448       -0.0155439223       -0.1263696890
         C         -1.9232732343       -0.6849210780       -0.0466890927
         H          1.1914397265        0.7321338619       -1.3635085567
         H          0.7858921189       -0.6373779302       -0.3204311762
         H          0.0218209785        2.2505182304        0.1468292069
         H         -0.2266241551        0.9455046686        1.3247226359
         H          2.5278796928        1.7590277621        0.4727119199
         H          2.1882724928        0.3782271446        1.5058219746
         H         -1.3025118280        0.8032883597       -1.5259809861
         H          4.4479425383        0.3027435209        0.3921583620
         H          3.6346463266        0.2349775607       -1.1836334244
         H         -3.0211573542        2.1410074345       -0.8045922540
         H         -3.2218629847        0.8371482423        0.3587042034
         H          4.2253281871       -1.9726822174       -0.4155872948
         H          3.3489651634       -1.7581308116        0.9983748528
         H          2.5570336567       -1.8144640232       -0.4679619280
         H         -2.3203269015        2.3008181314        0.7098687755
          25
                -34.15085064
         O         -3.5353488917        0.0322108098        0.4084906022
         O         -2.4254579214       -1.9084093375        0.6284254091
         N         -1.7744403653        1.3652775888       -0.6438663864
         N          4.1843308757       -0.1227935311       -1.0149931633
         C          1.2054477125        0.3338345853       -0.2573250215
         C         -0.0723683835       -0.0922253390        0.4701395235
         C          2.4067626747        0.2476116899        0.6871054133
         C         -1.3095998224       -0.0201188789       -0.4106994862
         C          3.6989393857        0.7735207317        0.0732627710
         C         -2.5481677099       -0.7347193516        0.2919952398
         H          1.1039179513        1.3593179418       -0.6150772671
         H          1.3305507433       -0.3269971917       -1.1156830038
         H         -0.2323641779        0.5179203592        1.3619095074
         H          0.0148258366       -1.1272987239        0.8068454593
         H          2.1939049448        0.8432977651        1.5748900270
         H          2.5356727482       -0.7824695024        1.0211816116
         H         -1.1430786561       -0.5391980852       -1.3555500184
         H          4.4681954117        0.8303339300        0.8434799998
         H          3.5410323285        1.7707403176       -0.3387001734
         H         -2.7959421798        1.2370421526       -0.2418926774
         H         -1.2663143192        2.0543977636       -0.0827359543
         H          4.3101055621       -1.0795234870       -0.6518667702
         H          3.4997443822       -0.1538144186       -1.7841531054
         H          5.0842467659        0.2120203831       -1.3873716549
         H         -1.8037773550        1.6476905491       -1.6249441216
          25
                -34.15048917
         O         -1.9774584219       -0.8822136242       -1.3447844876
         O         -0.7353222350       -1.3040874260        0.4712779300
         N         -1.2018711229        0.8204001109        1.6759883927
         N          1.9464051556       -1.6923491374        0.3496868798
         C          0.8058411616        1.0462579377       -0.9468506072
         C         -0.4754178942        1.7785998054       -0.5275051890
         C          1.8226970853        0.7789686777        0.1752315710
         C         -1.5019161161        0.9155320919        0.2220611780
         C          2.6576744598       -0.4659344426       -0.1177787426
         C         -1.4255171488       -0.5796126398       -0.2987743324
         H          1.2927227489        1.6467822241       -1.7161034319
         H          0.5115436171        0.1136959367       -1.4299162738
         H         -0.9626535770        2.1244807715       -1.4395462399
         H         -0.2313854006        2.6631384020        0.0636669169
         H          2.4836824282        1.6401220787        0.2631946203
         H          1.3379895832        0.6415415675        1.1385387246
         H         -2.5044279955        1.3042479639        0.0511624497
         H          3.6220420553       -0.4105315490        0.3870533340
         H          2.8346807725       -0.5412209492       -1.1903941633
         H         -0.5537202797        1.5469435382        2.0029686770
         H         -2.0510266978        0.8448716653        2.2497725116
         H          0.8939724574       -1.6086997720        0.2334441992
         H          2.2630899289       -2.5235198565       -0.1606712473
         H          2.1155048181       -1.8450780227        1.3513380594
         H         -0.7798223195       -0.1709536166        1.7101046448
          25
                -34.15043307
         O         -3.3078583007       -0.3518099727        0.5480566241
         O         -1.8441149941       -2.0073969491        0.1397595703
         N         -1.9672328473        1.5403727742       -0.2372000800
         N          3.6044925126       -0.8358440764       -0.6659647320
         C          1.1684565146        1.0446332415       -0.4943940738
         C          0.1082467096        0.2177955398        0.2395309885
         C          2.4785715426        1.1657908669        0.2896724500
         C         -1.2458520269        0.2676289576       -0.4513144135
         C          3.1696429470       -0.1592315605        0.5890169973
         C         -2.2263428078       -0.8414226742        0.1422529421
         H          0.7932826990        2.0569151989       -0.6482792542
         H          1.3354698371        0.6191303209       -1.4843783659
         H         -0.0031461344        0.5661490514        1.2686433794
         H          0.3791631733       -0.8378892566        0.2841197320
         H          3.1579098277        1.8188328559       -0.2597110183
         H          2.2713923792        1.6478155935        1.2450536588
         H         -1.1432114953        0.0569418065       -1.5170262136
         H          2.5004236326       -0.8240708177        1.1341470919
         H          4.0493855213        0.0280822082        1.2049984895
         H         -1.5070190112        2.1488324010        0.4454597884
         H         -2.1837208522        2.0584218679       -1.0901715209
         H          4.2094106392       -0.2068158824       -1.2153183037
         H          4.1256865653       -1.6973771969       -0.4514692989
         H          2.7834959501       -1.0854874671       -1.2358687369
         H         -2.8845837684        1.1083956166        0.2016053653


Restarting calculations
"""""""""""""""""""""""

CENSO keeps track of all performed calculations, if they succeed, fail or are 
pending. The data is stored in enso.json and enables restarting of CENSO runs. 
Restarting a calculation is useful if you only considered few conformers in the 
first run and want to increase the number of investigated conformers (e.g., start 
from 30 and go to 100 conformers). Or if after a previous calculation of a free ensemble 
energy, a property, e.g. OR, is to be calculated. This can be done by first 
calculating the free energy, checking the results and the final ensemble and then 
restart to calculate the property (OR). Some restart limitations are given though: 
You can not change anything that concerns the geometry optimization, since all 
results have to be created with the same method-combination.

When restarting CENSO, files with information of the previous calculation 
(enso.json, enso_ensemble\_partx.xyz, ...)  are automatically backed up.

Restarting a calculation is performed by calling censo and adjusting the new settings 
by command line:

.. code:: sh

    # increase the number of considered conformers to 200
    $ censo -restart -nc 200 > censo-restart.out &

    # or calculate OR after previous free energy calculation
    $ censo -restart -OR on > censo-restart-OR.out &

