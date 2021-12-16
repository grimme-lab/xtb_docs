PyVibMS
=======

To visualize the vibrational modes calculated by xtb (via ``--hess`` or ``--ohess``), find the ``g98.out`` file which collects the vibrational modes and frequencies.

Lauch PyVibMS in a new PyMOL window, load the  ``g98.out`` file by changing the file type into **Output File (*.out)**. Change the program type from **XYZ** to **xtb (g98.out file)**, check the **Has. Vib. Info.** box, click **Load** button.

Then vibrational modes can be visualized as following

.. image:: https://raw.githubusercontent.com/smutao/PyVibMS/83e503c0abf3f5f93cc667d28f67a433073c6757/examples/xtb-640/xtb-pyvibms-shot.png
   :width: 650
   :target: https://github.com/smutao/PyVibMS
  
The project homepage can is https://github.com/smutao/PyVibMS. It requires PyMOL 2.5+ to be installed.  

