.. _xtb_info:

----------------
 Info Submodule
----------------

.. note::
   This feature is only present in version 6.3 and newer

The info submodule allows to acquire information about the geometry as it
would be processed in a full ``xtb`` run, but without actually performing
a calculation.

The info submodule is invoked by

.. code-block:: text

   xtb info <geometry> [geometry] ...

and can handle multiple (read several thousand) input files sequentially.

In the normal run mode, with one input file, the info submodule will show
the number of atoms found, the identifier map and geometry specific information
as printed in the property printout.
These information can be used to verify ``xtb`` will correctly read the input
structure, in case of errors the info submodule will return with a non-zero
exit code.

The identifier map printed can be used in the detailed input to select certain
group of atoms and is printed to show the found species and mapping in the input
file

.. code-block:: text

   ID    Z sym.   atoms
    1    6 C      1, 6-7, 13, 14
    2    7 N      2, 4, 9, 12
    3    6 C*     3, 10
    4    8 18O    8
    5    8 O      11
    6    1 D      15-17
    7    1 H      18-24

In batch mode, with multiple input files, the info submodule will generate
additional diagnostics regarding the input files

.. code-block:: text

   ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Processed 2474 individual inputs
    11/2474 inputs failed info check(  0.4%)
   ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

An summary of the errors will be found in the standard output when the
file was processed, in case an error is encounter the info submodule will
return with a non-zero exit code. Processing is not aborted due to errors.
This can be used to validate structures before performing actual calculations.
