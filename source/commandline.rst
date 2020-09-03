.. _commandline:

-------------------
 Commandline Usage
-------------------

For ``xtb`` we usually enjoy to build our workflows via commandline,
so most tasks can be performed without ever writing any kind of input file
(except for the geometry input, of course).
The man page can be found `here <https://github.com/grimme-lab/xtb/blob/master/man/xtb.1.adoc>`_.

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
   :flag: ``--scc``
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
   :flag: ``--opt``
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

Simulated annealing
   :flag: ``--siman``
   :description:
     performs a number of simulated annealing steps on the input
     coordinates and tries to find a conformer ensemble.
     We recommend the CREST workflow (see :ref:`crest`) instead of this runtyp
     since it is faster and more reliable in finding the lowest conformer.
     **This runtyp has been deprecated and removed in version 6.2!**
   :queue:
     setup, SP, properties, MD, ANCopt, ...

Options
=======

-c, --chrg INT
    specify molecular charge as *INT*, overrides ``.CHRG`` file and ``xcontrol`` option

-u, --uhf INT
    specify Nalpha-Nbeta as *INT*, overrides ``.UHF`` file and ``xcontrol`` option

--gfn INT
    specify parametrisation of GFN-xTB (default = 2)

--etemp REAL
    electronic temperature (default = 300K)

-a, --acc REAL
    accuracy for SCC calculation, lower is better (default = 1.0)

--vparam FILE
    Parameter file for vTB calculation

--alpb SOLVENT [reference]
    analytical linearized Poisson-Boltzmann (ALPB) model,
    available solvents are *acetone*, *acetonitrile*, *aniline*, *benzaldehyde*,
    *benzene*, *ch2cl2*, *chcl3*, *cs2*, *dioxane*, *dmf*, *dmso*, *ether*,
    *ethylacetate*, *furane*, *hexandecane*, *hexane*, *methanol*, *nitromethane*,
    *octanol*, *woctanol*, *phenol*, *toluene*, *thf*, *water*.
    The solvent input is not case-sensitive.
    The Gsolv reference state can be chosen as *reference* or *bar1M* (default).

-g, --gbsa SOLVENT [reference]
    generalized born (GB) model with solvent accessable surface (SASA) model,
    available solvents are *acetone*, *acetonitrile*, *benzene* (only GFN1-xTB),
    *CH2Cl2*, *CHCl3*, *CS2*, *DMF* (only GFN2-xTB), *DMSO*, *ether*, *H2O*,
    *methanol*, *n-hexane* (only GFN2-xTB), *THF* and *toluene*.
    The solvent input is not case-sensitive.
    The Gsolv reference state can be chosen as *reference* or *bar1M* (default).

--cma 
    shifts molecule to center of mass and transforms cartesian coordinates
    into the coordinate system of the principle axis (not affected by
    ``isotopes``-file).

--pop
    requests printout of Mulliken population analysis (done by default)

--molden
    requests printout of molden file

--dipole
    requests dipole printout (done by default)

--wbo
    requests Wiberg bond order printout (done by default)

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

--citation
     print citation and terminate

--license
     print license and terminate

-v, --verbose
     be more verbose (not supported in every unit)

-s, --silent
     clutter the screen less (not supported in every unit)

--strict
     turns all warnings into hard errors

-h, --help
     show help page
