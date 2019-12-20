.. _crestversions:

-------------------------------
 CREST Versions and Changelog
-------------------------------

Recent binaries can be found on `GitHub <https://github.com/grimme-lab/xtb/releases>`_ (Linux operating systems only).


.. note:: Release candidates and beta versions are not listed here.

Version 2.8.1
   - Minor bug fixes e.g. in ``-scratch``, ``-gbsa``
   - property mode now distinguished between ``hess`` and ``ohess``
   - GFN-FF interface (requires xtb 6.3 or newer, currently prone to errors since bias potentials have not been adjusted to the force field PES yet)


Version 2.8
   - Automated geometry optimization prior to calculations
   - New flags ``-dry``, ``-scratch``, ``-cinp``, ``--constrain``, ``-subrmsd``
   - Initial framework for "property" mode (``-prop``)

.. - GFN-FF support (requires capable XTB version)
