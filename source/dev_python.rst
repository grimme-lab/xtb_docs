.. _python:

-------------------------
 Using ``xtb`` in Python
-------------------------

In this section the application programmable interface (API) of the
``xtb`` program package is described.

This section targets mainly developers trying to interface their (Python) scripts
with ``xtb``.
The necessary files are currently *not* distributed in the main version of ``xtb``,
but are available upon request.

.. contents::

Setting up ASE
==============

First of all, get a version of the atomic simulation environment (ASE), usually

.. code:: bash

   > pip3 install ase [--user]

works fine on most machines. For more details refer to the `ASE`_ documentation.

.. _ASE: https://wiki.fysik.dtu.dk/ase/

Loading the Shared Library
==========================

.. note:: This is the basic approach to include an interface to a C-API
          in Python, in most circumstances you can skip this section
          since I already wrapped up everything nicely.
          If you plan to modify the C-API and the Python wrappers,
          this section is *important* for everything you do.

The ``xtb`` program package contains a shared object which has to be included
in your ``LD_LIBRARY_PATH``, you can simply do this by using

.. code:: bash

   > export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/path/to/xtb/build

.. warning:: (Ab)using your ``LD_LIBRARY_PATH`` this way is generally
             not recommended, unless I have figured out how to do it
             correctly in Python this might be your best choice.

to allow loading of the shared library.
Test this by running:

.. code:: python

   import ctypes
   from ctypes import cdll

   try:
      xtb = cdll.LoadLibrary('libxtb.so')
   except OSError:
      print("xtb library was not found in your LD_LIBRARY_PATH")

If you can successfully load the shared object, specify the necessary interface
for calling ``xtb`` by defining the PEEQ_options structure and argument types
as in:

.. code:: python

   import ctypes
   from ctypes import cdll, Structure, c_int, c_double, c_bool, c_char_p, POINTER

   xtb = cdll.LoadLibrary('libxtb.so')

   class PEEQ_options(Structure):
       _fields_ = [('prlevel',c_int),
                   ('parallel',c_int),
                   ('acc',    c_double),
                   ('etemp',  c_double),
                   ('grad',    c_bool),
                   ('ccm',     c_bool)]

   c_int_p = POINTER(c_int)
   c_bool_p = POINTER(c_bool)
   c_double_p = POINTER(c_double)
   peeq.GFN0_PBC_calculation.argtypes = [c_int_p, c_int_p,
           c_double_p, c_double_p, c_double_p,
           c_bool_p, POINTER(PEEQ_options), c_char_p,
           c_double_p, c_double_p, c_double_p]

now Python knows how to call ``xtb`` from the shared object. Remember that
``xtb`` is a Fortran program, so we prefer passing by reference over passing by
value.

.. tip:: You can always check the header definitions in ``include/xtb.h``.

Using as ASE Calculator
=======================

To perform a calculation with the ASE we not only need Python bindings
but also an abstract interface to other ASE functions.
The easiest way to provide such an interface is by creating an ASE ``Calculator``
class. My current approach is to have an abstract class performing all
the nasty interfacing stuff (loading the library, storing default values and
stuff like that) and specific instances of this class for every
available method from ``xtb``, namely GFN2-xTB (as ``GFN2``),
GFN1-xTB (as ``GFN1``) and GFN0-xTB (as ``GFN0`` and ``GFN0_PBC`` for molecular
and periodic calculations, respectively).
An complete implementation of this setup is shipped with ``xtb`` at
``python/xtb.py`` and should be ready-to-use with some minor tweaking.
To make it available for scripting in Python use

.. code:: bash

   > export PYTHONPATH=$PYTHONPATH:/path/to/peeq/python

Here is an example with rutile using this VASP geometry input:

.. code:: text

   Ti  O
    1.0000000000000000
        4.6257    0.0000    0.0000
        0.0000    4.6257    0.0000
        0.0000    0.0000    2.9806
      2   4
   Cartesian
     0.00000000  0.00000000  0.00000000
     2.31285000  2.31285000  1.49030000
     1.30490997  1.30490997  0.00000000
     1.00794003  3.61775997  1.49030000
     3.32079003  3.32079003  0.00000000
     3.61775997  1.00794003  1.49030000

To give you an idea how this is going to work out, here is the final
code snippet:

.. code:: python

   import xtb
   from xtb import GFN0_PBC

   import ase
   from ase.io import read, write
   from ase.units import Hartree
   from ase.optimize.precon import Exp, PreconFIRE
   from ase.constraints import ExpCellFilter

   # read molecular structure data, here from a VASP geometry input
   mol = read("POSCAR", format = 'vasp')

   # create the calculator for GFN0-xTB under periodic boundary conditions
   calc = GFN0_PBC(print_level = 3)
   mol.set_calculator(calc)

   # initial single point calculation
   e = mol.get_potential_energy()
   print("Initial energy: eV, Eh", e, e/Hartree)

   # setup optimization of cell parameters
   ecf = ExpCellFiler(mol)
   precon = Exp(A = 3)
   relax = preconFire(ecf, precon = precon, trajectory = 'peeqopt.traj')

   # do the optimization
   relax.run(fmax = 5e-2)

   # get the final single point energy
   e = mol.get_potential_energy()
   print("Final energy:   eV, Eh", e, e/Hartree)

   # write final geometry to file
   write("xtbopt.POSCAR", mol, format = 'vasp')

running this script with the input for rutile we should find something similar
to this output (maybe including some warnings from the ASE).

.. code:: text

   Initial energy: eV, Eh -440.6471068912027 -16.193482628801494
   PreconFIRE:   0  09:28:06     -440.647107       1.7119       0.1061
   PreconFIRE:   1  09:28:07     -440.673281       1.7110       0.1056
   PreconFIRE:   2  09:28:07     -440.725466       1.7076       0.1045
   PreconFIRE:   3  09:28:07     -440.803152       1.6977       0.1026
   PreconFIRE:   4  09:28:07     -440.905138       1.6747       0.0993
   PreconFIRE:   5  09:28:07     -441.028875       1.6284       0.0941
   PreconFIRE:   6  09:28:08     -441.169498       1.5430       0.0860
   PreconFIRE:   7  09:28:08     -441.318524       1.3969       0.0738
   PreconFIRE:   8  09:28:08     -441.462322       1.1298       0.0539
   PreconFIRE:   9  09:28:08     -441.600489       0.6531       0.0220
   PreconFIRE:  10  09:28:08     -441.654277       0.1566       0.0277
   PreconFIRE:  11  09:28:09     -441.515093       0.1524       0.0275
   PreconFIRE:  12  09:28:09     -441.652546       0.1441       0.0270
   PreconFIRE:  13  09:28:09     -441.653083       0.1319       0.0264
   PreconFIRE:  14  09:28:09     -441.653747       0.1161       0.0256
   PreconFIRE:  15  09:28:09     -441.654502       0.0972       0.0247
   PreconFIRE:  16  09:28:10     -441.655309       0.0756       0.0236
   PreconFIRE:  17  09:28:10     -441.656129       0.0519       0.0225
   PreconFIRE:  18  09:28:10     -441.656933       0.0242       0.0212
   Final energy:   eV, Eh -441.65702130913525 -16.230596299418206

The final geometry can be found in ``xtbopt.POSCAR`` and can be viewed
with *e.g.*

.. code:: bash

   > ase gui xtbopt.POSCAR

The optimization log is kept in a ``pickle`` trajectory and can also be
viewed with the ``ase gui``.

