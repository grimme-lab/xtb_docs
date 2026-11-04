.. _commandline:

-------------------
 Commandline Usage
-------------------

For ``xtb`` we usually enjoy to build our workflows via commandline,
so most tasks can be performed without ever writing any kind of input file
(except for the geometry input, of course).
The man page can be found `here <https://github.com/grimme-lab/xtb/blob/main/man/xtb.1.adoc>`_.

.. note:: If you need more control you should resort to the :ref:`detailed-input`
          file.

.. contents::

Runtypes
========

The most basic flags used in ``xtb`` are the runtypes. We have a bunch of
them, but many of the elaborate composite runtypes are constructed from
four basic runtypes: singlepoint (SP), geometry optimization (ANCopt),
frequency calculation (Hessian) and molecular dynamics (MD).
Every calculation performs the basic setup and at some (resonable) point
a property calculation.

Singlepoint
   :flag: ``--scc, --sp``
   :description:
     just self-consistent charge (SCC) calculation.
     See :ref:`sp` for details.
   :queue:
     setup, SP, properties

Gradient
   :flag: ``--grad``
   :description:
     self-consistent charge (SCC) calculation, afterwards energy and gradient
     will be printed in a Turbomole readable format
   :queue:
     setup, SP, properties

Vertical IP
   :flag: ``--vip``
   :description:
     vertical ionisation potential (IP), calculates SCC on input structure and
     than removes an electron to perform another SCC calculation.
   :queue:
     setup, SP, SP, properties

Vertical EA
   :flag: ``--vea``
   :description:
     vertical electron affinity (EA), calculates SCC on input structure and
     than adds an electron to perform another SCC calculation.
   :queue:
     setup, SP, SP, properties

Vertical IP and EA
   :flag: ``--vipea``
   :description:
     both IP and EA are calculate by removing and adding an electron, 
     respectively.
   :queue:
     setup, SP, SP, SP, properties

Global Electrophilicity Index
   :flag: ``--vomega``
   :description:
     global electrophilicity index from vertical IP and EA.
   :queue:
     setup, SP, SP, SP, properties

Fukui Indices
   :flag: ``--vfukui``
   :description:
     calculates Mulliken partial charges from the neutral, positive and
     negatively charged structure and calculates Fukui indices.
   :queue:
     setup, SP, SP, SP, properties

Dimer Projection Method DIPRO     
   :flag: ``--dipro [REAL]``
   :description:
     calculates the electronic coupling integrals J_ab and J_ab,eff between
     two fragments (requires tblite support). *REAL* sets the threshold for
     nearly degenerate orbitals to still be considered (default = 0.1 eV).
   :queue:
     setup, SP, SP, SP, properties

Electrostatic Potential
   :flag: ``--esp``
   :description:
     calculate electrostatic potential on VdW-grid
   :queue:
     setup, SP, properties (with ESP calculation)

STM picture
   :flag: ``--stm``
   :description:
     simulate a STM measurement (molecule should be aligned to xy-plane)
   :queue:
     setup, SP, properties (with STM calculation)

Geometry optimization
   :flag: ``-o, --opt [LEVEL]``
   :description:
     approximate normal coordinate optimization, performs an initial singlepoint
     calculation and a final singlepoint calculation on the optimized structure.
     See :ref:`geometry optimization` for details.
   :queue:
     setup, SP, ANCopt, SP, properties

Minimum Hopping
   :flag: ``--metaopt``
   :description:
     try to find conformers by geometry optimization, for each minimum located
     a bias potential is generated to push the optimizer to another local minimum.
   :queue:
     setup, SP, ANCopt, SP, properties, ANCopt, ...

Guided Path Finder
   :flag: ``--path [file]``
   :description:
     apply a bias potential between the input and final geometry (from `file`)
     and force the geometry optimizer to generate a path between the two structures.
   :queue:
     setup, SP, properties, ANCopt, ...

Modefollowing
   :flag: ``--modef mode``
   :description:
     follow ``mode`` which specifies the nth eigenmode from a previously done
     frequency calculation.
   :queue:
     setup, SP, properties, ANCopt, ...

Frequency calculation
   :flag: ``--[o]hess``
   :description:
     second derivative calculation, see :ref:`frequencies`
   :queue:
     setup, SP, [ANCopt, SP,] SP, Hessian, properties

Biased frequency calculation
   :flag: ``--bhess [LEVEL]``
   :description:
     perform a biased numerical Hessian calculation on an optimized geometry,
     tightens default accuracy setting to 0.2 (overwritten by ``--acc`` flag)
   :queue:
     setup, SP, ANCopt, SP, SP, Hessian, properties

