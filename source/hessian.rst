Calculation of Vibrational Frequencies
======================================

In this chapter, all necessary information about the calculation of vibrational spectra and thermostatistical contributions are given. 

.. contents::

Performing simple Vibrational Frequency calculations
____________________________________________________

Vibrational frequency calculations are available only through two-sided numerical differentiation of analytical gradients. 

Consider a simple example like the following hydrogen abstraction reaction:

.. code:: text

    7

    C          -0.12888312425142   -0.00640246259879   -0.00997057133406
    H           1.44011699709596    0.12229812355524   -0.02854203428735
    H          -0.41612454870604    1.02694842152161   -0.04938812535015
    H          -0.26306601703832   -0.58286887757121   -0.90445094952445
    H          -0.26440375738028   -0.51708010658031    0.92386857306799
    O           2.45008586500521    0.26032015001761    0.01133571198248
    H           2.61210033905108    0.98358191645276    0.62026402303033

By invoking the ``--hess`` command line argument, xtb executes a calculation of the Hessian matrix. The  ``--ohess`` keyword may be used instead if a prior optimization of the structure is desired. 

.. code:: text
    
    xtb min.xyz --hess --uhf 1

At the end of the frequency job you get an output like this:

.. code:: text

            -------------------------------------------------
            |               Frequency Printout                |
            -------------------------------------------------
    projected vibrational frequencies (cm-1)
    eigval :       -0.00    -0.00    -0.00     0.00     0.00     0.00
    eigval :       40.69   211.99   360.40   405.89   601.08   759.17
    eigval :      829.07  1371.91  1375.70  1477.42  2297.37  3115.69
    eigval :     3190.88  3197.05  3648.64
    reduced masses (amu)
    1:  4.63   2: 12.19   3: 12.31   4:  9.70   5:  9.30   6: 12.74   7:  1.74   8: 12.17
    9:  1.45  10:  1.77  11:  1.92  12:  1.45  13:  2.39  14:  2.02  15:  2.03  16:  2.07
    17:  2.17  18:  1.07  19:  2.09  20:  2.09  21:  1.86
    IR intensities (amu)
    1:  0.17   2:  0.46   3:  0.44   4:  0.41   5:  0.08   6:  0.35   7:  0.48   8:  0.26
    9:  0.26  10:  0.27  11:  0.25  12:  0.41  13:  0.30  14:  0.04  15:  0.07  16:  0.52
    17:  0.51  18:  0.06  19:  0.14  20:  0.12  21:  0.18
    Raman intensities (amu)
    1:  0.00   2:  0.00   3:  0.00   4:  0.00   5:  0.00   6:  0.00   7:  0.00   8:  0.00
    9:  0.00  10:  0.00  11:  0.00  12:  0.00  13:  0.00  14:  0.00  15:  0.00  16:  0.00
    17:  0.00  18:  0.00  19:  0.00  20:  0.00  21:  0.00
    output can be read by thermo (or use thermo option).
    writing <g98.out> molden fake output.
    recommended (thermochemical) frequency scaling factor: 1.0
   
This output consists of the calculated vibrational frequencies and the vibrational modes. In the example above there are six frequencies which are identically zero. These frequencies correspond to the rotations and translations of the molecule. They have been projected out of the Hessian before the calculation of the frequencies and thus, the zero values do not tell you anything about the quality of the Hessian that has been diagonalized.

Xtb writes an ``g98.out`` file in *GAUSSIAN*-format, which can be opened with the popular *MOLDEN* program to visualize the vibrational modes.
Further, a ``hessian`` file is written, containing the projected Hessian matrix in *turbomole* format.

Calculation of thermochemical properties
________________________________________

Each frequency job provides the thermochemical properties at 298.15 K. (for other temperatures, see below). No further user-input is required to obtain all important thermostatistical contributions. The contributions are calculated following a coupled rigid-rotor-harmonic-oscillator approach. If a molecular symmetry is detected, the resulting rotational number is automatically accounted for. The symmetry detection can be adjusted in the ``$symmetry`` block of the ``xcontrol`` file if necessary. 

