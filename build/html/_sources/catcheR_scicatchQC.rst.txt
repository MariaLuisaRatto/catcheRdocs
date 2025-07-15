catcheR_scicatchQC - Fine-tune iPS2-sci-seq perturbation assignment
========================================================
.. _par-SPFourParFive:

.. 
   As for catcheR_10Xcatch, the catcheR_scicatch pipeline may also need to be re-run. 
   This can happen, for example, when thresholds have to be changed to custom values.

1. Run :func:`catcheR_scicatchQC` following the same steps described for :func:`catcheR_10XcatchQC` (see :ref:`par-SPFourParThree`).  
   The two functions share the same arguments and outputs, but :func:`catcheR_scicatchQC` uses the outputs from :func:`catcheR_scicatch`.

..
   The function generates the QC plots and suggests threshold values, but stops before applying them to allow for manual adjustment.

.. code-block:: R

    catcheR_scicatchQC(
        group = "docker", 
        folder = "path/to/working/folder", 
        mode = "noise")

..
    Arguments:
        - group: either "docker" or "sudo"
        - folder: path to the working directory
        - reference: reverse complement of the barcode reference sequence (default OPTtetR)
        - mode: either "bimodal" (default) or "noise"