.. _pbc:

------------------------------
 Periodic Boundary Conditions
------------------------------

``xtb`` supports three-dimensional (3D) periodic boundary conditions (PBC) at the
Γ-point for tight-binding and force-field calculations, including single-point
evaluations and periodic geometry optimizations. For a more complete periodic
integration (such as Brillouin-zone *k*-point sampling), `DFTB+`_
(see the `tblite DFTB+ documentation
<https://tblite.readthedocs.io/en/latest/users/dftbplus.html>`_) or `CP2K`_
(see the `CP2K xTB documentation
<https://manual.cp2k.org/trunk/CP2K_INPUT/FORCE_EVAL/DFT/QS/XTB.html>`_)
should be used.

.. contents::

Supported Methods
=================

The following methods can be used for 3D periodic systems in ``xtb``:

* GFN2-xTB and GFN1-xTB (via ``tblite``):
  Periodic GFN2-xTB and GFN1-xTB calculations are carried out through the
  `tblite`_ backend and require passing the ``--tblite`` flag on the command line:

  .. code:: bash

     > xtb coord --tblite            # periodic GFN2-xTB (default GFN level)
     > xtb coord --gfn 2 --tblite    # periodic GFN2-xTB
     > xtb coord --gfn 1 --tblite    # periodic GFN1-xTB

  Both methods provide total energies, analytical nuclear gradients, and
  analytical lattice stress/strain derivatives. Spin-polarized periodic
  GFN1-xTB and GFN2-xTB calculations are also available by combining
  ``--spinpol`` with ``--tblite``.

  .. important::

     The legacy native periodic implementations of GFN1-xTB and GFN2-xTB in
     ``xtb`` are deprecated and unsupported. Running a periodic GFN1-xTB or
     GFN2-xTB calculation without ``--tblite`` will terminate with an error.

* GFN-FF and mcGFN-FF:
  The general force field GFN-FF (``--gfnff`` / ``--gff``) and its
  reparameterization for molecular crystals mcGFN-FF (``--mcgfnff``) natively
  support 3D periodic boundary conditions. See :ref:`gfnff` for details and
  examples.

* GFN0-xTB:
  The non-self-consistent GFN0-xTB method (``--gfn 0``) is available for 3D
  periodic systems via the native ``xtb`` implementation.

Periodic Geometry Input
=======================

``xtb`` reads 3D periodic structures from several file formats (see
:ref:`geometry` for full specifications):

* Turbomole ``coord`` files (``coord``, ``*.coord``, ``*.tmol``) containing
  ``$periodic 3`` together with ``$lattice [bohr|angs]`` or ``$cell [bohr|angs]``
  and either Cartesian (``$coord [bohr|angs]``) or fractional (``$coord frac``)
  atomic coordinates:

  .. code-block:: text
     :caption: mgo.coord

     $coord frac
         0.00      0.00      0.00      mg
         0.50      0.50      0.50      o
     $periodic 3
     $lattice bohr
         0.0000000000      4.1000443765      4.1000443765
         4.1000443765      0.0000000000      4.1000443765
         4.1000443765      4.1000443765      0.0000000000
     $end

* VASP ``POSCAR`` / ``CONTCAR`` files (``POSCAR``, ``CONTCAR``, ``*.vasp``,
  ``*.poscar``, ``*.contcar``).
* DFTB+ genFormat files (``*.gen``) with periodic ``S`` (supercell/Cartesian)
  or ``F`` (fractional) coordinates and lattice vectors.

.. note::

   Molecular formats such as ``.xyz``, ``.pdb``, ``.mol``, and ``.sdf`` do not
   store periodic boundary conditions in ``xtb``.

Periodic Geometry Optimization
==============================

Geometry optimizations of periodic structures are requested with ``--opt [LEVEL]``:

.. code:: bash

   > xtb POSCAR --gfn 2 --tblite --opt
   > xtb mgo.coord --gfn 1 --tblite --opt tight
   > xtb benzene.coord --mcgfnff --opt


For periodic systems, ``--opt`` automatically selects the periodic L-BFGS
optimizer (``pbc_lbfgs``) instead of ``ANCopt``, while the optimization engine
can also be set in the :ref:`detailed-input` file (e.g., ``engine=pbc_lbfgs`` or
``engine=inertial``, see :ref:`geometry optimization`). By default, both atomic
coordinates and unit-cell lattice vectors are relaxed simultaneously.

To optimize only the atomic coordinates while keeping the unit-cell lattice
vectors fixed, pass the ``--nocellopt`` flag:

.. code:: bash

   > xtb POSCAR --tblite --opt --nocellopt

Performing a fixed-cell pre-optimization with ``--nocellopt`` can also be
helpful on rough potential energy surfaces before relaxing the unit cell.

The final optimized structure (atomic coordinates and updated lattice vectors)
is written to ``xtbopt.<ext>`` in the input format (e.g., ``xtbopt.coord``,
``xtbopt.poscar``, ``xtbopt.gen``), and the optimization trajectory is written
to ``xtboptlog.cif`` in CIF format with the step energy included in a comment
header for each frame. If the final structure does not have the lowest energy
encountered along the trajectory, the lowest-energy structure is additionally
saved to ``xtbopt_emin.coord``.

.. _DFTB+: https://www.dftbplus.org/
.. _CP2K: https://www.cp2k.org/
.. _tblite: https://tblite.readthedocs.io/
