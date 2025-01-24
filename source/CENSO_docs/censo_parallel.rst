.. _censo_parallel:

========================
Parallelization in CENSO
========================

CENSO parallelizes the local execution of external program calls. For this it uses the built-in
``multiprocessing`` and ``concurrent`` libraries. At the heart of the parallelization is 
the ``execute`` function of the ``censo.parallel`` module. It consists of seven steps:

1. Create the joblist from the provided information (conformers, instructions for QM programs, jobtype),
2. set up the processor for parallel execution,
3. copying of MO-files (if used),
4. set the number of OpenMP threads (OMP) per subprocess for each job,
5. parallel execution,
6. collection of results and metadata,
7. optionally: restarting failed jobs.

.. hint::
   CENSO cannot be parallelized over multiple nodes.

If using the ``balance`` option from the general settings section, CENSO will assign the 
number of OpenMP threads per job while trying to maximize core utilization at all times,
assuming roughly equal computation times for each job. Since parallelization efficiency
drops off exponentially, this means trying to create as many jobs as possible with as 
little number of cores as possible. For the remainder jobs, core utilization is maximized.

Good results can be expected for small ensembles with expensive computations, although for 
large ensembles the new algorithm approaches the efficiency of the basic load balancing strategy
of assigning the same number of OpenMP threads for each job. Generally it is recommended
to enable the ``balance`` option.

.. warning::

   In order for CENSO to be able to correctly determine the number of OpenMP threads per job,
   the ``maxcores`` option needs to be set appropriately. If you use a workload manager such 
   as PBS or Slurm, it is probably a good idea to set ``maxcores`` according to the queue
   you're submitting to.
