.. _run_qcxms:

--------------
Running QCxMS
--------------

.. contents::

The program basically runs in two modes: ``EI`` and ``CID``. Both methods require three individual steps to create the final spectrum.
To obtain a QCxMS simulated mass spectrum, please follow this protocol:

Workflow of QCxMS
=================

1. Setup
--------

1. Prepare a file with the equilibrium structure of your desired molecule M. For the CID mode,
   the molecule has to be protonated. This can be done with the protonation tool of CREST 
   (see: :ref:`crestcmd`). 
   **Important**: The structure file has to be named ``coord`` and should have the *TURBOMOLE*
   coord format.    In most cases you will have an .xyz file. This file can be easily converted by typing
   ``x2t <xyzfile> > coord`` if you have installed *TURBOMOLE*. If you do not have *TURBOMOLE*, you may have to write a
   script converting .xyz files to *TUROBOMOLE* coord files. Be advised that the coord file has
   to be in atomic units.
2. Prepare an input file called `qceims.in`_ . For the input options, see section 4 or the
   qceims.in file in the examples folder. If no such file is prepared, default options are:
   run GFN1-xTB with 25 × the number of atoms in the molecule trajectories (ntraj).


2. Preparing for Production Runs
--------------------------------

3. Run ``QCxMS`` for the first time. This will generate the first (ground state, GS) trajectory from
   which information is taken for the production trajectories. **Be aware** that this already uses
   the QC method specified. After some steps for equilibration, the files *trjM* and *qceims.gs*
   are generated. It is highly recommended to conduct the initial run with a low-cost method,
   e.g. GFN1-xTB (default) or OM2-D3.
4. Run ``QCxMS`` for the second time after the GS run is finished. This genereates the *TMPQCEIMS* file.  
   If the file qceims.gs exists, this will prepare the specifications for the parallel production run in 
   the *TMPQCEIMS* folder.

3. Excecuting the Production Runs
---------------------------------

5. If you use a computer cluster with a queing system, type ``q-batch`` for the execution of
   the batch script. This will initiate the production run with ntraj trajectories. For each
   trajectory this starts a single core calculation. 
   **Reminder**: Look into the *q-batch* script, as it might need to be changed to match for your queing system.
   If you want to run QCxMS locally, use the ``pqceims`` script. You might want to leave
   your work station and even log out in the meantime. In this case, type, where ``<proc>`` referres to the 
   number of processors your workstation is allowed to use:

.. code:: sh

   nohup pqceims <proc> &


6. Optionally, check the status of your QCxMS run by changing to your working directory and typing ``getres``,
   which will provide an output of the form:

.. code:: 

   XXX runs done and written to tmpqceims.res/out

7. The steps that are conducted in the production runs are displayed in the following figure. For details look into
   the *qceims.out* file provided after the calculations are finished. For details on individual runs, look into the *TMPQCEIMS/TMP.X* 
   folder. 


.. figure:: ../../figures/qceims/QCEIMS.png
  :align: center

  Steps during the production runs in the EI and CID modules


Input keywords in *qceims.in*
=============================

.. _qceims.in:

If no input file is given, default settings are taken. This means an **EI** calculation is conducted.
The general *qceims.in* input file can be manipulated by providing *<parameters>* : 

+-------------------+-----------------------------+-------------------+----------------------------+
| **<Parameter>**   | **Specification**           |  **Default**      | **Alt. settings**          |
+-------------------+-----------------------------+-------------------+----------------------------+
| *<method>*        | Mass Spec. Method           | ei                | cid, dea                   | 
+-------------------+-----------------------------+-------------------+----------------------------+
| *<program>*       | QC Program                  | *xtb*             | tmol, orca, mndo, dftb     |
+-------------------+-----------------------------+-------------------+----------------------------+
| *<method>*        | QC Method                   | xtb               | see: Input Details         |
+-------------------+-----------------------------+-------------------+----------------------------+
| *<qc settings>*   | Basisset and/or Functional  | pbe0 sv(p)        | see: Input Details         |  
+-------------------+-----------------------------+-------------------+----------------------------+
| *<ip method>*     | Ionization Potential Method | ip-xtb, ip-xtb2   | ip-tmol, ip-orca, ip-mndo  |
+-------------------+-----------------------------+-------------------+----------------------------+
| ntraj *<integer>* | Number of trajectories      | 25 × no. of atoms | *<integer>*                |
+-------------------+-----------------------------+-------------------+----------------------------+
| tmax *<real>*     | Maximum MD time             | 5 ps              | *<integer>*                |
+-------------------+-----------------------------+-------------------+----------------------------+
| tinit *<real>*    | Initial Temperature         | 500 K             | *<real>*                   |
+-------------------+-----------------------------+-------------------+----------------------------+

