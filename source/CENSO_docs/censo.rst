.. _CENSO:

=====================
Introduction to CENSO
=====================

General information
-------------------

.. figure:: ../../figures/CENSO/censo_logo_300dpi.png
	:scale: 40%
	:align: center
	:alt: CENSO logo

**C**\ommandline **E**\nergetic **SO**\rting (**CENSO**) is a sorting algorithm 
for efficient evaluation of **S**\tructure **E**\nsembles (**SE**). 

**WIP**

CENSO can be structured into two components:

1. Ensemble refinement,
2. Ensemble property calculations.

The first part (ensemble refinement) can use up to three steps:

1. Prescreening,
2. Screening,
3. Optimization.

Ensemble properties available for calculation are:

1. NMR spectra,
2. Optical rotation (**TODO**),
3. UV/Vis spectra (**TODO**).

Ensemble Refinement
===================

Prescreening
-------------

The first step after obtaining a rough ensemble of the most important conformers, 
the number of which can range in the hundreds, is to improve on the preliminary
ranking using a lightweight DFT method. This should usually already yield significant
improvements compared to the preliminary ranking, usually obtained using SQM/FF methods.
In the case that solvation effects should be included, CENSO will use `xtb` to 
calculate the energy of solvation using the ALPB or GBSA solvation model. The threshold
for this step should be rather high (up to 10 kcal/mol).

Screening
---------

After prescreening the ensemble in the first step, this step is supposed to further 
improve on the ranking quality by increasing the quality of the utilized DFT method.
Also, in this step one may choose to include thermal contributions to the free enthalpy
by activating `evaluate_rrho`, which will lead to CENSO using `xtb` to calculate
single-point Hessians. This will also include solvation if the user chose to do so.
The threshold for this step should be lower than before (up to 7.5 kcal/mol) to account
for the decreasing uncertainty due to improvements in the ranking method. CENSO will 
increase the threshold by up to 1 kcal/mol, proportional to the (exponential of the) 
standard deviation of the thermal contributions. The solvation contributions will be 
calculated using DFT, if necessary explicitly, though this will double the computational
effort due to two required single-point calculations.

Optimization
------------

To further improve the ranking, the geometries of the conformers in this step will be 
optimized using DFT gradients. For this, the `xtb` optimizer will be used as driver.
The level of the chosen DFT method should be roughly equivalent to the Screening step.
Solvation effects will be included implicitly. Furthermore, thermal contributions will
be included for the ranking if `evaluate_rrho` is set to True. One can also utilize
a macrocycle optimizer in CENSO (set `macrocycle` to True). This will run a number
(`opcycles`) of geometry optimization steps (microcycles) for every macrocycle and 
update the ensemble every macrocycle. The single-point Hessian evaluation using `xtb` 
will take place once after at least 6 microcycles and once after all conformers are 
either converged or removed from consideration. The energy threshold for this step
is determined by a minimum threshold (`threshold`) and a contribution of at most 1.5
kcal/mol based on the (normed) mean trajectory similarity (MTS). This threshold will 
be applied once the gradient norm of a conformer is below a specified threshold (`gradthr`)
for all the microcycles in the current macrocycle.

Ensemble Properties 
===================

NMR Spectra
-----------

For the calculation of the NMR spectrum of an ensemble, single-points to compute the 
nuclear shieldings and couplings will be run. The computational parameters for shieldings
and couplings can be set to different values. In this case two separate single-points 
will be run. If the settings are identical only one single-point will be run for both.
After that, CENSO will generate files for the simulation of the NMR spectrum using ANMR.
Please note that the user needs to setup the `.anmrrc` file.

Optical Rotation
----------------

TODO

UV/Vis Spectra
--------------

TODO
