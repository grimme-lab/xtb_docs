.. _CENSO:

=====================
Introduction to CENSO
=====================

.. figure:: ../../figures/CENSO/censo_logo_300dpi.png
	:scale: 40%
	:align: center
	:alt: CENSO logo

**C**\ommandline **E**\nergetic **SO**\rting (**CENSO**) is a sorting algorithm 
for efficient evaluation of **S**\tructure **E**\nsembles (**SE**). 

.. contents::

General information
-------------------

CENSO can be structured into two components:

1. Ensemble optimization,
2. Ensemble property calculations.

The first part (ensemble optimization) can use up to four steps:

1. Prescreening,
2. Screening,
3. (Geometry-)Optimization,
4. Refinement.

In these steps, the ensemble is optimized, using increasingly accurate settings.

Ensemble properties available for calculation are:

1. NMR spectra,
2. Optical Rotation,
2. UV/Vis spectra.

In the property calculation steps the ensemble is not further modified. However, they require at least 
one ensemble optimization step to be run beforehand for energy rankings and Boltzmann populations.
 
For now, all ensemble optimization steps can be performed using both ORCA and TURBOMOLE for DFT calculations.
Which program is available for property calculations currently depends on the property: 
NMR supports ORCA and TURBOMOLE, OR supports TURBOMOLE, UV/Vis supports ORCA.

All output will be provided in formatted text files as well as in json format.

Installation
------------

To install CENSO, simply clone the `github repository <https://github.com/grimme-lab/CENSO>`_. 
Then, you have two options:

1. install *via* ``pip``,
2. setup usage *via* ``$PATH``.

To install in your current environment (e.g. after creating a new conda environment using ``conda create -f environment.yaml``) using ``pip``, run:

.. code:: sh 

    pip install .

or

.. code:: sh
    
    pip install censo

CENSO can then be called using ``censo``.

Alternatively, after cloning the repository, you could also add the CENSO directory to your ``$PATH``.
In this directory, there is a helper script called ``censo``, calling the command line entry point of CENSO
as if you were calling it using ``python3 -m censo``. Additionally, you would want to add ``CENSO/src`` to 
your ``$PYTHONPATH``.

Configuration
-------------

To configure CENSO in the command line version, you can use a mix of command line arguments and a configuration file, 
which can be newly generated using 

.. code:: sh

    censo --new-config

This will create a new configuration file containing default parameters.
An example configuration file is also provided as ``example.censo2rc``.
You can then adapt the settings to your needs.
The configuration file can then be either moved into the ``$HOME`` directory with the name ``.censo2rc``, which will result 
in this file being used by default for the CLI tool. Otherwise, you can use a specific file by invoking:

.. code:: sh

    censo --inprc /path/to/rcfile

When you need to configure CENSO with the Python API, you can proceed like this:

.. code:: python

    from censo.config import PartsConfig
    config = PartsConfig()

This will provide a configuration instance that needs to be passed to the separate functions like ``prescreening``.
There is no assignment validation since setting multiple settings at once is not possible, except on initialization.
Therefore, the user needs to make sure that the settings are valid, e.g. by using ``model_validate``.
However, before each ensemble optimization/property function is run, the specific settings are validated automatically.
It should not be possible to run CENSO with invalid settings if the settings are actually being used.

Requirements
------------

CENSO requires xTB in version 6.4.0 or above. In order to use ORCA, it should be installed in version
4.x or above. TURBOMOLE has been tested with version 7.7.1, a bug when combining D4 and GCP is accounted for. 
It is recommended to use CREST for initial ensemble generation, as well as for better 
interfacing with ANMR for ensemble NMR spectra calculation.
However, any ensemble input can be used as long as the file format conforms to xyz-format (e.g. the GOAT output works OOTB).

CENSO requires Python >= 3.12, ``pydantic`` and the ``tabulate`` package.
To use the ``nmrplot``/``uvvisplot`` scripts, ``numpy``, ``matplotlib`` and ``pandas`` are required.

New features since CENSO 2.0.0
------------------------------

Python API
==========

CENSO now implements a fully modular Python API. The user can use the API to run CENSO from within Python
and create custom workflows that go beyond the funnel-like approach of previous versions.
Below is an example of how to use the API:

.. code:: python

    from censo.ensemble import EnsembleData
    from censo.config.setup import configure
    from censo.ensembleopt import prescreening, screening, optimization
    from censo.properties import nmr
    from censo.config import PartsConfig
    from censo.config.parallel_config import ParallelConfig
    from censo.parallel import get_client

    # CENSO outputs files in the current working directory (os.getcwd())
    # When called from the CLI version, the output dir will be the same as the input file's location
    input_path = "rel/path/to/your/inputfile"  # Relative to working directory
    ensemble = EnsembleData()
    ensemble.read_input(input_path)

    # For charged/open-shell systems:
    # ensemble = EnsembleData()
    # ensemble.read_input(input_path, charge=-1, unpaired=1)

    # Load a custom rcfile (optional)
    config = configure(rcpath="/path/to/rcfile")

    # Configure parallelization
    parallel_config = ParallelConfig(ncores=os.cpu_count(), ompmin=4)
    # ompmin denotes the minimum number of OMP threads assigned to each task

    # Ensure valid configuration
    config.general.solvent = "dmso"
    config = config.model_validate(config)

    # Set up task management
    client, cluster = get_client(parallel_config)

    # Execute workflow steps
    results = [
        part(ensemble, config, parallel_config, client=client)
        for part in [prescreening, screening, optimization, nmr]
    ]

    # The results are then also output to json files in the working directory
    # The molecules stored in the ensemble contain the most up-to-date energy values and geometries


