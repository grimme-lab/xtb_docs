-----------------------
 Compiling from Source
-----------------------

This guide is intended to help developers to get our programs running.

.. note:: To install our programs refer to :ref:`setup`.
          If you come here from :ref:`setup`, since our instructions have not
          worked for you, why not drop us a hint at the issue tracker?

The ``xtb`` source code is hosted at `GitHub <https://github.com/grimme-lab/xtb>`_.
The ``xtb`` project officially supports both the
`Meson <https://mesonbuild.com>`_ and `CMake <https://cmake.org>`_ build
systems; additional instructions on compiling with Meson and CMake can also be
found on the `xtb GitHub repository <https://github.com/grimme-lab/xtb>`_
(including the `Meson guide <https://github.com/grimme-lab/xtb/blob/main/meson/README.adoc>`_
and `CMake guide <https://github.com/grimme-lab/xtb/blob/main/cmake/README.adoc>`_).

.. contents::

Building with meson
===================

To build ``xtb`` with Meson, version 0.62.0 or newer is required, along with
`ninja <https://ninja-build.org/>`_ (1.7 or newer). Note that Meson versions
1.8.0 and 1.8.1 have known issues and are explicitly unsupported.

Getting meson
-------------

To install the meson build system first check your package manager for an up-to-date meson version,
usually this will also install ninja as dependency.
Alternatively you can install the latest version of meson and ninja with ``pip`` (or ``pip3`` depending on your system):

.. code-block:: none

   pip install meson ninja [--user]

If you prefer ``conda`` as a package manage you can install meson and ninja from the conda-forge channel.
Make sure to select the conda-forge channel for searching packages.

.. code-block:: none

   conda config --add channels conda-forge
   conda install meson ninja

Configure Intel Fortran build with MKL
--------------------------------------

``xtb`` can be built with the Intel oneAPI compilers (``ifx``/``icx``, or the
classic ``ifort`` compiler) and Intel's Math Kernel Library (MKL).
With the default ``-Dlapack=auto`` setting, Meson automatically selects MKL
when an Intel Fortran compiler is used:

.. code-block::

   export FC=ifx CC=icx
   meson setup build --buildtype=release

After the configuration step the build can be performed with ninja:

.. code-block::

   ninja -C build

Note, ninja will by default use all the threads available on your system.

.. note::

   If you share the build machine with others it might be helpful to reduce the number of concurrent jobs using the ``-j`` flag.

The linear algebra backend can be selected explicitly via ``-Dlapack=...``.
The supported ``lapack`` options in Meson are:

============ ============================================================
 backend      linked against
============ ============================================================
 auto         MKL for Intel compilers, standard BLAS/LAPACK otherwise (default)
 mkl          Intel MKL
 mkl-rt       Intel MKL single dynamic library (``mkl_rt``)
 openblas     OpenBLAS and if required LAPACK
 netlib       reference BLAS and LAPACK
 custom       ``-Dcustom_libraries=...``
============ ============================================================

.. note::

   If you are using the MKL provided by conda-forge you have to link against the netlib backend


Configure GCC build with OpenBLAS
---------------------------------

``xtb`` can also be compiled with GCC (``gfortran`` and ``gcc``).
For this example we choose to set the linear algebra backend to OpenBLAS:

.. code-block::

   export FC=gfortran CC=gcc
   meson setup build --buildtype=release -Dlapack=openblas

The build system will check if the OpenBLAS library provides LAPACK features as well, if this is not the case it will additionally search for LAPACK.
If you are compiling ``xtb`` on Darwin platforms, ensure that GCC is the actual GCC and not clang.
The build can be performed just like before:

.. code-block::

   ninja -C build


Testing the build with meson
----------------------------

After successfully building the `xtb` program ensure that it is working as expected.
Run the testsuite with

.. code-block::

   ninja -C build test

All tests should pass, otherwise `open an issue <https://github.com/grimme-lab/xtb/issues/new/choose>`_.


Installing with meson
---------------------

To use ``xtb`` in production or to pack a release with precompiled binaries the project should be installed with ninja.
The installation prefix defaults to ``/usr/local`` on Linux systems, you might want to adjust this first by configuring your build with

.. code-block:: none

   meson configure build --prefix=$HOME/.local

To perform the actual installation run

.. code-block:: none

   ninja -C build install

Depending on the installation prefix and your user rights ninja might ask for the ``root`` access to perform the installation.


Building with CMake
===================

Alternatively, ``xtb`` can be built with CMake (version 3.18 or newer):

.. code-block:: none

   export FC=gfortran CC=gcc
   cmake -B build -G Ninja -DCMAKE_BUILD_TYPE=Release
   cmake --build build

To run the unit test suite after building with CMake:

.. code-block:: none

   ctest --test-dir build --output-on-failure


Building with GPU support
-------------------------

This projects can run on accelerator devices from NVIDIA.
The compilation of the GPU version requires the NVIDIA HPC SDK, dupped NVHPC for brevity.
The NVHPC compilers are available for free `here <https://developer.nvidia.com/nvidia-hpc-sdk-downloads>`_.

