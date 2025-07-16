catcheR_step1QC - Pooled Cloning Step 1 QC
=========================

This step complements SupplementalProtocolOne.

1. In a new working folder, prepare the following files:

   a. Fastq/fq or fastq.gz files with demultiplexed read 1 from the NGS run.

   b. A CSV file with the shRNA names and their full sequences:

   .. code-block:: text

      SMAD2.1,GCAAGTACTCCTTGCTGGATTGCTCGAGCAATCCAGCAAGGAGTACTTG
      ...

   c. *(Optional)* A `.txt` file with a newline-separated list of clones of interest in the format `BC_UCI` (e.g., from a subsequent iPS2seq-edited-hPSC experiment). Each clone is identified by its shRNA barcode and UCI separated by an underscore:

   .. code-block:: text

      CAAGAGCC_CATCGT
      ...

2. Run ``catcheR_step1QC``:

   .. code-block:: R

      catcheR_step1QC(
          group = c("docker", "sudo"),
          folder,
          fastq.read1,
          DIs = 100,
          ratio = 10,
          plot.threshold = 2000,
          clones = NULL
      )

   **`catcheR_step1QC` arguments:**

   a. ``group``: string, either `"sudo"` or `"docker"` depending on your user group.  
      *(For Docker group setup, see:* https://docs.docker.com/engine/install/linux-postinstall/ *)

   b. ``folder``: string with the path to the working directory.

   c. ``fastq.read1``: string with the filename from step 1a.

   d. ``DIs``: integer, minimum number of diversity indexes (DIs, pseudo-unique reads) for the most represented shRNA matched to a given UCI-BC.  
      Used with ``ratio`` to select reliable UCI-BC/shRNA associations. *(Default = 100)*

   e. ``ratio``: integer, the minimum ratio between the DIs of the most represented and second most represented shRNAs.  
      Used with ``DIs`` to confirm assignment. *(Default = 10)*

   f. ``plot.threshold``: integer, minimum number of DIs per UCI-BC to be included in bar plots.

   g. ``clones``: *(optional)* string with the `.txt` file name from step 1c.

   **Example usage:**

   .. code-block:: R

      catcheR_step1QC(
          group = "docker",
          folder = "path/to/folder",
          fastq.read1 = "filename.fq",
          clones = "filename.txt"
      )

   **`catcheR_step1QC` key outputs:**

   a. ``reliable_clones_swaps.csv`` — lists UCI-BCs with strong evidence of shRNA-barcode swap.  
      Can be used as input for :ref:`SPFourEight`.

   b. Bar chart showing the number of DIs for each clone above the ``plot.threshold``.

   c. Bar charts showing the number of DIs associated with each barcode, shRNA, and reliable swap.

   d. *(If the ``clones`` argument is provided)*  
      CSV files and bar charts showing the number of DIs for each shRNA matched to each clone of interest  
      (see :ref:`FigureSPFourTwo`).