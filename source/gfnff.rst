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

.. tabbed:: command

    .. code:: sh

      xtb --gfnff methionine.xyz --wrtopo nb,bpair,alist,blist,tlist,vtors,vbond,vangl 

.. tabbed:: methionine.xyz

    .. code:: sh

       23
      
       C     1.6161400    3.7269200    5.6749000 
       S     2.9683400    4.8443200    6.1703000 
       C     3.8734400    4.7727200    4.5802000 
       C     3.9410400    6.1686200    3.9510000 
       C     4.5947400    6.1846200    2.5463000 
       C     4.4632400    7.5907200    1.9543000 
       C     5.5898400    8.5315200    1.5624000 
       O     6.8719400    8.0387200    1.8415000 
       O     3.3290400    8.0102200    1.7756000 
       N     5.9829400    5.7196200    2.5996000 
       H     6.3400400    5.6459200    1.6195000 
       H     6.0219400    4.7426200    2.9630000 
       H     7.2762400    7.7642200    0.9767000 
       H     2.0212400    2.7359200    5.3807000 
       H     1.0175400    4.1846200    4.8597000 
       H     0.9502400    3.5698200    6.5478000 
       H     4.8798400    4.3588200    4.7904000 
       H     3.3699400    4.0842200    3.8688000 
       H     2.8943400    6.5330200    3.8527000 
       H     4.4883400    6.8567200    4.6325000 
       H     4.0262400    5.4945200    1.8822000 
       H     5.4882400    8.7853200    0.4803000 
       H     5.4691400    9.4827200    2.1225000 

