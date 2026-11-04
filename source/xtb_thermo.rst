.. _xtb_thermo:

------------------
 Thermo Submodule
------------------

.. note::
   This feature is only present in version 6.3 and newer

The thermo submodule allows to evaluate thermodynamic functions from data produced
in previous calculations, generally the results are equivalent to the ones
produced in a frequency calculation in ``xtb``.

To run the thermo submodule use

.. code-block:: text

   xtb thermo [options] <geometry> [hessian]

The geometry can be any format accepted by ``xtb``, while the hessian format
depends on the options passed to the thermo submodule.
It defaults to a Cartesian, non-mass-weighted Turbomole Hessian data group,
as written by ``xtb`` to the ``hessian`` file in a frequency calculation.
The thermo submodule mass-weights this Hessian itself; it does not project
out translations and rotations for Turbomole input, so the Hessian should
already be projected (the ``hessian`` file written by ``xtb`` is).

Possible options are:

--sthr REAL
   Rotor cutoff for RRHO partition function in rcm (default: 50.0)

--ithr REAL
   Imaginary frequency cutoff for RRHO in rcm,
   enter a positive value, the sign will be inverted (default: 20.0)

--scale REAL
   Frequency scaling factor for RRHO (default: 1.0)

--desy REAL
   Threshold for symmetry recognition (default: 0.1)

--temp REAL[,...]
   Temperature for thermodynamic functions in K,
   takes a comma separated list of temperatures (default: 298.15)

--turbomole
   Read a non-mass-weighted Turbomole Hessian file (default format)

--mass-weighted, --massweighted
   Assume the input Hessian is already mass-weighted, so no mass-weighting
   is applied before diagonalization.
   Can be combined with any of the Hessian formats.

--orca
   Read an Orca .hess file, projection will be performed automatically

--dftb+, --dftbplus
   Read a DFTB+ hessian.out file, projection will be performed automatically

--bhess
   Read an additional ``hessian_sph`` file from a previous ``xtb`` SPH calculation
   (``--bhess``) and remove the frequency shift caused by the biasing potential,
   usage is ``xtb thermo --bhess <geometry> hessian hessian_sph``

.. note::
   Up to version 6.7.1, ``--turbomole`` also marked the Hessian as mass-weighted.
   Since then, ``--turbomole`` only selects the file format, and a
   mass-weighted Hessian (e.g. from a Turbomole calculation without
   ``$nomw`` in the ``control`` file) has to be requested explicitly with
   ``--mass-weighted``.

An example calculation is given here, reevaluating at a different rotor cutoff
for multiple temperatures

.. code-block:: text

   xtb benzene.xyz --namespace benzene --ohess
   ...
   xtb thermo --sthr 100 --temp 50,100,150,200,250,300 benzene.xtbopt.xyz benzene.hessian
   ...

   Molecule has the following symmetry elements: (i) (C6) (C3) 7*(C2) (S6) (S3) 7*(sigma)
   It seems to be the D6h point group
   d6h symmetry found (for desy threshold:  0.10E+00) used in thermo

   ...

          T/K    H(0)-H(T)+PV         H(T)/Eh          T*S/Eh         G(T)/Eh
      ------------------------------------------------------------------------
        50.00    0.633464E-03    0.981387E-01    0.362609E-02    0.945126E-01
       100.00    0.128528E-02    0.987905E-01    0.815319E-02    0.906373E-01
       150.00    0.203561E-02    0.995409E-01    0.131373E-01    0.864036E-01
       200.00    0.296494E-02    0.100470E+00    0.185801E-01    0.818901E-01
       250.00    0.413423E-02    0.101639E+00    0.245239E-01    0.771156E-01
       300.00    0.558192E-02    0.103087E+00    0.310070E-01    0.720801E-01 (used)
      ------------------------------------------------------------------------

   ...
