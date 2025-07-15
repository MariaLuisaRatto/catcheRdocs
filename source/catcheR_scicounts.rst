catcheR_scicounts - Obtain iPS2-sci-seq Count Matrices
==================================

1. ``catcheR_scicount`` is a wrapper for the `bbi-sci pipeline <https://github.com/bbi-lab/bbi-sci>`_ developed by the Brotman Baty Institute for Precision Medicine.  
   This pipeline was dockerized and integrated into ``catcheR`` to enable use across operating systems.  
   It performs demultiplexing and alignment of sci-RNA-seq data, generating a gene expression matrix from FASTQ files.

2. Demultiplex Illumina base calls to FASTQ files:

   a. Create a ``SampleSheet.csv`` file with one row per PCR well (see :ref:`SPTwoFive` of :ref:`SupplementalProtocolTwo`).  
      - Use well IDs as ``Sample_ID`` (format: ``[A-H][01-12]``)  
      - Use appropriate ``index`` and ``index2`` fields for i7 and i5 barcodes  
      - For reference, see :ref:`TableSPTwoOne`

   b. Run Illumina ``bcl2fastq`` according to Illumina documentation.

   c. Run ``FastQC`` to confirm FASTQ quality.

3. In a new working folder:

   a. Create a subfolder called ``fastq/`` and copy all demultiplexed ``.fastq.gz`` files into it.  
      File names must start with the well coordinate (e.g. ``A01_``).

   b. Create a tab-separated file called ``sci-RNA-seq-8.RT.oligos`` that maps RT well IDs to barcode sequences  
      (refer to :ref:`TableSPTwoOne`):

   .. code-block:: text

      A01	TTCTCGCATG
      ...

   c. Create a subfolder called ``GENOMES/`` and copy the annotated genome files into it  
      (e.g., `GRCh38 <https://doi.org/10.5281/zenodo.11243110>`_).  
      Ensure sufficient disk space (~60 GB).

4. Run ``catcheR_scicount``:

   .. code-block:: R

      catcheR_scicount(
          group = c("docker", "sudo"),
          folder,
          sample.name,
          UMI.cutoff
      )

   **`catcheR_scicount` arguments:**

   a. ``group``: string, either `"docker"` or `"sudo"` depending on user permissions  
      *(see:* https://docs.docker.com/engine/install/linux-postinstall/ *)

   b. ``folder``: string with the full path to the working folder

   c. ``sample.name``: string, name of the experiment

   d. ``UMI.cutoff``: integer, minimum number of UMIs per nucleus to consider the transcriptome valid

   **Example usage:**

   .. code-block:: R

      catcheR_scicount(
          group = "docker",
          folder = "path/to/file",
          sample.name = "experiment",
          UMI.cutoff = 500
      )

5. **`catcheR_scicount` outputs** (saved in the ``final-output/`` folder):

   - UMI per cell knee plot (see :ref:`FigureSOnePartTwo:H`)
   - Summary statistics
   - Sparse cell-by-gene matrix
   - Dense expression matrices:
     - ``exp_mat.csv``
     - ``exp_mat_no0.csv`` (genes with zero counts removed)
   - Corresponding ``.Rdata`` files