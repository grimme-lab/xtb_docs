.. _oniom:

-------
 ONIOM
-------

This guide is aimed to give the general overview over the ONIOM implementation within ``xtb``.

.. contents::

Theoretical excurse
====================


The ONIOM is a multiscale calculation model used generally to handle large molecular systems. The detailed review of the ONIOM and its applications can be found at: `Chung2015 <https://pubs.acs.org/doi/10.1021/cr5004419>`_.

The total 2-layer ONIOM energy expression is given by:

.. math::
   E_{ONIOM} = E_{whole,low} - E_{model,low} + E_{model,high}

where :math:`E_{whole,low}` and :math:`E_{model,low}` refer to the single point energies calculated at low level of theory applied to the whole system and its specific model cutoff, :math:`E_{modle,high}` is the SP energy of the same system cutoff at high level of theory. The idea is to combine different levels of quantum chemical methods in the subtructive manner.


Input
=====

To perform the ONIOM calculation with ``xtb`` one has to use ``--oniom`` flag, to specify calculation **methods**, and to list the **inner region cutoff**:

.. code:: sh
   
   > xtb inp.xyz --oniom high:low inner_region_cutoff

Default methods: ``gfn2:gfnff`` 


Methods
-------

xTB:

* *gfnff, gfn1, gfn2*

External:

.. important::

   The ONIOM routine includes but not limited to the ``xtb`` functionality. To expand the range of the available methods, the ONIOM utilizes ``ORCA`` and ``TURBOMOLE`` software packages. This can be done by including the path to the binary in the environment variable ``$PATH``.  

The best way to configure the external settings for the xtb run is to use `xcontrol <https://github.com/grimme-lab/xtb/blob/main/man/xcontrol.7.adoc>`_ instructions.


* *orca*

The ORCA uses its own `input format <https://www.orcasoftware.de/tutorials_orca/first_steps/input_output.html>`_ to run calculations. To specify your own input file, use xcontrol:

.. code:: sh
   
   $external
      orca input file = <filename>.inp

In this case one has to be aware that ``.xyz`` specified in the structure section will be overwritten by the xtb with the inner region geometry. If no input is provided, xtb will create its own ORCA input with the default settings('*b97-3c*'). It is possible to change the default calculation method by providing ``orca input string`` variable in the ``xcontrol`` set.


* *turbomole*

Similarly, the `Turbomole <https://www.turbomole.org/>`_ uses its own format that is defined by the **control** file. 
Initially, the ``xtb`` searches for the user's **control** file in the calculation directory, and if no file is present, the default **control** file is written ('*b97-3c*').


.. note::
   
   Currently it is possible to use the ORCA/TURBOMOLE packages only for the inner region calculation as the **high** level method.

Inner Region
------------

To perform the multicsale ONIOM calculations with ``xtb`` one has to specify the ``inner_region_cutoff`` which is provided either explicitly - "1,3-5,9,10", or via a file with the same content.

If the covalent bonds are cut between inner region and the rest of the system, the ONIOM handles the resulting boundary by means of Hydrogen Linked Atoms:

.. tabbed:: inner region
   
   .. figure:: ../figures/ohne.png


.. tabbed:: inner region with LAs
   
   .. figure:: ../figures/mit.png


To distinguish between different bonds the topology information of the underlying method is used. 

.. warning:: 

   When using the GFN-FF as a low level method, one has to be very careful with the inner region specification. The topology of the GFN-FF does not allow to accurately differentiate between single and higher order bonds.


Functionality
=============

Flags
-----


--chrg int:int
   an extension of the classical ``--chrg`` flag for the ONIOM as **inner_region_charge:whole_system_charge**. If not specified, the ``xtb`` will automatically determine the **inner_region_charge**.

--cut
   To check initial inner region written in ``inner_region_without_h.xyz`` without performing any calculations. Note that hydrogen linked atoms are not present, because one needs wiberg bond orders to correctly adjust them. However, if this flag is abscent, you will get saturated system written in ``inner_region.xyz`` after single point calcultion for the whole region. In addition, this procedure can be used to test the abovementioned automatic inner region charge determination.

--ceasefiles
   additionally to its already existing functions, this flag would instruct ``xtb`` to delete all external files both for ORCA and TURBOMOLE(exception ``*.inp`` and ``control``) 
   


xcontrol
--------

In addition to the above-mentioned external settings, ``xcontrol`` allows the more deeper control over the ONIOM routine:

.. code:: text

   $oniom
   $end

*inner logs=bool*
   Normally ``xtb`` prints optimization log file only for the whole system, but with this keyword ``xtb`` additionally writes high- and low-level logs for the model system as well(``high.inner_region.log`` and ``low.inner_region.log``). 

