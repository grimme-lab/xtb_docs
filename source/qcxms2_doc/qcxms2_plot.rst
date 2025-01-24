.. _qcxms2_plot:

--------------
Visualization
--------------


QCxMS2 prints the computed spectrum in the ``peaks.csv`` file with the following specifications: 

+--------------------+----------------------------+--------------------+
| m/z *<real>*       |             ,              | intensity *<real>* | 
+--------------------+----------------------------+--------------------+
| m/z *<real>*       |             ,              | intensity *<real>* | 
+--------------------+----------------------------+--------------------+
|       `.....`      |             ,              |      `.....`       | 
+--------------------+----------------------------+--------------------+

The ``peaks.csv`` file can be visualized using any suitable plotting tools.


.. note::
    A python plotting package is currently under development to ease the visualization of the spectrum including 
    the fragmentation pathways leading to specific spectral peaks. 


Program flags and command-line options
---------------------------------------

You can generate the 'peaks.csv' of a finished QCxMS2 calculation with the following command:

.. code::

   qcxms2 [INPUT] -- oplot [OPTIONS]


There are some useful options for manipulating the output of the spectrum:


.. list-table:: Options for generating the mass spectrum:
   :widths: 30 100
   :header-rows: 1

   * - ``-noisotope``
     - only plot peak with the highest isotope probability
   * - ``-int_masses``
     - only plot masses as integers
   * - ``-mthr [real]``
     - m/z at which spectral peaks plotted (default 0)
   * - ``-intthr [real]``
     - peak intensity threshold at which spectral peaks are plotted in percent (default 0)





Investigation of fragmentation pathways
=============================


A QCxMS2 calculation contains also information the fragmentation pathways leading to a specific peak in the mass spectrum.

QCxMS2 prints the computed spectrum in the ``peaks.csv`` file with the following specifications:


+--------------------+---------------------+-----------------------------+
|   input structure  |  m/z *<real>*       | relative intensity *<real>* |
+--------------------+---------------------+-----------------------------+
| fragment directory |  m/z *<real>*       | relative intensity *<real>* |
+--------------------+---------------------+-----------------------------+
|                    |                     |                             |
+--------------------+---------------------+-----------------------------+

The first column gives the location of the xyz coordingate file of the respective species.
The product of a fragmentation reaction can be found in the pXX directories.
For isomer products, the ``isomer.xyz`` file can be found directly in this directory. For fragmentation products, 
the ``pair.xyz`` file can be found in the respective pair directory pXX and the ``fragment.xyz`` files in the respective
fragment directories pXXf1 and pXXf2.

The  second column gives the m/z value of the respective species. It is the exact mass of the isotope combination with the highest probability
for the specific atom composition.

The third column contains the relative intensity normalized to 10000 arbitrary units.

For subsequent fragmentations, the directory name is contains the complete reaction path to end up at the specific fragment.
For example, the p1XXfX directories contain all products of the first isomerization product in the first fragmentation step.
Likewise, the the p10f1pXXfX directories contain all products of fragment 1 of the 10th fragmentation product in the first fragmentation step.

Additional information about reaction energies and barriers of the reaction network can be found in the ``allframgents`` file.
The file contains the following information:

+-----+---------------+--------------------+---------------+--------------------+------------+
+ Dir | fragment_type | sumreac (kcal/mol) | de (kcal/mol) | barrier (kcal/mol) | irc (cm-1) |  
+-----+---------------+--------------------+---------------+--------------------+------------+

For each species, the following information is given:

Dir: Directory of the species in the nomenclature described above.

fragment_type: isomer, fragmentpair

sumreac: Sum of the reaction energies of the reaction network leading to the specific species including the zero-point vibrational energy (ZPVE).

de: Reaction energy of the specific reaction leading to the specific species including the zero-point vibrational energy (ZPVE).

barrier: Reaction barrier of the specific reaction leading to the specific species including the zero-point vibrational energy (ZPVE).

irc: Imaginary frequency of the transition state of the respective reaction. If no irc could be identified, 0 is printed here. 

The next line contains information about the product directory, the molecular mass, the sum formula and the relative intensity in percent of the species.






