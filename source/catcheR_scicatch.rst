caycheR_scicatch - iPS2-sci-seq perturbation deconvolution
========================================================
.. _subsec-SPFourSeven:

This section describes a typical analysis pipeline similar to the one described in the previous section, but using the following functions: :func:`catcheR_scicatch`, :func:`catcheR_scicatchQC`, :func:`catcheR_filtercatch`, and :func:`catcheR_scinocatch`.

1. Prepare the working directory:

   a. Create a subfolder named ``fastq`` and copy all the demultiplexed ``.fastq.gz`` files 
   (see https://marialuisaratto.github.io/catcheRdocs/catcheR_scicounts.html). Ensure all filenames begin with the well coordinate (e.g., ``A01``).

   b. Copy the cell-by-gene expression matrix CSV file obtained with :func:`catcheR_scicount`  
   (see step :ref:`step-SPFourStepEight` of :ref:`par-SPFourParTwo`).

   .. 
      c. Identify the reference sequence similarly to what was done in the `catcheR_10Xcatch` pipeline.
      d. Check the length of the random UCI. 

   c. Create a file ``rc_barcodes_genes.csv`` with two columns: (1) shRNA barcode; (2) matching shRNA name (format: ``GENE.shRNAID``).

   .. code-block:: text

      CAAGAGCC,SMAD2.1
      ...

   .. 
      CTTCTTTC,CHD7.1
      GTACTCAA,CHD7.2
      TTCGTCAT,CHD7.3

   d. Copy the text file ``sci-RNA-seq-8.RT.oligos`` used by :func:`catcheR_scicount`.

   ..
      Example:
      A01    TTCTCGCATG
      A02    TCCTACCAGT
      A03    GCGTTGGAGC
      ...

2. Run :func:`catcheR_scicatch` to perform a full analysis with automatic thresholding.  
   The arguments are the same as in :func:`catcheR_10Xcatch`, except filenames are omitted — files must be present in the ``fastq`` folder.

.. code-block:: R

   catcheR_scicatch(
       group = c("docker", "sudo"),
       folder, 
       expression.matrix, 
       reference = "GGCGCGTTCATCTGGGGGAGCCG",
       UCI.length = 6, 
       threads = 2, 
       percentage = 15, 
       ratio = 5,
       mode = "bimodal",
       x = 100,
       y = 400)

..
   Arguments:
     - group: sudo or docker depending on permissions
     - folder: working directory
     - expression.matrix: CSV matrix from catcheR_scicount
     - reference: reverse complement of read2 reference (default: TetR cDNA)
     - UCI.length: length of UCI after reference (default 6)
     - threads: number of threads (default 2)
     - percentage: minimum % of UMIs for a UCI in a cell (default 15)
     - mode: "bimodal" or "noise" thresholding (as described in 10Xcatch)

Example usage:

.. code-block:: R

   catcheR_scicatch(
       group = "docker", 
       folder = "path/to/working/folder", 
       expression.matrix = "filename.csv", 
       threads = 12)

**Outputs**:

:func:`catcheR_scicatch` produces the same key outputs as ``catcheR_10Xcatch``, with the following differences:

- ``silencing_matrix.csv`` contains modified cell names reflecting PCR well and RT barcode:
  
  .. code-block:: text

     P24__RT_27_7_GCCTGTGT_SCR_ACGGTC

  where:
  
  - ``P24``: PCR well
  - ``RT_27_7``: RT barcode ID
  - ``GCCTGTGT``: shRNA barcode
  - ``SCR``: target gene (e.g. scramble)
  - ``ACGGTC``: UCI

- Additional QC plots include ``demux`` and ``RT`` distribution: cell counts per PCR row/column and RT barcode, respectively. These help assess biases during sci-RNA-seq library preparation.