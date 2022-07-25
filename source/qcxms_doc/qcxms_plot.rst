.. _plotms:

--------------
Visualization
--------------

Plotting Mass Spectra (PlotMS)
==============================


The **PlotMS** utility was developed to visualize the results of `QCxMS <https://github.com/qcxms/QCxMS/releases>`_
calculations. The newest version can be found in the `PlotMS repository <https://github.com/qcxms/PlotMS/releases/>`_.

The program analyzes the *qcxms.res* or *qcxms_cid.res* file and provides the results as *m/z*-values and abundances.
Versions 6.0 and higher provide **exact masses** of the fragments. 

To run the program, change into your working directory and run ``plotms``. This generates three files:
   - `mass.agr` -> *XMGRACE* file using the `~/.mass_raw.agr` template file
   - `results.jdx` -> *JCAMP-DX* ( *Joint Committee on Atomic and Molecular Physical* data) file 
   - `results.csv` -> *CSV* (*comma seperated values*) file for spreadsheet programs (e.g. Excel)

The spectra can be plotted as soon as the production run has started by using the `getres` script, which creates an 
*tmpqcxms.res* file. The file has to be deleted before ``getres`` is used a second time.

For comparison to experiment, a file including the intensity and *m/z* values can be copied into 
the folder of the *.res* file.
The file can be read in using `plotms -i <file>`.
Most recommended is using a *.csv* file format with the following specifications: 

+--------------------+-----------------------------+-------------------+
| intensity *<real>* |                             | m/z *<real>*      | 
+--------------------+-----------------------------+-------------------+
| intensity *<real>* |                             | m/z *<real>*      | 
+--------------------+-----------------------------+-------------------+
|       `.....`      |                             |      `.....`      | 
+--------------------+-----------------------------+-------------------+

PlotMS will than plot the *.agr* file with direct comparison between computed and experimental 
results. 
A dot matching score is printed at the end of the program output.

For a comparison to experimental integer spectra from the 
`NIST database <https://webbook.nist.gov/chemistry/>`_ (*JCAMP-DX* format), the file can be copied
into the working directory as `nist.dat`. 


Program flags and command-line options
---------------------------------------

There are some useful command-line options to manipulate your results.

-v, --verbose
    print verbose options
-f, --file <file>
    provide `.res` file for plotting the spectrum
-t <x> <y>
    couting ions with charge *x* to *y* (give the value, e.g. "-t 1 2" for charge 1 to 2)
-w, --width <real>
    broadening the charges by an standard distribution (given in decimal numbers between 0 and 1)
-s, --cascades <integer>
    account only for only secondary and tertiary fragmentations (give the value, e.g. "-s 2" for secondary)
-m, --min <integer>
    set minimum value for m/z, so rel. 100% value will be calculated for higher masses (x-axis)
-i, --mzmin <real>
    set the minimum rel. intensity from which the signals are counted (y-axis)
-p, --noisotope
    do not calculate the isotope pattern 
-e, --exp <file>
    use the following file as input for comparison to the calculated spectrum



Visualization of Trajectories
=============================

Trajectories are saved in the *TMPQCXMS/TMP.XXX* directories, where NUM is the first number of the specific 
trajectory track to be visualized. There are two programs that can easily display the trajectories, which 
are saved in the *trj.NUM.xxx* files. One is `(g)molden`, the other is `VMD`. Obtaining a video (.avi) of a 
given trajectory is possibly most easily done by loading the trajectory of choice into VMD (file type .xyz).

The `msmovie` and `movie.tcl` scripts for `VMD` generate a visualization using the dynamic bonds graphical representations 
setting. This allows for movie generation employing the `VMD` movie maker plug-in. Simply type msmovie X Y in the 
working directory to load the trajectory *TMPQCXMS/TMP.X/trj.X.Y*. For instance, `msmovie 1 1` loads the first 
trajectory of the first folder. 
Some adjustments according to personal preferences for movie-making may have to be made in these scripts.

