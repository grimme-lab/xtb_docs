.. _crestversions:

-------------------------------
 CREST Versions and Changelog
-------------------------------

Recent binaries can be found on `GitHub <https://github.com/grimme-lab/crest/releases>`_ (Linux operating systems only).


.. note:: Release candidates and beta versions are not listed here.

Version 2.11 (1. Feb. 2020)
   - Code cleanup (pt. 2)
   - Moved ``crest`` source code to the `repository <https://github.com/grimme-lab/crest>`_ 
   - Rewritten ensemble sorting routine CREGEN
   - Connectivity/topology checks in CREGEN (can be turned off with ``--notopo``)
   - Optional PCA and k-Means clustering after sorting (``--cluster <number>``)
   - Multiple smaller tools and improvements implemented
   - Updated input reader for Turbomole coord, .xyz and 3D .sdf (V2000,V3000) formats
   - Turned off ``zsort`` default input sorting of coordinates
   - Enabled ``--alpb`` usage (new implicit solvation model in ``xtb``)
   - Reduced creation of files during optimizations
   - Dedicated ensemble entropy (``--entropy``) mode and iMTD-sMTD workflow (``--v4``)


Version 2.10 (20th Apr. 2020)
   - Major code cleanup (pt. 1)
   - Moved ``crest`` from ``xtb`` to its own `repository <https://github.com/grimme-lab/crest>`_ 
   - Proper ``SIGTERM`` and ``SIGINT`` handling implemented
   - `Bugfix`: Repaired integer overflow in ensemble sorting routine
   - Reduced memory consumption in ensemble sorting
   - Improved efficiency of ensemble sorting (for large ensembles)
   - Implemented automatic bond length constraint (``-cbonds``)

Version 2.9 (19th Feb. 2020)
   - Version consistent with main publication at `PCCP <https://pubs.rsc.org/en/content/articlelanding/2020/CP/C9CP06869D>`_
   - Literature references added, can be shown with ``--cite``


Version 2.8.1
   - Minor bug fixes e.g. in ``-scratch``, ``-gbsa``
   - property mode now distinguished between ``hess`` and ``ohess``
   - GFN-FF interface (requires xtb 6.3 or newer)


Version 2.8
   - Automated geometry optimization prior to calculations
   - New flags ``-dry``, ``-scratch``, ``-cinp``, ``--constrain``, ``-subrmsd``
   - Initial framework for "property" mode (``-prop``)
