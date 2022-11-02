.. _xtb_ir:

------------------
 IR Submodule
------------------

.. note::
   This feature will be present in the upcoming release version 6.5.2

The IR submodule allows to evaluate IR intensities from data produced
in previous calculations.
To run the IR submodule use

.. code-block:: text

   xtb ir [options] <geometry> --dftbplus [hessian] --born [borncharges]

The geometry can be any format accepted by ``xtb``, while the hessian format depends on the 
options passed to the IR submodule. Right now, only the projected DFTB+ hessian data group is 
available.

Mandatory keywords are:

--born
   Read a DFTB+ dipole derivatives / born charges ``born.out`` file

--dftbplus
   Read a DFTB+ ``hessian.out`` file, projection will be performed automatically

Please find additional keywords in the documentation for the ``thermo`` submodule.
IR intensities, sometimes also called *Born charges*, can equivalently be obtained from 
   1. mixed 2nd derivatives of the energy with respect to atomic positions and external
      electric field
   2. 1st derivative of dipole moment with respect to atomic positions
   3. 1st derivative of forces with respect to external electric field

In DFTB+, the second pathway is employed.
This module aims to easen the calculation of SQM solid state IR spectra for large molecular
crystals or organic crystals with very large unit cells, where automated supercell
generation and phonon vibration evaluation is no longer computationally feasible, not even
with SQM methods. Please keep in mind, that these missing lattice vibrations introduce an 
unkown error into the low-wavenumber area.
