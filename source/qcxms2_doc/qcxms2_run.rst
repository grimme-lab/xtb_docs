.. _qcxms2_run:

--------------------
Settings for QCxMS2
--------------------

Settings of a QCxMS2 run can be changed via the following command line options:

.. code::

   qcxms2 [INPUT] [OPTIONS]

.. list-table:: General Settings
   :widths: 30 100
   :header-rows: 1

   * - Command
     - Description
   * - ``-h, --help`` 
     - Print an overview of most available options (i.e., more or less this site completely).
   * - **Runtype Options**:
     -
   * - ``-ei``    
     - Compute an electron impact (EI) spectrum (default).
    * - ``-cid``    
     - Compute a collission-induced dissociation (CID) spectrum.  
   * - ``-esim``  
     - Simulate only different IEE distributions for a given reaction network from a previous calculation.
   * - ``-oplot``  
     - Plot only peaks from the previous QCxMS2 calculation.
   * - **General and Technical Options**:
     -
   * - ``-chrg [int]``
     - Give charge of the input molecule.
   * - ``-edist`` 
     - Choose distribution for IEE ("poisson" (default) or "gaussian").
   * - ``-eimp0 [real]`` 
     - Specify impact energy of electrons in eV (default: 70).
   * - ``-eimpw [real]`` 
     - Specify width of energy distribution (default: 0.1).
   * - ``-ieeatm [real]`` 
     - Specify energy per atom in eV (default: 0.8), works only for EI mode.
   * - ``-esiatom [real]`` 
     - Specify energy per atom in eV, works only for CID mode.
   * - ``-esi [real]``
     - increase average internal energy in eV for CID mode (default 0.0)
   * - ``esiw [real]``
     - Specify width of ionization energy distribution in eV, works only for CID mode.
   * - ``-nots`` 
     - Take reaction energy instead of barrier → no path search (quick mode for fragments).
   * - ``-T [integer]``
     - Select the number of overall cores (default: 4). 
   * - ``-nfrag [integer]``
     - Select the number of subsequent fragmentations you want to simulate (default: 6). 
   * - ``-pthr [real]`` 
     - Specify the intensity at which a fragment is further fragmented in % (default: 1%).

.. list-table:: Quantum Chemical calculation settings
   :widths: 30 100
   :header-rows: 1

   * - **Options for QC Calculations**:
     -
   * - ``-nebnormal`` 
     - Use normal settings instead of loose settings for NEB search.
   * - ``-notsgeo``
     - Do not use geodesic interpolation as a guess for restarting non-converged NEB runs. 
   * - ``-tsnodes [integer]`` 
     - Select the number of nodes for the path (default: 9).  
   * - ``-geolevel [method]``
     - Method for geometry optimization and path search.
   * - ``-tslevel [method]``
     - Select level for computing reaction barriers.
   * - ``-iplevel [method]``
     - Select level for computing IPs for charge assignment.
   * - ``-ip2level [method]`` 
     - Select level for computing IPs for charge assignment of critical cases with close IPs (difference below 2 eV).
   
The default for all levels is GFN2-xTB.
The following methods are available for all four different levels:

+-------------+----------------------+-------------+
| **Keyword** | **QM Method**        | **Program** |
+-------------+----------------------+-------------+
| gfn2        | GFN2-xTB             | xtb         |
+-------------+----------------------+-------------+
| gfn1        | GFN1-xTB             | xtb         |
+-------------+----------------------+-------------+
| r2scan3c    | r2SCAN-3c            | ORCA        |
+-------------+----------------------+-------------+
| pbeh3c      | PBEh-3c              | ORCA        |
+-------------+----------------------+-------------+
| wb97x3c     | wB97X-3c             | ORCA        |
+-------------+----------------------+-------------+
| pbe0        | PBE0-D4/def2-TZVP    | ORCA        |
+-------------+----------------------+-------------+
| pbe0matzvp  | PBE0-D4/ma-def2-TZVP | ORCA        |
+-------------+----------------------+-------------+

.. note::
  The recommended settings are GFN2-xTB for geometry optimizations (``-geolevel``) and IP prescreening (``-iplevel``)
  and the efficient range-seperated hybrid wB97X-3c for barrier and energy calculations and refinement of 
  close IPs (``-tslevel`` and ``-ip2level``). Using DFT for the geometry optimization
  is very expensive and only recommend for very small systems. For the prescreening of IPs, GFN2-xTB is 
  more moste cases sufficient. However, for difficult systems, e.g., negative ions, DFT should be used.
  The hybrid functional PBE0-D4 with mininimal augmented basis set (``pbe0matzvp``) is recommended here both for 
  ``-iplevel`` and ``-ip2level``.


.. list-table:: Further Settings
   :widths: 30 100
   :header-rows: 1

   * - **Options for Fragment Generation with CREST msreact**:
     -
   * - ``-msnoiso`` 
     - Sort out non-dissociated fragments in CREST msreact (i.e., rearranged structures). 
   * - ``-msiso`` 
     - Sort out dissociated fragments in CREST msreact (i.e., rearranged structures). 
   * - ``-msfulliso`` 
     - Allow rearrangements for subsequent fragmentations in CREST msreact (activated by default).
   * - ``-msnoattrh``
     - Deactivate the attractive potential between hydrogen and LMO centers.
   * - ``-msnshifts [int]`` 
     - Perform `n` optimizations with randomly shifted atom positions (default: 0). 
   * - ``-msnshifts2 [int]`` 
     - Perform `n` optimizations with randomly shifted atom positions and a repulsive potential applied to bonds (default: 0). 
   * - ``-msnbonds [int]``
     - Maximum number of bonds between atom pairs for applying a repulsive potential (default: 3).
   * - ``-msmolbar`` 
     - Sort out topological duplicates by molbar codes (activated by default—requires "molbar").
   * - ``-msinchi`` 
     - Sort out topological duplicates by InChI codes (requires "obabel").
   * - ``-msnfrag [int]`` 
     - Number of fragments printed by msreact (random selection).
   * - ``-msfragdist [real]`` 
     - Separate fragments before TS search from each other (default: 2.5 Å). 
   * - **Special Options**:
     -
   * - ``-noKER`` 
     - Do not compute kinetic energy release (KER). 
   * - ``-usetemp``
     - Use G_mRRHO contributions instead of only ZPVE (ZPVE is default).  
   * - ``-scaleeinthdiss [real]`` 
     - Decrease the internal energy only for -H or -H2 abstractions (default: 0.5).
   * - ``-nsamples [int]`` 
     - Number of simulated runs in Monte Carlo (default: 10E05).
   * - ``-rrkm``
     - Compute rate constants via the RRKM equation instead of Eyring (default: Eyring).
   * - ``-sthr [int]``
     - RRHO cutoff for thermo contributions (default: 150 cm⁻¹).
   * - ``-ithr [int]``
     - Imaginary RRHO cutoff for thermo contributions (default: 100 cm⁻¹).
   * - ``-nthermosteps [int]``
     - Number of increments to compute thermal corrections for the IEE distribution (default: 200; take a multiple of 10).

     


