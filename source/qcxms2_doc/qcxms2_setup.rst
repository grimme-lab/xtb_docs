*****************
Setting up QCxMS2
*****************

.. contents:: Table of Contents

Pre-compiled binaries from GitHub
=================================

A pre-compiled executable of the latest program version can be found on the `latest release
<https://github.com/grimme-lab/QCxMS2/releases/latest>`_ page on GitHub.
Follow the instructions to use QCxMS2 without the need of building anything. 

Untar the zipped archive:

.. code-block:: text

   > tar -xvzf QCxMS2.v.X.X.tar.xz

Replace the *X.X* with the current version number. 
The program is designed to run on Linux operating systems.

The following files are being extracted:

+-----------------+----------------------------------------------------------------------------------------------+
| qcxms2          |  main program (`executable`)                                                                 |
+-----------------+----------------------------------------------------------------------------------------------+


1. Place the `executable` into your ``$HOME/bin`` directory or path.  



Quantum Chemistry Codes Required
================================

QCxMS2 needs several external codes. 

They have to be installed on your system,
either by using ``environment modules`` or by placing the executables in your ``$HOME/bin`` directory or path
with **exactly** the name as stated in the following Table.
You should check that each of these programs works on your system before running QCxMS2.
The QCxMS2 program will only check for the presence of these programs at the beginning of the calculation.

+----------------------------------+--------------------+----------------------------------------------------------------------+
| required name of the executable  | required version   |    link to the software repository                                   |
+----------------------------------+--------------------+----------------------------------------------------------------------+
| xtb                              | version >= 6.7.1   | `xtb <https://github.com/grimme-lab/xtb>`_                           |
+----------------------------------+--------------------+----------------------------------------------------------------------+
| crest                            | version >= 3.0.2   | `crest <https://github.com/crest-lab/crest>`_                        |
+----------------------------------+--------------------+----------------------------------------------------------------------+
| orca                             | version >= 6.0.0   | `orca <https://orcaforum.kofo.mpg.de>`_                              |
+----------------------------------+--------------------+----------------------------------------------------------------------+
| molbar                           | version >= 1.1.3   | `molbar <https://git.rwth-aachen.de/bannwarthlab/molbar>`_           |
+----------------------------------+--------------------+----------------------------------------------------------------------+
| geodesic_interpolate (optional)  |  version >= 1.0.0  |  `geodesic <https://github.com/virtualzx-nad/geodesic-interpolate>`_ |
+----------------------------------+--------------------+----------------------------------------------------------------------+

Please cite the respective publications when using the software in your research.

