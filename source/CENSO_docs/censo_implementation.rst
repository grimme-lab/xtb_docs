.. _censo_implementation:

==============
Implementation
==============

.. contents::

Implementing a new part
-----------------------

To implement a new part, the ``CensoPart`` class should be used as parent. Depending on 
specifics of the new part, the constructor of ``CensoPart`` needs to be extended. By
default, it only sets the ``_name`` attribute according to the class name and takes
an ``EnsembleData`` object as argument. ``CensoPart`` provides methods to print the settings
for the class, get and set the settings, including validation, as well as a method to
dump, for every conformer in the ensemble, the results for the part as a json file.

After potentially extending the constructor, each part should include a number of 
specific settings, as well as defaults and options for these settings. For this, the
native parts can be used as reference. The setting names, defaults, and potentially 
options should be defined in the ``_options`` class attribute. The ``_settings`` attribute
should be defined as an empty dictionary, otherwise the attributes of the parent class
will be inherited.

.. The ``_options`` dictionary of the ``Prescreening`` class as an example.
.. code:: python

   _options = {
       "threshold": {"default": 4.0, "range": [1.0, 10.0]},
       "func": {"default": "pbe-d4", "options": []},
       "basis": {"default": "def2-SV(P)", "options": []},
       "prog": {"default": "orca", "options": PROGS},
       "gfnv": {"default": "gfn2", "options": GFNOPTIONS},
       "run": {"default": True},
       "template": {"default": False},
   }

The next step would be implementing the ``run`` method of the new class. By default, 
it should use the ``timeit`` and ``CensoPart._create_dir`` decorators. Within the function,
the internal logic of what you would like to achieve should be implemented. Typically,
the native parts start by printing out their settings (``self.print_info()``). After that,
a jobtype would be defined. This is necessary if CENSO should function as interface for 
external programs and should be a list, containing strings referring to the jobtypes 
available in the processor types (e.g. ``["sp"]``). For using external programs, a method 
setting up a ``prepinfo`` dictionary should also be implemented for the new part. It is a 
dictionary with ``(key, value) = (jt, settings)``, where ``jt`` would be a jobtype key (such
as ``"sp"``) and ``settings`` another dictionary, containing values that need to be looked
up by the processor when running a job. 


.. For convenience, there is a parent class specifically for ensemble optimization steps called ``EnsembleOptimizer``, which already includes some boilerplate code.
.. code:: python

    @timeit
    @CensoPart._create_dir
    def run(self, ncores: int, cut: bool = True) -> None:
        """
        Boilerplate run logic for any ensemble optimization step. The 'optimize' method should be implemented for every
        class respectively.
        """
        # print instructions
        self.print_info()

        # Print information about ensemble before optimization
        self.print_update()

        # Perform the actual optimization logic
        self.optimize(ncores, cut=cut)

        # Print comparison with previous parts
        self.print_comparison()

        # Print information about ensemble after optimization
        self.print_update()

        # dump ensemble
        self.ensemble.dump_ensemble(self._name)

Note the ``cut`` keyword argument, which indicates wether to apply thresholds to the ensemble or not and also 
the ``ncores`` argument, which indicates the number of cores to use for running the calculations.

To run jobs of type ``jobtype``, the ``execute`` function from the ``parallel`` module is 
required. It will return the results for the external program calls in form of a 
dictionary with ``(key, value) = (id(conf), res)``, where ``id(conf)`` is the ``id`` of a 
conformer object contained in the ensemble and ``res`` is a dictionary containing the 
results of the external programs for the specified jobtype.

As a second return value, it will return a list of the ``id`` of failed conformers, 
i.e. conformers, for which at least one job failed to execute. These should most likely 
be removed from the ensemble using the ``ensemble.remove_conformers`` method. In the 
native parts, the results will be inserted into the ``results`` dictonary of the 
``MoleculeData`` objects (the conformers).

Using these steps, more complex behaviour can be achieved. Typical steps would also include 
resorting the conformers (``ensemble.conformers.sort``) as well as updating the conformer
list using a threshold (energy threshold in kcal/mol or Boltzmann population threshold 
between 0.0 and 1.0) (``ensemble.update_conformers``). Lastly, you might want to write 
your results, e.g. by implementing a custom method and/or using the inherited 
``self.write_json`` and ``ensemble.dump_ensemble`` methods.

