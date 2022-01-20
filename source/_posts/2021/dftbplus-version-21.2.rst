:date: 2021-12-13
:author: Sebastian Ehlert
:category: release
:tags: dftbplus xtb

DFTB+ version 21.2 released
===========================

A new version of DFTB+ is now available with support for the xTB methods.
Due to the integration in DFTB+ most of the features available for the DFTB Hamiltonian can be readily used with the xTB Hamiltonians as well, including periodic calculations with k-point sampling, geometry optimizations, molecular dynamics and frequency calculations.
Both GFN1-xTB and GFN2-xTB are available for the DFTB+ version 21.2 at the moment.

Furthermore, we integrated parts the battle-proven rational function optimizer from ``xtb --opt`` into DFTB+ to allow fast and robust geometry optimizations both for molecular and periodic systems.
Preliminary tests show an order of magnitude improvements in the convergence compared with the previous default (c.f. `dftbplus#862 <https://github.com/dftbplus/dftbplus/pull/862>`_).

You can find the full changelog for DFTB+ 21.2 `here <https://github.com/dftbplus/dftbplus/blob/21.2/CHANGELOG.rst>`_.


Installing DFTB+ with xTB support
---------------------------------

A complete DFTB+ package is available via `conda-forge`_.
To install the *conda* package manager we recommend the `miniforge <https://github.com/conda-forge/miniforge/releases>`_ installer.
If the *conda-forge* channel is not yet enabled, add it to your channels with

.. code-block:: bash

    conda config --add channels conda-forge

Once the *conda-forge* channel has been enabled, DFTB+ can be installed with:

.. code-block:: bash

   conda install 'dftbplus=*=nompi_*'

Or to install the MPI enabled version, you can add *mpich* or *openmpi* as MPI provider or just let conda choose:

.. code-block:: bash

   conda install 'dftbplus=*=mpi_*'

It is possible to list all of the versions available on your platform with:

.. code-block:: bash

   conda search dftbplus --channel conda-forge

Now you are ready to use the ``dftb+`` executable, find the *tblite* library is installed as well as part of the DFTB+ dependencies.

.. _conda-forge: https://anaconda.org/conda-forge/dftbplus


Input structure
---------------

Input to DFTB+ is provided by creating a file named ``dftb_in.hsd`` in the custom human-friendly structured data (HSD) format, which is inspired by XML.
The geometry input is provided in the *Geometry* group, either as *xyzFormat* for molecular geometries or as *vaspFormat* for periodic geometries.
To enable the xTB methods the *Hamiltonian* group is set to *xTB*.
In the *xTB* group the *Method* keyword is provided to select between GFN2-xTB, GFN1-xTB and IPEA1-xTB.

.. code-block:: bash
   :caption: Molecular GFN2-xTB calculation

   Geometry = xyzFormat {
   <<< "struc.xyz"
   }

   Hamiltonian = xTB {
     Method = "GFN2-xTB"
   }

To perform periodic calculations with the xTB Hamiltonian only the k-point sampling has to be added to the *xTB* group with *kPointsAndWeights*.
Using the *SuperCellFolding* provides Monkhorst–Pack k-point sampling.

.. code-block:: bash
   :caption: Periodic GFN1-xTB calculation

   Geometry = vaspFormat {
   <<< "POSCAR"
   }

   Hamiltonian = xTB {
     Method = "GFN1-xTB"
     kPointsAndWeights = SuperCellFolding {
       2   0   0
       0   2   0
       0   0   2
       0.5 0.5 0.5
     }
   }

Instead of providing a *Method* the xTB method can be initialized from a parameter file by providing its path in *ParameterFile*.

.. code-block:: bash

   # ...
   Hamiltonian = xTB {
     ParameterFile = "gfn2-xtb.toml"
     # ...
   }

Finally, to perform more than just single point calculations, the *Driver* group has to be provided.
Best is to use the new *GeometryOptimization* driver, which defaults to a rational function minimizer as present in the ``xtb`` program package.
For periodic structures the lattice optimization can be enabled by setting *LatticeOpt* to *Yes*.

.. code-block:: bash

   Driver = GeometryOptimization {
     LatticeOpt = Yes
   }

For the full capabilities for DFTB+ check the `reference manual <https://dftbplus.org>`_.
