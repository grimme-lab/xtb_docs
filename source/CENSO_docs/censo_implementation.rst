.. _censo_implementation:

Implementation
--------------

Implementing a new part
=======================

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
up by the processor when running a job. Specific jobtypes require specific settings to
be implemented **at all times**, for this refer to the ``_req_settings`` attribute of the
``OrcaProc`` class (**WIP**).

To run jobs of type ``jobtype``, the ``execute`` function from the ``parallel`` module is 
required. It will return the results for the external program calls in form of a 
dictionary with ``(key, value) = (id(conf), res)``, where ``id(conf)`` is the ``id`` of a 
conformer object contained in the ensemble and ``res`` is a dictionary containing the 
results of the external programs for the specified jobtype. As a second return value,
it will return a list of the ``id`` of failed conformers, i.e. conformers, for which at least one job 
failed to execute. These should most likely removed from the ensemble using the 
``ensemble.remove_conformers`` method. In the native parts, the results will be inserted
into the ``results`` dictonary of the ``MoleculeData`` objects (the conformers).

Using these steps and extending using new functionality, more complex behaviour can be 
achieved. Typical steps would also include resorting the conformers 
(``ensemble.conformers.sort``) as well as updating the conformer list using a threshold
(energy threshold in kcal/mol or Boltzmann population threshold between 0.0 and 1.0)
(``ensemble.update_conformers``). Lastly, you might want to write your results, e.g. by 
implementing a custom print method or using the inherited ``self.write_json`` and 
``ensemble.dump_ensemble`` methods.

Example:

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



Implementing a new jobtype
==========================

Implementing a new program
==========================
