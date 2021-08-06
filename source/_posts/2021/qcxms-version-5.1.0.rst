:date: 2021-08-06
:author: Jeroen Koopman
:category: release
:tags: qcxms

QCxMS version 5.1.0 released
=============================

.. image:: https://github.com/JayTheDog/QCxMS-Logo/raw/main/qcxms_logo_screen.svg 
   :alt: QCxMS logo

This new update of the `QCxMS program <https://github.com/qcxms/QCxMS/releases/tag/v.5.1.0>`_ changes the way xtb is used inside the code. Instead of a standalone implementation of xtb version 5.8.1 in the source code,
we switched to using the `tblite <https://github.com/awvwgk/tblite>`_ library, which allows updates to the latest version of xtb. This, in return, leads to a significant **increase in the computational
speed** of calculations done with the *GFNn-xTB* methods, while keeping the code independent from third party software. 

Furthermore, the `PlotMS program <https://github.com/qcxms/PlotMS/releases/tag/v.5.1>`_ has been updated as well. 

For a detailed description of all changes, check out the `GitHub repository <https://github.com/qcxms>`_.

