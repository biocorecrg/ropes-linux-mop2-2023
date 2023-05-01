.. _mop2_practical-page:

*******************
Direct RNA Sequencing data analysis with Master of Pores 2
*******************

Basics of Direct RNA Sequencing (DRS)
---------------------
Direct RNA sequencing is a technology developed by Oxford Nanopore Technologies (ONT) that allows the sequencing of native RNA molecules without the need of previous amplification nor fragmentation. Therefore, it provides information about the presence of RNA modifications, polyA tail length and composition at per-read level. 

This method is based on the use of protein nanopores that are embedded into a membrane. As the molecule goes through the pore, it alters the ionic current that is applied to it. These changes are then stored in **fast5 files**.

.. image:: images/Pore.png
  :width: 700

DRS data analysis with Master Of Pores 2 (MOP2)
---------------------

The complexity of analysing current intensity data together with the lack of systematic and reproducible pipelines have hindered the access of this technology to the general users. To overcome this limitation, we developed Master Of Pores 2 (MOP2) - a nextflow based workflow that simplifies the analysis of DRS datasets and aims to make it accessible to non-bioinformatic experts. 

MOP2 can perform all steps required to analyse DRS data - from converting raw current intensities into multiple types of processed data to RNA modified sites detection and polyA tail length predictions. This pipeline consists of four modules: *mop_preprocess*, *mop_tail*, *mop_mod* and *mop_consensus*.

.. image:: images/MOP_scheme.png
  :width: 700

Basic preprocessing (module: *mop_preprocessing*)
......................

The pre-processing module is able to perform base-calling, mapping (either to a genome or a transcriptome), feature counting (per-gene level) and quality control. Furthermore, if required by the user, it can run demultiplexing, filtering and discovery of novel transcripts. As the final step, the workflow generates a final report of the performance and results of each of the steps performed. 

.. note::
  Before proceeding to any other downstream analysis such as RNA modification detection and polyA tail analysis, this module **must** be executed. 
  
**Analysis overview:**

- **Step 1a: Basecalling**

  It is the process by which the current intensity data is translated into a nucleotide sequence by a machine learning algorithm called basecaller. Currently, the most widely used is **Guppy**, which was developed by ONT and it is only available if you are part of the ONT community. The model that Guppy uses to analyse RNA data is not modification aware and therefore, it can only identify the four canonical bases (A, U, C and G).
  
  - **Input:** Raw fast5 files
  - **Output:** Basecalled fast5 and fastq files
  
- **Step 1b: Demultiplexing**
  
  Demultiplexing is required when analysing a barcoded sample; otherwise, this step should be skipped. Here, **Deeplexicon** is used. This algorithm converts the barcode's signal into an image, which is then classified based on a machine-learning approach.
  
  - **Input:** Raw fast5 files
  - **Output:** Demuxed raw fast5 files
  
  
- **Step 2: Filtering**
  
  Filter out reads based on either quality and/or length performed by **Nanofilt**. For RNA modification detection using DRS data, this step should be turned off as modified reads tend to have lower quality than unmodified ones and thus, filtering based on quality would bias the results.
  
  - **Input:** Fastq files
  - **Output:** Filtered fastq files

- **Step 3: Alignment**
  
  Mapping step performed by either **minimap2** or **grapmap**. Both can perform spliced or unspliced alignments. Briefly, we would use spliced alignments when using a genome as a reference and; unspliced for transcriptome. Furthermore, it has been reported that minimap2 fails to align highly modified reads and thus, it should not be used to analyse data from highly modified RNA species such as rRNAs. 
  
  - **Input:** Fastq files and reference file (genome or transcriptome)
  - **Output:** Bam (and bai) files
  
- **Step 4: Feature counts**
  
  The software run by MOP2 to perform this step depends on the type of reference used in the mapping step. For transcriptome alignments, **NanoCount** is used and it reports per transcript abundances whereas for genome alignments, **htseq-count** is executed and it generates per-gene counts. 
  
  - **Input:** Reference and alignment file (bam) for NanoCount // Reference, annotation (*.gtf) and alignment files (*.bam) for htseq-count
  - **Output:** Transcript abundances' estimations // Per-gene counts

- **Step 5: Transcript discovery**

  **Bambu** aims to identify novel transcripts from mapped reads. For more information about how to use this tool, please visit its `GitHub page <https://github.com/GoekeLab/bambu#General-Usage>`_.
  
  - **Input:** Alignment (.bam), reference (.fa) and annotation (.gtf)
  - **Output:** Transcript's abundances and read id-transcript assignments
  
- **Step 6: Reporting and quality control**

  **multiQC** produces the final report, as a html page, which contains the quality control's results generated by **MinionQC** together with the stats from the previous executed steps. 
  
  - **Input:** all inputs and outputs
  - **Output:** final report

We will show how to use and configurate this module in the next hands-on exercise. 

PolyA tail length analysis (module: *mop_tail*)
......................

This module estimates poly(A) tail length at read level provided by **Tailfindr** and/or **Nanopolish**. This workflow uses as input all the files generated by *mop_preprocess*. 

**Analysis overview:**

- **Software 1: Tailfindr**
  
  - **Input:** Basecalled fast5 files and read id-gene assignments
  - **Output:** PolyA tail length estimations

- **Software 2: Nanopolish**

  - **Input:** Fastq (.fq.gz), alignment (.bam) and reference (.fa) files
  - **Output:** PolyA tail length estimations


Hands-on 1: *mop_preprocess* and *mop_tail*
---------------------

For installing the MoP2 pipeline and downloading guppy 3.4.2, please use the code below:

.. code-block:: console

  git clone --depth 1 --recurse-submodules https://github.com/biocorecrg/MOP2.git
  
  cd MoP2; bash INSTALL.sh

For this hands-on exercise, we will analyse several total RNA DRS samples from *Saccharomyces cerevisiae*:

- Sample 1: snR36 knock-out strain
- Samples 2, 3 and 4: wild-type strains
