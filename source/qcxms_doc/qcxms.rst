.. _qcxms:

-----------------------
Introduction to QCxMS
-----------------------

.. figure:: ../../figures/qcxms/qcxms_logo.svg
  :align: center

What is *QCxMS* ?
==================

`QCxMS` is a quantum chemical (**QC**) based program that enables users to calculate mass spectra (**MS**) 
using Born-Oppenheimer Molecular Dynamics (**MD**). 
It is the successor of the QCEIMS program, in which the **EI** part is exchanged to **x** (x=EI, CID) to account 
for the greater general applicability of the program. The different MS methods of the program are described in short 
below. A full list of :ref:`qcxmsrelatedrefs` is provided elsewhere.

The **QCxMS** program is available and free-of-charge at the QCxMS `GitHub page <https://github.com/qcxms/QCxMS/releases/>`_ 

MS methods available
====================

Electron Ionization
-------------------

The program was originally developed to calculate Electron Ionization (**EI**) mass spectra, in which a (typically
70 eV) electron beam is focused on a molecule in order to create an *open-shell* radical ion (uneven number of valence electrons). 
This process not only ionizes the molecule, but simultaneously increases the internal energy of the species, which 
in turn leads to bond breaking, fragmentation, rearrangement, etc of the ion. A more detailed description can be 
found in the `original publication`_. 

.. _original publication: https://doi.org/10.1002/anie.201300158 


Dissociative Electron Attachment
--------------------------------

In contrast to the positive ions created by the EI process, the Dissociative Electron Attachment (**DEA**) ionizes the 
molecule under study by adding an electron. This creates a negatively charged ion (*open-shell* ion), which has to be described by 
diffuse basis functions. This increases the computational cost because DFT has to be used to compute the PES. 
Details on this MS method are described in `this publication`_. 

.. _this publication: http://dx.doi.org/10.1039/C6CP06180J

Collision Induced Dissociation
------------------------------

Ionization of molecules can be done by (de)protonation, *e.g.*, by the popular electrospray ionization (ESI) method. 
This ionization process produces *closed-shell* ions (even number of valence electrons) and is softer than the EI or DEA methods. 
For this reason, the activation of the ion has to be conducted in a second step, *i.e.*, by acceleration and 
subsequent collisions with neutral collision gas atoms. The following Collision Induced Dissociation (**CID**) leads to 
a spectrum normally under lower energy conditions than in the other MS methods. The simulation conditions and 
specific details are extensively covered in `this paper`_.

.. _this paper: https://doi.org/10.1021/jasms.1c00098

