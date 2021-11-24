.. _censo_trouble:

Trouble shooting
================

CENSO is designed as an automated workflow. It can happen that for example 
single-point calculations fail. In this case CENSO will consider this calculation
as failed and sort out the corresponding conformer/structure. The calculation 
is not retried but can be manually checked and the results can be written to 
enso.json. With the updated enso.json the calculation can be restarted.

If many or all single-point calculations fail, this is a very good indication that either:

* the input geometry is far from being reasonable
* maybe a missing charge information
* a technical problem e.g., missing information on the QM program path 
