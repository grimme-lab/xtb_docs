:date: 2021-06-10
:author: Sebastian Ehlert
:category: release
:tags: xtb

xtb version 6.4.1 released
==========================

.. image:: https://github.com/awvwgk/xtb-logo/raw/master/xtb.svg
   :alt: xtb logo

We released a new version of ``xtb`` with a significantly improved memory footprint for large scale calculations and improved parallelisation for frequency calculations.
The parallel evaluation of hessians with GFN-FF is now possible, overall we improved the stablility of the parallelisation which was slightly degraded in version 6.4.0.
For xTB calculations the required ``OMP_STACKSIZE`` has been significantly reduced by restructuring the integral evaluation slightly.

Also, this version of ``xtb`` now supports the COSMO/CPCM solvation model using the `ddPCM <https://github.com/filippolipparini/ddPCM>`_ library.

Note that this version contains an important bugfix in the topology generation for the GFN-FF, which slightly changes the total energies with respect to previous versions.
Old topology files will be invalidated automatically by the new version to avoid running calculations on incorrect topologies.

Many thanks to Marcel Stahn (`@MtoLStoN <https://github.com/MtoLStoN>`_), Sebastian Spicher (`@sespic <https://github.com/sespic>`_), Christoph Plett (`@cplett <https://github.com/cplett>`_), Cyrille Lavigne (`@clavigne <https://github.com/clavigne>`_) and Miguel Steiner (`@steinmig <https://github.com/steinmig>`_) for contributing to this version.

Find the complete release notes `here <https://github.com/grimme-lab/xtb/releases/tag/v6.4.1>`_.
