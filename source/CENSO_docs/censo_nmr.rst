.. _nmr:


NMR
==========================

.. contents::




For the calculation of NMR spectra with ``CENSO``, the ``anmr`` program is needed.
The spectra can be plotted with the python script ``nrmplot.py``. Both programs can be obtained from the latest release page of ENSO:
https://github.com/grimme-lab/enso/releases/tag/v.2.0.2

ANMR
""""

``ANMR`` requires the files:




* *coord*, turbomole data group
* anmr_nucinfo (human readable), written by `crest`
  
  * information on which nuclei can interchange in your molecule (e.g. the hydrogen 
    atoms in a methlygroup)
  * = information on chemical and magnetical equivalent atoms
* anmr_rotamer (machine readable), written by `crest`
  
  * information on the rotamers detected by `crest`
* anmr_enso, written by `censo`
  
  * information on the contributing conformers, the Boltzmann weight and all 
    contributions to free energy
* .anmrrc, written by `censo`

  * `anmr` searches for the *.anmrrc* file first in the folder of execution and 
    if not found in the home directory of the user
  * The file contains the reference-shieldings of e.g TMS to convert the calculated 
    shielding to shifts
* the folders with the property calculations of the conformers

  * e.g. CONF1/NMR


Example **.anmrrc** file:

.. code::

   7 8 XH acid atoms
   ENSO qm= TM mf= 300 lw= 1.0  J= on S= on
   TMS[chcl3] pbe0[COSMO]/def2-TZVP//pbeh-3c[DCOSMO-RS]/def2-mSVP
   1  31.786    0.0    1
   6  189.674   0.0    0
   9  182.57    0.0    0
   15 291.9     0.0    0

The first line in .anmrrc informs `anmr` to ignore all protons bound to nitrogen 
(N=7) and oxygen (O=8). If you want to calculate protons bound to oxygen or nitrogen,
simply remove the corresponding number, but leave the rest of the line intact!
The next line starting with *ENSO* informs ``ANMR`` that the property calculation 
was performed by *TM* = TURBOMOLE (or *ORCA* = ORCA). The *mf=* 300 informs ``ANMR`` 
that the magnetic frequency of the NMR spectrometer is set to 300 MHz. The *lw* 
(linewidth for plotting) is 1.0 and J (couplings) and S (shieldings) are to be evaluated. 
If *S=* off then ``ANMR`` will terminate after calculating and averaging the shifts of the 
molecule under consideration. The next line explains how the reference shieldings are 
calculated: in this case the reference molecule is tetramethylsilane in chloroform and the 
shielding is calculated with PBE0/def2-TZVP + COSMO on PBEh-3c + DCOSMO-RS geometries. 

The following lines contain the data on **[atomic number]** **[calculated shielding valule 
of the reference molecule]** **[experimental shift]** **[active or not]**.

The lines show the reference shieldings for hydrogen (1), carbon (6) fluor (9) and 
phosphorus (15). The third number within the last four lines is 0.0 and can be used to adjust 
the shift of the reference (e.g. to the experimental shift).
The last number in the last four lines can either be 1 or 0 and this 
switches the 'element on or off' for the spectrum calculation.

Example **anmr_enso** file:

.. code::

   ONOFF NMR CONF BW      Energy     Gsolv    RRHO
   1     1   1    0.10042 -354.38939 -0.00899 0.22109
   1     2   2    0.32452 -354.39034 -0.00899 0.22093
   1     3   3    0.57506 -354.39287 -0.00902 0.22295

The file *anmr_enso* is written by the `censo` program and contains information on 
the conformers, which folder they are in, the Boltzmann weight, energy, solvation 
and thermostatistical contribution to free energy. The first number in the three last 
lines indicates to ``ANMR`` if the conformer is to be considered (1) or not (0). 
If one conformer is not considered (or more) the `anmr` program internally recalculates
the Boltzmann weights based on the free energies from the *anmr_enso* file. 


Usage of `anmr`:

.. important::

    `ANMR` still relies on autmatically created arrays on the stack. For this reason you have to run ``ulimit -s unlimited`` to prevent stackoverflows.


.. tabbed:: command
  
  .. code:: sh
  
        $ anmr --help
        # explanation of all possible command line arguments
        # shown in next tab
        
        
.. tabbed:: keywords

    .. code:: none
    
                +--------------------------------------+
                |              A N M R                 |
                |             S. Grimme                |
                |      Universitaet Bonn, MCTC         |
                |             1989-2019                |
                |            version 3.5.1             |
                |     Sat Feb  9 06:41:57 CET 2019     |
                +--------------------------------------+
                Based on a TurboPascal program written  
                in 1989 which was translated to F90 in  
                2005 and re-activated in 2017.          
                Please cite work employing this code as:
                ANMR Ver. 3.5: An automatic, QC based
                coupled NMR spectra simulation program.
                S. Grimme, Universitaet Bonn, 2019
                S. Grimme, C. Bannwarth, S. Dohm, A. Hansen
                J. Pisarek, P. Pracht, J. Seibert, F. Neese
                Angew. Chem. Int. Ed. 2017, 56, 14763-14769.
                DOI:10.1002/anie.201708266               
            
            
            =============================
                # OMP threads =           4
            =============================
            usage        :
            anmr [options]
            General options:
            
                -tm         : use TURBOMOLE J/sigma
                -orca       : use ORCA      J/sigma
                -adf        : use ADF       J/sigma
                -gauss      : use GAUSSIAN  J/sigma
                -plain      : use plain input for J/sigma
                -chk        : perform input check 
                -acid       : remove acidic XH protons 
                -nofrag     : no fragmentation 
                -mfrag      : fragmentation type mol 
                -afrag      : fragmentation type at 
                -mss        : maxsspin 
                -fragss     : fragmentation scheme 
                -mf         : magnetic frequency of exp. 
                -lw         : line width of generated spectrum
                -ascal      : chemical shift scaling a
                -bscal      : chemical shift scaling b
                -cscal      : spin-spin coupling scal factor
                -nc         : number of conformers
                -poff       : plot offset
                -r          : range min max [-r <real1> <real2]
                -pthr       : min population for which NMR data are read
                -nl         : points for lorentzian for plotting
                -onlyshifts : stop after shift averaging
                -h          : print help


.. note:: 
    
    The usage of the ``-plain`` option is recommended so that the coupling constants are read from the CONFXX/NMR/nmrprop.dat
    file written by ``CENSO`` instead of the output files of the used QM program package, whose formatting
    often changes with new versions.


First of all: the spin problem is of :math:`2^{N}` complexity! Depending on the 
size of the maximalspinsystem (*mss*) the program might use a lot of RAM! 
If this is the case, run `anmr` with a decreased spinsystem size:


.. code:: sh

  $ anmr -mss 12 -plain > anmr.out 2> anmr.error &


`anmr` will then write a file called *anmr.dat* (which is quiet large). The file
contains the information ppm vs intesity. This file can then be plotted with any 
plotting tool or our 'nmrplot.py'.

To reduce the large size of the file you can remove entries which are close to 
zero with either this awk or python code:

.. code-block:: sh

    head -1 anmr.dat > newanmr.dat
    awk '($2 > 0.001){print $0}' anmr.dat >> newanmr.dat
    tail -1 anmr.dat >> newanmr.dat

.. code-block:: python3

    import numpy as np 
    data = np.genfromtxt('anmr.dat')
    threshold = 0.001
    data2 = data[np.logical_not(data[:,1] < threshold)]
    data2 = np.insert(data2, 0, (data[0][0], threshold), axis=0)
    data2 = np.insert(data2, len(data2), (data[-1][0], threshold), axis=0)
    np.savetxt('newanmr.dat', data2, fmt='%2.5e' )
    
    
Spectra Plotting
""""""""""""""""