*derived k=bool*
   Prefactor *k* is used in the ONIOM scheme to correct the output gradients:

.. math::
   g_{ONIOM} = g_{whole,low} - g_{model,low}*J + g_{model,high}*J

where J is the Jacobian matrix, which by the construction depends on the *k* prefactor, that is usually constant and uses standard distances between atoms. However, the *derived k* keyword allows it to vary due to the calculating some distances explicitely.

*silent=bool*
   to redirect output of the ORCA or TURBOMOLE 



Examples
========

S30L-23
-------

The first example is non-covalently bounded complex number 23 from `S30L benchmark <https://pubs.acs.org/doi/full/10.1021/acs.jctc.5b00296>`_. 

.. collapse:: xyz file

   .. code-block:: none

      98
      
      C     0.7800079    6.8678780    5.5368969 
      C    -0.7524620   -6.8540954    5.5601280 
      C    -1.8617928   -4.6117174    5.6477616 
      C     0.6504601   -4.7941213    5.7959390 
      C     1.8907976    4.6261439    5.6192277 
      C    -0.6200120    4.8079116    5.7907513 
      C    -0.6206618   -5.3815968    5.1397097 
      C     0.6446437    5.3943761    5.1211667 
      C    -0.0083487   -0.0004324   -0.3233281 
      C     0.0816478    2.3154959   -0.3654186 
      C    -0.1026742   -2.3162587   -0.3590787 
      C    -0.2959652   -3.7207698    1.6988510 
      C     0.2900184    3.7253335    1.6873006 
      C    -0.1686909   -2.4104886    1.0817263 
      C     0.1605795    2.4132860    1.0744672 
      C     0.3073296    4.8471713    0.8195874 
      C    -0.3253307   -4.8444637    0.8338615 
      C     0.5276492    5.2249147    3.6051512 
      C    -0.5183472   -5.2156660    3.6222398 
      C    -0.0021327    0.0013275    1.1173800 
      C    -0.3936683   -3.9343044    3.0790781 
      C     0.4013815    3.9420198    3.0659794 
      C     0.1802918    4.6204737   -0.5828574 
      C    -0.2109397   -4.6213534   -0.5702540 
      C    -0.0947791   -1.2261541    1.7924088 
      C     0.0957022    1.2304873    1.7885605 
      C     0.5422958    6.3241699    2.7203124 
      C    -0.5452450   -6.3167433    2.7399789 
      C     0.4338755    6.1417501    1.3535797 
      C    -0.4505106   -6.1375070    1.3718405 
      N     0.0116094    1.1544899   -1.0315142 
      N    -0.0353108   -1.1570386   -1.0285348 
      N     0.0706724    3.4479104   -1.1540574 
      N    -0.1024078   -3.4505370   -1.1451747 
      H    -0.3766699   -3.0846938    3.7536939 
      H     0.3946574    3.0937118    3.7424387 
      H     0.6402108    7.3293416    3.1126263 
      H    -0.6416612   -7.3208096    3.1354374 
      H    -0.1065165   -1.2240152    2.8785113 
      H     0.1173973    1.2309738    2.8745222 
      H     0.4453140    6.9962771    0.6820918 
      H    -0.4716396   -6.9933848    0.7023244 
      H    -0.2080001   -5.4867763   -1.2347620 
      H     0.1678296    5.4843584   -1.2492380 
      H    -0.8214158   -6.9116715    6.6504902 
      H     0.8607175    6.9279102    6.6263141 
      H    -0.7372921    3.7424986    5.5676570 
      H     1.8239111    3.5571595    5.3925453 
      H    -1.7966759   -3.5433842    5.4175217 
      H     0.7662278   -3.7292650    5.5694116 
      H    -1.6551297   -7.3154982    5.1456164 
      H     0.1168724   -7.4441425    5.2509428 
      H     1.6777844    7.3288991    5.1115045 
      H    -0.0930120    7.4567246    5.2359300 
      H     2.8023074    5.0177240    5.1566686 
      H    -1.5202507    5.3298528    5.4510796 
      H    -2.7778232   -5.0041987    5.1950003 
      H     1.5472087   -5.3172918    5.4490088 
      H    -1.9411358   -4.7181202    6.7344847 
      H     0.5868113   -4.9026714    6.8834941 
      H    -0.5461631    4.9189786    6.8774015 
      H     1.9805730    4.7354338    6.7048538 
      C    -0.4488171    2.3665805   -4.6544687 
      C     0.4333778   -2.3789163   -4.6469142 
      C    -0.7917246    3.9457007   -6.0685007 
      C     0.7866666   -3.9602320   -6.0559302 
      C    -0.7915911    4.5371499   -4.7810591 
      C     0.7743565   -4.5501742   -4.7678612 
      N    -0.5769336    3.4887852   -3.8927405 
      N     0.5526308   -3.5004778   -3.8828746 
      N    -0.2162293    1.1365996   -4.0461490 
      N     0.1977398   -1.1476848   -4.0424055 
      C    -0.0069054   -0.0062888   -4.7519033 
      C     1.1701962   -6.6894301   -5.7142314 
      C    -1.1806924    6.6749532   -5.7335372 
      C    -1.1835769    6.0993249   -7.0168863 
      C     1.1847144   -6.1154023   -6.9982147 
      C     0.9956430   -4.7491407   -7.1892085 
      C    -0.9916821    4.7329808   -7.2045356 
      C    -0.9861523    5.9032143   -4.5891788 
      C     0.9658492   -5.9162071   -4.5726041 
      N    -0.0024966   -0.0075912   -6.0685751 
      N    -0.5736206    2.5723997   -5.9542885 
      N     0.5690957   -2.5866178   -5.9453246 
      H    -0.1946193    0.9005726   -6.5259354 
      H     0.1937665   -0.9163433   -6.5228612 
      H     0.1239780   -1.1213863   -3.0079656 
      H    -0.1481455    1.1124645   -3.0112686 
      H    -0.3819862    3.5198035   -2.8723700 
      H     0.3536032   -3.5294854   -2.8634564 
      H    -1.3407700    6.7384801   -7.8800928 
      H     1.3486405   -6.7558037   -7.8592332 
      H     1.3243264   -7.7589379   -5.6100014 
      H    -1.3357299    7.7446004   -5.6320230 
      H     1.0094379   -4.3072917   -8.1801901 
      H    -0.9964687    4.2898655   -8.1950479 
      H     0.9656238   -6.3638842   -3.5831368 
      H    -0.9956242    6.3520069   -3.6002631 
     