.. code:: text

            -------------------------------------------------
            |             Thermodynamic Functions             |
            -------------------------------------------------

            ...................................................
            :                      SETUP                      :
            :.................................................:
            :  # frequencies                          15      :
            :  # imaginary freq.                       0      :
            :  linear?                             false      :
            :  only rotor calc.                    false      :
            :  symmetry                               C1      :
            :  rotational number                       1      :
            :  scaling factor                  1.0000000      :
            :  rotor cutoff                   50.0000000 cm⁻¹ :
            :  imag. cutoff                  -20.0000000 cm⁻¹ :
            :.................................................:

        mode    ω/cm⁻¹     T·S(HO)/kcal·mol⁻¹    T·S(FR)/kcal·mol⁻¹   T·S(vib)
    ------------------------------------------------------------------------
        1     40.69    -1.55795 ( 30.48%)    -1.11458 ( 69.52%)    -1.24972
        2    211.99    -0.60419 ( 99.69%)    -0.62804 (  0.31%)    -0.60426
    ------------------------------------------------------------------------

    temp. (K)  partition function   enthalpy   heat capacity  entropy
                                    cal/mol     cal/K/mol   cal/K/mol   J/K/mol
    298.15  VIB   13.7                 1501.827      9.485      9.210
            ROT  0.909E+04              888.752      2.981     21.094
            INT  0.125E+06             2390.579     12.466     30.304
            TR   0.184E+27             1481.254      4.968     36.401
            TOT                        3871.8331    17.4344    66.7050   279.0936

    temp. (K)  partition function   enthalpy   heat capacity  entropy
                                    cal/mol     cal/K/mol   cal/K/mol   J/K/mol
    298.15  VIB   13.7                 1501.827      9.485      9.210
            ROT  0.909E+04              888.752      2.981     21.094
            INT  0.125E+06             2390.579     12.466     30.304
            TR   0.184E+27             1481.254      4.968     36.401
            TOT                        3871.8331    17.4344    66.7050   279.0936

        T/K    H(0)-H(T)+PV         H(T)/Eh          T*S/Eh         G(T)/Eh
    ------------------------------------------------------------------------
        298.15    0.617016E-02    0.583013E-01    0.316937E-01    0.266076E-01
        298.15    0.617016E-02    0.583013E-01    0.316937E-01    0.266076E-01 (used)
    ------------------------------------------------------------------------

            :::::::::::::::::::::::::::::::::::::::::::::::::::::
            ::                  THERMODYNAMIC                  ::
            :::::::::::::::::::::::::::::::::::::::::::::::::::::
            :: total free energy          -8.613409150740 Eh   ::
            ::.................................................::
            :: total energy               -8.640016786693 Eh   ::
            :: zero point energy           0.052131167146 Eh   ::
            :: G(RRHO) contrib.           -0.025523531193 Eh   ::
            :::::::::::::::::::::::::::::::::::::::::::::::::::::

Dealing with imaginary modes and non-minimum structures
_______________________________________________________

If a frequency calculation is invoked using the ``--hess`` command line argument, xTB automatically checks the gradient norm for a non-zero value. For unoptimized structures with significant remaining grad. norm, a warning is printed. If you want xTB to exit with an error code instead of this warning, use the ``--strict`` command line argument. 

.. code:: text

    ########################################################################
    # WARNING! Some non-fatal runtime exceptions were caught, please check:
    #  - Hessian on incompletely optimized geometry!
    ########################################################################

A ``xtbhess.coord`` file is created in this case, containing the input structure distorted along the imaginary mode. In case of unwanted imaginary modes, this structure can be used as a starting point to perform further optimizations to get rid of the imaginary frequency and locate the true minimum. 


Advanced options
________________

Of course, the calculated frequencies depend on the masses used for each atom. Several options exist to modify/scale the default atomic masses in the ``$hess`` block of the ``xcontrol`` file.

.. code:: text


   $hess
       sccacc=real
           SCC accuracy level in Hessian runs

       step=real
           Cartesian displacement increment for numerical Hessian

       isotope: int,real
           set mass of atom type int to real

       modify mass: int,real
           set mass of atom number int to real

       scale mass: int,real
           scale mass of atom number int by real

Changes regarding ``sccacc``` or ```step``'' should be made with caution, as large displacements or loose SCC accuracy can lead to unreliable frequencies due to excessive numerical noise in the calculations.   


The thermostatistical calculations can be influenced by the ``$thermo`` block of the ``xcontrol`` file.

.. code:: text

   $thermo
       temp=real
           temperature for thermostatistical calculation (default: 298.15 K)

       sthr=real
           rotor cut-off (cm-1) in thermo (default: 50.0)