O1NumHess frequency calculation
   :flag: ``--o1nh``
   :description:
     perform the numerical Hessian calculation using the ODLR approximation
     (O1NumHess), which requires only O(1) gradient calculations, see
     :ref:`frequencies`. Not compatible with frozen atoms, PTB or single atoms.
   :queue:
     setup, SP, Hessian, properties

Molecular dynamics
   :flag: ``--[o]md``
   :description:
     molecular dynamics simulation, see :ref:`md` for details
   :queue:
     setup, SP, [ANCopt, SP,] properties, MD

Metadynamics
   :flag: ``--metadyn [snapshots]``
   :description:
     activates metadynamics simulation on start geometry, where
     ``snapshots`` is the number of structures from the trajectory
     should be used in the biasing potential.
     See :ref:`mtd` for details.
   :queue:
     setup, SP, properties, MD

Options
=======

-c, --chrg INT
    specify molecular charge as *INT*, overrides ``.CHRG`` file and ``xcontrol`` option

--ceh [grad [REAL]]
    calculate CEH (Charge-Extended Hückel model) charges and write them to
    the ``ceh.charges`` file (requires tblite support); optionally, calculate
    numerical gradients and write them to the ``ceh.charges.numgrad`` file with
    an adjustable step size for the numerical gradients.

-u, --uhf INT
    specify number of unpaired electrons as *INT*, overrides ``.UHF`` file and
    ``xcontrol`` option (default: 0 (singlet) for even electron number, 1
    (doublet) for odd electron number)

--gfn INT
    specify parametrisation of GFN-xTB (default = 2)

--gfnff, --gff
    specify parametrisation of GFN-FF

--tblite
    use tblite library as implementation for xTB

--ptb
    performs single-point calculation with the density tight-binding method
    PTB. Provides electronic structure and properties, such as, e.g., atomic
    charges, bond orders, and dipole moments. PTB itself does not provide
    total energies, nuclear gradients, or an energetic Hessian; Hessian
    workflows combine GFN2-xTB with PTB response properties.

--spinpol
    enables spin-polarization for xTB methods (tblite required)

--oniom [METHOD] LIST
    use subtractive embedding via ONIOM method. *METHOD* is given as
    ``inner:outer`` (default: ``gfn2:gfnff``) where ``inner`` can be ``orca``,
    ``turbomole``, ``gfn2``, ``gfn1``, or ``gfnff`` and ``outer`` can be
    ``gfn2``, ``gfn1``, or ``gfnff``. The inner region is given as
    comma-separated indices directly in the commandline or in a file with each
    index on a separate line. Combine with ``--tblite`` to evaluate ``gfn2``
    and ``gfn1`` with the tblite library.

--cut
    create inner region for ``--oniom`` calculation without performing any
    calculation

--efield REAL,REAL,REAL
    static electric field in Cartesian coordinates (in atomic units), overrides
    ``.EFIELD`` file, works only via tblite for the xTB Hamiltonians, or with
    GFN-FF and PTB.

--etemp REAL
    electronic temperature (default = 300K)

-a, --acc REAL
    accuracy for SCC calculation, lower is better (default = 1.0), overwrites
    also tighter defaults for optimization and hessians

--iterations, --maxiterations INT
    maximum number of SCC iterations per single point calculation (default = 250)

--vparam FILE
    Parameter file for xTB calculation

--alpb SOLVENT [STATE]
    analytical linearized Poisson-Boltzmann (ALPB) model,
    available solvents are *acetone*, *acetonitrile*, *aniline*, *benzaldehyde*,
    *benzene*, *ch2cl2*, *chcl3*, *cs2*, *dioxane*, *dmf*, *dmso*, *ethanol*, *ether*,
    *ethylacetate*, *furane*, *hexadecane*, *hexane*, *methanol*, *nitromethane*,
    *octanol*, *woctanol*, *phenol*, *toluene*, *thf*, *water*.
    The solvent input is not case-sensitive.
    The Gsolv reference state can be chosen as *reference*, *bar1M*, or *gsolv* (default).

-g, --gbsa SOLVENT [STATE]
    generalized born (GB) model with solvent accessible surface (SASA) model,
    available solvents are *acetone*, *acetonitrile*, *benzene*,
    *CH2Cl2*, *CHCl3*, *CS2*, *DMF* (only GFN2-xTB), *DMSO*, *ether*, *H2O*,
    *methanol*, *n-hexane* (only GFN2-xTB), *THF* and *toluene*.
    The solvent input is not case-sensitive.
    The Gsolv reference state can be chosen as *reference*, *bar1M*, or *gsolv* (default).

