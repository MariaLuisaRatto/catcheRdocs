catcheR_10Xnocatch - Identify cells expressing no shRNA
========================================================

This optional step identifies and annotates cells transduced with a cassette lacking the shRNA. These "empty" cells can be included as negative controls in downstream analyses.

In the same working directory used for :func:`catcheR_10Xcatch`, run:

.. code-block:: R

   catcheR_10Xnocatch(
       group = c("docker", "sudo"),
       folder,
       expression.matrix,
       threshold,
       sample = 1,
       reference = "TACGCGTTCATCTGGGGGAGCCG"
   )

**Arguments**:

- ``group``: either `"docker"` or `"sudo"`, depending on user permissions  
  (see: https://docs.docker.com/engine/install/linux-postinstall/)
- ``folder``: path to the working directory
- ``expression.matrix``: filename of the cell-by-gene count matrix (CSV format)
- ``threshold``: *integer* number of UMIs supporting the "empty" reference required to classify a cell as empty
- ``sample``: *integer* sample index (default = 1)
- ``reference``: string of the reverse complement of the empty vector’s unique identifier (default is the 3' end of the OPTtetR cDNA)

**Example usage**:

.. code-block:: R

   catcheR_10Xnocatch(
       group = "docker",
       folder = "path/to/folder",
       expression.matrix = "filename.csv",
       threshold = 5
   )

**Output**:

- An updated ``silencing_matrix.csv`` (also in RDS format), where empty cells are annotated using the following format:

.. code-block:: text

   cellID_?_empty_NA_empty

This enables easy tracking of negative control cells for further analysis.