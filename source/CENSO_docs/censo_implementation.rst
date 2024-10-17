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
       "threshold": {"default": 4.0},
       "func": {"default": "pbe-d4"},  # the 'options' key can be left out if there are no restrictions
       "basis": {"default": "def2-SV(P)"},
       "prog": {"default": "orca", "options": PROGS},
       "gfnv": {"default": "gfn2", "options": GFNOPTIONS},
       "run": {"default": True},
       "template": {"default": False},
   }

The ``CensoPart`` class implements a general validation methods for the ``_settings`` attribute. 
It checks for type and value, if options are defined. Each part can and probably should extend the 
general method (e.g. the ``EnsembleOptimizer`` class extends the ``_validate`` method) in order to check 
for e.g. setting combinations, such as trying to use DCOSMORS with ORCA.

The next step would be implementing the ``run`` method of the new class. By default, 
it should use the ``timeit`` and ``CensoPart._create_dir`` decorators. Within the function,
the internal logic of what you would like to achieve should be implemented. Typically,
the native parts start by printing out their settings (``self._print_info()``). After that,
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
    def __call__(self, cut: bool = True) -> None:
        """
        Boilerplate run logic for any ensemble optimization step. The 'optimize' method should be implemented for every
        class respectively.
        """
        # print instructions
        self._print_info()

        # Print information about ensemble before optimization
        self._print_update()
        self.results["nconf_in"] = len(self._ensemble.conformers)

        # Perform the actual optimization logic
        self._optimize(cut=cut)
        self.results["nconf_out"] = len(self._ensemble.conformers)

        # Write out results
        self._write_results()

        # Print comparison with previous parts
        self._print_comparison()

        # Print information about ensemble after optimization
        self._print_update()

        # dump ensemble
        self._ensemble.dump(f"{self._part_nos[self.name]}_{self.name.upper()}")

        # DONE


Note the ``cut`` keyword argument, which indicates wether to apply thresholds to the ensemble.

To run jobs of type ``jobtype``, the ``execute`` function from the ``parallel`` module is 
required. It will return the results for the external program calls in form of a 
dictionary with ``(key, value) = (conf.name, res)``, where ``conf.name`` is the name of a 
conformer contained in the ensemble, e.g. CONF23, and ``res`` is a dictionary containing the 
results of the external programs for the specified jobtype.

As a second return value, it will return a list of the names of failed conformers, 
i.e. conformers, for which at least one job failed to execute. These should most likely 
be removed from the ensemble using the ``ensemble.remove_conformers`` method. The results 
are then put into the parts ``data`` attribute under the ``results`` key. Furthermore,
in the ``run`` method of the ``CensoPart`` class, a reference to the part object will be 
attached to ``ensemble.results``.

Using these steps, more complex behaviour can be achieved. Typical steps would also include 
resorting the conformers (``ensemble.conformers.sort``) as well as updating the conformer
list using a threshold (energy threshold in kcal/mol or Boltzmann population threshold 
between 0.0 and 1.0). Lastly, you might want to write your results, e.g. by implementing a 
custom method and/or using the inherited ``self._write_json`` and ``ensemble.dump`` methods.

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
           "threshold": {"default": 0.95}
       }

       _settings = {}

       @timeit
       @CensoPart._create_dir
       def __call__(self) -> None:
           """
           docstring
           """

           # print settings
           self._print_info()

           # define jobtype
           jobtype = ["sp"]

           # Setup the prepinfo dict 
           # NOTE: This method needs to be implemented to be used
           prepinfo = self._setup_prepinfo()

           results, failed = execute(
               self.ensemble.conformers,
               self.dir,
               self.get_settings()["prog"]
               prepinfo,
               jobtype,
               ...
               # some other keyword arguments are possible here
           )

           # Remove failed conformers
           self.ensemble.remove_conformers(failed)

           # update results for each conformer
           self._update_results(results)

           # calculate boltzmann weights from values calculated here
           self._update_results(self._calc_boltzmannweights())

           # sort conformers list with specific key
           self.ensemble.conformers.sort(
               key=lambda conf: self.data["results"][conf.name]["sp"]["energy"],
           )

           # update conformers with threshold
           # in this example the threshold is supposed to be a Boltzmann population
           # threshold
           threshold = self.get_settings()["threshold"]

           # update the conformer list in ensemble (remove confs if below threshold)
           limit = min(self.data["results"][conf.name]["sp"]["energy"] for conf in self.ensemble.conformers)
           filtered = list(filter(lambda conf: self.data["results"][conf.name]["sp"]["energy"] - limit > threshold, self.ensemble.conformers))
           for conf in filtered:
               print(f"No longer considering {conf.name}.")
            
           self.ensemble.remove_conformers([conf.name for conf in filtered])

           # dump ensemble
           self.ensemble.dump(self.name)


After all these steps, the part can also be added to the core code of CENSO. For this, the class of the 
new part needs to be added in ``configuration.py`` in the ``configure`` method, where all parts are imported
in order to setup their settings by reading the rcfile. Also, make sure that the new class is added in the 
appropriate ``__init__.py`` files, so that it can be imported. It is also necessary to register the constructor 
in the ``Factory``, found in ``utilities``. In order to make the part run via the commandline,
it is necessary to also import the class in ``interface.py``, where the ``run`` settings of each part is checked.


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

Finally, the new processor constructor needs to be registered in the ``Factory`` class. 
Also, the key used there should be added to the ``PROGS`` parameter in the ``Config`` class 
in ``params.py``. This will be used by parts to determine available programs in the settings, 
so be careful to check whether your program supports the necessary jobtypes. You might want to 
raise a ``NotImplementedError`` for unsupported jobtypes.
