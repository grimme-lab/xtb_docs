.. _version:

------------------------
 Versions and Changelog
------------------------

.. note:: Release candidates and beta versions are not listed here.

Version 6.0
   - GFN2-xTB GBSA parameteres added
   - internal parameter files
   - detailed input
   - ``XTBPATH`` variable
   - parallel Hessian with GBSA
   - logfermi wallpotential added
   - sdf input files supported
   - automatic Fukui indices and electrophilicity index

Version 6.0.1
   - additional GFN2-xTB GBSA parameteres added
   - Bugfix: ``molden.input`` could not be disabled
   - Bugfix: deallocation error in mode following

Version 6.0.2
   - Bugfix: timings wrapped around

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

Version 6.1.1
   - Bugfix: wrong constraining energy in ``xtbscan.log``
   - Bugfix: symmetry finder was inactive

Version 6.1.2
   - Bugfix: wrong convergence threshold for RF solver

Version 6.1.3
   - added FIRE and L-ANCopt as optimization engines
   - Bugfix: FOD calculation was using wrong density
   - Bugfix: parallisation error in GBSA

Version 6.2
   - coming soon
   - Bugfix: Fukui index calculation
