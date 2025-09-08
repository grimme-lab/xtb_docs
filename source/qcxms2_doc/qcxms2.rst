-----------------------
Introduction to QCxMS2
-----------------------

.. figure:: ../../figures/qcxms2/qcxms2_logo.svg
  :align: center

What is *QCxMS2* ?
==================

`QCxMS2` is a quantum chemical (**QC**) based program that enables users to calculate mass spectra (**MS**) 
using automated reaction network discovery. The program is designed to simulate the fragmentation of molecules. 
It is the successor of the QCxMS program, in which fragmentation reactions are simulated via Born-Oppenheimer Molecular Dynamics (**MD**).
The different MS methods of the program are described in short below. A full list of :ref:`qcxms2relatedrefs` is provided elsewhere.

The **QCxMS2** program is available and free-of-charge at the QCxMS2 `GitHub page <https://github.com/grimme-lab/QCxMS2/releases/>`_


MS methods available
====================

Electron Ionization
-------------------

The program was originally developed to calculate Electron Ionization (**EI**) mass spectra, in which a (typically
70 eV) electron beam is focused on a molecule in order to create an *open-shell* radical ion (uneven number of valence electrons). 
This process not only ionizes the molecule, but simultaneously increases the internal energy of the species, which 
in turn leads to bond breaking, fragmentation, rearrangement, etc of the ion. A more detailed description can be 
found in the `original publication`_. 

.. _original publication: https://doi.org/10.1039/D5CP00316D


Dissociative Electron Attachment
--------------------------------

In contrast to the positive ions created by the EI process, the Dissociative Electron Attachment (**DEA**) ionizes the 
molecule under study by adding an electron. This creates a negatively charged ion (*open-shell* ion).

.. note::
    The DEA mode is not yet tested for QCxMS2 but works technically. It should therefore be used with caution.
    The negative ions have to be described by using DFT and diffuse basis functions which increases the computational costs
    significantely.

Collision Induced Dissociation
------------------------------

Ionization of molecules can be done by (de)protonation, *e.g.*, by the popular electrospray ionization (ESI) method. 
This ionization process produces *closed-shell* ions (even number of valence electrons) and is softer than the EI or DEA methods.
It is often implemented by (de-)protonation of the molecule under study, leading to positivly or negativly charged
molecular ions. The following Collision Induced Dissociation (**CID**) leads to a spectrum normally under lower energy conditions than in 
the other MS methods.

A more detailed description can be 
found in the respective `publication`_. 

.. _publication: https://doi.org/10.1021/jasms.5c00234

.. note::
    It is recommended to start with an upper estimate of the average energy,
    e.g. ``-esiatom 0.5 eV`` for an initial QCxMS2 calculation, and then
    simulate afterwards in the same directory with the ``-esim`` mode
    different energies (e.g. 0.2-0.5 eV) to find suitable settings for your respective molecule
    and experimental setup.
