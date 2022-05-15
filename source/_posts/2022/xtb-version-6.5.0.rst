:date: 2022-05-15
:author: Sebastian Ehlert
:category: release
:tags: xtb

xtb version 6.5.0 released
==========================

.. image:: https://github.com/awvwgk/xtb-logo/raw/master/xtb.svg
   :alt: xtb logo

We are happy to release a new version of ``xtb`` with exciting new features.
First of all, we improved the user-friendliness of the error messages in the geometry reader by adopting our `IO-library <https://github.com/grimme-lab/mctc-lib>`__, which is already in wide use in `dftd4 <https://github.com/dftd4/dftd4>`__, `gcp <https://github.com/grimme-lab/gcp>`__ and other projects.
No more *invalid input provided* obscure error messages, but actual pointers on what went wrong with the input.

.. code:: text

   Error: Conflicting lattice and cell groups
     --> struc.coord:37:1-5
      |
   35 | $lattice angs
      | -------- lattice first defined here
      :
   37 | $cell angs
      | ^^^^^ conflicting cell group
      |

We also improved the capability of existing geometry readers to keep up to date with the parent programs, for example we now support the ``$eht charge=0 unpaired=0`` line to set the system charge and number of unpaired electrons, which was added in a Turbomole 7.5.
Furthermore, we are happy to have now support for QChem molecule files (``.qchem``), FHI-aims geometry inputs (``geometry.in``) and QCSchema formatted JSON (``.json``).

Other new features in this release are the preliminary ONIOM calculator to combine GFNn-xTB and GFN-FF with DFT or the new inspection feature for the GFN-FF topology via the ``wrtopo`` option.
This release also includes several important bugfixes for the GFN-FF, the numerical hessian evaluation, overflow fixes and many others.

Many thanks to Christian Hölzer (`@hoelzerC <https://github.com/hoelzerC>`__), Albert Katbashev (`@Albkat <https://github.com/Albkat>`__), Jeroen Koopman (`@JayTheDog <https://github.com/JayTheDog>`__), Hagen Neugebauer (`@haneug <https://github.com/haneug>`__), Felix Pultar (`@pultar <https://github.com/pultar>`__), Thomas Rose (`@Thomas3R <https://github.com/Thomas3R>`__), Jordy Schifferstein (`@Jordy-prog <https://github.com/Jordy-prog>`_), Marcel Stahn (`@MtoLStoN <https://github.com/MtoLStoN>`__) for contributing to this version of ``xtb``.

Find the complete release notes `here <https://github.com/grimme-lab/xtb/releases/tag/v6.5.0>`__.
