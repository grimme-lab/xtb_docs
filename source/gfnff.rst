.. _gfnff:

----------------------------
GFN-Force-Field (GFN-FF)
----------------------------

.. contents::

Introducing GFN-FF
========================
``GFN-FF`` is a completely automated partially polarizable generic force-field for the accurate description of 
structures and dynamics of large molecules across the periodic table. This method combines force-field speed 
with almost quantum mechanical accuracy.
The main publication for ``GFN-FF`` can be found at: `Angewandte Chemie <https://onlinelibrary.wiley.com/doi/abs/10.1002/anie.202004239>`_.

.. figure:: ../figures/gfnff.jpg
   :scale: 25 %
   :alt: gfnff
   

Theoretical background
=================================

The latest progress in the field of semi empirical methods, regarding the evolution of GFN1, GFN2 and GFN0-xTB, inspired the development of a generic force-field. 
The main focus of this GFN force-field (GFN-FF) is directed towards the description of bio-macromolecular systems such as (metallo-) proteins, supramolecular assemblies 
and metal-organic frameworks. 
It is intended for the usage as a versatile tool for drug design in life sciences and structure screening in various fields of chemistry.
Therefore, GFN-FF introduces an approximation to the remaining quantum mechanics in GFN0-xTB by replacing the extended Hückel theory by molecular mechanical bond strech, 
bond angle angle and torsion angle terms for the description of covalent bonds. 
To remain accurate in the description of conjugated systems, GFN-FF retains an iterative Hückel scheme for a selected set of atoms. 
The resulting bond orders obtained by a Hückel calculation have an influence on force constants and energy relevant parameters of the system.
To yield accurate results the FF parameters are fitted to reproduce B97-3c minimum geometries and frequencies. 
Thereby a strictly global and element specific parameter strategy is applied and no element pair specific parameters are employed.
Special attention is paid to the simple application. As input only Cartesian coordinates and elemental composition are required from which fully automatically all potential energy terms are constructed.
The total GFN-FF energy expression is given by

.. math::
   E_{GFN-FF} = E_{cov} + E_{NCI},

where :math:`E_{cov}` refers to the bonded FF energy and :math:`E_{NCI}` describes the intra- and intermolecular noncovalent interactions.
In the covalent part interactions are described by asymptotically correct (dissociative) bonding, angular and torsional terms. 
Repulsive terms are added for bonded and non-bonded interactions separately. 
Additionally a three-body correction to the nuclear repulsion is added, that extends beyond the sum of pair-wise interactions.

.. math::
   E_{intra} = E_{bond} + E_{bend} + E_{tors} + E_{rep}^{bond} + E_{abc}^{bond}

In the noncovalent part, electrostatic interactions are described by an electronegativity equilibrium (EEQ) model. 
It is employed to calculate the isotropic electrostatic energy and atomic partial charges. 
Overall GFN-FF uses two sets of EEQ charges. One set depends on the standard geometry, whereas another set of charges is exclusively topology based, introducing partial polarizability to the FF method.
Dispersion interactions are taken into account by a topology based version of the D4 scheme, in which the dispersion coefficients are scaled by atomic charges instead of atomic polarizabilities.
For the description of hydrogen and halogen bonds additional charge scaled H- and X- bond corrections are applied to yield the right binding motifs.

.. math::
   E_{NCI} = E_{IES} + E_{disp} + E_{HB} + E_{XB} + E_{rep}^{NCI}

GFN-FF reaches quadratic scaling in terms of energy and gradient calculation, whereas all GFN-xTB methods show cubic scaling with respect to the number of atoms.
It is the computationally most efficient member of the GFN family.

How to use it
============================
GFN-FF is implemented in the ``xtb`` program. It extends the portfolio of different GFN parametrizations by a non-electronic variant. GFN-FF is applied with the keyword ``--gfnff`` as shown in the example below.

.. code:: bash

  > xtb --gfnff <geometry> [options]

Thus, the usage is in line with its semiempirical QM siblings and (almost) the same options are available. Only the request of electronic structure properties will be ignored, since those are not available at the force-field level of theory. With GFN-FF you can perform single point calculations, geometry optimizations, frequency calculations and molecular dynamics simulations, just to mention the most prominent run types. A GFN-FF calculation is indicated in the output by:

.. code:: bash

   ------------------------------------------------- 
  |                   G F N - F F                   |
  |          A general generic force-field          |
  |                  Version 1.0.1                  | 
   ------------------------------------------------- 

The topology file
--------------------

