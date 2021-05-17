.. _plot_qcxms:

--------------
Visaualization
--------------

Plotting Mass Spectra
=====================

The plotms Version 3.2 utility and the `~/.mass_raw.agr` template file for xmgrace are provided in order to 
visualize your results. New updates on the plotting program can be found in the `PlotMS repository <https://github.com/qcxms/PlotMS>`_. 
The following protocol should be applied. Be advised that you can plot your spectra as soon as the production 
run has started. This may be useful to check for convergence in case you are running more trajectories than 
actually needed for good statistics.

1. Change into your working directory and run ``plotms``. This should generate a mass.agr file.
2. With the mass.agr file, visualize your QCEIMS spectrum with: 

.. code:: none

   xmgrace mass.agr

3. For a comparision of EI spectra, the experimental spectrum can be downloaded from the NIST database if available
   (JCAMP-DX format). Copy it to the working directory as exp.dat. Plotms reads the exp.dat and compares
   your calculated spectrum directly to the experiment (inverted ).

Visualization of Trajectories
=============================

Trajectories are saved in the *TMPQCEIMS/TMP.XXX* directories, where NUM is the first number of the specific 
trajectory track to be visualized. There are two programs that can easily display the trajectories, which 
are saved in the *trj.NUM.xxx* files. One is (g)molden, the other is VMD. Obtaining a video (.avi) of a 
given trajectory is possibly most easily done by loading the trajectory of choice into VMD (file type .xyz).

The msmovie and movie.tcl scripts for VMD generate a visualization using the dynamic bonds graphical representations 
setting. This allows for movie generation employing the VMD movie maker plug-in. Simply type msmovie X Y in the 
working directory to load the trajectory *TMPQCEIMS/TMP.X/trj.X.Y*. For instance, msmovie 1 1 loads the first 
trajectory of the first folder. 
Some adjustments according to personal preferences for movie-making may have to be made in these scripts.


