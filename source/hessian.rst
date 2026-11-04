.. _frequencies:

Calculation of Vibrational Frequencies
======================================

In this chapter, all necessary information about the calculation of vibrational spectra and thermostatistical contributions are given. 

.. contents::

Performing simple Vibrational Frequency calculations
____________________________________________________

Vibrational frequency calculations are performed numerically from analytical gradients: 
the conventional semi-numerical Hessian uses two-sided finite differences, while O1NumHess 
(``--o1nh``, see below) reconstructs the Hessian using an ODLR approximation requiring 
only O(1) gradient evaluations.

Three Hessian run types are available on the command line:

- ``--hess``: calculates the numerical Hessian on the input geometry.
- ``--ohess [LEVEL]``: first performs an ANCopt geometry optimization and then calculates the Hessian on the optimized geometry. The optional *LEVEL* selects the optimization convergence level from ``crude``, ``sloppy``, ``loose``, ``normal`` (default), ``tight``, ``verytight`` to ``extreme``, e.g.:

  .. code:: bash

      xtb coord --ohess tight

- ``--bhess [LEVEL]``: performs a biased (SPH) geometry optimization to keep the structure close to the input geometry and then calculates the biased Hessian (see :ref:`SPH`). Accepts the same optional optimization *LEVEL* (default ``normal``).

Accuracy settings:

- Standalone Hessian (``--hess``): uses an SCC accuracy of ``0.2`` by default.
- Optimized and biased Hessians (``--ohess [LEVEL]``, ``--bhess [LEVEL]``): the optimization uses the accuracy associated with the selected *LEVEL*. The displaced Hessian evaluations use ``0.2``, or the *LEVEL* accuracy if it is tighter.
- ``--acc REAL`` sets the accuracy for both optimization and Hessian calculations. ``$hess sccacc`` can be used to set a separate accuracy for the displaced Hessian evaluations.

When a conventional Hessian run is combined with ``--ptb`` (or ``--raman [TEMP [WAVELENGTH]] --ptb``), ``xtb`` computes the energetic Hessian with GFN2-xTB and evaluates the response properties and intensities (such as IR and Raman activities) using PTB; PTB itself does not provide an energetic Hessian, and ``--o1nh`` cannot be combined with PTB. Phonon/Hessian calculations under periodic boundary conditions are not supported (``Phonon calculations under PBC are not implemented``).

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

By invoking the ``--hess`` command line argument, ``xtb`` executes a calculation of the Hessian matrix on the input geometry:

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
    IR intensities (km·mol⁻¹)
    1:  0.17   2:  0.46   3:  0.44   4:  0.41   5:  0.08   6:  0.35   7:  0.48   8:  0.26
    9:  0.26  10:  0.27  11:  0.25  12:  0.41  13:  0.30  14:  0.04  15:  0.07  16:  0.52
    17:  0.51  18:  0.06  19:  0.14  20:  0.12  21:  0.18
    Raman intensities (Å⁴·amu⁻¹)
    1:  0.00   2:  0.00   3:  0.00   4:  0.00   5:  0.00   6:  0.00   7:  0.00   8:  0.00
    9:  0.00  10:  0.00  11:  0.00  12:  0.00  13:  0.00  14:  0.00  15:  0.00  16:  0.00
    17:  0.00  18:  0.00  19:  0.00  20:  0.00  21:  0.00
    output can be read by thermo (or use thermo option).
    writing <g98.out> molden fake output.
    recommended (thermochemical) frequency scaling factor: 1.0
   
This output consists of the calculated vibrational frequencies and the vibrational modes (note that ordinary Hessian runs report zero Raman intensities unless Raman/PTB response properties are explicitly requested via ``--raman`` and ``--ptb``). In the example above there are six frequencies which are identically zero. These frequencies correspond to the rotations and translations of the molecule. They have been projected out of the Hessian before the calculation of the frequencies and thus, the zero values do not tell you anything about the quality of the Hessian that has been diagonalized.

``xtb`` writes an ``g98.out`` file in *GAUSSIAN*-format, which can be opened with the popular *MOLDEN* program to visualize the vibrational modes.
Further, a ``hessian`` file is written, containing the Cartesian, non-mass-weighted Hessian matrix in *Turbomole* format (the user-selected ``$hess scale`` factor is applied only to the Hessian used for vibrational analysis, not to the Cartesian ``hessian`` output file).

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

        T/K    H(0)-H(T)+PV         H(T)/Eh          T*S/Eh         G(T)/Eh
    ------------------------------------------------------------------------
        298.15    0.617016E-02    0.583013E-01    0.316937E-01    0.266076E-01
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
            
Multiple temperatures can be calculated using the build in thermodynamic functions
calculator by using a input file similar to this

.. code:: text

    $thermo
        temp=150.0,200.0,250.0,273.15,298.15
        
The final summary looks like

