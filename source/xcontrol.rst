.. _detailed-input:

----------------
 Detailed Input
----------------

The ``xcontrol`` instruction set is inspired by the Turbomole ``control``
file syntax. I decided to call it ``xcontrol`` instructions back than,
but here we will just call it (detailed) input for convenience.
To read an input file called ``xtb.inp`` use

.. code:: bash

  > xtb --input xtb.inp coord

In the detailed input you have control about almost very global
variable in the program, some instructions even check your input, but
most of the time you should know what you are doing.
Developed as a feature for developers, this is incredible powerful
and naturally way to complicated for the average application.
So in most cases you can safely rely on the internal defaults or
the shipped global configuration file (should usually be the same).

I will walk you through some selected instructions you might find useful
for your application.

Fixing, Constraining and Confining
==================================

In ``xtb`` different concepts of constraints are implemented,
so you should know which tool is best for you problem before you
start writing the detailed input.

Exact Fixing
------------

In the *exact fixing* approach the Cartesian position of the selected
atom is fixed in space by setting its gradient to zero and the degrees
of freedom are removed from the optimization procedure and therefore
the atoms stay in place in geometry optimizations.

For dynamics this exact fixing is *automatically deactivated*, since it
usually leads to instabilities in the simulation.

To activate the exact fixing for atoms 1--10 and atom 12 as well as for
all oxygen atoms, add

.. code::

  $fix
   atoms: 1-10,12
   elements: O
  $end

to your detailed input, the atoms keyword refers to the numbering
of the individual atoms in your input geometry.

Constraining Potentials
-----------------------

Almost absolute control about anything in your system is archived
by applying *constraining potentials*. First of all the constraining
potentials offer a weaker version of the exact fixing, which is
invoked by the same syntax in the ``$contrain`` data group as

.. code::

  $constrain
   atoms: 11
   elements: C,N,8
  $end

the program will not attempt to hold the Cartesian positions constant,
but the distances between all selected atoms, here number 11 and all
carbon, nitrogen and oxygen. For each atom pair a harmonic potential
is generated to hold the distances at roughly the starting value, this even
works without problems in dynamics.

To constrain the atoms more tightly the force constant can be adjusted

.. code::

  $constrain
   force constant=1.0
  $end

this variable goes directly into the constraining procedure and is given in
Hartree, for very high force constants this becomes equivalent to the exact fixing.
Note the difference in the syntax as you are required to use an equal-sign
instead of a colon, as you are modifying a global variable.

It is also possible to constrain selected internal coordinates, possible
are distances, angles and dihedral angles as done here

.. code::

  $constrain
     distance: 1, 2, 2.5
     angle: 5, 7, 8, 120
     dihedral: 3, 4, 1, 7, auto
  $end

Distance constraints are given in Ångström, while angle constraints are given
in degrees.
The distances are defined by two atom number referring to the order in
your coordinate input, angles are defined by three atom numbers and
dihedral angles by four atoms, in any case the atoms do not have to
be connected by bonds. The last argument is always the value which should
be used in the constraining potential as reference, if you decide to
use the current value ``auto`` can be passed. The constraints will be
printed to the screen (the newer implementation may require the verbose mode,
to trigger the printout of the constraint summary).


If you are not quite sure which distances or angles you want to constrain,
run

.. code:: bash

  > cat geosum.inp
  $write
     distances=true
     angles=true
     torsions=true
  $end
  > xtb --define --verbose --input geosum.inp coord

and have a look at the geometry summary for your molecule. The `$write`
data group toggles the printout in the property section and also some
printouts in the input section.

Confining in a Cavity
---------------------

If you are running dynamics for systems that are non-covalently bound,
you may encounter dissociation in the dynamics. If you want to
study the bound complex, you can try to *confine* the simulation
in a little sphere, which keeps the molecules from escaping.
The detailed input looks like

.. code::

  $wall
     potential=logfermi
     sphere: auto, all
  $end

You can be more precise on the radius by giving the value in bohr instead
of ``auto``. I personally recommend to use the logfermi potential, since it
is best suited for confinements, but yet not the default.
