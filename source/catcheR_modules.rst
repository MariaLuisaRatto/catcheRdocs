catcheR_modules - Gene Modules
====================================

The ``catcheR_modules`` function uses Monocle to identify **gene modules**—groups of genes whose expression varies across different perturbation groups (e.g., genes, shRNAs, or clones). These modules can be used to further analyze expression trends and cumulative frequency distributions.

Step-by-step
------------

#. Run ``catcheR_modules``:

   .. code-block:: r

      catcheR_modules(
          group = c("docker", "sudo"),
          folder,
          cds,
          resolution = 1e-2
      )

**Example usage:**

.. code-block:: r

   catcheR_modules(
       group = "docker",
       folder = "/30tb/3tb/data/ratto/testing/",
       cds = "processed_cds.RData"
   )

The ``resolution`` parameter is passed to Monocle’s ``find_gene_modules`` function.  
- A **higher resolution** will return **more, smaller modules**  
- A **lower resolution** will return **fewer, broader modules**

Outputs
-------

``catcheR_modules`` generates the following outputs:

#. ``gene_modules.csv``  
   - Lists the genes belonging to each identified module.

#. Heatmap plots  
   - Show Z-scores of module expression across perturbation groups.  
   - Includes either **all modules** or the **top 10 most variable modules**.
   
   ..image:: pheatmap_gene_module_top10.pdf
   
   ..image:: pheatmap_shRNA_module_top10.pdf

#. ``modules_cells/`` folder  
   - Contains tables with **aggregated expression values per cell** for each gene module.  
   - These CSV files can be used as input to ``catcheR_pseudotime``, allowing the analysis of cumulative frequency based on module expression rather than pseudotime.

Integration with Pseudotime
---------------------------

To analyze cumulative module expression trends, you can run ``catcheR_pseudotime`` using the CSV files in the ``modules_cells`` folder as pseudotime input.