While xTB is set as default programm and method, it is not required to define it twice.

EI method specific keywords
---------------------------

+--------------------------------------+-------------------------------------+-------------------+--------------------+
| **<Parameter>**                      | **Specification**                   |  **Default**      | **Alt. settings**  |
+--------------------------------------+-------------------------------------+-------------------+--------------------+
| *<mo method>*                        | Molecular Orbital                   | mo-xtb            | mo-orca            |
+--------------------------------------+-------------------------------------+-------------------+--------------------+
| eimp0 *<real>*                       | Electron-beam impact energy         | 70 eV             | *<real>*           |
+--------------------------------------+-------------------------------------+-------------------+--------------------+
| ieeatm *<real>*                      | Impact excess energy (IEE) per atom | 0.6 eV/atom       | *<real>*           |
+--------------------------------------+-------------------------------------+-------------------+--------------------+
| poisson *or* gauss (*<real> <real>*) | IEE distribution type               | poisson           | *<real>*           |
+--------------------------------------+-------------------------------------+-------------------+--------------------+

.. note:: **Poisson/Gauss IEE distribution:**
  Generated from the MO spectrum of the molecule. For low ionization energies and for 
  large molecules, the Poisson distribution may sometimes not converge. Switch to the 
  Gauss distribution by specifying the gauss keyword. Two parameters may be entered, 
  which manipulate the shape of the distribution. 
  **Caution!** Manipulating the IEE distribution can lead to unphysical spectra with 
  either over- or under-fragmentation of the precursor ion.


CID method specific keywords
----------------------------

+-----------------------------+-------------------------------+----------------------+--------------------+
| **<Parameter>**             | **Specification**             |  **Default**         | **Alt. settings**  |
+-----------------------------+-------------------------------+----------------------+--------------------+
| *<run-type>*                | Run-type specifics            | fullauto             | collauto, temprun  |
+-----------------------------+-------------------------------+----------------------+--------------------+
| eimpact *<real>*            | Collision Energy E(coll)      | 25 eV                | *<real>*           |
+-----------------------------+-------------------------------+----------------------+--------------------+
| eexact                      | do not distribute E(coll)     | *off*                | *none*             |
+-----------------------------+-------------------------------+----------------------+--------------------+
| iatom *<string>*            | Neutral gas atom              | ar                   | he, ne, kr, xe, n2 |
+-----------------------------+-------------------------------+----------------------+--------------------+
| esi *or* tscale *<real>*    | E(int) or Temp. scaling       | mol. size (auto)     | *<real>*           |
+-----------------------------+-------------------------------+----------------------+--------------------+
| noesi                       | switch off E(int) scaling     | *off*                | *none*             |
+-----------------------------+-------------------------------+----------------------+--------------------+
| pgas *<real>*               | gas pressure (Pa)             | 0.132 (=1mTorr)      |  *<real>*          |
+-----------------------------+-------------------------------+----------------------+--------------------+
| lchamb *<real>*             | coll. cell length (m)         | 0.25 (=25cm)         | *<real>*           |
+-----------------------------+-------------------------------+----------------------+--------------------+
| simmd  *<integer>*          | MD steps for mean-free-path   | 10000 (=5ps)         | *<integer>*        |
+-----------------------------+-------------------------------+----------------------+--------------------+
| setcoll *<integer>*         | number of *pgc* **and** *fgc* | 10 (*collauto*)      | *<integer>*        |
+-----------------------------+-------------------------------+----------------------+--------------------+
| maxcoll *<integer>*         | number of *pgc*, **no** *fgc* | 10 (*collauto*)      | *<integer>*        |
+-----------------------------+-------------------------------+----------------------+--------------------+
| collsec *<int> <int> <int>* | number of fragmentations      | 0 0 0                | *<integer>*        |
+-----------------------------+-------------------------------+----------------------+--------------------+

The run-types can be manipulated by different settings. For Fullauto (general activation mode), ``lChamb`` and ``PGas`` are important for the number of
precursor-gas collisions (*pgc*) and fragment-gas collisions (*fgc*), in the Collauto run-type (forced activation mode), the number of colllisions
can be set to a total number of collisions (pgc + fgc -> setcoll) or only precursor-gas collisions (pgc -> maxcoll).

Misc keywords
-------------

