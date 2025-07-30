:date: 2024-07-23
:author: Albert Katbashev
:category: release
:tags: xtb

xtb version 6.7.1 released
==========================

.. image:: https://github.com/awvwgk/xtb-logo/raw/master/xtb.svg
   :alt: xtb logo

We are happy to release a bugfix version of ``xtb``.
Please note that due to the deprecation of the currently used MSVC C++ compiler, the Windows version is 6.7.1pre. The compilation excluded C-API tests for xtb and CPCM-X and used the previous version of dftd4 (v3.4.0).
In the next release, we will likely fully switch to the icx-cl compiler.

Many thanks to Sebastian Ehlert (`@awvwgk <https://github.com/awvwgk>`_), 
Igor S. Gerasimov (`@foxtran <https://github.com/foxtran>`_), and 
Thomas Rose (`@Thomas3R <https://github.com/Thomas3R>`_) for contributing to this release.  
Special thanks to Marcel Stahn (`@MtoLStoN <https://github.com/MtoLStoN>`_), 
who made the compilation of the Windows version possible.

Find the complete release notes `here <https://github.com/grimme-lab/xtb/releases/tag/v6.7.1>`__.
