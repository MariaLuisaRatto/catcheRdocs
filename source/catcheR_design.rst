catcheR_design - Oligonucleotide Design
======================

This step complements Supplemental Protocol 1.

1. In a new working folder, prepare the following files:

   a. A comma-separated values (CSV) file with three columns listing:  
      (1) the forward oligos from the TRC shRNA library  
      (2) the corresponding barcodes (BC)  
      (3) the shRNA names  
      
      Below is an example for the shRNA described in :ref:`FigureSPOneOne`:

   .. code-block:: text

      CCGGCAAGTACTCCTTGCTGGATTGCTCGAGCAATCCAGCAAGGAGTACTTGTTTTTG,CAGTTCCA,SMAD2.1
      ...

   b. *(Optional)* A `.txt` file with a newline-separated list of 5'-3' restriction sites, or other sequences to be avoided in the shRNAs.  
      By default these are SalI, SwaI, and AscI:

   .. code-block:: text

      GTCGAC
      ATTTAAAT
      GGCGCGCC

2. Run ``catcheR_design``:

   .. code-block:: R

      catcheR_design(
          group = c("docker", "sudo"),
          folder,
          sequences,
          gibson.five = "AGTTCCCTATCAGTGATAGAGATCCC",
          gibson.three = "GTAGCTCGCTGATCAGC",
          fixed = "GTCGACATTTAAATGGCGCGCC",
          restriction.sites = NULL
      )

   **`catcheR_design` arguments:**

   a. ``group``: string, one of `"docker"` or `"sudo"` depending on user permissions  
      *(For a detailed explanation of Docker user groups, see:* https://docs.docker.com/engine/install/linux-postinstall/ *)

   b. ``folder``: string with the path to the working folder

   c. ``sequences``: string with the CSV file name from step 1a

   d. ``gibson.five``: *(optional)* string with the 5' Gibson homology sequence

   e. ``gibson.three``: *(optional)* string with the 3' Gibson homology sequence

   f. ``fixed``: *(optional)* string with the multicloning site

   g. ``restriction.sites``: *(optional)* string with the `.txt` file name from step 1b

   **Example usage:**

   .. code-block:: R

      catcheR_design(
          group = "docker",
          folder = "path/to/folder",
          sequences = "filename.csv",
          restriction.sites = "filename.txt"
      )

   **`catcheR_design` outputs:**

   a. ``output.txt`` – contains the oligo sequences to be used for synthesis  
      (see :ref:`FigureSPOneOne`)

   b. ``bad_oligos.txt`` – lists the shRNAs containing forbidden restriction sites  
      (highlighted in lowercase characters). This can be used to refine the shRNA list.

   Example output from ``bad_oligos.txt``:

   .. code-block:: text

      AGTTCCCTATCAGTGATAGAGATCCCGGACATAATCACTGCGTAATCCTCagatctTACGCAGTGATTATGTCCTTTTTTTGT-
      CGACATTTAAATGGCGCGCCNNNNNNGCTGAAGAGTAGCTCGCTGATCAGC,GATA4
      ...