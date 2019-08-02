.. _interface:

------------------------
 Short API Introduction
------------------------

The ``xtb`` program is *not* designed to be interfaced by an external program,
while one can write a wrapper script to start calculations and parse the output
this is in most cases cumbersome and error prone.

Interfacing ``xtb`` can be done using the application programmable interface (API)
shipped with the shared library version of ``xtb``.
This section targets mainly developers trying to interface their programs
with ``xtb``.

.. warning:: The current state of the C-API is still somewhat experimental,
             this means the API definitions might change in the future
             without prior announcement, but this also means suggestions
             and feature request regarding the API are welcome and likely
             to be included.

.. contents::

General Comments
================

Remember that ``xtb`` is written in Fortran, since modern Fortran usually
makes massive uses of name globbing and the automatically generated headers
(module files) tend to be compiler specific and incompatible,
the only two viable options are to write the interface without using
modules or to use ISO-C compatible bindings to create the API.
It turns out that we did both, we have an interface layer using
module-free Fortran subroutines which setups all datatypes and does the
actual calculation and an ISO-C compatible layer, which will eventually
call this Fortran layer.

Since we are not planning to let you directly interface with our Fortran layer
for now, we will focus on the ISO-C compatible layer, the C-API, here.
The header definitions are found in ``include/xtb.h``, for easy reference
parts of it are included here.

.. code-block:: c

   #pragma once
   
   #ifdef __cplusplus
   namespace xtb {
   #else
   #include <stdbool.h>
   #endif
   
   /* skip... */
   
   typedef struct _SCC_options {
      int prlevel;
      int parallel;
      double acc;
      double etemp;
      bool grad;
      bool restart;
      int maxiter;
      char solvent[20];
   } SCC_options;

   /* skip... */
   
   #ifdef __cplusplus
   extern "C" {
   #endif

   /* skip... */
   
   extern int
   GFN2_calculation(const int* natoms, const int* attyp, const double* charge,
         const double* coord, const SCC_options* opt, const char* output,
         double* energy, double* grad, double* dipole, double* q,
         double* dipm, double* qp, double* wbo);
   
   extern int
   GFN2_QMMM_calculation(const int* natoms, const int* attyp, const double* charge,
         const double* coord, const SCC_options* opt, const char* output,
         const int* npc, const double* pc_q, const int* pc_at, const double* pc_gam,
         const double* pc_coord, double* energy, double* grad, double* pc_grad);
   
   /* skip... */
   
   #ifdef __cplusplus
   }
   }
   #endif

As you might noticed, we prefer to get your data by reference, also
there are different interfaces depending on what you are planning to do.
For clarification we give you an interface definition for the
actual Fortran function as well

.. code:: fortran

   function gfn2_api &
         &   (natoms,attyp,charge,coord,opt_in,file_in, &
         &    etot,grad,dipole,q,dipm,qp,wbo) &
         &    result(status) bind(C,name="GFN2_calculation")
   
      use iso_c_binding
   
      implicit none

      type,bind(C) :: c_scc_options
         integer(c_int)  :: prlevel = 0_c_int
         integer(c_int)  :: parallel = 0_c_int
         real(c_double)  :: acc = 0.0_c_double
         real(c_double)  :: etemp = 0.0_c_double
         logical(c_bool) :: grad = .false._c_bool
         logical(c_bool) :: restart = .false._c_bool
         integer(c_int)  :: maxiter = 0_c_int
         character(kind=c_char) :: solvent(20) = c_null_char
      end type c_scc_options
   
      integer(c_int), intent(in) :: natoms
      integer(c_int), intent(in) :: attyp(natoms)
      real(c_double), intent(in) :: charge
      real(c_double), intent(in) :: coord(3,natoms)
      type(c_scc_options), intent(in) :: opt_in
      character(kind=c_char),intent(in) :: file_in(*)
   
      integer(c_int) :: status
   
      real(c_double),intent(out) :: q(natoms)
      real(c_double),intent(out) :: wbo(natoms,natoms)
      real(c_double),intent(out) :: dipm(3,natoms)
      real(c_double),intent(out) :: qp(6,natoms)
      real(c_double),intent(out) :: etot
      real(c_double),intent(out) :: grad(3,natoms)
      real(c_double),intent(out) :: dipole(3)

   end function gfn2_api

The C-API in general needs the information on the number of atoms in ``natoms``,
a ``natoms`` wide array of integers (``attyp``) with the ordinal numbers of
the atoms, the total charge of system (``charge``) and the cartesian coordinates
in a continous ``3*natoms`` wide array of doubles (``coord``), with the
coordinate triples next to each other.
Additionally we require you to give us a struct ``opt`` containing some more
specific information on the calculation and thresholds employed and
a location ``output`` to write your output to.
Since C file pointer and Fortran units might not be as compatible as
we would wish, we decided to pass this information around as string
(``"-"`` can be used for STDOUT).

The function will return its status so you can check if the calculation
done by the shared library was successful or not, note that Fortran can
be quite drastic when using features like ``error stop``, which is likely
to kill the caller program too, without giving you even the chance to
react or catch it. We promise to not use it when you are calling our API.

The calculated values are written to some location you have to
reserve before calling the shared library, so make sure that you have enough
memory reserved.

We will generally refrain from using any of the memory you reserved
on the caller side, except for copying the results from our arrays
to yours. This sounds actually quite wasteful on resources, it is
not that we are not trusting your memory management, but we prefer to
do the memory management on your side with proper Fortran.

Available Interfaces
====================

Currently we have interfaces available for the three Hamiltonians
(GFN2-xTB, GFN1-xTB and GFN0-xTB) which come in different flavours
depending on the things you attempt to calculate.

molecular GFN2-xTB calculation
   .. code-block:: c

      extern int
      GFN2_calculation(const int* natoms, const int* attyp, const double* charge,
            const double* coord, const SCC_options* opt, const char* output,
            double* energy, double* grad, double* dipole, double* q,
            double* dipm, double* qp, double* wbo);

molecular GFN1-xTB calculation
   .. code-block:: c

      extern int
      GFN1_calculation(const int* natoms, const int* attyp, const double* charge,
            const double* coord, const SCC_options* opt, const char* output,
            double* energy, double* grad);

molecular GFN0-xTB calculation
   .. code-block:: c

      extern int
      GFN0_calculation(const int* natoms, const int* attyp, const double* charge,
            const double* coord, const PEEQ_options* opt, const char* output,
            double* energy, double* grad);

periodic GFN0-xTB calculation
   .. code-block:: c

      extern int
      GFN0_PBC_calculation(const int* natoms, const int* attyp, const double* charge,
            const double* coord, const double* lattice, const bool* pbc,
            const PEEQ_options* opt, const char* output,
            double* energy, double* grad, double* glat);
