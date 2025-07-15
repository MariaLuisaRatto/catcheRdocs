catcheR_filtercatch - Fine-Tune iPS2-10X-seq Perturbation Assignment
========================================================

Still within the same working directory used for :func:`catcheR_10Xcatch`, you can re-apply filtering with user-defined thresholds by running:

.. code-block:: R

   catcheR_filtercatch(
       group = c("docker", "sudo"),
       folder,
       expression.matrix,
       UMI.count,
       percentage = 15,
       ratio = 5,
       sample = 1
   )

**Arguments**:

- ``group``: either `"docker"` or `"sudo"`, depending on your system permissions
- ``folder``: string with the working directory path
- ``expression.matrix``: string with the filename of the cell-by-gene count matrix (CSV format)
- ``UMI.count``: *integer* threshold for the minimum number of UMIs per UCI to be considered valid
- ``percentage``: *integer* percentage threshold for a UCI to be accepted in a cell  
  (e.g., if ``percentage = 15``, the UCI must account for ≥15% of all UMI in that cell)
- ``ratio``: *integer* minimum ratio of the top UMI count to the second-highest in a cell to accept the top UCI (default = 5)
- ``sample``: *integer* specifying the sample number (default = 1). Run this step separately for each sample.

**Example**:

.. code-block:: R

   catcheR_filtercatch(
       group = "docker",
       folder = "path/to/working/folder",
       expression.matrix = "filename.csv",
       UMI.count = 5,
       sample = 1
   )

**Notes**:

- This function reuses outputs from the initial ``catcheR_10Xcatch`` run.
- Arguments and outputs are otherwise the same as :func:`catcheR_10Xcatch`.