.. hint:: 
   By default, CENSO will always print information about what it's doing to stdout, as well as logging additional information 
   in the file ``censo.log``. It is not possible to call CENSO silently from command line, however you could redirect 
   stdout if you need a silent run. If you want a silent CENSO run from within Python you could use a context manager 
   to redirect stdout. To disable logging from the command line use ``--loglevel NONE``. With Python you can use 
   ``censo.logging.set_loglevel("NONE")`` or ``censo.logging.set_loglevel(51)`` (which corresponds to ``python.logging.CRITICAL + 1``).


Template files
==============

Since 2.0, CENSO supports template input files for all steps. They are located in ``$HOME/.censo2_assets``.
Since 3.0, template files are also supported for TURBOMOLE.
In order to use a template file for e.g. prescreening with ORCA, the file should be called ``prescreening.orca.template``.
It should contain two keywords: ``{main}`` and ``{geom}``. These are later replaced by the main argument line and the geometry
block, respectively. All further settings you add are inserted at the respective positions you put them in the
template file. Example:

.. code::
   {main}
   ! notrah

   {geom}
   # some comment
   
will yield:

.. code::
   ! pbe-d4 def2-sv(p) def2/j ri defgrid1 loosescf gcp(dft/sv(p)) printgap
   ! notrah
   ...
   * xyz 0 1 
   ...
   *
   # some comment

For TURBOMOLE, the file should be called ``prescreening.tm.template`` and the template file's content will just be inserted above the final ``$end`` line.

.. hint::
   Be careful when writing templates since generated input files will not be checked for validity.


Ensemble Optimization
---------------------

Prescreening
============

The first step after generating an ensemble of the most important conformers, e.g. using CREST, 
the number of which can range in the hundreds, is to improve on the preliminary
ranking using a lightweight DFT method. This should usually already yield significant
improvements compared to the preliminary ranking, usually obtained using SQM/FF methods.
In the case that solvation effects should be included, CENSO will use ``xtb`` to 
calculate the energy of solvation using the ALPB or GBSA solvation model. The threshold
for this step should be rather high (up to 10 kcal/mol).

Screening
=========

After prescreening the ensemble in the first step, this step is supposed to further 
improve on the ranking quality by increasing the quality of the utilized DFT method.
Also, in this step one may choose to include thermal contributions to the free enthalpy
by activating ``evaluate_rrho``, which will lead to CENSO using ``xtb`` to calculate
single-point Hessians. This will also include solvation if the user chose to do so.
The threshold for this step should be lower than before to account
for the decreasing uncertainty due to improvements in the ranking method. CENSO will 
increase the threshold by up to 1 kcal/mol, depending on the standard deviation of the 
thermal contributions. The solvation contributions will be calculated using DFT.
If you explicitly need the values of the solvation free enthalpy, 
you need to set ``gsolv_included`` to ``False``.

Optimization
============

To further improve the ranking, the geometries of the conformers in this step will be 
optimized using DFT gradients. For this, the ``xtb`` optimizer will be used as driver.
Solvation effects will be included implicitly. Furthermore, thermal contributions will
be included for the ranking if ``evaluate_rrho`` is set to ``True``. One can also utilize
a macrocycle optimizer in CENSO (set ``macrocycle`` to ``True``). This will run a number
(``optcycles``) of geometry optimization steps (microcycles) for every macrocycle and 
update the ensemble every macrocycle. The single-point Hessian evaluation using ``xtb`` 
will take place once after at least 6 microcycles and once after finishing the last
macrocycle. The energy threshold will be applied once the gradient norm of a conformer is below a
specified threshold (``gradthr``) for all the microcycles in the current macrocycle.

Refinement
==========

After geometry optimization of the ensemble, a high-level DFT calculation should be performed,
to obtain highly accurate single-point energies. In this step, the threshold is also 
more rigorous, using a Boltzmann population cutoff. The sorted (from highest to lowest)
populations (in %) of the conformers after calculating the high-level single-point are 
summed up until reaching the defined threshold, removing all further conformers from
consideration.

Ensemble Properties 
-------------------

NMR Spectra
===========

For the calculation of the NMR spectrum of an ensemble, single-points to compute the nuclear shieldings and couplings will be executed.
After that, CENSO can generate files for the simulation of the NMR spectrum using ANMR.
For this, you can use the ``c2anmr`` tool.

For more detailed instructions see :ref:`nmr`.

Optical Rotatory Disperson
==========================

CENSO will use TURBOMOLE to calculate the optical rotatory dispersion for the ensemble.
It will output the rotatory dispersion values averaged over the ensemble in length and velocity representation.
The separate conformer values can be found in the json output.

UV/Vis Spectra
==============

To calculate the ensemble UV/Vis spectrum, CENSO will run single-points to calculate the excitation
wavelengths and oscillator strengths using TD-DFT. For this, it is important to choose an appropriate 
number of roots sought (``nroots``). After finishing, CENSO will output the population weighted
excitation parameters to ``excitations.out`` in tabular format and to ``excitations.json`` for convenience.
The table contains all weighted excitation wavelengths together with their maximum extinction coefficients 
and the originating conformer.

To plot the spectra, the tool ``uvvisplot`` can be used. It needs to be provided with a file of the same structure as ``excitations.json``.
It outputs a file called ``contributions.csv`` which contains all Gaussian signals partitioned by conformer and state.
