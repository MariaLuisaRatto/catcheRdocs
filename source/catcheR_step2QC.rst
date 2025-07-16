catcheR_step2QC - Pooled Cloning Step 2 and hiPSC Genome Editing QC
==================================================

This step complements :ref:`SupplementalProtocolOne` – :ref:`SPOneFourB` and :ref:`SPOneSeven`.

1. In a new working folder, prepare the following files:

   a. Fastq/fq or fastq.gz files with demultiplexed read 1 from the NGS run.

   b. ``rc_barcodes_genes.csv`` — a CSV file with two columns:  
      (1) the shRNA barcodes  
      (2) the matching shRNA names in the format `"GENE.shRNAID"`

   .. code-block:: text

      CAAGAGCC,SMAD2.1
      ...

   .. note::
      The file must be comma-separated, with no extra spaces before or after barcodes or gene names.  
      It is automatically detected and must be named exactly ``rc_barcodes_genes.csv``.

   c. *(Optional)* A `.txt` file with a list of clones of interest, formatted as ``BC_UCI``  
      (see step 1c in :ref:`SPFourThree`)

2. Run ``catcheR_step2QC``:

   .. code-block:: R

      catcheR_step2QC(
          group = c("docker", "sudo"),
          folder,
          fastq.read1,
          DIs = 1000,
          clones = NULL
      )

   **`catcheR_step2QC` arguments:**

   a. ``group``: string, either `"sudo"` or `"docker"` depending on user permissions  
      *(See:* https://docs.docker.com/engine/install/linux-postinstall/ *)

   b. ``folder``: string with the path to the working directory

   c. ``fastq.read1``: string with the read 1 filename from step 1a

   d. ``DIs``: integer, minimum number of diversity indexes (DIs) for a given UCI-BC  
      Used to filter for reliably measured UCI-BCs

   e. ``clones``: *(optional)* string with the `.txt` file name from step 1c

   **Example usage:**

   .. code-block:: R

      catcheR_step2QC(
          group = "docker",
          folder = "path/to/working/folder",
          fastq.read1 = "filename.fq",
          clones = "filename.txt"
      )

   **`catcheR_step2QC` key outputs:**

   a. Pie charts showing DIs per shRNA and per gene target (optionally, also per clone of interest)

   b. Text file listing all clones above the DI threshold

   c. Bar chart of the number of DIs per clone above the DI threshold  
      (see :ref:`FigureSPFourThreeA`)

   d. Frequency histogram of the percentage of clones above the DI threshold per shRNA  
      (see :ref:`FigureSPFourThreeB`)
      
      
      