|

The fragments of this host-guest system are: 1-62 and 63-98, the latter having +1 charge.
To test the automatic charge identification routine:

.. tabbed:: cml input

   .. code-block:: none
      
      > xtb input.xyz --oniom orca:gfn2 1-62 --chrg +1 --cut

.. tabbed:: output

   .. code-block:: none

                 -------------------------------------------------
                |                Calculation Setup                |
                 -------------------------------------------------

                program call               : xtb input.xyz --oniom orca:gfn2 1-62 --chrg +1 --cut
                hostname                   : albert
                coordinate file            : input.xyz
                omp threads                :                    16

         ID    Z sym.   atoms
          1    6 C      1-30, 63-68, 73-81
          2    7 N      31-34, 69-72, 82-84
          3    1 H      35-62, 85-98

        ... skip ...
        ------------------------------------------------------------------------
        |                        INNER REGION CHARGE =  0                      |
        ------------------------------------------------------------------------

      normal termination of xtb

To start single point calculation with the user-defined orca input file: 

1) specify orca input and add its name in the xcontrol file:


.. tabbed:: orca.inp

   .. code-block:: none
      :emphasize-lines: 2

         ! r2SCAN-3c
         ! engrad
         * xyzfile 0 1 some.xyz
      
.. tabbed:: xcontrol
   
   .. code-block:: none

      $external
         orca input file=orca.inp 
      $end

Please use ``engrad`` keyword for the ORCA to allow xtb to read the output. The inner region is automatically written in  ``some.xyz`` file.

2) start ``xtb`` run:

.. code-block:: none
      
   > xtb input.xyz --oniom orca:gfn2 1-62 --chrg +1 --input xcontrol

The final ``xtb`` output for the given example will be divided in 3 parts with the ONIOM results will be printed in the property printout section:

.. code-block:: none
   :emphasize-lines: 30-31
   
      ------------------------------------------------------------------------

           Singlepoint calculation of whole system with low-level method

      ------------------------------------------------------------------------
      
      ... skip ...

      ------------------------------------------------------------------------

           Singlepoint calculation of inner region with low-level method

      ------------------------------------------------------------------------
      
      ... skip ...

      ------------------------------------------------------------------------

           Singlepoint calculation of inner region with high-level method

      ------------------------------------------------------------------------  

      ... skip ...
      
                -------------------------------------------------
               |                Property Printout                |
                -------------------------------------------------

                 -------------------------------------------------
                | TOTAL ENERGY            -1438.298999659396 Eh   |
                | GRADIENT NORM               0.062957205099 Eh/α |
                 -------------------------------------------------



Metall-complex
--------------

   The second example, Zr-functionalized complex, is taken from the `TMG-145 benchmark <https://doi.org/10.1002/ange.201904021>`_:

