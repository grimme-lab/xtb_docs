Documentation for xtb and related software
==========================================

.. image:: https://readthedocs.org/projects/xtb-docs/badge/?version=latest
   :alt: Documentation Status
   :target: https://xtb-docs.readthedocs.io
.. image:: https://img.shields.io/github/v/release/grimme-lab/xtb?label=xtb
   :alt: xtb release
   :target: https://github.com/grimme-lab/xtb/releases/latest
.. image:: https://img.shields.io/github/v/release/grimme-lab/crest?label=crest
   :alt: crest release
   :target: https://github.com/grimme-lab/crest/releases/latest
.. image:: https://img.shields.io/github/v/release/grimme-lab/enso?label=enso
   :alt: enso release
   :target: https://github.com/grimme-lab/enso/releases/latest
.. image:: https://img.shields.io/github/v/release/grimme-lab/CENSO?label=CENSO
   :alt: CENSO release
   :target: https://github.com/grimme-lab/CENSO/releases/latest

This is the official documentation for the extended tight binding (xtb) program
package and related or dependent methods, like the conformer-rotamer ensemble
search tool (crest) and the energetic sorting of conformer rotamer ensembles (enso)
program.

Get the latest version of of the programs by clicking on the respective badge.

This documentation is currently hosted at
`readthedocs.org <https://xtb-docs.readthedocs.io>`_.

Building this documentation
---------------------------

This documentation is built with sphinx.

You can easily setup sphinx with the provided conda environment file

.. code::

   conda env create -n sphinx -f env.yml
   conda activate sphinx


Build the documentation by invoking

.. code::

   make html


Contributors
------------

* Fabian Bohle
* Markus Bursch
* Eike Caldeweyher
* Sebastian Dohm
* Sebastian Ehlert
* Jeroen Koopman
* Hagen Neugebauer
* Philipp Pracht
* Karola Schmitz
* Sarah Schmitz
* Jakob Seibert
* Sebastian Spicher
* Johannes Gorges
