.. _censo_parallel:

========================
Parallelization in CENSO
========================

CENSO parallelizes the local execution of external program calls. For this it uses `dask <https://www.dask.org/>`_.

.. hint::
   CENSO cannot be parallelized over multiple nodes (yet).

If using the ``balance`` option from the general settings section, CENSO will assign the 
number of OpenMP threads per job while trying to maximize core utilization at all times,
assuming roughly equal computation times for each job. Since parallelization efficiency
drops off exponentially, this means trying to create as many jobs as possible with as 
little number of cores as possible. For the remainder jobs, core utilization is maximized.

Good results can be expected for small ensembles with expensive computations, although for 
large ensembles the new algorithm approaches the efficiency of the basic load balancing strategy
of assigning the same number of OpenMP threads for each job. Generally it is recommended
to enable the ``balance`` option.

.. hint::
   CENSO will try to automatically detect the CPU resources that are available in your current environment.
   It well first look for a provided argument to ``--maxcores`` or argument to the ``get_cluster`` function.
   If nothing was provided, it will look first for SLURM environment variables. If none are found, it will try to
   determine the number of CPUs with ``os.cpu_count()``. Make sure that you provide a valid number of cores.
