.. _PlotMS:

--------------
Visualization
--------------

Plotting Mass Spectra (PlotMS)
==============================

The PlotMS utility is provided in order to visualize your results. The newest version can be found in the 
`PlotMS repository <https://github.com/qcxms/PlotMS>`_. 
The program analyzes the `qcxms.res` or `qcxms_cid.res` file and combines the results using the `~/.mass_raw.agr` template file
to create the `mass.agr` and the `results.jdx` files in your working directory. 

This may be useful to check for convergence in case you are running more trajectories than 
actually needed for good statistics. You can plot your spectra as soon as the production run has started. 

1. Change into your working directory and run ``plotms``. This should generate the `mass.agr` file.
2. With the `mass.agr` file, visualize your QCxMS spectrum with: 

.. code:: none

   xmgrace mass.agr

3. For a comparision of EI spectra, the experimental spectrum can be downloaded from the NIST database if available
   (JCAMP-DX format). Copy it to the working directory as `exp.dat`. PlotMS reads the `exp.dat` and compares
   your calculated spectrum directly to the experiment (inverted).
   There is currently no feature to do this for the CID spectrum as well.

There are some useful commandline options to  your results.




Visualization of Trajectories
=============================

Trajectories are saved in the *TMPQCXMS/TMP.XXX* directories, where NUM is the first number of the specific 
trajectory track to be visualized. There are two programs that can easily display the trajectories, which 
are saved in the *trj.NUM.xxx* files. One is ``(g)molden``, the other is ``VMD``. Obtaining a video (.avi) of a 
given trajectory is possibly most easily done by loading the trajectory of choice into VMD (file type .xyz).

The `msmovie` and `movie.tcl` scripts for ``VMD`` generate a visualization using the dynamic bonds graphical representations 
setting. This allows for movie generation employing the ``VMD`` movie maker plug-in. Simply type msmovie X Y in the 
working directory to load the trajectory *TMPQCEIMS/TMP.X/trj.X.Y*. For instance, `msmovie 1 1` loads the first 
trajectory of the first folder. 
Some adjustments according to personal preferences for movie-making may have to be made in these scripts.


