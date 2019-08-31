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
   crestcmd
   crestxmpl

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