GFN-FF generates a topology file named ``gfnff_topo`` automatically upon first use on an input structure. This file saves system specific parameters and derived force constants as well as the entire topological information. If the force-field calculation is repeated, the topology file is read. 

.. code:: bash

   GFN-FF topology read from file successfully!

Depending on the system size, this speeds up you the overall computation time If large structural changes are applied to the structure, the topology file should better be deleted. The next calculation will generate a new one, according to the modified structure. If parameters are changes in the code, the topology file should also be deleted, as the old parameters are also saved there.

The following lists from the GFN-FF topology can be exported to a JSON file named ``gfnff_lists.json`` using the keyword ``--wrtopo``. The neighbor list **nb** contains the indices of neighboring atoms for each atom and the number of its neighbors in the last entry. The bond pair list **bpair** is a packed matrix holding the number of bonds between atom pairs. A maximum of five bonds between atoms is considered, setting pairs with more bonds inbetween to this value. The angle list **alist** contains sets of three bonded atoms forming an angle. The bond list **blist** contains all bonded atoms. The torsion list **tlist** contains sets of four bonded atoms forming a torsion and a torsion angle prefactor as the last entry. The bond, angle and torsion parameters are requested with **vbond**, **vangl** and **vtors** respectively. The requested lists have to be separated with a comma, as shown in the example below.

.. code:: bash

  > xtb --gfnff <geometry> --wrtopo nb,bpair,alist,blist,tlist,vtors,vbond,vangl 


GFN-FF specific settings
============================

``xtb`` is a semiempirical extended tight-binding program package and its default values are chosen to yield robust and accurate results for all GFN-xTB methods. GFN-FF represents the first non-electronic variant and thus it should come as no surprise, that some of the default values do not work with a generic force-field. Settings that deviate from the defaults are discussed below.

Parallelisation
-------------------

The ``xtb`` program uses OMP parallelisation. To calculate larger systems an appropriate OMP stacksize must be provided. Since the system size may easily exceed 5000 atoms in force-field calculations, a large number should be chosen. Otherwise you may encounter a segmentation fault. For 5000 atoms you may choose:

.. code:: bash

  > export OMP_STACKSIZE=5G
  
As a rule of thumb, add 1G for every additional 1000 atoms. 
  
MD Simulations
------------------

For molecular dynamics simulations, the default time step of 4 ps is not stable in GFN-FF. Below you can find our recommended settings for a stable MD run.

.. code:: bash

   $md
     step=2.0
     hmass=4.0
     shake=0
   $end

GFN-FF specific input
============================

``xtb`` accepts various input formats. Especially the possibility to directly read ``pdb`` files as input might be something you want to use in combination with GFN-FF. If the pdb file includes charge information, ``xtb`` reads this information, determines the overall charge of the system automatically and applies this charge constrain per residue. There is no need to further specify the total charge of the system. The following output is generated.

.. code:: bash

   charge from pdb residues: <integer>

Use of additional charge information
-------------------------------------

In GFN-FF the computed atomic charges from the EEQ model may be improved by constrains if additional information about the charge distribution in the system is known. There are two further ways to incorporate this information besides using a pdb file. If the system consists of more than one NCI fragment, the charges per fragment can be written by the user into a specific file
(named ``.CHRG``) and will be constrained accordingly in the EEQ model, thus preventing artificial charge transfer between the NCI fragments. If a GFN-xTB calculation is performed in advance, the written file ``charges`` is read by the program and the corresponding QM charges are used to constrain the values on the molecular fragments.


2D to 3D structure converter
============================

``xtb`` feaetures a 2D to 3D structure converter for ``sdf`` files. If a two-dimensional sdf file input is passed to ``xtb`` and a GFN2-xTB single point calculation is requested, it will automatically perform a combination of GFN-FF optimization and molecular dynamics steps to generate a three dimensional structure, on which the GFN2-xtB calculation is performed.

.. code:: bash

  > xtb 2d_input.sdf --gfn 2 --sp
  
The keyword ``--gfnff`` is not needed here. The start of the structure conversion is indicated in the output by,

.. code:: bash

   ------------------------------------------------- 
  |                     2D => 3D                    |
  |          A structure converter based on         |
  |                   G F N - F F                   |
   ------------------------------------------------- 

and the successful conversion is confirmed by:

.. code:: bash
   
   ------------------------------------------------- 
  |           2D => 3D conversion done!             |
   -------------------------------------------------
   converted geometry written to: gfnff_convert.sdf