--gbe SOLVENT/EPSILON
    generalized Born for finite epsilon (GBe) solvation model (tblite
    required), includes only the electrostatic solvation contribution (no
    SASA model), available solvents are all solvents that are available for
    alpb. Additionally, the dielectric constant can be set manually.

--gb SOLVENT/EPSILON
    generalized Born (GB) solvation model (tblite required), includes only
    the electrostatic solvation contribution (no SASA model), available
    solvents are all solvents that are available for alpb. Additionally, the
    dielectric constant can be set manually.

--cosmo SOLVENT/EPSILON
    domain decomposition conductor-like screening model (ddCOSMO), available
    solvents are all solvents that are available for alpb. Additionally, the
    dielectric constant can be set manually or an ideal conductor can be
    chosen by setting epsilon to infinity. Available via tblite (requires ddX
    support) or in the native legacy version in xtb lacking density response
    to the continuum polarization (non-variational).

--cpcm SOLVENT/EPSILON
    domain decomposition conductor-like polarizable continuum solvation model
    (ddCPCM), available solvents are all solvents that are available for
    alpb. Additionally, the dielectric constant can be set manually or an
    ideal conductor can be chosen by setting epsilon to infinity. Requires
    tblite with ddX support.

--pcm SOLVENT/EPSILON
    domain decomposition polarizable continuum model (ddPCM), available
    solvents are all solvents that are available for alpb. Additionally, the
    dielectric constant can be set manually or an ideal conductor can be
    chosen by setting epsilon to infinity. Requires tblite with ddX support.

--tmcosmo SOLVENT/EPSILON
    same as ``--cosmo``, but uses TM convention for writing the ``.cosmo``
    files (native xTB only).

--cpcmx SOLVENT
    extended conductor-like polarizable continuum solvation model (CPCM-X,
    requires CPCM-X support; parameterized for GFN2-xTB; single-point only,
    not available for geometry optimization or with GFN-FF), available
    solvents are all solvents included in the Minnesota Solvation Database.

--cma 
    shifts molecule to center of mass and transforms cartesian coordinates
    into the coordinate system of the principal axis (not affected by
    ``isotopes``-file).

--pop
    requests printout of Mulliken population analysis (enabled by default)

--molden
    requests printout of molden file (name: ``molden.input``), format differs
    with ``--tblite``

--dipole
    requests dipole printout (enabled by default)

--quadrupole
    requests quadrupole printout for tblite calculations

--raman [TEMP [WAVELENGTH]]
    requests Raman spectrum calculation via combination of GFN2-xTB and PTB
    (requires ``--ptb``) using the temperature *TEMP* in K (default 298.15 K)
    and the wavelength of the incident laser *WAVELENGTH* in nm (default 514 nm).

--alpha
    requests the extension of electrical properties to static molecular
    dipole polarizabilities (requires ``--ptb``).

--cycles INT
    maximum number of optimization cycles

--o1nh
    perform the numerical Hessian calculation using the ODLR approximation
    (O1NumHess), see :ref:`frequencies`

--imagmin REAL
    magnitudes below this value (cm⁻¹) are ignored in the O1NumHess
    imaginary frequency repair (default 5)

--imagmax REAL
    deepest imaginary frequency (cm⁻¹) considered in the O1NumHess
    imaginary frequency repair (default 200)

--wbo
    requests Wiberg bond order printout (enabled by default)

--lmo
    requests localization of orbitals

--fod
    requests FOD calculation

-I, --input FILE
     use *FILE* as input source for ``xcontrol(7)`` instructions

--namespace STRING
     give this ``xtb(1)`` run a namespace. All files, even temporary
     ones, will be named according to *STRING*.

--copy, --nocopy
     copies the ``xcontrol`` file at startup (default = false)

--restart, --norestart
     restarts calculation from ``xtbrestart`` (default = true)

-P, --parallel INT
     number of parallel processes

--define
     performs automatic check of input and terminate

--json
     write ``xtbout.json`` file for native xTB and PTB calculations

--citation
     print citation and terminate

--license
     print license and terminate

-v, --verbose
     be more verbose (not supported in every unit)

-s, --silent
     clutter the screen less (not supported in every unit)

--ceasefiles
     reduce the amount of output and files written

--strict
     turns all warnings into hard errors

-h, --help
     show help page
