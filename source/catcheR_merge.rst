catcheR_merge - Merge multiple samples
========================================================

If fine-tuning is applied after :func:`catcheR_10Xcatch` and multiple samples are analyzed independently, use :func:`catcheR_merge` to aggregate the results into a single annotated matrix. 

.. note::

   If all samples were processed at once using the ``samples`` argument in :func:`catcheR_10Xcatch`, this step is done automatically and does not need to be repeated.

Run:

.. code-block:: R

   catcheR_merge(
       group = c("docker", "sudo"),
       folder,
       samples = 2,
       empty = TRUE
   )

**Arguments**:

- ``group``: either ``"docker"`` or ``"sudo"``, depending on user permissions  
  (see: https://docs.docker.com/engine/install/linux-postinstall/)
- ``folder``: path to the working directory containing the per-sample outputs
- ``samples``: integer, the number of samples to be merged
- ``empty``: boolean, whether to include cells identified as empty (from :func:`catcheR_10Xnocatch`)

**Example usage**:

.. code-block:: R

   catcheR_merge(
       group = "docker", 
       folder = "/20tb/ratto/catcheR/test_CM5/", 
       samples = 4
   )

**Output**:

- A single merged ``silencing_matrix.csv`` file (and RDS version), aggregating data across all provided samples. If ``empty = TRUE``, the merged matrix includes cells with no shRNA integration for use as negative controls.