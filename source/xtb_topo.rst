.. _xtb_topo:

--------------------
 Topology Submodule
--------------------

.. note::
   This feature is only present in version 6.3 and newer

The topology submodule allows to create a GFN-FF topology from a geometry
input, which can be used to restart a calculation with the GFN-FF without
having to perform a costly reevaluation of the force field topology

To start the generation use

.. code-block:: text

   xtb topo [--namspace <name>] <geometry>

If you want to use a particular namespace in the calculation later include
the namespace flag.
Note that even if you want to perform several GFN-FF calculation on different
namespaces, generate the topology file without a namespace is recommend,
as ``xtb`` will first search the local namespace and than fall back to the
default file name to finding the generated topology file.

.. important::
   The force field generation step is a cubic scaling step as it requires
   an all pair shortest path (APSP) algorithm for generating the topological
   charges and for the diagonalization of the Hückel matrices for the
   torsion potentials, which can take, depending on the system of interest
   a significant portion of computation time.

The generated file will be named ``gfnff_topo`` or ``<name>.gfnff_topo`` and
can become quite large.
