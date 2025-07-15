catcheR_sortcatch - Barcode reassignment
====================

``catcheR_sortcatch`` is an optional function that corrects the annotated cell-by-gene count matrix obtained with ``catcheR_10Xcatch`` or ``catcheR_scicatch``. It reassigns the perturbation of any cell belonging to an hPSC clone with reliable evidence of a shRNA-barcode swap, based on the results of ``catcheR_step1QC``.

Steps
-----

#. In a new working folder:
   
   a. Copy the annotated count matrix (e.g., ``silencing_matrix.csv``)  
   
   b. Copy the CSV file with the list of UCI-BCs with reliable evidence of a shRNA-barcode swap (e.g., ``reliable_clones_swaps.csv``; output of step :ref:`step-SPFourStepEleven` from :ref:`subsec-SPFourThree`)  
      .. _step-SPFourStepTwelve:

#. Run ``catcheR_sortcatch``:

   .. code-block:: r

      catcheR_sortcatch(
          group = c("sudo", "docker"),
          folder,
          expression.matrix,
          swaps
      )

   **Example usage:**

   .. code-block:: r

      catcheR_sortcatch(
          group = "docker",
          folder = "path/to/working/folder",
          expression.matrix = "silencing_matrix.csv",
          swaps = "reliable_clones_swaps.csv"
      )

Arguments
---------

- **group**: string with two options: ``sudo`` or ``docker``, depending on the user group (`info <https://docs.docker.com/engine/install/linux-postinstall/>`__)
- **folder**: string with the working folder path
- **expression.matrix**: string with the filename of the annotated count matrix CSV
- **swaps**: string with the filename of the swap list CSV file (see step :ref:`step-SPFourStepTwelve`)

Output
------

``catcheR_sortcatch`` outputs an updated annotated gene expression matrix CSV file called ``silencing_matrix_updated.csv``, in which the "BC" and "GENE" annotations have been corrected to reflect the actual shRNA encoded in each cell.