.. Example for a new class for ensemble optimization.
.. code:: python

   from censo.part import CensoPart
   from censo.parallel import execute
   from censo.ensembledata import EnsembleData

   class NewPart(CensoPart):

       _options = {
           ...,
           "prog": {"default": "orca", "options": ["orca", "tm"]},
           ...,
           "threshold": {"default": 0.95, "range": [0.5, 0.99]}
       }

       _settings = {}

       def __init__(self, ensemble: EnsembleData): 
           super().__init__(ensemble)

       @timeit
       @CensoPart._create_dir
       def run(self) -> None:
           """
           docstring
           """

           # print settings
           self.print_info()

           # define jobtype
           jobtype = ["sp"]

           # Setup the prepinfo dict 
           # NOTE: This method needs to be implemented to be used
           prepinfo = self.setup_prepinfo()

           sucess, _, failed = execute(
               self.ensemble.conformers,
               self.dir,
               self.get_settings()["prog"]
               prepinfo,
               jobtype,
               ...
               # some other keyword arguments are possible here
           )

           # Remove failed conformers
           for confid in failed:
               self.ensemble.remove_conformers(failed)

           # update results for each conformer
           for conf in self.ensemble.conformers:
               # store results of single jobs for each conformer
               conf.results.setdefault(self._name, {}).update(results[id(conf)])

           # calculate boltzmann weights from values calculated here
           self.ensemble.calc_boltzmannweights(
               self.get_general_settings().get("temperature", 298.15), self._name
           )

           # sort conformers list with specific key
           self.ensemble.conformers.sort(
               key=lambda conf: conf.results[self._name]["sp"]["energy"],
           )

           # write results
           # NOTE: this method needs to be implemented to be used
           self.write_results()

           # update conformers with threshold
           # in this example the threshold is supposed to be a Boltzmann population
           # threshold
           threshold = self.get_settings()["threshold"]

           # update the conformer list in ensemble (remove confs if below threshold)
           for confname in self.ensemble.update_conformers(
               lambda conf: conf.results[self._name]["bmw"], 
               threshold,
               boltzmann=True
           ):
               print(f"No longer considering {confname}.")

           # dump ensemble
           self.ensemble.dump_ensemble(self._name)


After all these steps, the part can also be added to the core code of CENSO. For this, the class of the 
new part needs to be added in ´´configuration.py´´ in the ´´configure´´ method, where all parts are imported
in order to setup their settings by reading the rcfile. Also, make sure that the new class is added in the 
appropriate ´´__init__.py´´ files, so that it can be imported. In order to make the part run via the commandline,
it is necessary to also import the class in ´´interface.py´´, where the ´´run´´ settings of each part is checked.


Implementing a new jobtype
--------------------------

In order to implement a new jobtype for a specific processor, a new instance method 
in the respective processor should be created. This method should be marked as *protected*
(using ``_``). The method should then be added to the ``_jobtypes`` dictionary of the 
processor class with an appropriate name as key. 

For implementing the functionality, you should first think about if the external program 
call can be handled by the ``_sp``/``_xtb_sp`` method of the processor. The output files
are created in the directory provided by the ``jobdir`` argument. You might need to 
implement the setup of an input file for this job though. In the case of ORCA, this means
configuring the ``__prep`` method of the ``OrcaProc`` class.

Implementing a new program
--------------------------

To implement a new external program to be used with ``CENSO``, it is necessary to create 
a new processor class, inheriting from the ``QmProc`` parent class. This is because ``CENSO``
relies on calling the ``run`` method of the ``QmProc`` class in order to execute jobs.
The ``run`` method in turn will call the respective methods defined in the ``_jobtypes``
dictionary and automatically collects results as well as metadata.

Each method to be implemented as a jobtype should return two dictionaries: a ``results``
dictionary and a ``meta`` dictionary, containing metadata about the jobtype. The external program 
calls should be handled using the ``_make_call`` method of the ``QmProc`` class. It automatically 
creates a subprocess to execute the external program. It needs to be provided with a call 
in form of a list (of strings representing the command line arguments), a directory to execute
in and a file to redirect ``stdout``.

Finally, the new processor class needs to be added to the ``__proctypes`` dictionary of the 
``ProcessorFactory`` class. Also, the key used there should be added to the ``PROGS`` parameter
in ``params.py``.
