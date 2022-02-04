SCINE - XTB Wrapper -- A Wrapper for Tight Binding
==================================================

Introduction
------------

SCINE XTB Wrapper is a wrapper around `xtb <https://github.com/grimme-lab/xtb>`_, it
exports:

- GFN0-xTB
- GFN1-xTB
- GFN2-xTB
- GFN-FF

into the SCINE tool chain.
Each method is represented by its own ``Calculator`` and the entire wrapper
constitutes a SCINE module that can be loaded dynamically at runtime.
For more information on these concepts see the ``Scine::Core``
`repository <https://github.com/qcscine/core>`_.

License and Copyright Information
---------------------------------

The SCINE XTB wrapper is distributed under the BSD 3-clause "New" or "Revised"
License. For more license and copyright information, see the file ``LICENSE.txt``
in the `repository <https://github.com/qcscine/xtb_wrapper>`_.

Note: this license does not cover the original `xtb` source code.

Installation and Usage
----------------------

The wrapper can be built and installed using the following commands::

    mkdir build
    cd build
    cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=<desired path> ..
    make -j<number of cores to use>
    make install

This will generate and install both the main `xtb` code and the wrapper in the
form of the file ``xtb.module.so`` that can be used in SCINE.

In order to make XTB available to SCINE the following two environment variables
need to be set::

    export XTBPATH=<prefix-path>/share/xtb
    export SCINE_MODULE_PATH=$SCINE_MODULE_PATH:<prefix-path>/lib

Afterwards, SCINE programs such as `ReaDuct <https://github.com/qcscine/readuct>`_
will pick up XTB's existence and it will be possible to request the implemented
methods.

Python
......

The SCINE XTB wrapper is also available via Python.
The underlying SCINE module can be loaded and its implemented calculators
accessed using the standard ``scine_utilities`` Python bindings.
A minimal workflow could look like this::

    import scine_utilities as su
    import scine_xtb_wrapper
    
    # Generate Structure
    structure = su.AtomCollection()
    structure.elements = [su.ElementType.H, su.ElementType.H]
    structure.positions = [[-0.7, 0, 0], [0.7, 0, 0]]
    
    # Get calculator
    manager = su.core.ModuleManager()
    calculator = manager.get('calculator', 'GFN2')

    # Configure calculator
    calculator.structure = structure
    calculator.set_required_properties([su.Property.Energy, su.Property.Gradients])
    
    # Calculate
    results = calculator.calculate()
    print(results.energy)
    print(results.gradients)

How to Cite
-----------

When publishing results obtained with SCINE we kindly ask you to cite the
appropriate references, e.g. the one for ReaDuct.
This wrapper does not have its own reference; it should also not be mistaken
for the actual `xtb` code it wraps.

For the latter code and its citations, we refer you to the original `xtb`
repository. There you will find the references of the actual methods used.
They are listed in the
`README.md <https://github.com/grimme-lab/xtb/blob/master/README.md>`_.

Support and Contact
-------------------

In case you should encounter problems or bugs with the wrapper, please write a
short message to scine@phys.chem.ethz.ch.

