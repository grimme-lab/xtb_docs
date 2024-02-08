.. _censo_extensive_keywords:

=========================
Extensive Keyword Options
=========================

.. _censo_funcs:

Functionals
-----------

For automated input preparation, CENSO will try to determine the type and dispersion correction
of the provided functional keyword. If the functional is not found within the internal database,
CENSO will act like it is provided with a GGA without dispersion correction.

.. hint::

   If you want to use a functional unknown to CENSO with a dispersion correction, you might want to use the 
   template system to add a dispersion correction to the inputs.


.. _censo_bs:

Basis Sets 
----------

The policy for basis sets is to take the provided keyword literally without validation.
This means, you need to provide the basis set keyword already with the correct name 
for the QM program you want to utilize.

.. _censo_solv:

Solvents
--------

**WIP** extensive list of solvents available coming soon. For now, just use the name of the solvent in 
the solvation model you want to use.

.. list-table:: Solvents Availability
    :widths: 30 30 30 30
    :header-rows: 1

* - Name 
  - CPCM
  - SMD
  - GBSA/ALPB