.. code:: text

       T/K    H(0)-H(T)+PV         H(T)/Eh          T*S/Eh         G(T)/Eh
   ------------------------------------------------------------------------
    150.00    0.250495E-02    0.546739E-01    0.135034E-01    0.411705E-01
    200.00    0.361203E-02    0.557809E-01    0.192424E-01    0.365386E-01
    250.00    0.484240E-02    0.570113E-01    0.253913E-01    0.316200E-01
    273.15    0.545010E-02    0.576190E-01    0.283634E-01    0.292557E-01
    298.15    0.617016E-02    0.583013E-01    0.316937E-01    0.266076E-01 (used)
   ------------------------------------------------------------------------
   
``xtb`` will always use the last entry from the temperature list for all
further calculations and printouts.

Dealing with imaginary modes and non-minimum structures
_______________________________________________________

If a frequency calculation is invoked using the ``--hess`` command line argument, *xTB* automatically checks the gradient norm for a non-zero value. For unoptimized structures with significant remaining grad. norm, a warning is printed. If you want *xTB* to exit with an error code instead of this warning, use the ``--strict`` command line argument. 

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
           SCC/calculator accuracy level for displaced numerical Hessian
           evaluations (default: 0.2, or the optimization level accuracy in
           ohess/bhess runs if tighter; overrides --acc for the displaced
           Hessian evaluations)

       step=real
           Cartesian displacement increment for numerical Hessian (default: 0.005)

       scale=real
           Hessian scaling factor applied to the Hessian used for vibrational
           analysis, not to the Cartesian hessian output file (default: 1.0)

       isotope: int,real
           set mass of atom number int to real (synonym to modify mass)

       modify mass: int,real
           set mass of atom number int to real (synonym to isotope)

       scale mass: int,real
           scale mass of atom number int by real
           
       element mass: int,real
           set mass of elements int to real

       imagmin=real
           frequencies with magnitude below this imaginary frequency (cm⁻¹,
           given as negative value) are treated as numerical noise, used in
           the O1NumHess imaginary frequency repair and for the distorted
           structure printout (default -5.0)

       imagmax=real
           deepest imaginary frequency (cm⁻¹, given as negative value)
           considered in the O1NumHess imaginary frequency repair
           (default -200.0)

Changes regarding ``sccacc`` or ``step`` should be made with caution, as large displacements or loose SCC accuracy can lead to unreliable frequencies due to excessive numerical noise in the calculations.

O1NumHess calculations
_______________________

By invoking the ``--o1nh`` command line argument, ``xtb`` performs the
numerical Hessian calculation using the O1NumHess algorithm
(`JCTC <https://doi.org/10.1021/acs.jctc.5c01354>`_). Based on the
off-diagonal low rank (ODLR) property of molecular Hessians, the full
Hessian is reconstructed from local contributions and a low-rank
correction, requiring only O(1) gradient evaluations instead of the usual
O(3N) ones for the default semi-numerical Hessian. This makes
frequency calculations on large systems considerably cheaper.

.. code:: text

    xtb min.xyz --hess --o1nh

The ``--o1nh`` flag is used together with ``--hess``, ``--ohess``, or
``--bhess``, and respects the same ``$hess`` displacement ``step`` and
``sccacc`` controls described above. Frozen atoms and PTB calculations are not
supported (rejected with an error), whereas for single atoms O1NumHess is
disabled and ``xtb`` falls back to the default semi-numerical Hessian path.

Compliance constants
____________________

Hessian runs automatically perform a compliance analysis for nonperiodic
systems with more than one atom and no frozen atoms. The analysis uses the
redundant internal coordinates of the molecular graph: bond stretches,
angles, dihedrals, and two fixed-frame bend components for each near-linear
angle.

For every coordinate, the stdout table reports the diagonal compliance
element :math:`C_{ii}` in column ``C`` and its reciprocal
:math:`1/C_{ii}`, the relaxed force constant, in column ``1/C``.
Bond rows also report a local-mode frequency in column ``nu_loc/cm-1``.

For compliance constants and relaxed force constants, see
`Brandhorst and Grunenberg, Chem. Soc. Rev. 37 (2008), 1558
<https://doi.org/10.1039/B717781J>`_ and
`Grunenberg, Chem. Sci. 6 (2015), 4086
<https://doi.org/10.1039/C5SC01322D>`_.
For bond local-mode frequencies, see
`Zou et al., J. Mol. Model. 19 (2013), 2865
<https://doi.org/10.1007/s00894-012-1697-4>`_.

For bond force constants,
:math:`1\,E_\mathrm{h}/a_0^2 = 15.570\,\mathrm{N/cm}`.
This conversion does not apply to angular coordinates or mixed-coordinate
couplings. An exactly zero diagonal compliance is reported with
``1/C = +Infinity`` (zero response).

The stdout compliance table looks like this:

.. code-block:: text

        type                atoms                   coord value       C       1/C    nu_loc/cm-1
   ---------------------------------------------------------------------------------------------
     1 bond stretch  C   1   H  17                         2.06      3.11      0.32   3022.34
     2 bond stretch  C   1   H  15                         2.06      3.23      0.31   2964.55
     3 bond stretch  C   1   H  16                         2.06      3.24      0.31   2963.59

