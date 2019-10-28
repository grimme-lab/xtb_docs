==============================================
 User Guide to Semiempirical Tight Binding
==============================================

This user guide focuses on the semiempirical quantum mechanical methods GFNn-xTB, their
descendants, and corresponding composite schemes  as implemented in the
`xtb <https://www.chemie.uni-bonn.de/pctc/mulliken-center/software/xtb>`_ (extended tight binding) program package.

We provide a number of detailed guides dealing with common task that can
be performed easily with the ``xtb`` program.
All guides are usually structured the same way, starting with some simple
examples using only the commandline and the default settings followed
by a trouble shooting section.
Detailed inputs are provided in a ready to use fashion to solve some
more special but still common tasks with ``xtb`` together with some
insights into the theory used behind the scences.

-----------------------------------------
 xTB in Other Quantum Chemistry Programs
-----------------------------------------

The xTB-methods are now officially available in other quantum chemistry programs!

 - in `Orca`_ 4.2 an IO-based interface to the ``xtb`` binary is available
 - `AMS`_ 2019 implements GFN1-xTB in their DFTB module

.. _Orca: https://orcaforum.kofo.mpg.de
.. _AMS: https://www.scm.com/product/dftb/

We missed your project here?
No problem, just give us hint at the mailing list or open an issue at `github`_.

.. _github: https://github.com/grimme-lab/xtb_docs/issues/new


.. toctree::
   :maxdepth: 3
   :caption: Quickstart

   setup
   basics
   commandline
   xcontrol

.. toctree::
   :maxdepth: 3
   :caption: Guides

   sp
   properties
   optimization
   scan
   gbsa
   hessian
   md
   mtd
   gsm
   periodic_boundary_conditions
   pcem

.. toctree::
   :maxdepth: 3
   :caption: CREST

   crest
   crestversions
   crestcmd
   crestxmpl

.. toctree::
   :maxdepth: 3
   :caption: ENSO
   
   enso_doc/enso
   enso_doc/enso_setup
   enso_doc/enso_usage
   enso_doc/enso_anmr
   enso_doc/plotting

.. toctree::
   :maxdepth: 3
   :caption: API

   dev_interface
   dev_python

.. toctree::
   :maxdepth: 3
   :caption: Misc

   versions
   xtbrelatedrefs
   license
   help

