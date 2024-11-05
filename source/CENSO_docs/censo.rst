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
2. UV/Vis spectra.

In the property calculation steps the ensemble is not further modified. However, they require at least 
one ensemble optimization step to be run beforehand for energy rankings and Boltzmann populations.
 
For now, all ensemble optimization steps can be performed using both ORCA and TURBOMOLE for DFT calculations.

All output will be provided in formatted text files as well as in json format.

Installation
------------

To install CENSO, simply clone the `github repository <https://github.com/grimme-lab/CENSO>`_. 
Then, you have two options:

1. install *via* ``pip``,
2. setup usage *via* ``$PATH``.

To install in your current environment using ``pip``, run:

.. code:: sh 

    pip install .

CENSO can then be run by configuring a custom runner script (see below) or by calling 
``python3 -m censo`` in the terminal.

Alternatively, after cloning the repository, you could also add the ``bin`` folder to your ``$PATH``.
In this directory, there is a helper script called ``censo``, calling the command line entry point of CENSO
as if you were calling it using ``python3 -m censo``. Additionally, you would want to add ``CENSO/src`` to 
your ``$PYTHONPATH``.

Requirements
------------

CENSO requires xTB in version 6.4.0 or above. In order to use ORCA, it should be installed in version
4.x or above. TURBOMOLE has been tested with version 7.7.1, a bug when combining D4 and GCP is accounted for. 
It is recommended to use CREST for initial ensemble generation, as well as for better 
interfacing with ANMR for ensemble NMR spectra calculation.

CENSO requires Python >= 3.10, there are no further dependencies. To use the ``uvvisplot`` script 
``numpy`` and ``pandas`` are required.

New features in CENSO 2.0.0
---------------------------

Usage *via* runner script
=========================

It is possible to run CENSO from a custom runner script. An example might look like this:

.. code :: python

    from censo.ensembledata import EnsembleData
    from censo.configuration import configure
    from censo.ensembleopt import Prescreening, Screening, Optimization
    from censo.properties import NMR
    from censo.params import Config

    # CENSO will put all files in the current working directory (os.getcwd())
    input_path = "rel/path/to/your/inputfile" # path relative to the working directory
    ensemble = EnsembleData(input_file=input_path) 
    # the above can be used if you molecule is neutral and closed shell, otherwise
    # it is necessary to proceed with e.g.
    # ensemble = EnsembleData()
    # ensemble.read_input(input_path, charge=-1, unpaired=1)

    # If the user wants to use a specific rcfile:
    configure("/abs/path/to/rcfile")

    # Get the number of available cpu cores on this machine
    # This is also the default value that CENSO uses
    # This number can also be set to any other integer value and automatically checked for validity
    Config.NCORES = os.cpu_count()

    # Another possibly important setting is OMP, which will get used if you disabled the automatic 
    # load balancing in the settings
    Config.OMP = 4

    # The user can also choose to change specific settings of the parts
    # Please take note of the following:
    # - the settings of certain parts, e.g. Prescreening are changed using set_setting(name, value)
    # - general settings are changed by using set_general_setting(name, value) (it does not matter which part you call it from)
    # - the values you want to set must comply with limits and the type of the setting
    Prescreening.set_setting("threshold", 5.0)
    Prescreening.set_general_setting("solvent", "dmso")

    # It is also possible to use a dict to set multiple values in one step
    settings = {
        "threshold": 3.5,
        "func": "pbeh-3c",
        "implicit": True,
    }
    Screening.set_settings(settings, complete=False)  
    # the complete kwarg tells the method whether to set the undefined settings using defaults or leave them on their current value


    # Setup and run all the parts that the user wants to run
    # Running the parts in order here, while it is also possible to use a custom order or run some parts multiple times
    # Running a part will return an instance of the respective type
    # References to the resulting part instances will be appended to a list in the EnsembleData object (ensemble.results)
    # Note though, that currently this will lead to results being overwritten in your working directory
    # (you could circumvent this by moving/renaming the folders)
    results, timings = zip(*[part.run(ensemble) for part in [Prescreening, Screening, Optimization, NMR]])

    # You access the results using the ensemble object
    # You can also find all the results the <part>.json output files
    print(ensemble.results[0].data["results"]["CONF5"]["sp"]["energy"])


Template files
==============

Since 2.0, CENSO supports template input files for all steps. They are located in ``$HOME/.censo2_assets``.
In order to use a template file for e.g. prescreening with ORCA, the file should be called ``prescreening.orca.template``.
It should contain two keywords: ``{main}`` and ``{geom}``. These are later replaced by the main argument line and the geometry
block, respectively. All further settings you add are inserted at the respective positions you put them in the
template file.

.. hint::
   Template files are not yet implemented for TURBOMOLE.

Dummy functionals
=================

Since only a limited amount of functionals are preconfigured in CENSO, the ``dummy`` option exists as value 
for ``func``. This tells CENSO to write no functional specific settings automatically into the input (such as 
``frozencore`` for double-hybrids in ORCA). By combining this with a template file, it is possible to also use 
functionals that are not defined as keywords in ORCA, such as e.g. revDSD-PBEP86-D4 (J. M. L. Martin et al., J Phys Chem A 2019
doi: 10.1021/acs.jpca.9b03157).

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
The threshold for this step should be lower than before (up to 7.5 kcal/mol) to account
for the decreasing uncertainty due to improvements in the ranking method. CENSO will 
increase the threshold by up to 1 kcal/mol, proportional to the (exponential of the) 
standard deviation of the thermal contributions. The solvation contributions will be 
calculated using DFT, if required explicitly, though explicitly calculating the solvation 
contribution will double the computational effort due to two required single-point calculations.

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
macrocycle. The energy threshold for this step is based on a minimum threshold (``threshold``) 
and the fraction of converged conformers (this is subject to change).
This threshold will be applied once the gradient norm of a conformer is below a
specified threshold (``gradthr``) for all the microcycles in the current macrocycle.

It is also possible to use ``xtb``-constraints for this step if using ANCOPT as driver.
The constraints should be provided as a file called ``constraints.xtb`` in the working directory.
Also, the ``constrain`` option for the optimization part should be set to ``True``.

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

For the calculation of the NMR spectrum of an ensemble, single-points to compute the 
nuclear shieldings and couplings will be executed. The computational parameters for shieldings
and couplings can be set to different values. In this case two separate single-points 
will be run. If the settings are identical, only one single-point will be run for both.
After that, CENSO will generate files for the simulation of the NMR spectrum using ANMR.
Please note that the user needs to setup the ``.anmrrc`` file.

For more detailed instructions see :ref:`nmr`.

UV/Vis Spectra
==============

To calculate the ensemble UV/Vis spectrum, CENSO will run single-points to calculate the excitation
wavelengths and oscillator strengths using TD-DFT. For this, it is important to choose an appropriate 
number of roots sought (``nroots``). After finishing, CENSO will output the population weighted
excitation parameters to ``excitations.out`` in tabular format and to ``excitations.json`` for convenience.
The table contains all weighted excitation wavelengths together with their maximum extinction coefficients 
and the originating conformer.

To plot the spectra, the tool ``uvvisplot`` provided in the ``bin`` directory (where the runner helper is also located)
can be used. It needs to be provided with a file of the same structure as ``excitations.json``.
It outputs a file called ``contributions.csv`` which contains all Gaussian signals partitioned by conformer and state.
