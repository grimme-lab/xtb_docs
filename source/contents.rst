==============================================
 User Guide to Semiempirical Tight Binding
==============================================

This user guide focuses on the semiempirical quantum mechanical methods GFNn-xTB,
their descendants, and corresponding composite schemes as implemented in the
`xtb <https://github.com/grimme-lab/xtb>`_ (extended tight binding) program package.

We provide a number of detailed guides dealing with common task that can
be performed easily with the ``xtb`` program.
All guides are usually structured the same way, starting with some simple
examples using only the commandline and the default settings followed
by a trouble shooting section.
Detailed inputs are provided in a ready to use fashion to solve some
more special but still common tasks with ``xtb`` together with some
insights into the theory used behind the scences.


-------------
 Quick Links
-------------

.. panels::
   :column: + text-center

   .. image:: https://github.com/awvwgk/xtb-logo/raw/master/xtb.svg
      :width: 75%
      :alt: xtb

   +++

   .. link-button:: setup
      :type: ref
      :text: Documentation for xtb
      :classes: btn-block stretched-link

   ---

   .. image:: https://github.com/grimme-lab/crest/raw/master/assets/crest.png
      :width: 50%
      :alt: CREST

   +++

   .. link-button:: crest
      :type: ref
      :text: Documentation for CREST
      :classes: btn-block stretched-link

   ---

   .. image:: ../figures/CENSO/censo_logo_300dpi.png
      :alt: CENSO

   +++

   .. link-button:: censo
      :type: ref
      :text: Documentation for CENSO
      :classes: btn-block stretched-link

   ---

   .. image:: ../figures/qcxms/qcxms_logo.svg
      :alt: QCxMS

   +++

   .. link-button:: qcxms
      :type: ref
      :text: Documentation for QCxMS
      :classes: btn-block stretched-link

   ---
   :column: + col-lg-12 p-2

   .. image:: ../logo/akgrimme_logo_final2018_xtbdocs.png
      :width: 33%
      :alt: Grimme-lab

   +++

   .. link-button:: https://github.com/grimme-lab
      :text: Explore on GitHub
      :classes: btn-block stretched-link

--------------------------------------------
 Recent developments, news and publications
--------------------------------------------

.. postlist:: 5
   :date: %Y-%m-%d
   :format: {date}: {title} by {author}
   :excerpts:

See the :ref:`news archive <news>` for all posts.

-----------------------------------------
 xTB in Other Quantum Chemistry Programs
-----------------------------------------

The xTB-methods are now officially available in other quantum chemistry programs!

 - in `Orca`_ 4.2 an IO-based interface to the ``xtb`` binary is available
 - `AMS`_ 2019 implements GFN1-xTB in their DFTB module
 - the `entos`_ program implements GFN1-xTB (also available in the webinterface)
 - the computational chemistry framework `cuby4`_ supports ``xtb``
 - `Turbomole`_ does support GFN1-xTB and GFN2-xTB since version 7.4
 - `QCEngine`_ supports calculations with the ``xtb`` API
 - the `GMIN`_, `OPTIM`_, and `PATHSAMPLE`_ global optimization tools provide a ``xtb`` interface
 - `CP2K`_ has an GFN1-xTB implementation since version 7.1


.. _Orca: https://orcaforum.kofo.mpg.de
.. _AMS: https://www.scm.com/product/dftb/
.. _entos: https://www.entos.info/
.. _cuby4: http://cuby4.molecular.cz/interface_xtb.html
.. _Turbomole: https://www.turbomole.org/
.. _QCEngine: http://docs.qcarchive.molssi.org/projects/QCEngine
.. _GMIN: http://www-wales.ch.cam.ac.uk/examples/GMIN
.. _OPTIM: http://www-wales.ch.cam.ac.uk/OPTIM
.. _PATHSAMPLE: http://www-wales.ch.cam.ac.uk/PATHSAMPLE/
.. _CP2K: https://www.cp2k.org/version_history

We missed your project here?
No problem, just give us hint at the mailing list or open an issue at `github`_.

.. _github: https://github.com/grimme-lab/xtb_docs/issues/new


.. toctree::
   :maxdepth: 3
   :caption: Quickstart

   setup
   basics
   commandline
   geometry
   xcontrol
   development

.. toctree::
   :maxdepth: 3
   :caption: Guides

   sp
   properties
   optimization
   scan
   gbsa
   hessian
   md
   mtd
   path
   gsm
   periodic_boundary_conditions
   pcem
   gfnff
   capi
   python
   community/index

.. toctree::
   :maxdepth: 3
   :caption: Submodules

   xtb_info
   xtb_thermo
   xtb_topo

.. toctree::
   :maxdepth: 3
   :caption: CREST

   crest
   crestversions
   crestcmd
   crestxmpl
   crestqcg

.. toctree::
   :maxdepth: 3
   :caption: ENSO
   
   enso_doc/enso
   enso_doc/enso_setup
   enso_doc/enso_usage
   enso_doc/enso_anmr
   enso_doc/plotting


.. toctree::
   :maxdepth: 3
   :caption: CENSO
   
   CENSO_docs/censo
   CENSO_docs/censo_setup
   CENSO_docs/censorc
   CENSO_docs/censo_thresholds
   CENSO_docs/censo_solvation
   CENSO_docs/censo_nmr
   CENSO_docs/censo_troubleshooting
   CENSO_docs/censo_usage
   CENSO_docs/abbreviations

.. toctree::
   :maxdepth: 3
   :caption: QCxMS
   
   qcxms_doc/qcxms
   qcxms_doc/qcxms_setup
   qcxms_doc/qcxms_run
   qcxms_doc/qcxms_plot
   qcxms_doc/qcxms_ex
   qcxms_doc/qcxms_cites

.. toctree::
   :maxdepth: 3
   :caption: Misc

   news
   versions
   xtbrelatedrefs
   license
   help

