catcheR_10XcatchQC - Fine-Tune iPS2-10X-seq Perturbation Assignment
========================================================

Optionally, uou can refine shRNA assignment thresholds without re-running the full ``catcheR_10Xcatch`` analysis using:

- ``catcheR_10XcatchQC``: regenerate QC plots with alternative thresholds or thresholding mode (e.g. switch from ``bimodal`` to ``noise``)
- ``catcheR_filtercatch``: re-apply filters using chosen thresholds

These tools only rerun the second half of the pipeline, saving time and computing resources.

Step-by-step
------------

1. In the same working folder used previously with ``catcheR_10Xcatch``, run ``catcheR_10XcatchQC``.  
   This will regenerate QC plots and suggest new thresholds.

.. code-block:: R

   catcheR_10XcatchQC(
       group = c("docker", "sudo"),
       folder = "path/to/working/folder",
       reference = "GGCGCGTTCATCTGGGGGAGCCG",
       mode = "bimodal",
       sample = 1,
       x = 100,
       y = 400
   )

**Arguments**:

- ``group``: `"docker"` or `"sudo"` depending on your system permissions
- ``folder``: working directory
- ``reference``: reverse complement of constant region before the UCI (default: ``GGCGCGTTCATCTGGGGGAGCCG``)
- ``mode``: thresholding strategy, either:
  - ``"bimodal"``: uses the valley in the UMIxUCI distribution (default)
  - ``"noise"``: 1.35 × number of UCIs with 1 UMI
- ``sample``: sample number (default = 1)
- ``x`` / ``y``: crop limits for the x and y axes of UMIxUCI plots (defaults: 100, 400)

**Example**:

.. code-block:: R

   catcheR_10XcatchQC(
       group = "docker",
       folder = "path/to/working/folder",
       mode = "noise"
   )

.. note::

   This function should be run once per sample (``sample = 1``, ``sample = 2``, etc.)

The arguments and outputs are identical to those in ``catcheR_10Xcatch``, but this step focuses only on quality control plot generation and threshold estimation.