.. collapse:: xyz file

   .. code-block:: none
         
         77
         
         Zn    3.6937360    5.1063180   18.7964011 
         Cl    5.5077870    6.3517350   19.2580401 
         P     6.0686750    2.6641570   18.2610791 
         C     3.7280450    2.8356680   16.5523611 
         C     3.8544850    3.9118910   15.6318391 
         C     4.4179300    5.1755470   15.9849611 
         H     4.7663290    5.2863940   16.8616251 
         C     4.4786650    6.2225480   15.1316041 
         H     4.8394060    7.0498690   15.4280861 
         C     4.0134910    6.1006160   13.8078361 
         H     4.0530550    6.8422830   13.2151231 
         C     3.5112450    4.9216070   13.3890521 
         H     3.2188670    4.8359520   12.4883291 
         C     3.4079040    3.7960060   14.2684731 
         C     2.8775700    2.5928120   13.8311441 
         H     2.5682090    2.5202580   12.9369361 
         C     2.7876110    1.4893810   14.6719691 
         C     2.2823510    0.2468870   14.1962951 
         H     1.9786640    0.1783630   13.2978271 
         C     2.2298460   -0.8343760   15.0002801 
         H     1.8867120   -1.6546430   14.6662051 
         C     2.6835910   -0.7537600   16.3383331 
         H     2.6578100   -1.5236420   16.8942051 
         C     3.1561030    0.4222260   16.8315501 
         H     3.4456820    0.4585040   17.7362841 
         C     3.2304100    1.6032510   16.0365881 
         C     6.4388270    0.9603380   17.7816461 
         H     6.0136030    0.3436260   18.4129541 
         H     7.4082700    0.8212760   17.7914201 
         H     6.0945690    0.7920520   16.8791681 
         C     7.0990190    3.6579510   17.1608631 
         H     7.0819010    4.5930970   17.4530851 
         H     6.7540270    3.5954740   16.2451031 
         H     8.0206690    3.3244020   17.1874291 
         C     6.8389680    2.8870610   19.8698011 
         H     6.6673000    3.7970140   20.1923471 
         H     7.8062500    2.7449750   19.7913571 
         H     6.4640740    2.2401170   20.5031141 
         B     4.1430940    2.9444990   18.1244921 
         P     1.3187980    2.6641570   19.3317221 
         C     3.6594270    2.8356680   21.0404411 
         C     3.5329880    3.9118910   21.9609631 
         C     2.9695430    5.1755470   21.6078411 
         H     2.6211430    5.2863940   20.7311771 
         C     2.9088070    6.2225480   22.4611981 
         H     2.5480660    7.0498690   22.1647161 
         C     3.3739810    6.1006160   23.7849661 
         H     3.3344170    6.8422830   24.3776791 
         C     3.8762270    4.9216070   24.2037491 
         H     4.1686060    4.8359520   25.1044731 
         C     3.9795690    3.7960060   23.3243291 
         C     4.5099030    2.5928120   23.7616581 
         H     4.8192640    2.5202580   24.6558661 
         C     4.5998620    1.4893810   22.9208321 
         C     5.1051210    0.2468870   23.3965071 
         H     5.4088090    0.1783630   24.2949751 
         C     5.1576270   -0.8343760   22.5925221 
         H     5.5007600   -1.6546430   22.9265971 
         C     4.7038810   -0.7537600   21.2544691 
         H     4.7296620   -1.5236420   20.6985971 
         C     4.2313700    0.4222260   20.7612511 
         H     3.9417900    0.4585040   19.8565181 
         C     4.1570620    1.6032510   21.5562141 
         C     0.9486460    0.9603380   19.8111561 
         H     1.3738690    0.3436260   19.1798471 
         H    -0.0207980    0.8212760   19.8013821 
         H     1.2929040    0.7920520   20.7136341 
         C     0.2884530    3.6579510   20.4319381 
         H     0.3055720    4.5930970   20.1397171 
         H     0.6334460    3.5954740   21.3476991 
         H    -0.6331960    3.3244020   20.4053731 
         C     0.5485040    2.8870610   17.7230011 
         H     0.7201720    3.7970140   17.4004551 
         H    -0.4187770    2.7449750   17.8014451 
         H     0.9233990    2.2401170   17.0896881 
         B     3.2443780    2.9444990   19.4683101 
         Cl    1.8796850    6.3517350   18.3347611 

In comparison to the ORCA, the ONIOM searches for the ``control`` file automatically. In this example the optimization of this complex with the small inner region written to the ``list`` file is shown:

.. tabbed:: cml input
   
   > xtb zn.xyz --oniom turbomole:gfn2 list --opt 

.. tabbed:: list
   
   1,2,39,76,77
