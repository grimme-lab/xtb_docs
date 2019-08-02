.. _version:

------------------------
 Versions and Changelog
------------------------

.. note:: Release candidates and beta versions are not listed here.

Version 6.2
   - Bugfix: Fukui index calculation
   - Bugfix: wrong forces in FIRE optimizer
   - Bugfix: $cube instructions were not read
   - Bugfix: Input error for ``$gbsa`` data group
   - GFN0-xTB Hamiltonian consistent with ChemRxiv preprint
   - periodic boundary conditions for GFN0-xTB
   - preliminary implicit solvation model GBSA for GFN0-xTB

Version 6.1.4
   - Bugfix: parallisation error in GBSA

Version 6.1.3
   - added FIRE and L-ANCopt as optimization engines
   - Bugfix: FOD calculation was using wrong density

Version 6.1.2
   - Bugfix: wrong convergence threshold for RF solver

Version 6.1.1
   - Bugfix: wrong constraining energy in ``xtbscan.log``
   - Bugfix: symmetry finder was inactive

Version 6.1
   - removed ``isotope`` input
   - Turbomole ``basis`` and ``mos`` printout
   - ORCA GBW file printout
   - metadynamics runtyp added
   - GFN0-xTB implemented
   - completely tunable model Hessian for optimizer
   - separated fixing and constraining
   - elementwise fixing and constraining
   - new geometry summary printout for optimizations
   - better printout for optimizer (RMSD, energy gain)
   - profiling printout for SCC and optimizer
   - adjustable SASA grid for GBSA
   - case insensitive solvent strings for GBSA
   - Bugfix: mode following printout crashes for large systems (>100 atoms)

Version 6.0.2
   - Bugfix: timings wrapped around

Version 6.0.1
   - additional GFN2-xTB GBSA parameteres added
   - Bugfix: ``molden.input`` could not be disabled
   - Bugfix: deallocation error in mode following

Version 6.0
   - GFN2-xTB GBSA parameteres added
   - internal parameter files
   - detailed input
   - ``XTBPATH`` variable
   - parallel Hessian with GBSA
   - logfermi wallpotential added
   - sdf input files supported
   - automatic Fukui indices and electrophilicity index