.. tabbed:: output

    .. code:: sh

      {
         "nb":[
         [      2,     14,     15,     16,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      4],
         [      1,      3,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      2],
         [      2,      4,     17,     18,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      4],
         [      3,      5,     19,     20,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      4],
         [      4,      6,     10,     21,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      4],
         [      5,      7,      9,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      3],
         [      6,      8,     22,     23,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      4],
         [      7,     13,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      2],
         [      6,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      1],
         [      5,     11,     12,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      3],
         [     10,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      1],
         [     10,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      1],
         [      8,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      1],
         [      1,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      1],
         [      1,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      1],
         [      1,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      1],
         [      3,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      1],
         [      3,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      1],
         [      4,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      1],
         [      4,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      1],
         [      5,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      1],
         [      7,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      1],
         [      7,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      0,      1]
         ],
         "bpair":[
               0,      1,      0,      2,      1,      0,      3,      2,      1,      0,      5,      3,      2,      1,      0,      5,      5,      3,      2,      1,      0,      5,      5,      5,      3,      2,      1,      0,      5,      5,      5,      5,      3,      2,      1,      0,      5,      5,      5,      3,      2,      1,      2,      3,      0,      5,      5,      3,      2,      1,      2,      3,      5,      3,      0,      5,      5,      5,      3,      2,      3,      5,      5,      5,      1,      0,      5,      5,      5,      3,      2,      3,      5,      5,      5,      1,      2,      0,      5,      5,      5,      5,      5,      3,      2,      1,      5,      5,      5,      5,      0,      1,      2,      3,      5,      5,      5,      5,      5,      5,      5,      5,      5,      5,      0,      1,      2,      3,      5,      5,      5,      5,      5,      5,      5,      5,      5,      5,      2,      0,      1,      2,      3,      5,      5,      5,      5,      5,      5,      5,      5,      5,      5,      2,      2,      0,      3,      2,      1,      2,      3,      5,      5,      5,      5,      5,      5,      5,      5,      5,      5,      5,      0,      3,      2,      1,      2,      3,      5,      5,      5,      5,      5,      5,      5,      5,      5,      5,      5,      2,      0,      5,      3,      2,      1,      2,      3,      5,      5,      5,      3,      5,      5,      5,      5,      5,      5,      3,      3,      0,      5,      3,      2,      1,      2,      3,      5,      5,      5,      3,      5,      5,      5,      5,      5,      5,      3,      3,      2,      0,      5,      5,      3,      2,      1,      2,      3,      5,      3,      2,      3,      3,      5,      5,      5,      5,      5,      5,      3,      3,      0,      5,      5,      5,      5,      3,      2,      1,      2,      3,      5,      5,      5,      3,      5,      5,      5,      5,      5,      5,      5,      5,      0,      5,      5,      5,      5,      3,      2,      1,      2,      3,      5,      5,      5,      3,      5,      5,      5,      5,      5,      5,      5,      5,      2,      0   ],
         "alist":[
         [       1,      14,       2],
         [       1,      15,       2],
         [       1,      15,      14],
         [       1,      16,       2],
         [       1,      16,      14],
         [       1,      16,      15],
         [       2,       3,       1],
         [       3,       4,       2],
         [       3,      17,       2],
         [       3,      17,       4],
         [       3,      18,       2],
         [       3,      18,       4],
         [       3,      18,      17],
         [       4,       5,       3],
         [       4,      19,       3],
         [       4,      19,       5],
         [       4,      20,       3],
         [       4,      20,       5],
         [       4,      20,      19],
         [       5,       6,       4],
         [       5,      10,       4],
         [       5,      10,       6],
         [       5,      21,       4],
         [       5,      21,       6],
         [       5,      21,      10],
         [       6,       7,       5],
         [       6,       9,       5],
         [       6,       9,       7],
         [       7,       8,       6],
         [       7,      22,       6],
         [       7,      22,       8],
         [       7,      23,       6],
         [       7,      23,       8],
         [       7,      23,      22],
         [       8,      13,       7],
         [      10,      11,       5],
         [      10,      12,       5],
         [      10,      12,      11]
         ],
         "blist":[
         [       2,       1],
         [       3,       2],
         [       4,       3],
         [       5,       4],
         [       6,       5],
         [       7,       6],
         [       8,       7],
         [       9,       6],
         [      10,       5],
         [      11,      10],
         [      12,      10],
         [      13,       8],
         [      14,       1],
         [      15,       1],
         [      16,       1],
         [      17,       3],
         [      18,       3],
         [      19,       4],
         [      20,       4],
         [      21,       5],
         [      22,       7],
         [      23,       7]
         ],
         "tlist":[
         [      14,       2,       1,       3,       3],
         [      15,       2,       1,       3,       3],
         [      16,       2,       1,       3,       3],
         [       1,       3,       2,       4,       3],
         [       1,       3,       2,       4,       1],
         [       1,       3,       2,      17,       3],
         [       1,       3,       2,      18,       3],
         [       2,       4,       3,       5,       3],
         [       2,       4,       3,       5,       1],
         [      17,       4,       3,       5,       3],
         [      18,       4,       3,       5,       3],
         [       2,       4,       3,      19,       3],
         [      17,       4,       3,      19,       3],
         [      18,       4,       3,      19,       3],
         [       2,       4,       3,      20,       3],
         [      17,       4,       3,      20,       3],
         [      18,       4,       3,      20,       3],
         [       3,       5,       4,       6,       3],
         [      19,       5,       4,       6,       3],
         [      20,       5,       4,       6,       3],
         [       3,       5,       4,      10,       3],
         [       3,       5,       4,      10,       1],
         [      19,       5,       4,      10,       3],
         [      20,       5,       4,      10,       3],
         [       3,       5,       4,      21,       3],
         [      19,       5,       4,      21,       3],
         [      20,       5,       4,      21,       3],
         [       4,       6,       5,       7,       3],
         [      10,       6,       5,       7,       3],
         [      21,       6,       5,       7,       3],
         [       4,       6,       5,       9,       3],
         [      10,       6,       5,       9,       3],
         [      21,       6,       5,       9,       3],
         [       5,       7,       6,       8,       3],
         [       9,       7,       6,       8,       3],
         [       5,       7,       6,      22,       3],
         [       9,       7,       6,      22,       3],
         [       5,       7,       6,      23,       3],
         [       9,       7,       6,      23,       3],
         [       6,       8,       7,      13,       3],
         [      22,       8,       7,      13,       3],
         [      23,       8,       7,      13,       3],
         [       4,      10,       5,      11,       3],
         [       6,      10,       5,      11,       3],
         [      21,      10,       5,      11,       3],
         [       4,      10,       5,      12,       3],
         [       6,      10,       5,      12,       3],
         [      21,      10,       5,      12,       3],
         [       6,       9,       7,       5,       0],
         [      10,      12,      11,       5,      -1]
         ],
         "vtors":[
         [        3.141592653589793,        0.118386604068957],
         [        3.141592653589793,        0.118386604068957],
         [        3.141592653589793,        0.118386604068957],
         [        3.141592653589793,        0.076976005023595],
         [        3.141592653589793,       -0.069278404521235],
         [        3.141592653589793,        0.114204638622952],
         [        3.141592653589793,        0.114204638622952],
         [        3.141592653589793,        0.072738354750599],
         [        3.141592653589793,       -0.065464519275539],
         [        3.141592653589793,        0.140686351535105],
         [        3.141592653589793,        0.140686351535105],
         [        3.141592653589793,        0.107917493454928],
         [        3.141592653589793,        0.208727822797810],
         [        3.141592653589793,        0.208727822797810],
         [        3.141592653589793,        0.107917493454928],
         [        3.141592653589793,        0.208727822797810],
         [        3.141592653589793,        0.208727822797810],
         [        3.141592653589793,        0.096670154428164],
         [        3.141592653589793,        0.143423655835469],
         [        3.141592653589793,        0.143423655835469],
         [        3.141592653589793,        0.039382001805517],
         [        3.141592653589793,       -0.035443801624965],
         [        3.141592653589793,        0.058428691941974],
         [        3.141592653589793,        0.058428691941974],
         [        3.141592653589793,        0.137761976556490],
         [        3.141592653589793,        0.204389104680023],
         [        3.141592653589793,        0.204389104680023],
         [        3.141592653589793,        0.058204814188074],
         [        3.141592653589793,        0.024686283129367],
         [        3.141592653589793,        0.086354959164580],
         [        3.141592653589793,        0.078881875373477],
         [        3.141592653589793,        0.033456000786343],
         [        3.141592653589793,        0.117032263082078],
         [        3.141592653589793,        0.076710273966552],
         [        3.141592653589793,        0.103961336451352],
         [        3.141592653589793,        0.092535374976494],
         [        3.141592653589793,        0.125408250474742],
         [        3.141592653589793,        0.092535374976494],
         [        3.141592653589793,        0.125408250474742],
         [        3.141592653589793,        0.186253915770626],
         [        3.141592653589793,        0.265425329355552],
         [        3.141592653589793,        0.265425329355552],
         [        3.141592653589793,        0.132817434288604],
         [        3.141592653589793,        0.138275905010317],
         [        3.141592653589793,        0.197053186653533],
         [        3.141592653589793,        0.132817434288604],
         [        3.141592653589793,        0.138275905010317],
         [        3.141592653589793,        0.197053186653533],
         [        0.000000000000000,       31.992304703128671],
         [        1.396263401595464,        2.400000000000000]
         ],
         "vbond":[
         [       -0.110000000000000,        0.467899259009000,       -0.112215986839532],
         [       -0.110000000000000,        0.467899259009000,       -0.112851087553402],
         [       -0.110000000000000,        0.467792780000000,       -0.150438734862904],
         [       -0.110000000000000,        0.467792780000000,       -0.154170276474606],
         [       -0.110000000000000,        0.475292448176000,       -0.160210176201971],
         [       -0.110000000000000,        0.475292448176000,       -0.160171412041268],
         [       -0.110000000000000,        0.561506138921000,       -0.136837663693344],
         [       -0.322487948010800,        0.584232406121000,       -0.285054206400398],
         [       -0.110000000000000,        0.496199013401000,       -0.152975058069485],
         [       -0.182000000000000,        0.551272323056000,       -0.175937864842309],
         [       -0.182000000000000,        0.551272323056000,       -0.175937864842309],
         [       -0.182000000000000,        0.649706251376000,       -0.138077009113789],
         [       -0.182000000000000,        0.482285756225000,       -0.167943800868609],
         [       -0.182000000000000,        0.482285756225000,       -0.167943800868609],
         [       -0.182000000000000,        0.482285756225000,       -0.167943800868609],
         [       -0.182000000000000,        0.482285756225000,       -0.166952944346268],
         [       -0.182000000000000,        0.482285756225000,       -0.166952944346268],
         [       -0.182000000000000,        0.482285756225000,       -0.167671071232576],
         [       -0.182000000000000,        0.482285756225000,       -0.167671071232576],
         [       -0.182000000000000,        0.482285756225000,       -0.161689434312679],
         [       -0.182000000000000,        0.482285756225000,       -0.161120558946464],
         [       -0.182000000000000,        0.482285756225000,       -0.161120558946464]
         ],
         "vangl":[
         [        1.911135530933791,        0.200778114133477],
         [        1.911135530933791,        0.200778114133477],
         [        1.895427567665842,        0.208204952873680],
         [        1.911135530933791,        0.200778114133477],
         [        1.895427567665842,        0.208204952873680],
         [        1.895427567665842,        0.208204952873680],
         [        1.553343034274953,        0.182883959629499],
         [        1.911135530933791,        0.243354063736497],
         [        1.911135530933791,        0.200742258301133],
         [        1.911135530933791,        0.251234156795870],
         [        1.911135530933791,        0.200742258301133],
         [        1.911135530933791,        0.251234156795870],
         [        1.895427567665842,        0.208435528613969],
         [        1.911135530933791,        0.304181363605821],
         [        1.911135530933791,        0.251145411902355],
         [        1.911135530933791,        0.250829369984220],
         [        1.911135530933791,        0.251145411902355],
         [        1.911135530933791,        0.250829369984220],
         [        1.895427567665842,        0.208288109961894],
         [        1.911135530933791,        0.304861560227867],
         [        1.911135530933791,        0.304313734371915],
         [        1.911135530933791,        0.305384647135346],
         [        1.911135530933791,        0.251387506989717],
         [        1.911135530933791,        0.252265268996720],
         [        1.911135530933791,        0.251816667453799],
         [        2.094395102393195,        0.246255994720080],
         [        2.094395102393195,        0.180087663620925],
         [        2.094395102393195,        0.180453005836934],
         [        1.893682238413847,        0.325682889245059],
         [        1.911135530933791,        0.253033590008831],
         [        1.893682238413847,        0.268303564511390],
         [        1.911135530933791,        0.253033590008831],
         [        1.893682238413847,        0.268303564511390],
         [        1.895427567665842,        0.209663659187475],
         [        1.823869068334074,        0.324258070921634],
         [        1.815142422074103,        0.179091512984941],
         [        1.815142422074103,        0.179091512984941],
         [        1.815142422074103,        0.189219815802861]
         ],
         "program call": "../build/xtb --gfnff methionine.xyz --wrtopo nb,bpair,alist,blist,tlist,vtors,vbond,vangl",
         "method": "GFN-FF",
         "xtb version": "6.4.1 (546be4a)"
      }

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
