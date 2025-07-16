catcheR_10Xcatch - iPS2-10X-seq Perturbation Deconvolution
=======================================
.. _catcherbarcodes:

shRNA perturbations can be assigned to single cells using ``catcheR_10Xcatch``. This tool identifies NGS reads containing UCI-BCs, matches them to transcriptomes via shared cell barcodes, and applies several noise-reduction filters to select cells with strong evidence of a single shRNA integration.

The filtering involves:

1. Removing UCI-BCs supported by few UMIs (likely artifacts),
2. Filtering UCI-BCs with a low UMI fraction compared to others in the same cell,
3. Removing ambiguous cases where multiple UCI-BCs are close to threshold.

Only cells with a single robust shRNA integration are retained.

This pipeline applies to experiments using 10X-based platforms (iPS2-10X-seq, iPS2-CITE-seq, or iPS2-multi-seq), which share similar count matrix structure. The analysis proceeds with:

- ``catcheR_10Xcatch``: complete pipeline with automatic thresholding
- ``catcheR_10XcatchQC``: optional refinement of thresholds
- ``catcheR_filtercatch``: re-filtering using refined thresholds
- ``catcheR_10Xnocatch``: add unperturbed cells as negative controls

.. note::

   Cell Ranger may be required to obtain count matrices.  
   You can install it manually or use Docker containers from:
   
   - `cellranger7hedge <https://hub.docker.com/repository/docker/hedgelab/cellranger7hedge>`_
   
   - `cellranger9 <https://hub.docker.com/repository/docker/hedgelab/cellranger_9>`_

Step-by-step
------------

1. In a new working folder:

   a. Copy demultiplexed Read 1 and Read 2 files of the UCI-BC library  

   b. Copy the gene expression count matrix CSV  

   c. Create a CSV file named ``rc_barcodes_genes.csv`` with:

   .. code-block:: text

      CAAGAGCC,SMAD2.1
      ...

2. Run ``catcheR_10Xcatch``:

.. code-block:: R

   catcheR_10Xcatch(
       group = c("docker", "sudo"),
       folder = "path/to/folder",
       fastq.read1 = "R1.fastq.gz",
       fastq.read2 = "R2.fastq.gz",
       expression.matrix = "filename.csv",
       reference = "GGCGCGTTCATCTGGGGGAGCCG",
       UCI.length = 6,
       threads = 2,
       percentage = 15,
       mode = "bimodal",
       ratio = 5,
       samples = 1,
       x = 100,
       y = 400
   )

**Arguments**:

- ``group``: `"docker"` or `"sudo"` depending on permissions (`Docker install guide <https://docs.docker.com/engine/install/linux-postinstall/>`_)
- ``folder``: working directory
- ``fastq.read1``: filename for Read 1 (UCI-BC library)
- ``fastq.read2``: filename for Read 2
- ``expression.matrix``: cell-by-gene count matrix (CSV format)
- ``reference``: (optional) reverse complement of constant region before UCI; default is `"GGCGCGTTCATCTGGGGGAGCCG"`
- ``UCI.length``: (optional) UCI length, default is 6
- ``threads``: (optional) number of threads for parallel processing
- ``percentage``: (optional) minimum % of UMIs for a UCI to be valid; default 15
- ``mode``: (optional) `"bimodal"` (default) or `"noise"` for thresholding strategy
- ``ratio``: (optional) minimum UMI ratio between top 2 UCIs; default 5
- ``samples``: (optional) number of multiplexed samples; default 1
- ``x``, ``y``: (optional) plot axis limits for UMIxUCI distribution; defaults: 100 (x), 400 (y)

**Example**:

.. code-block:: R

   catcheR_10Xcatch(
       group = "docker",
       folder = "path/to/folder",
       fastq.read1 = "R1.fastq.gz",
       fastq.read2 = "R2.fastq.gz",
       expression.matrix = "matrix.csv",
       threads = 12
   )

**Output Files** (in ``Result/`` folder):

- ``log.txt``: number of reads processed
- ``log2.txt``: number of cells, UCIs, UMIs, threshold values (bimodal & noise)
- Barplots of UMI counts per shRNA and per gene  
  
  .. image:: barcode_distribution.pdf
  
  .. image:: gene_distribution.pdf

- Histogram: UMI counts per UCI (UMIxUCI)  
  
  .. image:: UMIxUCI.pdf
  
  .. image:: UMIxUCI_400_400.pdf
  
- Histogram: UCI UMI percentage in cell (UMIpercentagexUCI)  
  
  .. image:: percentage_of_UMIxUCI_dist.pdf
  
- 2D dot plots:
  - UMI vs UMI% per UCI, colored by valid integration count or status  
    
  .. image:: 2D_percentage_of_UMIxUCI_UMI_count_trueorfalse.pdf
  
  .. image:: 2D_percentage_of_UMIxUCI_UMI_count_ValidCells.pdf
    
- ``log_part3.txt``: number of single-integration vs filtered cells
- ``silencing_matrix.csv``: annotated expression matrix with shRNA assignment  
  (also in RDS format)

   Annotated cell names follow this format:

.. code-block:: text

   TTCTAACCACAGTCGC_180_CGTGATGC_NKX2.5_ACAGTG

Where:

- ``TTCTAACCACAGTCGC`` = original 10X barcode (cellID)

- ``180`` = number of UMIs supporting the shRNA

- ``CGTGATGC`` = shRNA barcode (BC)

- ``NKX2.5`` = target gene

- ``ACAGTG`` = UCI

You can use this matrix directly for downstream analysis functions.