Getting started
=======

**catcheR** is a comprehensive bioinformatic package for designing and analyzing iPS2-seq experiments. It comprises the following functions:

1. **`catcheR_design`**  
   
   Designs oligonucleotides for *Supplemental Protocol 1 – Design shRNA oligonucleotides*, facilitating shRNA library cloning.

2. **`catcheR_step1QC`**  
   
   Analyzes the results of *Supplemental Protocol 1 – Intermediate plasmid pool QC*, assessing pooled cloning step 1 plasmids for barcode swaps.

3. **`catcheR_step2QC`**  
   
   Analyzes the results of *Supplemental Protocol 1 – Final plasmid pool QC or hiPSC pool QC*, assessing pooled cloning step 2 or genome-edited hiPSC pools for shRNA representation.

4. **`catcheR_scicount`**  
   
   Analyzes 2-level indexing sci-RNA-seq data, facilitating the generation of gene expression matrix for iPS2-sci-seq experiments.

5. **`catcheR_scicatch`**  
   
   Assigns shRNA perturbations to single nuclei transcriptomes obtained by *Supplemental Protocol 2*, enabling the primary analysis of iPS2-sci-seq.

6. **`catcheR_10Xcatch`**  
   
   Assigns shRNA perturbations to single cell transcriptomes obtained by *Supplemental Protocol 3*, enabling the primary analysis of iPS2-10X-seq.

7. **`catcheR_scicatchQC`** and **`catcheR_10XcatchQC`**  
   
   Use the outputs of `catcheR_scicatch` and `catcheR_10Xcatch`, respectively, to fine-tune shRNA assignment thresholds.

8. **`catcheR_filtercatch`**  
   
   Leverages the output of `catcheR_scicatchQC` and `catcheR_10XcatchQC` to filter single nuclei/cell transcriptomes expressing a single shRNA.

9. **`catcheR_sortcatch`**  
   
   Quality-controls the cell-by-gene matrix based on the results of `catcheR_step1QC`, reassigning hPSC clones with barcode swaps to the correct shRNA.

10. **`catcheR_scinocatch`** and **`catcheR_10Xnocatch`**  
    
    Identify cells expressing no shRNA in iPS2-sci-seq and iPS2-10X-seq experiments, respectively, adding them to the cell-by-gene matrix to be used as additional controls.

11. **`catcheR_load`**  
    
    Loads gene expression matrices annotated with shRNA perturbations into a *Monocle* object, preparing the dataset for downstream analysis.

12. **`catcheR_pseudotime`**  
    
    Analyzes the effects of shRNA perturbations on pseudotime dynamics, highlighting shifts along differentiation trajectories.

13. **`catcheR_modules`**  
    
    Assesses perturbation-induced changes in gene module expression, such as coordinated activation or repression of functional programs.

14. **`catcheR_enrichment`**  
    
    Quantifies differences in perturbation representation across experimental samples or cell clusters.


Availability and Installation
-----------------------------

**catcheR** is available at: https://github.com/alessandro-bertero/catcheR

The GitHub repository folder **scripts** contains all the bash and R scripts that can be run independently. However, for reproducible analyses, it is strongly recommended to install the catcheR package from GitHub, since its functions run inside Docker containers.

**Installation steps:**

1. Install the Docker engine  
   Follow the instructions at: https://docs.docker.com/engine/install/

2. Install ``catcheR``

   b. Install **catcheR** from GitHub:

   .. code-block:: R

      devtools::install_github("alessandro-bertero/catcheR")

   c. Install **rrundocker** from GitHub:

   .. code-block:: R

      devtools::install_github("Reproducible-Bioinformatics/rrundocker")

   d. Load **catcheR** and **rrundocker** in your R environment:

   .. code-block:: R

      library(catcheR)
      library(rrundocker)
      
      
      
Notes on obtaining iPS2-10X-seq, iPS2-CITE-seq, or iPS2-multi-seq Count Matrices
-----------------------------------------------------------------------

1. Download and install:

   - `cellranger <https://www.10xgenomics.com/support/software/cell-ranger/latest>`_  
     *(for iPS2-10X-seq and iPS2-CITE-seq)*

   - `cellranger-arc <https://www.10xgenomics.com/support/software/cell-ranger-arc/latest>`_  
     *(for iPS2-multi-seq)*

   Alternatively, Docker containers are available:

   - `cellranger v7 (Docker) <https://hub.docker.com/repository/docker/hedgelab/cellranger7hedge/general>`_
   - `cellranger v9 (Docker, recommended for iPS2-CITE-seq) <https://hub.docker.com/repository/docker/hedgelab/cellranger_9/general>`_
   - `cellranger-arc (Docker) <https://hub.docker.com/repository/docker/hedgelab/cellranger_atac/general>`_

2. For **iPS2-10X-seq** and **iPS2-CITE-seq**, demultiplex Illumina BCL files using ``cellranger mkfastq``, following the official 10X Genomics guide.  
   In the sample sheet CSV, include the index sequences used in :ref:`SupplementalProtocolThree` for:

   - GEX libraries  
   - UCI-BC libraries  
   - (optional) CMO and/or ADT libraries

3. For **iPS2-multi-seq**, use ``cellranger-arc mkfastq`` to demultiplex GEX + ATAC dual-index libraries.  
   Ensure the sample sheet is properly formatted for dual-modality runs and includes index sequences for both GEX and ATAC libraries.

4. Run ``FastQC`` to assess the quality of each FASTQ file per library type.

5. Generate cell-by-gene count matrices:

   - Use ``cellranger count`` for single-sample experiments
   - Use ``cellranger multi`` for multiplexed experiments (e.g., iPS2-CITE-seq)
   - For iPS2-multi-seq, use ``cellranger-arc count`` to obtain both GEX and ATAC matrices


      In multiplexed experiments (e.g., using CMO or ADT barcodes in iPS2-CITE-seq), individual sample matrices can be aggregated using ``cellranger aggr``.  
      This produces a unified dataset for joint analysis with ``catcheR_10Xcatch``, specifying the number of samples via the ``samples`` argument.

6. Use ``cellranger mat2csv`` to convert sparse matrix outputs into dense CSV files for downstream compatibility.  
   For iPS2-multi-seq, use ``cellranger-arc mat2csv`` separately for the GEX and ATAC outputs if needed.


Notes on gene annotation
----------------------------

After running catcheR and before the exploratory analysis, the gene expression matrix should be annotated with gene symbols using the `scannobyGtf <https://kendomaniac.github.io/rCASC/reference/scannobyGtf.html>`_ function from the R package `rCASC <https://kendomaniac.github.io/rCASC/articles/rCASC_vignette.html>`_.

As part of quality control, we recommend evaluating the fraction of ribosomal and mitochondrial reads — for example, using the `mitoRiboUmi <https://kendomaniac.github.io/rCASC/reference/mitoRiboUmi.html>`_ function from the same package — and considering the exclusion of cells with abnormally high proportions, which may indicate poor quality or stress.

.. note::

   After this step, the row names of the matrix (the genes) will have the following format:

   .. code-block:: text

      GeneSymbol:EnsemblID

   **Example:**

   .. code-block:: text

      ENSG00000000003:TSPAN6
