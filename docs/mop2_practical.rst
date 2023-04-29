.. _mop2_practical-page:

*******************
Direct RNA Sequencing data analysis with Master of Pores 2
*******************

Basics of Direct RNA Sequencing (DRS)
---------------------
Direct RNA sequencing is a technology developed by Oxford Nanopore Technologies (ONT) that allows the sequencing of native RNA molecules without the need of previous amplification nor fragmentation. Therefore, it provides information about the presence of RNA modifications, polyA tail length and composition at per-read level. 

This method is based on the use of protein nanopores that are embedded into a membrane. As the molecule goes through the pore, it alters the ionic current that is applied to it. These changes are then stored in **fast5 files**.

TO ADD PORE HERE

DRS data analysis with Master Of Pores 2 (MOP2)
---------------------

The complexity of analysing current intensity data together with the lack of systematic and reproducible pipelines have hindered the access of this technology to the general users. To overcome this limitation, we developed Master Of Pores 2 (MOP2) - a nextflow based workflow that simplifies the analysis of DRS datasets and aims to make it accessible to non-bioinformatic experts. 

MOP2 can perform all steps required to analyse DRS data - from converting raw current intensities into multiple types of processed data to RNA modified sites detection and polyA tail length predictions. This pipeline consists of four modules: *mop_preprocess*, *mop_tail*, *mop_mod* and *mop_consensus*.

TO ADD MOP2 SCHEME

Basic preprocessing (module: *mop_preprocessing*)
......................

In order to analyse this type of data, and before proceeding to any other downstream analysis such as RNA modification detection and polyA tail analysis, the steps below must be performed:

- **Step 1: Basecalling**

  It is the process by which the current intensity data is translated into a nucleotide sequence by a machine learning algorithm called basecaller. Currently, the most widely used is **Guppy**, which was developed by ONT and it is only available if you are part of the ONT community. The model that Guppy uses to analyse RNA data is not modification aware and therefore, it can only identify the four canonical bases (A, U, C and G). 
  
- **Step 2: Alignment**
  
  XXX
  
- **Step 3: Counting and read-gene assignment**
  
  XXX


PolyA tail length analysis (module: *mop_tail*)
......................


Hands-on 1: *mop_preprocess* and *mop_tail*
---------------------

For installing the MoP2 pipeline and downloading guppy 3.4.2, please use the code below:

.. code-block:: console

  git clone --depth 1 --recurse-submodules https://github.com/biocorecrg/MOP2.git
  
  cd MoP2; bash INSTALL.sh

For this hands-on exercise, we will analyse several total RNA DRS samples from *Saccharomyces cerevisiae*:

- Sample 1: snR36 knock-out strain
- Samples 2, 3 and 4: wild-type strains




What are containers?
.....................

.. image:: https://lifeboat.com/images/layout/lightbox_mask.png
  :width: 700