.. note::

   It is highly recommended to carefully compare the performance of the CPU version
   with the GPU version before starting production runs.
   Certain problem sizes can profit more from different accelerator devices than others.

   To throw in some numbers as guidance for a single point calculation of a
   3000 atom system with GFN2-xTB(ALPB) using `xtb` version 6.4.0:

   ============= ===================================== ==========
    Compiler      Hardware                              Walltime
   ============= ===================================== ==========
    Intel 18      4 cores @ Intel Xeon CPU E3-1270 v5     13 min
    Intel 18      8 cores @ Intel Xeon Gold 6148 CPU       7 min
    NVHPC 20.7    Tesla K80 (cc35)                         7 min
    NVHPC 20.7    Tesla V100 (cc70)                        2 min
   ============= ===================================== ==========


The NVHPC provides TCL environment modules which are the preferred way to setup
the compilers, if your module environment is already configured, you can just go
ahead and

.. code-block:: none

   module load nvhpc

.. note::

   The TCL environment modules are usually installed in the highest level of
   your chosen install prefix, *i.e.* ``/opt/nvhpc/modulefiles`` if you
   installed into ``/opt/nvhpc``.

   If you do not have a module environment available on your (local) system
   you can install the TCL environment modules under Ubuntu with the
   ``environment-module`` package or the newer Lua environment modules with
   the ``lmod`` package.

With the NVHPC compilers available, configure a build with

.. code-block:: none

   export FC=nvfortran CC=nvc
   meson setup build_gpu --prefix=$HOME/.local -Dlapack=netlib -Dgpu=true -Dcusolver=true

You can select the correct compute capability of your device with ``-Dgpu_arch=70``.

Compile and install the project with

.. code-block:: none

   ninja -C build_gpu install

If you used the provided TCL environment modules of the NVHPC, you can use `xtb`
in a similar way by including the automatically generated TCL environment module
in the install prefix with:

.. code-block:: none

   echo "prereq nvhpc" >> ~/.local/share/modules/modulefiles/xtb/*
   module use ~/.local/share/modules/modulefiles
   module load xtb

Now you have a working version of `xtb` which can make use of your GPU.

To check if your GPU is utilized correctly you can either track the GPU usage with
``nvidia-smi`` command line tool or set ``PGI_ACC_NOTIFY=3`` when running `xtb`
as environment variable to get information on which kernels are launched on which device.

If you have multiple accelerator devices attached to your system you can select them
at runtime with ``CUDA_VISIBLE_DEVICES=<int>``.


Supported Compilers
===================

``xtb`` is developed and tested with GCC (``gfortran``/``gcc``) and Intel
oneAPI (``ifx``/``icx`` as well as classic ``ifort``). Current compiler and
platform coverage is defined and continuously verified by the
`GitHub Actions CI workflows <https://github.com/grimme-lab/xtb/blob/main/.github/workflows/fortran-build.yml>`_
in the ``xtb`` repository.

Below is a non-comprehensive historical list of tested compilers for ``xtb``
6.4 with the meson build system:

========= ================ ===================== ================== ======================
Compiler   Version          Platform              Architecture       ``xtb``
========= ================ ===================== ================== ======================
 GCC       10.2             Ubuntu 20.04          x86_64                    6.4.1, latest
 GCC       10.2, 11.1       Manjaro Linux         x86_64             6.4.0, 6.4.1
 GCC       10.2             Windows Server 2019   x86_64             6.4.0, 6.4.1, latest
 GCC        9.3             Ubuntu 18.04          x86_64             6.4.0
 GCC        9.3             Ubuntu 20.04          x86_64                    6.4.1, latest
 GCC        9.3             Centos 7              ppc64le, aarch64   6.4.0, 6.4.1
 GCC        9.3             Centos 6              x86_64             6.4.0, 6.4.1
 GCC        9.3             MacOS 10.15.7         x86_64             6.4.0, 6.4.1, latest
 GCC        8.4             Ubuntu 20.04          x86_64                    6.4.1, latest
 GCC        7.5             Ubuntu 18.04          x86_64             6.4.0
 Intel     2021.1, 2021.2   Ubuntu 20.04          x86_64             6.4.0, 6.4.1, latest
 Intel      18.0.2          OpenSuse 42.1         x86_64             6.4.0, 6.4.1
 NVHPC      20.11, 21.1     Manjaro Linux         x86_64             6.4.0
 NVHPC      20.9            Centos 8              x86_64 + cc70      6.4.0
 NVHPC      20.7            OpenSuse 42.1         x86_64 + cc35      6.4.0
========= ================ ===================== ================== ======================

For GPU enabled builds the compute-capability is given together with the architecture.

.. note::

   First class compiler support in ``xtb`` comes only with continuous testing, if you
   want to see a particular compiler, platform or architecture supported,
   please reach out to us at the `discussion board <https://github.com/grimme-lab/xtb/discussions>`_,
   `open an issue <https://github.com/grimme-lab/xtb/issues/new/choose>`_ or submit
   a continuous integration workflow with a pull request to ``xtb``.