+--------------------------------------------------------------------+-------------------------------------------------------------------+
| isotope <atomnumber> <mass_isotope> <atomnumber> <mass_isotope> ...| Switches *<atom> <mass>* to simulate isotopes. (integer masses)   |
+--------------------------------------------------------------------+-------------------------------------------------------------------+
| iseed *<integer>*                                                  | Random number seed (Default: 42)                                  | 
+--------------------------------------------------------------------+-------------------------------------------------------------------+
| etemp *<real>*                                                     | Electronic temperature of convergenc of MD (Default: Auto)        | 
+--------------------------------------------------------------------+-------------------------------------------------------------------+
| nfragexit *<integer>*                                              | Stop at *<integer>* simultaneously created fragments (Default: 3) | 
+--------------------------------------------------------------------+-------------------------------------------------------------------+
| ecp / no-ecp                                                       | Use ECPs / Do not use ECPs (ORCA /TMOL only!)                     |
+--------------------------------------------------------------------+-------------------------------------------------------------------+


Input Details
=============

QC Programs:
------------
 
+-------------+-------------+-------------------------------------------------------------------+
| **Keyword** | **Program** | **Specifics**                                                     |
+-------------+-------------+-------------------------------------------------------------------+
| xtb         | xTB 5.8.1   | built-in GFN1-xTB Hamiltonian                                     |
+-------------+-------------+-------------------------------------------------------------------+
| xtb2        | xTB 5.8.1   |  built-in GFN2-xTB Hamiltonian                                    |
+-------------+-------------+-------------------------------------------------------------------+
| tmol        | TURBOMOLE   | The ridft and rdgrad programs are called.distribution type        |
+-------------+-------------+-------------------------------------------------------------------+
| orca        | ORCA        | large QC program package free for academic use. distribution type |
+-------------+-------------+-------------------------------------------------------------------+
| mndo        | MNDO99      |  semiempirical QC program available from Walter Thiel             |
+-------------+-------------+-------------------------------------------------------------------+
| dftb        | DFTB+       | semiempirical tight-binding QC program free for academic use      |
+-------------+-------------+-------------------------------------------------------------------+


SQM Programs:
-------------

The GFN1- and GFN2-xTB methods are available without any third-party software. All other semi-empirical quantum mechanical (SQM) methods have to be explicitly called with their corresponding program:

+-------------+----------------+-------------+----------------------------+
| **Keyword** | **SQM Method** | **Program** | **Specifics**              |
+-------------+----------------+-------------+----------------------------+
| xtb         | GFN1-xTB       | QCxMs       | D3-dispersion              |
+-------------+----------------+-------------+----------------------------+
| xtb2        | GFN2-xTB       | QCxMS       | **D4**-dispersion          |
+-------------+----------------+-------------+----------------------------+
| om2         | OM2-D3         | MNDO99      | D3-dispersion              |
+-------------+----------------+-------------+----------------------------+
| om3         | OM3-D3         | MNDO99      | D3-dispersion              |
+-------------+----------------+-------------+----------------------------+
| *am1*       | *AM1-D3*       | MOPAC       | D3-dispersion              |
+-------------+----------------+-------------+----------------------------+
| *pm3*       | *PM3-D3*       | MOPAC       | D3-dispersion              |
+-------------+----------------+-------------+----------------------------+
| *pm6*       | *PM6-DH2*      | MOPAC       | **D2**-dispersion + h-bond |
+-------------+----------------+-------------+----------------------------+
| dftb        | DFTB3-D3       | DFTB+       | D3-dispersion              |
+-------------+----------------+-------------+----------------------------+

To decide which method should be used, it is recommended to read the original publication first!
For using GFN1-xTB and GFN2-xTB with QCxMS, refer to the publications 4,5.

.. note::
   The usage of *AM1* or *PM3/PM6* are not recommended, due to their bad performances!

Available Functionals in ORCA/TURBOMOLE:
----------------------------------------

+-------------+-------------+------------------------+------------------+
| **Keyword** | **Method**  | **DFT type**           | **Availability** |
+-------------+-------------+------------------------+------------------+
| pbe         | PBE-D3BJ    | GGA                    | ORCA / TURBOMOLE |
+-------------+-------------+------------------------+------------------+
| pbe0        | PBE0-D3BJ   | global hybrid          | ORCA / TURBOMOLE |
+-------------+-------------+------------------------+------------------+
| pbeh3c      | PBEh3-c     | global hybrid          | ORCA             |
+-------------+-------------+------------------------+------------------+
| revpbe      | REVPBE-D3BJ | GGA                    | ORCA             |
+-------------+-------------+------------------------+------------------+
| blyp        | BLYP-D3BJ   | GGA                    | ORCA / TURBOMOLE |
+-------------+-------------+------------------------+------------------+
| b3lyp       | B3LYP-D3BJ  | global hybrid          | ORCA / TURBOMOLE |
+-------------+-------------+------------------------+------------------+
| tpss        | TPSS-D3BJ   | meta-GGA               | ORCA / TURBOMOLE |
+-------------+-------------+------------------------+------------------+
| b97d        | B97-D3BJ    | GGA                    | ORCA / TURBOMOLE |
+-------------+-------------+------------------------+------------------+
| bp86        | BP86-D3BJ   | GGA                    | ORCA / TURBOMOLE |
+-------------+-------------+------------------------+------------------+
| b3pw91      | B3PW91-D3BJ | global hybrid          | ORCA             |
+-------------+-------------+------------------------+------------------+
| m062x       | M062X       | meta-GGA global hybrid | ORCA / TURBOMOLE |
+-------------+-------------+------------------------+------------------+
| pw6b95      | PW6B95-D3BJ | meta-GGA global hybrid | ORCA / TURBOMOLE |
+-------------+-------------+------------------------+------------------+