The corresponding ``compliance.dat`` block looks like this:

.. code-block:: text

   # coord bond C 1-H 17 C_ii =    3.110887458847E+000 1/C_ii =    3.214516800201E-001
   #   j  type atoms                C_ij
      71  dih  H 15-C 1-N 2-C 3       -3.442827458512E-001
      72  dih  H 15-C 1-N 2-C 6       -3.416049204564E-001
      74  dih  H 16-C 1-N 2-C 6        3.388026166467E-001

Here, coordinate :math:`i` is the C1–H17 bond identified in the block
header. The columns mean:

* ``j``: the other coordinate's number in the stdout table.
* ``type``: ``bond`` for a stretch, ``ang`` for an angle, ``dih`` for a
  dihedral, or ``lb1``/``lb2`` for the two linear-bend components.
* ``atoms``: element symbols and atom numbers defining coordinate
  :math:`j`.
* ``C_ij``: the signed off-diagonal compliance coupling between the
  block's coordinate :math:`i` and coordinate :math:`j`.

Each block lists up to 20 off-diagonal couplings, ordered by decreasing
absolute value. Couplings with absolute values below :math:`10^{-12}`
are omitted.

O1NumHess imaginary-mode repair
_______________________________

In the O1NumHess Hessian, small imaginary frequencies arising from
numerical noise can be repaired automatically by iteratively adding gradient
displacements along the projected imaginary modes. Two cutoffs control this
repair and can be adjusted via command line arguments or xcontrol options:

- ``--imagmin REAL``: magnitudes below this value (cm⁻¹) are treated as
  numerical noise and ignored (default 5)
- ``--imagmax REAL``: deepest imaginary frequency (cm⁻¹) considered in the
  repair, more deeply imaginary modes are left to the regular Hessian path
  (default 200)


The thermostatistical calculations can be influenced by the ``$thermo`` block of the ``xcontrol`` file.

.. code:: text

   $thermo
       temp=real
           temperature for thermostatistical calculation (default: 298.15 K)

       sthr=real
           rotor cut-off (cm-1) in thermo (default: 50.0)

       imagthr=real
           threshold (cm-1) for inverting imaginary modes in thermo (default: -20.0)

       scale=real
           frequency scaling factor for thermostatistical calculation (default: 1.0)

.. _SPH:

Single Point Hessian (SPH) calculations
_______________________________________________________

A prerequisite for accurate thermostatistics so far was to optimize the molecular input structures in order to avoid imaginary frequencies as discussed above.  This inevitably leads to changes in the geometry if different theoretical levels are applied for geometry optimization and frequency calculations. Therefore, we propose a new method termed single-point Hessian (SPH) for the computation of HVF and thermodynamic contributions to the free energy within the modified RRHO approximation for general nonequilibrium molecular geometries. The main publication for ``SPH`` can be found at: `JCTC <https://doi.org/10.1021/acs.jctc.0c01306>`_.
A SPH calculation is invoked using the ``--bhess [LEVEL]`` command line argument (where the optional optimization *LEVEL* defaults to ``normal``). *xTB* automatically applies a biasing potential given as Gaussian functions expressed with the RMSD in Cartesian space in order to retain the initial geometry. In the *xTB* printout this is indicated by e.g.:

.. code:: text

   metadynamics with 1 initial structures loaded
              -------------------------------------------------
             |           Optimal kpush determination           |
              -------------------------------------------------
   target rmsd / Å         0.100000
   unbiased initial rmsd   0.415461
 
   iter. min.        max.        rmsd       kpush
    1    0.000000    1.000000    0.023187   -0.500000
    2    0.000000    0.500000    0.052359   -0.250000
    3    0.000000    0.250000    0.090094   -0.125000
    4    0.000000    0.125000    0.135494   -0.062500
    5    0.062500    0.125000    0.108071   -0.093750
    6    0.093750    0.125000    0.098169   -0.109375
    7    0.093750    0.109375    0.102794   -0.101562
    8    0.101562    0.109375    0.100426   -0.105469
   final kpush: -0.105469
              -------------------------------------------------
             |            Biased Numerical Hessian             |
              -------------------------------------------------
   kpush                :  -0.10547
   alpha                :   1.00000
   step length          :   0.00500
   SCC accuracy         :   0.20000
   Hessian scale factor :   1.00000
   frozen atoms in %    :   0.00000    0
   RMS gradient         :   0.00028
   estimated CPU  time      0.06 min
   estimated wall time      0.02 min

   writing file <hessian>.
   
The systematic shift of the HVF caused by the modification of the PES due to the biasing potential is subsequently removed approximately by individual frequency scaling, giving access to accurate thermostatistical contributions for general nonequilibrium geometries with low-level methods.
The desired target RMSD between the input and constrained optimized structure can be influenced by the ``$metadyn`` block of the ``xcontrol`` file.

.. code:: text

   $metadyn
       rmsd=real
           target RMSD between input and optimized structure (default: 0.10 Å)
