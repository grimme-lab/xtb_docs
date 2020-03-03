


====
ANMR
====

``ANMR`` requires the files:

* *coord*, turbomole data group
* anmr_nucinfo (human readable), written by `crest`
  
  * information on which nuclei can interchange in your molecule (e.g. the hydrogen 
    atoms in a methlygroup)
  * = information on chemical and magnetical equivalent atoms
* anmr_rotamer (machine readable), written by `crest`
  
  * information on the rotamers detected by `crest`
* anmr_enso, written by `enso`
  
  * information on the contributing conformers, the Boltzmann weight and all 
    contributions to free energy
* .anmrrc, written by `enso`

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

The file *anmr_enso* is written by the `enso` program and contains information on 
the conformers, which folder they are in, the Boltzmann weight, energy, solvation 
and thermostatistical contribution to free energy. The first number in the three last 
lines indicates to ``ANMR`` if the conformer is to be considered (1) or not (0). 
If one conformer is not considered (or more) `anmr` program internally recalculates
the Boltzmann weights based on the free energies from the *anmr_enso* file. 


Usage of `anmr`:

First of all: the spin problem is of :math:`2^{N}` complexity! Depending on the 
size of the maximalspinsystem (*mss*) the program might use a lot of RAM! 
If this is the case, run `anmr` with a decreased spinsystem size:


.. code:: sh

   anmr -mss 12 > anmr.out 2> anmr.error &


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
