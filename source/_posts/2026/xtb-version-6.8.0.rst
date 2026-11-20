:date: 2026-11-05
:author: Thomas Froitzheim
:category: release
:tags: xtb

xtb version 6.8.0 released
==========================

.. image:: https://github.com/awvwgk/xtb-logo/raw/master/xtb.svg
   :alt: xtb logo

We are happy to release a new version of ``xtb``.
This release combines new features with extensive bug fixes and numerical consistency improvements. As a result, ``xtb`` 6.8.0 introduces small changes to calculated energies compared to previous releases: differences in absolute total energies can reach the mEh range, particularly for large systems, but the impact on relative energies is generally negligible (e.g., WTMAD2 on GMTKN55 changes by less than 0.0002 kcal/mol for GFN0–GFN2 compared to ``xtb`` 6.7.1). Furthermore, the native ``xtb`` and `tblite <https://github.com/tblite/tblite>`__ implementations are now fully consistent (< 1e-10 Eh deviations), preparing the complete transition to ``tblite`` in the next releases.

Major updates include expanded `tblite <https://github.com/tblite/tblite>`__ integration (supporting solvation models, electric fields, localized orbitals, docking, ONIOM, and periodic GFN1/GFN2 calculations).
Hessian calculations now support the O1NumHess algorithm and include several improvements to vibrational analysis and thermochemistry.
GFN-FF has been reparameterized for lanthanides, extended to actinides, and improved in topology handling and stability.
Docking, ONIOM, and molecular dynamics now feature improved constraint handling.
In addition, the C-API supports solvent-model selection and Hessian access, and we have improved the build system and extended platform support (Linux x86_64 and aarch64, macOS x86_64 and arm64, and Windows x86_64).
Numerous performance optimizations and bug fixes accelerate SCF, gradient, and integral evaluations while reducing memory usage and improving OpenMP parallelization and numerical reliability.

Many thanks to Alberto Boldrini (`@AlbertoBoldrini <https://github.com/AlbertoBoldrini>`__),
Ty Balduf (`@TyBalduf <https://github.com/TyBalduf>`__),
M. Chandler Bennett (`@mcbennet <https://github.com/mcbennet>`__),
James H. Brown (`@james-qbraid <https://github.com/james-qbraid>`__),
Robin Dahl (`@robin-dahl <https://github.com/robin-dahl>`__),
Sebastian Ehlert (`@awvwgk <https://github.com/awvwgk>`__),
Ikko Eltociear Ashimine (`@eltociear <https://github.com/eltociear>`__),
Thomas Froitzheim (`@thfroitzheim <https://github.com/thfroitzheim>`__),
Marvin Friede (`@marvinfriede <https://github.com/marvinfriede>`__),
Igor Gerasimov (`@foxtran <https://github.com/foxtran>`__),
Johannes Gorges (`@gorges97 <https://github.com/gorges97>`__),
Albert Katbashev (`@Albkat <https://github.com/Albkat>`__),
Sreekanth Kunchapu (`@sreekanth221998 <https://github.com/sreekanth221998>`__),
Rasmus Sloth Lundkvist (`@rasmusslothlundkvist <https://github.com/rasmusslothlundkvist>`__),
Niels Maeder (`@nmaeder <https://github.com/nmaeder>`__),
Marcel Müller (`@marcelmbn <https://github.com/marcelmbn>`__),
Christoph Plett (`@cplett <https://github.com/cplett>`__),
Felix Pultar (`@pultar <https://github.com/pultar>`__),
Thomas Rose (`@Thomas3R <https://github.com/Thomas3R>`__),
Tim Schramm (`@tkschramm <https://github.com/tkschramm>`__),
Leopold Seidler (`@lmseidler <https://github.com/lmseidler>`__),
Christian Selzer (`@chselz <https://github.com/chselz>`__),
Yoel Sevi (`@g8j39 <https://github.com/g8j39>`__),
Philip Turner (`@philipturner <https://github.com/philipturner>`__),
SY Wang (`@Growl1234 <https://github.com/Growl1234>`__),
Daniel Wiczew (`@DanielWicz <https://github.com/DanielWicz>`__),
Lukas Wittmann (`@lukaswittmann <https://github.com/lukaswittmann>`__), and
Shan Yu (`@shaneraphel <https://github.com/shaneraphel>`__) for contributing to this release.

Find the complete release notes `here <https://github.com/grimme-lab/xtb/releases/tag/v6.8.0>`__ and the full changelog `here <https://github.com/grimme-lab/xtb/compare/v6.7.1...v6.8.0>`__.
