Introduction
============

This user guide focuses on the semiempirical quantum mechanical (SQM) method
which is called extended tight binding (xTB) as it is implemented in the
`xtb <https://www.chemie.uni-bonn.de/pctc/mulliken-center/software/xtb/xtb>`_ program package.


Getting the Program
===================

The `xtb <https://www.chemie.uni-bonn.de/pctc/mulliken-center/software/xtb/xtb>`_ program is available for academic use free of charge on request
from Stefan Grimme at the `xtb-mailing list <xtb@thch.uni-bonn.de>`_.
It usually comes as a tarball with following content::
  bin/xtb
  .xtbrc
  Config_xtb_env.bash
  Config_xtb_env.csh
  man/xtb.1.html
  man/xtb.1.pdf
  man/man1/xtb.1
  man/xcontrol.7.html
  man/xcontrol.7.pdf
  man/man7/xcontrol.7
  info/RELEASE_NOTES.html
  info/RELEASE_NOTES.pdf

The binary is usually compiled with the Intel Fortran compiler and statically
linked against Intel's Math Kernel Library (Intel MKL).

First check the version by::
  > xtb --version

This should print some fancy banner, the version number, say 6.1 RC1, the
last programmer worked on the project (usually `SAW`, meaning Sebastian Ehlert)
and the date the program was last compiled and tested by this programmer,
as `YYMMDD`. If this timestamp and the timestamp on the tarball differ
by more than a week be alert.

Setting up `xtb`
================

This section will give you the basic information you need to
know about the `xtb` program. Some of the steps are elemental
for your calculation to succeed, so please consider to follow
my instructions carefully.

Some part of the `xtb` program can be quite wasteful with stack memory,
to avoid stack overflows when calculating large molecules, you should
unlimit the system stack, *e.g.* with `bash` by::
  > ulimit -s unlimited

Note that the memory management of `xtb` is constantly improved to avoid
using large amounts of stack memory, but to be on the save side
include this option for production runs.

Parallelisation
---------------

The `xtb` program uses OMP parallelisation, to calculate larger systems
an appropriate OMP stacksize must be provided, chose a reasonable large number by::
  > export OMP_STACKSIZE=1G

To distribute the number of threads reasonable in the OMP section
it is recommended to use::
  > export OMP_NUM_THREADS=<ncores>,1

You might want to deactivate nested OMP constructs by::
  > export OMP_MAX_ACTIVE_LEVELS=1

Environment Variables for `xtb`
-------------------------------

A number of environment variables is used by `xtb` to perform calculations.
Please set the `XTBPATH` variable to include all locations were
you store information relevant for your `xtb` calculation, like configuration
files and parameter files.
The present working directory is implicitly included for most files that
are searched in the `XTBPATH`.

The old `XTBHOME` variable is used if you have not set the `XTBPATH`
variable and is used in the same manner. `xtb` will print the values
of `XTBPATH` and `XTBHOME` at the beginning of each calculation
if set to verbose mode.

Getting Help inside `xTB`
=========================

Beside this manual you are almost on your own. You can check the
in-program help by::
  > xtb --help

Unfortunately, this might be outdated, since nobody feels responsible for it,
therefore, you should refer to the man-pages distributed with the `xtb` program.
Please check for the man-pages of `xtb(1)` and `xcontrol(7)`.


The Verbose Mode
----------------

If you think some information is missing in your calculation you can
switch to the verbose mode by using `--verbose` in the command line
arguments. This will increase the print level almost everywhere in the
`xtb` program, also the input parser will print a lot of information
that might be interesting for your current calculation.

Overall this can be an awful lot of information, so it is not recommended
as a default option.

Guide
-----
.. toctree::
   :maxdepth: 3

   basics
   xcontrol
   license
   help   