Available Basissets in ORCA/TURBOMOLE:
--------------------------------------

+---------------+-------------------------------------+--------------------------------------+------------------+
| **Keyword**   | **Basisset type**                   | **Specification**                    | **Availability** |
+---------------+-------------------------------------+--------------------------------------+------------------+
| sv            | double :math:`\zeta`                | Split-valence (SV)                   | ORCA / TURBOMOLE |
+---------------+-------------------------------------+--------------------------------------+------------------+
| svx           | double :math:`\zeta` + pol.         | SV + pol. func. on O,N               | ORCA             |
+---------------+-------------------------------------+--------------------------------------+------------------+
| sv(p)         | double :math:`\zeta` + pol.         | SV + pol. func. on all except H      | ORCA / TURBOMOLE |
+---------------+-------------------------------------+--------------------------------------+------------------+
| svp           | double :math:`\zeta` + pol.         | SV + pol. func. on all               | ORCA / TURBOMOLE |
+---------------+-------------------------------------+--------------------------------------+------------------+
| tzvp          | triple :math:`\zeta` + pol.         | TZ + pol. func. on all               | ORCA / TURBOMOLE |
+---------------+-------------------------------------+--------------------------------------+------------------+
| qzvp          | quad. :math:`\zeta`  + pol.         | QZ + pol. func. on all               | ORCA / TURBOMOLE |
+---------------+-------------------------------------+--------------------------------------+------------------+
| def2-sv(p)    | double :math:`\zeta` + pol.         | SV + pol. func. on all except H      | ORCA / TURBOMOLE |
+---------------+-------------------------------------+--------------------------------------+------------------+
| def2-svp      | double :math:`\zeta` + pol.         | SV + pol. func. on all               | ORCA / TURBOMOLE |
+---------------+-------------------------------------+--------------------------------------+------------------+
| def2-svpd     | double :math:`\zeta` + pol. + diff. | SV + pol. and diff. func. on all     | TURBOMOLE        |
+---------------+-------------------------------------+--------------------------------------+------------------+
| def2-tzvp     | triple :math:`\zeta` + pol.         | TZ + pol. func. on all               | ORCA             |
+---------------+-------------------------------------+--------------------------------------+------------------+
| def2-tzvpd    | triple :math:`\zeta` + pol. + diff. | TZ + pol. and diff. func. on all     | TURBOMOLE        |
+---------------+-------------------------------------+--------------------------------------+------------------+
| def2-qzvp     | quad. :math:`\zeta`  + pol.         | QZ + pol. func. on all               | ORCA / TURBOMOLE |
+---------------+-------------------------------------+--------------------------------------+------------------+
| ma-def2-svp   | double :math:`\zeta` + pol.         | min. aug.  SV + pol. func. on all    | ORCA             |
+---------------+-------------------------------------+--------------------------------------+------------------+
| ma-def2-tzvp  | triple :math:`\zeta` + pol.         | min. aug.  TZ + pol. func. on all    | ORCA             |
+---------------+-------------------------------------+--------------------------------------+------------------+
| ma-def2-tzvpp | triple :math:`\zeta` + pol. + pol.  | min. aug.  TZ + 2x pol. func. on all | ORCA             | 
+---------------+-------------------------------------+--------------------------------------+------------------+
| ma-def2-qzvp  | quad. :math:`\zeta`  + pol.         | min. aug.  QZ + pol. func. on all    | ORCA             | 
+---------------+-------------------------------------+--------------------------------------+------------------+

Command line Options
====================

-**c**
    check IEE but do nothing (requires ground state trajectory). Writes IEE distribution in file *eimp.dat*.
-**p**
    production (fragmentation) mode. Possible in any existing *TMPQCEIMS/TMP.XXX* directory.
-**eonly**
    use the requested QC (as specified in qceims.in) and do a single-point energy.                        
-**e0**
    same as above, charge = 0                                                                             
-**e1**
    same as above, charge = 1                                                                             
-**qcp <string>**
    <string> = path to the QC code. /usr/local/bin is default 
-**unity**
    enforces uniform velocity scaling during the vibrational heating phase (in **EI** mode only) 

