.. _uvvis:

=============================
Calculation of UV/Vis-Spectra
=============================

.. contents ::


For the calculation of ensemble UV/Vis-Spectra CENSO can use ORCA to compute the excitation wavelengths and 
oscillator strengths for each conformer. From the oscillator strengths the maximum extinction coefficients 
are calculated based on Boltzmann population. You can find the data for all the excitations
in ``json``-format the file ``excitations.json`` or in tabular format in the output file of the UV/Vis 
property calculation.

Plotting the spectrum
---------------------

The calculated extinction coefficients are to be used to calculate a spectrum as a superposition of Gaussians.
The maximum extinction coefficients are calculated for every excitation. Furthermore, the coefficients get weighted
by the population of the originating conformer. For plotting the spectrum, the ``uvvisplot`` script provided in the
``bin`` directory can be used.
