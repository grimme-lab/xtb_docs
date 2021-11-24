.. _api:

---------------------------------------------
 C API to the extended tight binding program
---------------------------------------------

The ``xtb`` program can included in other application by its
C application programmable interface (API) for all languages that
cannot access the Fortran modules directly.
The API declarations are given in ``xtb.h``

.. important::

   The C API is considered stable with version 6.3.0 and will
   be kept compatible to following versions.

   Previous experimental API versions from the 6.2 series are
   deprecated.

The C side has to work with four different Fortran objects which are
allocated and deallocated on the Fortran side and only provided as
opaque data pointer.

Generally the first set for every API call is to construct a
calculation environment ``xtb_TEnvironment``, which handles the input
and output streams and the error log.
The API will not perform any calculation if a null pointer is passed
instead of the environment and fail silently.
Note that once the environment has caught on an error all further
API calls will fail unless the environment is freed again and the
error has been handled.

``xtb`` provides a persistent environment as well which is handled
entirely by the API. Producing an error for this environment is
consider fatal and will halt the program (usually via drastic means,
*i.e.*, without giving the caller a way to react).
This case is always considered a bug in the API and should be reported.

The input data for the calculation is generated as molecular
structure data ``xtb_TMolecule``. Note that the number of atoms,
the atomic number, the boundary conditions, total charge and
multiplicity are immutable and require to reconstruct the object.
Cartesian coordinates and lattice parameters are provided in Bohr
and can be updated after the object has been constructed.
Both the update and the constructor will fail on invalid input,
which are to close interatomic distances or atomic number outside of
the range 1 to 118 (while individual methods might impose additional
constraints).

The actual calculation can be performed with a single point
calculator ``xtb_TCalculator`` which must be loaded with a particular
parametrisation before it can be used for a calculation.
GFN2-xTB and GFN1-xTB can savely load their parametrisation without
a parameter file. For the experimental GFN0-xTB a parameter file
is required in the ``XTBPATH`` or must be provided by the API.
The GFN-FF can also provide its own parameters internally but
API support is still experimental.

Finally, the calculation results are stored in the ``xtb_TResults``
object which can also be used to restart calculations from previous
runs (*e.g.* after updating coordinates).
The calculation results can be queried for properties they will
return in error in case the property is not available.
Passing calculation results from one calculator to another will
usually result in dimension missmatches, it is recommended to
reconstruct the results object for this purpose.
Generally, the API tries to be particular flexible in this case
and will try to reallocate the individual components as necessary.
Note that this can fail in particular if the total charge or
multiplicity of the molecular structure has changed.

A simple example program to obtain the total energy from a
GFN2-xTB calculation is given here.

.. code-block:: c

   #include "xtb.h"

   int
   main (int argc, char** argv)
   {
     int    const natoms = 7;
     int    const attyp[7] = {6,6,6,1,1,1,1};
     double const charge = 0.0;
     int    const uhf = 0;
     double const coord[3*7] =
         {0.00000000000000, 0.00000000000000,-1.79755622305860,
          0.00000000000000, 0.00000000000000, 0.95338756106749,
          0.00000000000000, 0.00000000000000, 3.22281255790261,
         -0.96412815539807,-1.66991895015711,-2.53624948351102,
         -0.96412815539807, 1.66991895015711,-2.53624948351102,
          1.92825631079613, 0.00000000000000,-2.53624948351102,
          0.00000000000000, 0.00000000000000, 5.23010455462158};

     /*
      * All objects except for the molecular structure can be
      * constructued without other objects present.
      *
      * The construction of the molecular structure locks the
      * number of atoms, atomic number, total charge, multiplicity
      * and boundary conditions.
     **/
     xtb_TEnvironment env = xtb_newEnvironment();
     xtb_TCalculator calc = xtb_newCalculator();
     xtb_TResults res = xtb_newResults();
     xtb_TMolecule mol = xtb_newMolecule(
         env, &natoms, attyp, coord, &charge, &uhf, NULL, NULL);
     if (xtb_checkEnvironment(env)) {
       xtb_showEnvironment(env, NULL);
       return 1;
     }

     /*
      * Apply changes to the environment which will be respected
      * in all further API calls.
     **/
     xtb_setVerbosity(env, XTB_VERBOSITY_FULL);
     if (xtb_checkEnvironment(env)) {
       xtb_showEnvironment(env, NULL);
       return 1;
     }

     /*
      * Load a parametrisation, the last entry is a char* which can
      * be used to provide a particular parameter file.
      *
      * Otherwise the XTBPATH environment variable is used to find
      * parameter files.
      *
      * The calculator has to be reconstructed if the molecular
      * structure is reconstructed.
     **/
     xtb_loadGFN2xTB(env, mol, calc, NULL);
     if (xtb_checkEnvironment(env)) {
       xtb_showEnvironment(env, NULL);
       return 1;
     }

     /*
      * Actual calculation, will populate the results object,
      * the API can raise errors on failed SCF convergence or other
      * numerical problems.
      *
      * Not supported boundary conditions are usually raised here.
     **/
     xtb_singlepoint(env, mol, calc, res);
     if (xtb_checkEnvironment(env)) {
       xtb_showEnvironment(env, NULL);
       return 1;
     }

     /*
      * Query the environment for properties, an error in the environment
      * is not considered blocking for this calls and allows to query
      * for multiple entries before handling possible errors
     **/
     xtb_getEnergy(env, res, &energy);
     if (xtb_checkEnvironment(env)) {
       xtb_showEnvironment(env, NULL);
       return 1;
     }

     /*
      * deconstructor will deallocate the objects and overwrite the
      * pointer with NULL
     **/
     xtb_delResults(&res);
     xtb_delCalculator(&calc);
     xtb_delMolecule(&mol);
     xtb_delEnvironment(&env);

     return 0;
   }

The header additionally defines macros for the version of the ``xtb`` API
to guard usage of API functionalities introduced in particular versions.
