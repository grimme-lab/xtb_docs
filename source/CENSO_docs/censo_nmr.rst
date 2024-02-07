.. _nmr:

==========================
Calculation of NMR Spectra
==========================

.. contents::


For the calculation of NMR spectra with CENSO, the ANMR program is needed.
The spectra can be plotted with the python script ``nmrplot.py``. 
Both programs can be obtained from the latest `release page of ENSO <https://github.com/grimme-lab/enso/releases/tag/v.2.0.2>`_.


.. important::

   To calculate NMR Spectra using the CENSO ensemble and the ANMR program, it is necessary to create your
   initial CREST ensemble using the ``-nmr`` flag.


ANMR
----

ANMR requires the files:

* ``coord``, turbomole data group ???
* ``anmr_nucinfo``, written by CREST using the ``-nmr`` flag

  * information on which nuclei can interchange in your molecule (e.g. the hydrogen 
    atoms in a methlygroup)
  * = information on chemical and magnetical equivalent atoms

* ``anmr_rotamer``, written by CREST using the ``-nmr`` flag

  * information on the rotamers detected by ``crest``

* ``anmr_enso``, written by CENSO

  * information on the contributing conformers, the Boltzmann weight and all 
    contributions to free energy

* ``.anmrrc``, needs to be configured by the user

  * ``anmr`` searches for the ``.anmrrc`` file first in the folder of execution and 
    if not found in the home directory of the user
  * The file contains the reference-shieldings of e.g TMS to convert the calculated 
    shielding to shifts

* the folders with the property calculations of the conformers

  * e.g. ``CONF1/NMR``

Example ``.anmrrc`` file:

.. code::

   7 8 XH acid atoms
   ENSO qm= TM mf= 300 lw= 1.0  J= on S= on
   TMS[chcl3] pbe0[COSMO]/def2-TZVP//pbeh-3c[DCOSMO-RS]/def2-mSVP
   1  31.786    0.0    1
   6  189.674   0.0    0
   9  182.57    0.0    0
   15 291.9     0.0    0

The first line in .anmrrc informs ANMR to ignore all protons bound to nitrogen 
(N=7) and oxygen (O=8). If you want to calculate protons bound to oxygen or nitrogen,
simply remove the corresponding number, but leave the rest of the line intact!
The next line starting with ``ENSO`` informs ANMR that the property calculation 
was performed by ``TM`` = TURBOMOLE (or ``ORCA`` = ORCA). The ``mf= 300`` informs ANMR 
that the magnetic frequency of the NMR spectrometer is set to 300 MHz. The ``lw`` 
(linewidth for plotting) is 1.0 and J (couplings) and S (shieldings) are to be evaluated. 
If ``S=`` off then ANMR will terminate after calculating and averaging the shifts of the 
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

Example ``anmr_enso`` file:

.. code::

   ONOFF NMR CONF BW      Energy     Gsolv    RRHO
   1     1   1    0.10042 -354.38939 -0.00899 0.22109
   1     2   2    0.32452 -354.39034 -0.00899 0.22093
   1     3   3    0.57506 -354.39287 -0.00902 0.22295

The file ``anmr_enso`` is written by the CENSO program and contains information on 
the conformers, which folder they are in, the Boltzmann weight, energy, solvation 
and thermostatistical contribution to free energy. The first number in the three last 
lines indicates to ANMR if the conformer is to be considered (1) or not (0). 
If one conformer is not considered (or more) the ANMR program internally recalculates
the Boltzmann weights based on the free energies from the ``anmr_enso`` file. 


Usage of `anmr`:

.. important::

    ANMR still relies on autmatically created arrays on the stack. For this reason you have to run ``ulimit -s unlimited`` to prevent stackoverflows.


.. tab-set:: 
    .. tab-item:: command
  
        .. code:: sh
        
              $ anmr --help
              # explanation of all possible command line arguments
              # shown in next tab
        
        
    .. tab-item:: keywords

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


ANMR will then write a file called ``anmr.dat`` (which is quiet large). The file
contains the information ppm vs intesity. This file can then be plotted with any 
plotting tool or our ``nmrplot.py``.

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
----------------

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
-----------------------------

For user convenience shielding constants of the reference molecules (TMS (Tetramethylsilane), CFCl3, PH3, TMP 
(Trimethylphosphine oxide)) were precalculated (for some method combinations) and stored within the CENSO program. 
The reference shielding values are used in the ANMR program to calculate the shifts and the reference values are 
written to the file ``.anmrrc``.

To be consistent with your calculation, the reference shielding values were calculated on the
reference molecules with many possible geometry-optimization-settings eg. {TURBOMOLE/ORCA, PBEh-3c /
TPSS-D3/def2-TZVP / B97-3c, (gas phase or solvent)}. The shieldings were then calculated either with
TPSS or PBE0 and depending on ORCA (gas or SMD and def2-TZVP basis set) or TURBOMOLE (gas or
DCOSMO-RS with the def2-TZVP basis set). At the end of part4 the file ``.anmrrc`` is written into the
calculation folder and stores the reference shielding values of your settings for the subsequent
ANMR calculation.

.. note:: 
   The CENSO program only writes the reference shielding values to the file ``.anmrrc`` but 
   does not do anything with it. Hence, no results of CENSO are influenced 
   by a non-matching reference value. If you want to change the reference shielding values, 
   you can simply modify the file ``.anmrrc`` manually before calling the ANMR program. 

Procedure for generating the refrence shielding constants:
Geometry optimization with the respective reference molecule with PBEh-3c/B97-3c/TPSS-D3/def2-TZVP + implicit solvation model 
(either SMD or DCOSMO-RS). NMR shielding constant calculation with the respective functional and the def2-TZVP basis set 
(again with implicit solvation model).

Input structures for the respective reference molecules:

.. tab-set:: 
    
    .. tab-item:: Tetramethylsilane:

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

    .. tab-item:: PH3:

        .. code:: bash

            $ cat coord
            $coord
            0.00000000000000      0.00000000000000      1.08780842165939  p
            1.12108786201329      1.94178113675579     -0.36261095596909  h
            1.12108786201329     -1.94178113675579     -0.36261095596909  h
            -2.24217572402658      0.00000000000000     -0.36261095596909  h
            $end

    .. tab-item:: TMP = Trimethylphosphine oxide:

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

    .. tab-item:: CFCl3:

        .. code:: bash

            $ cat coord
            $coord
            0.00000038126763   -0.00000000884504    0.13419916242803      c 
            0.00000870296281    0.00000001369727    2.66116007348966      f 
            3.17274491422955   -0.00000000906271   -0.93176725824334      cl
            -1.58637567202181   -2.74767202581384   -0.93179226251812      cl
            -1.58637568491745    2.74767203002431   -0.93179224376158      cl
            $end

