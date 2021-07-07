Gaussian interface
==================

Projects to interface *xtb* with with `Gaussian <http://gaussian.com/>`_.

xtb-gaussian
------------

A perl wrapper script to make the *xtb* binary usable with the ``external`` command.
The source code is hosted on GitHub: `aspuru-guzik-group/xtb-gaussian <https://github.com/aspuru-guzik-group/xtb-gaussian>`_.

Requires an existing *xtb* installation, the *xtb* binary has to be available in the *PATH*.
The script *xtb-gaussian* has to be available in the *PATH* as well and is used to invoke *xtb* from Gaussian using the ``external`` command:

.. code-block:: text

   %chk=ts
   # external="xtb-gaussian -P 12 --etemp 300.0"
    opt=(calcfc,ts,noeigentest,nodownhill,nomicro)

Command line arguments provided to the *xtb-gaussian* wrapper will be passed through to the *xtb* program.


gau_xtb
-------

A shell script wrapper for *xtb* with additional tools written in Fortran.
The project homepage can be `here <http://sobereva.com/soft/gau_xtb/>`_.
