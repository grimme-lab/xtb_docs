================
Spectra Plotting
================

The NMR spectrum can be plotted from the file `anmr.dat`. It contains the 
information ppm vs intensity and can be plotted with any plotting tool 
(e.g GNUPLOT ...).

The provided `nmrplot.py` plotting tool uses `matplotlib` for plotting. 
Information on all possible commandline arguments is documented:

.. code-block:: text

	> nmrplot.py --help

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