The NMR spectrum can be plotted from the file `anmr.dat`. It contains the 
information ppm vs intensity and can be plotted with any plotting tool 
(e.g GNUPLOT ...).

The provided `nmrplot.py` plotting tool uses `matplotlib` for plotting. 
Information on all possible commandline arguments is documented:

.. code-block:: text

	$ nmrplot.py --help

	     __________________________________________________
	    |                                                  |
	    |                    NMRPLOT                       |
	    |          Plotting of NMR spectral data           |
	    |             University of Bonn, MCTC             |
	    |                 January 2019                     |
	    |                     v 1.05                       |
	    |                   F. Bohle                       |
	    |__________________________________________________|

	Information on arguments:

	     End     Endremove    Startremove                 Start
	    +               +    +                               +
	    +---------------+----+-------------------------------+
	    lower field                               higher field
	                        delta /ppm
	    
	optional arguments:
	  -h, --help            show this help message and exit
	  -start START, --startppm START
	                        Start plotting from '<start>' ppm. (default: 0)
	  -end END, --endppm END
	                        End plotting at '<end>' ppm. Value of end has to be
	                        larger than value of start. (default: 11)
	  -startremove STARTREMOVE, --startremove STARTREMOVE
	                        Start cutting from spectrum at '<startremove>' ppm.
	                        (default: None)
	  -endremove ENDREMOVE, --endremove ENDREMOVE
	                        End cutting from spectrum at '<endremove>' ppm. Value
	                        of endremove has to be larger than value of
	                        startremove. (default: None)
	  -title TITLE, --title TITLE
	                        Set title of entire plot. If no title is required use
	                        '<--title ''>'. (default: NMR-PLOT)
	  -lw LINEWIDTH, --linewidth LINEWIDTH
	                        Set linewidth. (default: 0.8)
	  -i FILE [FILE ...], --input FILE [FILE ...]
	                        Provide input_file(s) [max 3 files] -i input1(theory1)
	                        input2(theory2) input3(experiment/predicition);
	                        inputfiles format is two columns: column1 ppm ,
	                        column2 intensity; if several files are provided the
	                        last one will be inverted (default: None)
	  -l LABEL [LABEL ...], --label LABEL [LABEL ...]
	                        Provide labels for all files provided [max 3 files] -l
	                        label1 label2 label3, if no labels are provided,
	                        filename is used as label (default: [])
	  -fontsize FONTSIZE, --fontsize FONTSIZE
	                        Set fontsize for entire plot. (default: 15)
	  -keybox, --keybox     Set Frame around key. (default: False)
	  -ontop, --ontop       Plot all spectra ontop of each other. (default: False)
	  -stacked, --stacked   Plot all spectra stacked over each other. (default:
	                        False)
	  -orientation ORIENTATION [ORIENTATION ...], --orientation ORIENTATION [ORIENTATION ...]
	                        Up (1) or down (-1). (default: [1, 1, 1, 1, 1, 1, 1,
	                        1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1])
	  -c  [ ...], --colors  [ ...]
	                        Select colors. Possible are: ['gray', 'blue', 'cyan',
	                        'red', 'green', 'magenta', 'yellow', 'black']
	                        (default: ['blue', 'black', 'red', 'magenta',
	                        'green'])
	  -cut CUT [CUT ...], --cut CUT [CUT ...]
	                        Cut intensity. Accepts values from 0.0 (flat line) to
	                        1.0 (full intensity). (default: [1.0, 1.0, 1.0, 1.0,
	                        1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0])
	  -o OUT, --output OUT  Provide name of the output file without fileending.
	                        (default: nmrplot)
	  -s SHIFT [SHIFT ...], --shift SHIFT [SHIFT ...]
	                        Shift ppm of each inputfile separately using: --shift
	                        float float float, e.g. --shift 10.0 0.0 -5.0, each
	                        file needs its own value (default: [])


