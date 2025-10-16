.. _censo_extensive_keywords:

=========================
Extensive Keyword Options
=========================

.. _censo_funcs:

Functionals
-----------

For automated input preparation, CENSO will try to find the functional in an internal lookup dict. 
This is due to differences in naming between different QM programs. If the functional string is not 
found within this lookup dict, CENSO will not run.

.. hint::

   If you want to use a functional unknown to CENSO using ORCA, you might want to provide a custom lookup dict.
   You can provide the dict in form of a JSON file located in ``~/.censo2_assets/dfa.json``.
   Make sure to follow the format of the original lookup dict located in ``src/censo/assets/dfa.json``.
   Your lookup dict will be merged with the original lookup dict such that keys can also be overwritten.


.. _censo_bs:

Basis Sets
----------

The policy for basis sets is to take the provided keyword literally without internal lookup except for TURBOMOLE.
This means, you need to provide the basis set keyword already with the correct name 
for the QM program you want to utilize. This is not case-sensitive and for TURBOMOLE there is
an internal lookup to check for capitalization.

.. hint::

   Please note that, when applying composite methods, the basis keyword will be ignored, except for TURBOMOLE,
   where the basis set must always be provided.

.. _censo_solv:

Solvents
--------

For each solvation model all available solvents should also be found by CENSO. If there are solvents 
missing you can create a complimentary lookup dict (``~/.censo2_assets/solvents.json``) in the same 
format as the original lookup dict (located in ``src/censo/assets/solvents.json``).
