Gaussian interface
==================

A perl wrapper script for using *xtb* together with `Gaussian <http://gaussian.com/>`_.
The source code is hosted on GitHub: `aspuru-guzik-group/xtb-gaussian <https://github.com/aspuru-guzik-group/xtb-gaussian>`_.


Usage
-----

Requires an existing *xtb* installation, the *xtb* binary has to be available in the *PATH*.
The script *xtb-gaussian* has to be put available in *PATH* as well and is used to invoke *xtb* from Gaussian using the ``external`` command:

.. code-block:: text

   %chk=ts
   # external="xtb-gaussian -P 12 --etemp 300.0"
    opt=(calcfc,ts,noeigentest,nodownhill,nomicro)

Command line arguments provided to the *xtb-gaussian* wrapper will be passed through to the *xtb* program.