Reference shielding constants
"""""""""""""""""""""""""""""

For user convenience shielding constants of the reference molecules (TMS (Tetramethylsilane), CFCl3, PH3, TMP 
(Trimethylphosphine oxide)) were precalculated (for some method combinations) and stored within the `CENSO` program. 
The reference shielding values are used in the `ANMR`
program to calculate the shifts and the reference values are written to the file *.anmrrc*.

To be consistent with your calculation, the reference shielding values were calculated on the
reference molecules with many possible geometry-optimization-settings eg. {TURBOMOLE/ORCA, PBEh-3c /
TPSS-D3/def2-TZVP / B97-3c, (gas phase or solvent)}. The shieldings were then calculated either with
TPSS or PBE0 and depending on ORCA (gas or SMD and def2-TZVP basis set) or TURBOMOLE (gas or
DCOSMO-RS with the def2-TZVP basis set). At the end of part4 the file *.anmrrc* is written into the
calculation folder and stores the reference shielding values of your settings for the subsequent
*ANMR* calculation.

.. note:: The `CENSO` program only writes the reference shielding values to the file '.anmrrc' but 
      does not do anything with it. Hence, no results of `CENSO` are influenced 
      by a non-matching reference value. If you want to change the reference shielding values, 
      you can simply modify the file '.anmrrc' manually before calling the `ANMR` program. 

Procedure for generating the refrence shielding constants:
Geometry optimization with the respective reference molecule with PBEh-3c/B97-3c/TPSS-D3/def2-TZVP + implicit solvation model 
(either SMD or DCOSMO-RS). NMR shielding constant calculation with the respective functional and the def2-TZVP basis set 
(again with implicit solvation model).

Input structures for the respective reference molecules:

.. tabbed:: Tetramethylsilane:

    .. code:: bash

        $ cat coord
        $coord
        2.05833045453195     -2.05833045453195      2.05833045453195  c
        3.27901073396930     -3.27901073396930      0.93023223253204  h
        3.27901073396930     -0.93023223253204      3.27901073396930  h
        0.93023223253204     -3.27901073396930      3.27901073396930  h
        -0.00000000000000      0.00000000000000      0.00000000000000  si 
        -2.05833045453195      2.05833045453195      2.05833045453195  c
        -3.27901073396930      3.27901073396930      0.93023223253204  h
        -0.93023223253204      3.27901073396930      3.27901073396930  h
        -3.27901073396930      0.93023223253204      3.27901073396930  h
        2.05833045453195      2.05833045453195     -2.05833045453195  c
        0.93023223253204      3.27901073396930     -3.27901073396930  h
        3.27901073396930      0.93023223253204     -3.27901073396930  h
        3.27901073396930      3.27901073396930     -0.93023223253204  h
        -2.05833045453195     -2.05833045453195     -2.05833045453195  c
        -3.27901073396930     -3.27901073396930     -0.93023223253204  h
        -3.27901073396930     -0.93023223253204     -3.27901073396930  h
        -0.93023223253204     -3.27901073396930     -3.27901073396930  h
        $end

.. tabbed:: PH3:

    .. code:: bash

        $ cat coord
        $coord
        0.00000000000000      0.00000000000000      1.08780842165939  p
        1.12108786201329      1.94178113675579     -0.36261095596909  h
        1.12108786201329     -1.94178113675579     -0.36261095596909  h
        -2.24217572402658      0.00000000000000     -0.36261095596909  h
        $end

.. tabbed:: TMP = Trimethylphosphine oxide:

    .. code:: bash

        $ cat coord
        $coord
        2.10707881159693     -2.37905657209703     -0.95048934768032       c
        -0.00002761513490     -0.00001720463363      0.42981024146152       p
        0.00022116674358     -0.00003978704989      3.20441724940919       o
        -3.11402725504898     -0.63518697865997     -0.95026063129186       c
        -4.41578089847492      0.80223353974588     -0.26675109605744       h
        -3.74806612133726     -2.46831651344230     -0.26795802048584       h
        -3.07053848205114     -0.62555829073221     -3.00039235368914       h
        1.00685206250598      3.01430306976026     -0.95039040993479       c
        2.90134987179607      3.42432987586201     -0.26440712265899       h
        -0.26551500181645      4.47957166601373     -0.27057128439357       h
        0.99633316768277      2.97084963842055     -3.00047015163533       h
        4.01209383139734     -2.01044112204817     -0.27010522766248       h
        1.51433033394466     -4.22477273833643     -0.26505344320048       h
        2.07522150306901     -2.34774660838157     -3.00060121737073       h
        $end

.. tabbed:: CFCl3:

    .. code:: bash

        $ cat coord
        $coord
        0.00000038126763   -0.00000000884504    0.13419916242803      c 
        0.00000870296281    0.00000001369727    2.66116007348966      f 
        3.17274491422955   -0.00000000906271   -0.93176725824334      cl
        -1.58637567202181   -2.74767202581384   -0.93179226251812      cl
        -1.58637568491745    2.74767203002431   -0.93179224376158      cl
        $end
        

Example calculation for reference shielding constant
-----------------------------------------------------


In this usage example, ``CENSO`` printed an error-message that the reference absolute shielding constant at the level of
theory chosen is missing for hydrogen and has not been precalculated.

.. code:: none

    ERROR:       The reference absolute shielding constant for element h could not be found!          
                 You have to edit the file .anmrrc by hand!
                 


To calculate it, a NMR calculation at the respective level of theory
has to be performed for TMS in a new directory. In this case, the theory level is PBE0/def2-TZVP for the NMR part on
r2SCAN-3c geometries with the implicit SMD solvation model for CHCl3 (PBE0[SMD]/def2-TZVP//r2scan-3c[SMD]/def2-mTZVPP).

.. code:: sh

    $ mkdir tms 
    $ cd tms
    $ cat coord 
    $coord 
     2.05833045453195     -2.05833045453195      2.05833045453195  c 
     3.27901073396930     -3.27901073396930      0.93023223253204  h 
     3.27901073396930     -0.93023223253204      3.27901073396930  h 
     0.93023223253204     -3.27901073396930      3.27901073396930  h 
    -0.00000000000000      0.00000000000000      0.00000000000000  si  
    -2.05833045453195      2.05833045453195      2.05833045453195  c 
    -3.27901073396930      3.27901073396930      0.93023223253204  h 
    -0.93023223253204      3.27901073396930      3.27901073396930  h 
    -3.27901073396930      0.93023223253204      3.27901073396930  h 
     2.05833045453195      2.05833045453195     -2.05833045453195  c 
     0.93023223253204      3.27901073396930     -3.27901073396930  h 
     3.27901073396930      0.93023223253204     -3.27901073396930  h 
     3.27901073396930      3.27901073396930     -0.93023223253204  h 
    -2.05833045453195     -2.05833045453195     -2.05833045453195  c 
    -3.27901073396930     -3.27901073396930     -0.93023223253204  h 
    -3.27901073396930     -0.93023223253204     -3.27901073396930  h 
    -0.93023223253204     -3.27901073396930     -3.27901073396930  h 
    $end
    
.. code:: bash    
    
    $ crest coord -gfn2 -alpb chcl3 -T 4 -nmr > crest.out              
    $ mkdir censo    
    $ cp crest_conformers.xyz coord anmr_nucinfo anmr_rotamer censo/    
    $ cd censo/    
    
.. tabbed:: input

    .. code-block:: bash    
        
       $ censo --input crest_conformers.xyz -func0 b97-d3 -solvent chcl3  -smgsolv1 smd -sm2 smd
               --smgsolv2 smd --prog orca -part4 on  -prog4J orca -prog4S orca -funcJ pbe0 
                -funcS pbe0 -basisJ def2-TZVP -basisS def2-TZVP -cactive off > censo.out   
        
.. tabbed:: global censorc file   
    
        .. code:: sh
            

            $CENSO global configuration file: .censorc
            $VERSION:1.1.2 

            ORCA: /home/$USER/orca_5_0_1_linux_x86-64_openmpi411
            ORCA version: 5.0.1 
            GFN-xTB: /home/$USER/bin/xtb
            CREST: /home/$USER/bin/crest
            mpshift: /home/$USER/TURBOMOLE.7.5/bin/em64t-unknown-linux-gnu/mpshift
            escf: /home/$USER/TURBOMOLE.7.5/bin/em64t-unknown-linux-gnu/escf

            #COSMO-RS
            ctd = BP_TZVP_C30_1601.ctd cdir = "/home/$USER/COSMOthermX16/COSMOtherm/CTDATA-FILES" ldir = "/home/$USER/COSMOthermX16/COSMOtherm/CTDATA-FILES"
            $ENDPROGRAMS

            $CRE SORTING SETTINGS:
            $GENERAL SETTINGS:
            nconf: all                       # ['all', 'number e.g. 10 up to all conformers'] 
            charge: 0                        # ['number e.g. 0'] 
            unpaired: 0                      # ['number e.g. 0'] 
            solvent: gas                     # ['gas', 'acetone', 'acetonitrile', 'aniline', 'benzaldehyde', 'benzene', 'ccl4', '...'] 
            prog_rrho: xtb                   # ['xtb'] 
            temperature: 298.15              # ['temperature in K e.g. 298.15'] 
            trange: [273.15, 378.15, 5]      # ['temperature range [start, end, step]'] 
            multitemp: on                    # ['on', 'off'] 
            evaluate_rrho: on                # ['on', 'off'] 
            consider_sym: on                 # ['on', 'off'] 
            bhess: on                        # ['on', 'off'] 
            imagthr: automatic               # ['automatic or e.g., -100    # in cm-1'] 
            sthr: automatic                  # ['automatic or e.g., 50     # in cm-1'] 
            scale: automatic                 # ['automatic or e.g., 1.0 '] 
            rmsdbias: off                    # ['on', 'off'] 
            sm_rrho: alpb                    # ['alpb', 'gbsa'] 
            progress: off                    # possibilities 
            check: on                        # ['on', 'off'] 
            prog: tm                         # ['tm', 'orca'] 
            func: r2scan-3c                  # ['b3-lyp', 'b3lyp', 'b3lyp-3c', 'b3lyp-d3', 'b3lyp-d3(0)', 'b3lyp-d4', 'b3lyp-nl', '...'] 
            basis: automatic                 # ['automatic', 'def2-TZVP', 'def2-mSVP', 'def2-mSVP', 'def2-mSVP', 'def2-mSVP', '...'] 
            maxthreads: 7                    # ['number of threads e.g. 2'] 
            omp: 4                           # ['number cores per thread e.g. 4'] 
            balance: off                     # possibilities 
            cosmorsparam: automatic          # ['automatic', '12-fine', '12-normal', '13-fine', '13-normal', '14-fine', '...'] 

            $PART0 - CHEAP-PRESCREENING - SETTINGS:
            part0: on                        # ['on', 'off'] 
            func0: b97-d                     # ['b3-lyp', 'b3lyp', 'b3lyp-3c', 'b3lyp-d3', 'b3lyp-d3(0)', 'b3lyp-d4', '...'] 
            basis0: def2-SV(P)               # ['automatic', 'def2-SV(P)', 'def2-TZVP', 'def2-mSVP', 'def2-mSVP', 'def2-mSVP', '...'] 
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
            func3: pw6b95                    # ['b3-lyp', 'b3lyp', 'b3lyp-3c', 'b3lyp-d3', 'b3lyp-d3(0)', 'b3lyp-d4', 'b3lyp-nl', '...'] 
            basis3: def2-TZVPD               # ['DZ', 'QZV', 'QZVP', 'QZVPP', 'SV(P)', 'SVP', 'TZVP', 'TZVPP', 'aug-cc-pV5Z', '...'] 
            smgsolv3: cosmors                # ['alpb_gsolv', 'cosmo', 'cosmors', 'cosmors-fine', 'cpcm', 'dcosmors', '...'] 
            part3_gfnv: gfn2                 # ['gfn1', 'gfn2', 'gfnff'] 
            part3_threshold: 99              # ['Boltzmann sum threshold in %. e.g. 95 (between 1 and 100)'] 

            $NMR PROPERTY SETTINGS:
            $PART4 SETTINGS:
            part4: off                       # ['on', 'off'] 
            couplings: on                    # ['on', 'off'] 
            progJ: prog                      # ['tm', 'orca', 'adf', 'prog'] 
            funcJ: PBE0                      # ['b3-lyp', 'b3lyp', 'b3lyp-3c', 'b3lyp-d3', 'b3lyp-d3(0)', 'b3lyp-d4', 'b3lyp-nl', '...'] 
            basisJ: def2-TZVP                # ['DZ', 'QZV', 'QZVP', 'QZVPP', 'SV(P)', 'SVP', 'TZVP', 'TZVPP', 'aug-cc-pV5Z', '...'] 
            sm4J: default                    # ['cosmo', 'cpcm', 'dcosmors', 'smd'] 
            shieldings: on                   # ['on', 'off'] 
            progS: prog                      # ['tm', 'orca', 'adf', 'prog'] 
            funcS: PBE0                      # ['b3-lyp', 'b3lyp', 'b3lyp-3c', 'b3lyp-d3', 'b3lyp-d3(0)', 'b3lyp-d4', 'b3lyp-nl', '...'] 
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
            
        
.. tabbed:: output
                            
    .. code:: none
                                
                                              
                        
                                 ______________________________________________________________
                                |                                                              |
                                |                                                              |
                                |                   CENSO - Commandline ENSO                   |
                                |                           v 1.1.2                            |
                                |    energetic sorting of CREST Conformer Rotamer Ensembles    |
                                |                    University of Bonn, MCTC                  |
                                |                           Feb 2021                           |
                                |                 based on ENSO version 2.0.1                  |
                                |                     F. Bohle and S. Grimme                   |
                                |                                                              |
                                |______________________________________________________________|
                        
                                Please cite: 
                                S. Grimme, F. Bohle, A. Hansen, P. Pracht, S. Spicher, and M. Stahn 
                                J. Phys. Chem. A 2021, 125, 19, 4039-4054.
                                DOI: https://doi.org/10.1021/acs.jpca.1c00971
                                
                                This program is distributed in the hope that it will be useful,
                                but WITHOUT ANY WARRANTY; without even the implied warranty of
                                MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
                        
                        
                        ----------------------------------------------------------------------------------------------------
                                                                        PARAMETERS                                             
                        ----------------------------------------------------------------------------------------------------
                        
                        program call: censo --input crest_conformers.xyz -func0 b97-d3 -solvent chcl3 -smgsolv1 smd -sm2 smd --smgsolv2 smd --prog orca -part4 on -prog4J orca -prog4S orca -funcJ pbe0 -funcS pbe0 -basisJ def2-TZVP -basisS def2-TZVP
                        The configuration file .censorc is read from /home/$USER/.censorc.
                        Reading conformer rotamer ensemble from: /tmp1/$USER/3881229.majestix.thch.uni-bonn.de/crest_conformers.xyz.
                        Reading file: censo_solvents.json
                        
                        
                        --------------------------------------------------
                                        CRE SORTING SETTINGS               
                        --------------------------------------------------
                        
                        number of atoms in system:                                     17
                        number of considered conformers:                               2
                        number of all conformers from input:                           2
                        charge:                                                        0
                        unpaired:                                                      0
                        solvent:                                                       chcl3
                        temperature:                                                   298.15
                        evaluate at different temperatures:                            on
                        temperature range:                                             273.15, 278.15, 283.15, 288.15, ...
                        calculate mRRHO contribution:                                  on
                        consider symmetry for mRRHO contribution:                      on
                        cautious checking for error and failed calculations:           on
                        checking the DFT-ensemble using CREST:                         off
                        maxthreads:                                                    7
                        omp:                                                           4
                        automatically balance maxthreads and omp:                      off
                        
                        --------------------------------------------------
                                    CRE CHEAP-PRESCREENING - PART0          
                        --------------------------------------------------
                        part0:                                                         on
                        starting number of considered conformers:                      2
                        program for part0:                                             orca
                        functional for fast single-point:                              b97-d3
                        basis set for fast single-point:                               def2-SV(P)
                        threshold g_thr(0) for sorting in part0:                       4.0
                        Solvent model used with xTB:                                   alpb
                        
                        short-notation:
                        b97-d3/def2-SV(P) // GFNn-xTB (Input geometry)
                        
                        --------------------------------------------------
                                        CRE PRESCREENING - PART1             
                        --------------------------------------------------
                        part1:                                                         on
                        program for part1:                                             orca
                        functional for initial evaluation:                             r2scan-3c
                        basis set for initial evaluation:                              def2-mTZVPP
                        calculate mRRHO contribution:                                  on
                        program for mRRHO contribution:                                xtb
                        GFN version for mRRHO and/or GBSA_Gsolv:                       gfn2
                        Apply constraint to input geometry during mRRHO calculation:   on
                        solvent model applied with xTB:                                alpb
                        evaluate at different temperatures:                            off
                        threshold g_thr(1) and G_thr(1) for sorting in part1:          3.5
                        solvent model for Gsolv contribution of part1:                 smd
                        
                        short-notation:
                        r2scan-3c + SMD[chcl3] + GmRRHO(GFN2[alpb]-bhess) // GFNn-xTB (Input geometry)
                        
                        --------------------------------------------------
                                        CRE OPTIMIZATION - PART2             
                        --------------------------------------------------
                        part2:                                                         on
                        program:                                                       orca
                        functional for part2:                                          r2scan-3c
                        basis set for part2:                                           def2-mTZVPP
                        using xTB-optimizer for optimization:                          on
                        using the new ensemble optimizer:                              on
                        optimize all conformers below this G_thr(opt,2) threshold:     2.5
                        spearmanthr:                                                   0.941
                        optimization level in part2:                                   lax
                        solvent model applied in the optimization:                     smd
                        solvent model for Gsolv contribution:                          smd
                        evaluate at different temperatures:                            on
                        Boltzmann sum threshold G_thr(2) for sorting in part2:         99.0
                        calculate mRRHO contribution:                                  on
                        program for mRRHO contribution:                                xtb
                        GFN version for mRRHO and/or GBSA_Gsolv:                       gfn2
                        Apply constraint to input geometry during mRRHO calculation:   on
                        solvent model applied with xTB:                                alpb
                        
                        short-notation:
                        r2scan-3c + SMD[chcl3] + GmRRHO(GFN2[alpb]-bhess) // r2scan-3c[SMD] 
                        
                        --------------------------------------------------
                                            NMR MODE SETTINGS                
                        --------------------------------------------------
                        part4:                                                         on
                        calculate couplings (J):                                       on
                        program for coupling calculations:                             orca
                        solvation model for coupling calculations:                     smd
                        functional for coupling calculation:                           PBE0
                        basis set for coupling calculation:                            def2-TZVP
                        
                        calculate shieldings (S):                                      on
                        program for shielding calculations:                            orca
                        solvation model for shielding calculations:                    smd
                        functional for shielding calculation:                          PBE0
                        basis set for shielding calculation:                           def2-TZVP
                        
                        Calculating proton spectrum:                                   on
                        reference for 1H:                                              TMS
                        resonance frequency:                                           300.0
                        END of parameters
                        
                        
                        ------------------------------------------------------------
                                        PATHS of external QM programs                
                        ------------------------------------------------------------
                        
                        The following program paths are used:
                            ORCA:         /tmp1/orca_5_0_1_linux_x86-64_openmpi411
                            ORCA Version: 5.01
                            xTB:          /home/abt-grimme/AK-bin/xtb
                            TURBOMOLE:    /home/abt-grimme/TURBOMOLE.7.5//bin/em64t-unknown-linux-gnu_smp
                        
                            Using cefine from /tmp/_MEIaCcz3S/cefine
                            PARNODES for TM or COSMO-RS calculation was set to 4
                        
                        ----------------------------------------------------------------------------------------------------
                                                    Processing data from previous run (enso.json)                           
                        ----------------------------------------------------------------------------------------------------

                        INFORMATION: No restart information exists and is created during this run!
                        
                        
                        ----------------------------------------------------------------------------------------------------
                                                            CRE CHEAP-PRESCREENING - PART0                                   
                        ----------------------------------------------------------------------------------------------------
                        
                        program:                                                       orca
                        functional for part0:                                          b97-d3
                        basis set for part0:                                           def2-SV(P)
                        threshold g_thr(0):                                            4.0
                        starting number of considered conformers:                      2
                        temperature:                                                   298.15
                        
                        Calculating efficient gas-phase single-point energies:
                        The efficient gas-phase single-point is calculated for:
                        CONF1, CONF2

                        Constructed folders!
                        
                        Starting 2 ALPB-Gsolv calculations
                        Running single-point in CONF1/part0_sp
                        Running single-point in CONF2/part0_sp
                        Running ALPB_GSOLV calculation in 3881229.majestix.thch.uni-bonn.de/CONF2/part0_sp
                        Running ALPB_GSOLV calculation in 3881229.majestix.thch.uni-bonn.de/CONF1/part0_sp
                        Tasks completed!
                        
                        The efficient gas-phase single-point was successful for CONF1/part0_sp: E(DFT) = -448.78335711 Gsolv = -0.00964593
                        The efficient gas-phase single-point was successful for CONF2/part0_sp: E(DFT) = -448.78106942 Gsolv = -0.00949351
                        
                        ----------------------------------------------------------------------------------------------------
                                            Removing high lying conformers by improved energy description                    
                        ----------------------------------------------------------------------------------------------------
                        
                        CONF#       E [Eh] ΔE [kcal/mol]            E [Eh]   Gsolv [Eh]         gtot    ΔE(DFT)     ΔGsolv      Δgtot
                                    GFN2-xTB      GFN2-xTB b97-d3/def2-SV(P)         alpb         [Eh] [kcal/mol] [kcal/mol] [kcal/mol]
                                    [alpb]        [alpb]                         [gfn2]                                              
                        CONF1  -16.3966231          0.00      -448.7833571   -0.0096459 -448.7930030       0.00       0.00       0.00     <------
                        CONF2  -16.3954819          0.72      -448.7810694   -0.0094935 -448.7905629       1.44       0.10       1.53
                        ----------------------------------------------------------------------------------------------------
                        
                        Number of conformers observed within the following Δg windows:
                        Δg [kcal/mol]  #CONF   sum(Boltzmann_weights)
                        ---------------------------------------------
                            0 - 0.5        1          0.93
                            0 - 1.0        1          0.93
                            0 - 1.5        1          0.93
                            0 - 2.0        2          1.00
                        ---------------------------------------------
                        
                        All relative (free) energies are below the initial g_thr(0) threshold of 4.0 kcal/mol.
                        All conformers are considered further.

                        Calculating Boltzmann averaged (free) energy of ensemble on input geometries (not DFT optimized)!
                        
                        temperature /K:   avE(T) /a.u.   avG(T) /a.u. 
                        ----------------------------------------------------------------------------------------------------
                            298.15        -448.7831966    -448.7928319     <<==part0==
                        ----------------------------------------------------------------------------------------------------
                        
                        
                        >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>END of Part0<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<
                        Ran part0 in 4.4449 seconds
                        
                        ----------------------------------------------------------------------------------------------------
                                                                CRE PRESCREENING - PART1                                      
                        ----------------------------------------------------------------------------------------------------
                        
                        program:                                                       orca
                        functional for part1 and 2:                                    r2scan-3c
                        basis set for part1 and 2:                                     def2-mTZVPP
                        Solvent:                                                       chcl3
                        solvent model for Gsolv contribution:                          smd
                        threshold g_thr(1) and G_thr(1):                               3.5
                        starting number of considered conformers:                      2
                        calculate mRRHO contribution:                                  on
                        program for mRRHO contribution:                                xtb
                        GFN version for mRRHO and/or GBSA_Gsolv:                       gfn2
                        Apply constraint to input geometry during mRRHO calculation:   on
                        temperature:                                                   298.15

                        Calculating single-point energies and solvation contribution (G_solv):
                        The prescreening_single-point is calculated for:
                        CONF1, CONF2
                        
                        Constructed folders!
                        Running single-point in CONF1/r2scan-3c
                        Running single-point in CONF2/r2scan-3c
                        Tasks completed!
                        
                        prescreening_single-point calculation was successful for CONF1/r2scan-3c: -449.11349203
                        prescreening_single-point calculation was successful for CONF2/r2scan-3c: -449.11143980
                        
                        --------------------------------------------------
                                    Removing high lying conformers          
                        --------------------------------------------------
                        
                        CONF#  E(GFNn-xTB) ΔE(GFNn-xTB)       E [Eh]   Gsolv [Eh]         gtot      Δgtot
                                    [a.u.]   [kcal/mol]    r2scan-3c   incl. in E         [Eh] [kcal/mol]
                                                                [SMD]                                     
                        CONF1  -16.3952414         0.00 -449.1134920    0.0000000 -449.1134920       0.00     <------
                        CONF2  -16.3940994         0.72 -449.1114398    0.0000000 -449.1114398       1.29
                        
                        All relative (free) energies are below the g_thr(1) threshold of 3.5 kcal/mol.
                        All conformers are considered further.
                        --------------------------------------------------
                        
                        Calculating prescreening G_mRRHO with implicit solvation!
                        The prescreening G_mRRHO calculation is now performed for:
                        CONF1, CONF2
                        
                        Constructed folders!

                        Starting 2 G_RRHO calculations.
                        Running GFN2-xTB mRRHO in CONF1/rrho_part1
                        Running GFN2-xTB mRRHO in CONF2/rrho_part1
                        WARNING:     found 1 significant imaginary frequencies in CONF2/rrho_part1
                        Tasks completed!
                        
                        The prescreening G_mRRHO run @ td was successful for CONF1/rrho_part1: 0.11573915 S_rot(sym)= 0.0023462 using= 0.1157391
                        The prescreening G_mRRHO run @ c3v was successful for CONF2/rrho_part1: 0.11540064 S_rot(sym)= 0.0010373 using= 0.1154006
                        
                        --------------------------------------------------
                                    * Gibbs free energies of part1 *         
                        --------------------------------------------------
                        
                        CONF#  G(GFNn-xTB) ΔG(GFNn-xTB)       E [Eh]   Gsolv [Eh]  GmRRHO [Eh]         Gtot      ΔGtot
                                    [a.u.]   [kcal/mol]    r2scan-3c   incl. in E         GFN2         [Eh] [kcal/mol]
                                                                [SMD]              [alpb]-bhess                        
                        CONF1  -16.2795022         0.00 -449.1134920    0.0000000    0.1157391 -448.9977529       0.00     <------
                        CONF2  -16.2786988         0.50 -449.1114398    0.0000000    0.1154006 -448.9960392       1.08
                        
                        Number of conformers observed within the following ΔG windows:
                        ΔG [kcal/mol]  #CONF   sum(Boltzmann_weights)
                        ---------------------------------------------
                            0 - 0.5        1          0.86
                            0 - 1.0        1          0.86
                            0 - 1.5        2          1.00
                        ---------------------------------------------
                        
                        Additional global 'fuzzy-threshold' based on the standard deviation of (G_mRRHO):
                        Std_dev(G_mRRHO) = 0.150 kcal/mol
                        Fuzzythreshold   = 0.107 kcal/mol
                        Final sorting threshold G_thr(1) = 3.500 + 0.107 = 3.607 kcal/mol
                        Spearman correlation coefficient between (E + Solv) and (E + Solv + mRRHO) = 1.000
                        
                        All relative (free) energies are below the initial G_thr(1) threshold of 3.5 kcal/mol.
                        All conformers are considered further.
                        
                        Calculating Boltzmann averaged free energy of ensemble on input geometries (not DFT optimized)!
                        
                        temperature /K:   avE(T) /a.u. avGmRRHO(T) /a.u. avGsolv(T) /a.u.   avG(T) /a.u.
                        ----------------------------------------------------------------------------------------------------
                            298.15        -449.1132047        0.1156917        0.0000000   -448.9975129      <<==part1==
                        ----------------------------------------------------------------------------------------------------

                        
                        Calculating unbiased GFNn-xTB energy
                        Constructed folders!

                        Starting 2 xTB - single-point calculations.
                        gfn2-xTB energy for CONF1/GFN_unbiased = -16.3966231
                        gfn2-xTB energy for CONF2/GFN_unbiased = -16.3954819
                        Tasks completed!
                        
                        
                        >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>END of Part1<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<
                        Ran part1 in 9.1081 seconds
                        
                        ----------------------------------------------------------------------------------------------------
                                                                CRE OPTIMIZATION - PART2                                      
                        ----------------------------------------------------------------------------------------------------
                        
                        program:                                                       orca
                        functional for part2:                                          r2scan-3c
                        basis set for part2:                                           def2-mTZVPP
                        using the xTB-optimizer for optimization:                      on
                        using the new ensemble optimizer:                              on
                        optimize all conformers below this G_thr(opt,2) threshold:     2.5
                        Spearman threshold:                                            0.941
                        number of optimization iterations:                             8
                        radsize:                                                       10
                        optimization level in part2:                                   lax
                        solvent:                                                       chcl3
                        solvent model applied in the optimization:                     smd
                        solvent model for Gsolv contribution:                          smd
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
                        CONF1, CONF2
                        
                        Constructed folders!

                        Preparing 2 calculations.
                        Tasks completed!
                        
                        ************************Starting optimizations************************
                        
                        Starting threshold is set to 2.5 + 60.0 % = 4.0 kcal/mol
                        
                        Lower limit is set to G_thr(opt,2) = 2.5 kcal/mol
                        
                        *******************************CYCLE 1********************************
                        
                        Starting 2 optimizations.
                        Running optimization in CONF1/r2scan-3c   
                        Running optimization in CONF2/r2scan-3c   
                        Tasks completed!
                        
                        Geometry optimization converged for: CONF1 within   3 cycles
                        Geometry optimization converged for: CONF2 within   3 cycles
                        Constructed folders!
                        
                        Starting 2 G_RRHO calculations.
                        Running GFN2-xTB mRRHO in r2scan-3c/rrho_crude
                        Running GFN2-xTB mRRHO in r2scan-3c/rrho_crude
                        Tasks completed!
                        
                        The G_mRRHO calculation on crudely optimized DFT geometry @ td was successful for CONF1/r2scan-3c/rrho_crude: 0.1157450 S_rot(sym)= 0.0023462 using= 0.1157450
                        The G_mRRHO calculation on crudely optimized DFT geometry @ c3v was successful for CONF2/r2scan-3c/rrho_crude: 0.1146578 S_rot(sym)= 0.0010373 using= 0.1146578
                        ***********************Finished optimizations!************************
                        Timings:
                        Cycle:  [s]  #nconfs  Spearman coeff.
                            1   34.85     2     
                        sum:   34.85
                        
                        CONVERGED optimizations for the following remaining conformers:
                        Converged optimization for CONF1 after   3 cycles: -449.1155065
                        Converged optimization for CONF2 after   3 cycles: -449.1133322
                        
                        Calculating single-point energies and solvation contribution (G_solv)!
                        CONF1, CONF2
                        
                        Running single-point in CONF1/r2scan-3c
                        Running single-point in CONF2/r2scan-3c
                        Tasks completed!

                        lowlevel single-point calculation was successful for CONF1/r2scan-3c: -449.11550627
                        lowlevel single-point calculation was successful for CONF2/r2scan-3c: -449.11333157
                        
                        Calculating lowlevel G_mRRHO with implicit solvation on DFT geometry!
                        The lowlevel G_mRRHO calculation is now performed for:
                        CONF1, CONF2
                        
                        Constructed folders!
                        
                        Starting 2 G_RRHO calculations.
                        Running GFN2-xTB mRRHO in CONF1/rrho_part2
                        Running GFN2-xTB mRRHO in CONF2/rrho_part2
                        Tasks completed!
                        
                        The lowlevel G_mRRHO calculation @ td was successful for CONF1/rrho_part2: 0.11574502 S_rot(sym)= 0.0023462 using= 0.1157450
                        The lowlevel G_mRRHO calculation @ c3v was successful for CONF2/rrho_part2: 0.11465779 S_rot(sym)= 0.0010373 using= 0.1146578
                        
                        --------------------------------------------------
                                    * Gibbs free energies of part2 *         
                        --------------------------------------------------
                        
                        CONF#  E(GFNn-xTB) ΔE(GFNn-xTB)       E [Eh]   Gsolv [Eh]  GmRRHO [Eh]         Gtot      ΔGtot Boltzmannweight
                                    [a.u.]   [kcal/mol]    r2scan-3c   incl. in E         GFN2         [Eh] [kcal/mol]   % at 298.15 K
                                                                [SMD]              [alpb]-bhess                                        
                        CONF1  -16.3952414         0.00 -449.1155063    0.0000000    0.1157450 -448.9997612       0.00           75.98     <------
                        CONF2  -16.3940994         0.72 -449.1133316    0.0000000    0.1146578 -448.9986738       0.68           24.02
                        
                        Number of conformers observed within the following ΔG windows:
                        ΔG [kcal/mol]  #CONF   sum(Boltzmann_weights)
                        ---------------------------------------------
                            0 - 0.5        1          0.76
                            0 - 1.0        2          1.00
                        ---------------------------------------------

                        Calculating Boltzmann averaged free energy of ensemble!

                        temperature /K:   avE(T) /a.u. avGmRRHO(T) /a.u. avGsolv(T) /a.u.   avG(T) /a.u.
                        ----------------------------------------------------------------------------------------------------
                            273.15        -449.1150651        0.1190010        0.0000000   -448.9960641 
                            278.15        -449.1150486        0.1183075        0.0000000   -448.9967412 
                            283.15        -449.1150323        0.1176087        0.0000000   -448.9974236 
                            288.15        -449.1150158        0.1169054        0.0000000   -448.9981104 
                            293.15        -449.1149998        0.1161973        0.0000000   -448.9988025 
                            298.15        -449.1149840        0.1154839        0.0000000   -448.9995001      <<==part2==
                            303.15        -449.1149681        0.1147661        0.0000000   -449.0002020 
                            308.15        -449.1149528        0.1140434        0.0000000   -449.0009094 
                            313.15        -449.1149376        0.1133166        0.0000000   -449.0016210 
                            318.15        -449.1149227        0.1125847        0.0000000   -449.0023381 
                            323.15        -449.1149077        0.1118481        0.0000000   -449.0030596 
                            328.15        -449.1148932        0.1111069        0.0000000   -449.0037863 
                            333.15        -449.1148790        0.1103615        0.0000000   -449.0045175 
                            338.15        -449.1148646        0.1096115        0.0000000   -449.0052531 
                            343.15        -449.1148509        0.1088570        0.0000000   -449.0059939 
                            348.15        -449.1148374        0.1080973        0.0000000   -449.0067401 
                            353.15        -449.1148237        0.1073340        0.0000000   -449.0074897 
                            358.15        -449.1148107        0.1065661        0.0000000   -449.0082446 
                            363.15        -449.1147975        0.1057936        0.0000000   -449.0090039 
                            368.15        -449.1147853        0.1050171        0.0000000   -449.0097682 
                            373.15        -449.1147730        0.1042360        0.0000000   -449.0105370 
                        ----------------------------------------------------------------------------------------------------
                        
                        
                        
                        --------------------------------------------------
                                    Conformers considered further           
                        --------------------------------------------------
                        
                        
                        Conformers that are below the Boltzmann threshold G_thr(2) of 99.0%:
                        CONF1, CONF2
                        
                        
                        >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>END of Part2<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<
                        Ran part2 in 39.8941 seconds
                        
                        ----------------------------------------------------------------------------------------------------
                                                                    NMR MODE - PART4                                          
                        ----------------------------------------------------------------------------------------------------
                        
                        calculate coupling constants:                                  on
                        prog4J - program for coupling constant calculation:            orca
                        funcJ  - functional for coupling constant calculation:         PBE0
                        basisJ - basis for coupling constant calculation:              def2-TZVP
                        sm4J - solvent model for the coupling calculation:             smd
                        
                        calculate shielding constants σ:                               on
                        prog4S - program for shielding constant calculation:           orca
                        funcS - functional for shielding constant calculation:         PBE0
                        basisS - basis for shielding constant calculation:             def2-TZVP
                        sm4S - solvent model for the shielding calculation:            smd
                        
                        Calculating proton spectrum:                                   on
                        reference for 1H:                                              TMS
                        spectrometer frequency:                                        300.0
                        
                        Considering the following 2 conformers:
                        CONF1, CONF2


                        --------------------------------------------------
                                * Gibbs free energies used in part4 *       
                        --------------------------------------------------
                        
                        CONF#       E [Eh]   Gsolv [Eh]  GmRRHO [Eh]         Gtot      ΔGtot Boltzmannweight
                                    r2scan-3c   incl. in E         GFN2         [Eh] [kcal/mol]   % at 298.15 K
                                        [SMD]              [alpb]-bhess                                        
                        CONF1 -449.1155063    0.0000000    0.1157450 -448.9997612       0.00           75.98     <------
                        CONF2 -449.1133316    0.0000000    0.1146578 -448.9986738       0.68           24.02
                        
                        Conformers that are below the Boltzmann-thr of 99.0:
                        CONF1, CONF2
                        
                        Constructed folders!
                        
                        Performing coupling constant calculations:
                        
                        Starting 2 coupling constants calculations
                        Running coupling calculation in CONF1/NMR
                        Running coupling calculation in CONF2/NMR
                        Tasks completed!
                        
                        Coupling constant calculation was successful for CONF1/NMR
                        Coupling constant calculation was successful for CONF2/NMR
                        
                        Performing shielding constant calculations:
                        
                        Starting 2 shielding constants calculations
                        Running shielding calculation in CONF1/NMR         
                        Running shielding calculation in CONF2/NMR         
                        Tasks completed!
                        
                        Shielding constant calculation was successful for CONF1/NMR
                        Shielding constant calculation was successful for CONF2/NMR
                        
                        Generating file anmr_enso for processing with the ANMR program.
                        
                        Writing .anmrrc!
                        ERROR:       The reference absolute shielding constant for element h could not be found!
                                        You have to edit the file .anmrrc by hand!
                        INFORMATION: The KeyError is: 'r2scan-3c'
                        
                        Generating plain nmrprop.dat files for each populated conformer.
                        These files contain all calculated shielding and coupling constants.
                        The files can be read by ANMR using the keyword '-plain'.
                        
                        Tasks completed!
                        
                        
                        Averaged shielding constants:
                        # in coord  element  σ(sigma)  SD(σ based on SD Gsolv)  SD(σ by 0.4 kcal/mol)       shift        σ_ref
                        ---------------------------------------------------------------------------------------------------------
                            2             h       31.59          0.000000                 0.002263          -31.59         0.000
                            3             h       31.59          0.000000                 0.002263          -31.59         0.000
                            4             h       31.59          0.000000                 0.002263          -31.59         0.000
                            7             h       31.59          0.000000                 0.002263          -31.59         0.000
                            8             h       31.59          0.000000                 0.002263          -31.59         0.000
                            9             h       31.59          0.000000                 0.002263          -31.59         0.000
                            11            h       31.59          0.000000                 0.002263          -31.59         0.000
                            12            h       31.59          0.000000                 0.002263          -31.59         0.000
                            13            h       31.59          0.000000                 0.002263          -31.59         0.000
                            15            h       31.59          0.000000                 0.002263          -31.59         0.000
                            16            h       31.59          0.000000                 0.002263          -31.59         0.000
                            17            h       31.59          0.000000                 0.002263          -31.59         0.000
                        ---------------------------------------------------------------------------------------------------------
                        
                        # in coord  element  σ(sigma)  min(σ)* CONFX   max(σ)* CONFX  Δ(max-min)
                        ---------------------------------------------------------------------------------------------------------
                            2             h       31.59    31.58 CONF2     31.60 CONF1      0.01
                            3             h       31.59    31.58 CONF2     31.60 CONF1      0.01
                            4             h       31.59    31.58 CONF2     31.60 CONF1      0.01
                            7             h       31.59    31.58 CONF2     31.60 CONF1      0.01
                            8             h       31.59    31.58 CONF2     31.60 CONF1      0.01
                            9             h       31.59    31.58 CONF2     31.60 CONF1      0.01
                            11            h       31.59    31.58 CONF2     31.60 CONF1      0.01
                            12            h       31.59    31.58 CONF2     31.60 CONF1      0.01
                            13            h       31.59    31.58 CONF2     31.60 CONF1      0.01
                            15            h       31.59    31.58 CONF2     31.60 CONF1      0.01
                            16            h       31.59    31.58 CONF2     31.60 CONF1      0.01
                            17            h       31.59    31.58 CONF2     31.60 CONF1      0.01
                        ---------------------------------------------------------------------------------------------------------
                        * min(σ) and max(σ) are averaged over the chemical equivalent atoms, but not Boltzmann weighted.
                        
                        >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>END of Part4<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<
                        Ran part4 in 66.5507 seconds
                        
                        
                        Part                : #conf       time      time (including restarts)
                        -----------------------------------------------------------------------
                        Input               :     2        -            -    
                        Part0_all           :     2       4.44 s     4.44 s
                        Part1_initial_sort  :     2       8.03 s     8.03 s
                        Part1_all           :     2       9.11 s     9.11 s
                        Part2_opt           :     2      34.85 s    34.85 s
                        Part2_all           :     2      39.89 s    39.89 s
                        Part4               :     2      66.55 s    66.55 s
                        -----------------------------------------------------------------------
                        All parts           :     -     120.00 s   120.00 s
                        
                        CENSO all done!
                        
The calculated shift has now to be inserted  into the .anmrrc file of the NMR-calculation
for the respective molecule:

.. code:: none

                        $ cat .anmrrc
                            
                        7 8 XH acid atoms
                        ENSO qm= ORCA mf= 300.0 lw= 1.0  J= on S= on T= 298.15
                        TMS[chcl3] PBE0[SMD]/def2-TZVP//r2scan-3c[SMD]/def2-mTZVPP
                        1  31.59    0.0     1
