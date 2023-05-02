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

Master Of Pores 2 (MOP2) - preprocessing and polyA tail estimation (1)
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

.. tip::
  **How do we know if fast5 files are bassecalled or not?**

  Raw and basecall fast5 files have the same extension (.fast5) and in consequence, the only way of knowing if a fast5 file is basecalled or not is to check its contents. Please use the code below:
  
  .. code-block:: console

    h5ls /path/to/fast5 | head -n15
  
  
  .. |raw| image::
    :alt: Missing raw fast5

  .. |basecalled| image:: 
    :alt: Missing basecalled fast5
    
  .. list-table::
   :widths: 10 100
   :header-rows: 1

   * - Raw
     - Basecalled

   * - |raw|
     - |basecalled|

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

MOP2 installation and data preprocessing:
......................

For installing the MoP2 pipeline and downloading guppy 3.4.2, please use the code below:

.. code-block:: console

  git clone --depth 1 --recurse-submodules https://github.com/biocorecrg/MOP2.git
  
  cd MoP2; bash INSTALL.sh

For this hands-on exercise, we will perform polyA tail length estimation and RNA modification detection on total RNA DRS samples from *Saccharomyces cerevisiae* (see list below):

- Sample 1: snR36 knock-out strain
- Samples 2, 3 and 4: wild-type strains

Before setting up *mop_preproceess* module, it is important that you think about which softwares and parameters should be used - otherwise you might run analysis that are not suitable to your sample (and you will lose time and resources). Please, answers the questions below:

- **Question 1:** Which is the most abundant RNA specie in your samples? Is it highly or lowly modified?

- **Question 2:** Which reference would you use (genome or transcriptome)? 

- **Question 3:** Would you use spliced or unspliced alignment? Why?

- **Question 4:** Which counter would you use? Why?


Now, we can start setting up the *mop_preproceess* module. Please follow the code below:

.. code-block:: console

  #Enter the mop_preprocess directory:
  cd mop_preprocess
  
  #List all files and directories:
  ls -l 
  
  #Summary of files:
  ## bin directory: it contains all the binaries used by this module. If you wanna change guppy version, you should go here.
  ## *_opt.tsv files: it is used to input additional parameters to the individual softwares executed by the workflow.
  ## params.config file: it is the file that the user must edit to introduce the inputs required by the workflow.
  
  #Edit params.config file:
  nano params.config
  
  #Params.config content:
  params {
    conffile            = "final_summary_01.txt"
    fast5               = "/home/andelgado/Documents/cluster/users/andelgado/ROPES_training/data_mod_consensus/**/*.fast5"
    fastq               = ""

    reference           = "/home/andelgado/Documents/software/NanoConsensus/ref/Saccharomyces_cerevisiae.rRNA.fa"
    annotation          = ""
    ref_type            = "transcriptome"

    pars_tools          = "drna_tool_unsplice_opt.tsv" 
    output              = "$baseDir/output"
    qualityqc           = 5
    granularity         = 1

    basecalling         = "guppy"
    GPU                 = "OFF"
    demultiplexing      = "NO"
    demulti_fast5       = "NO" 

    filtering           = "NO"

    mapping             = "graphmap"
    counting            = "nanocount"
    discovery           = "NO"

    cram_conv           = "NO"
    subsampling_cram    = 50

    saveSpace           = "NO"

    email               = "username@domain"
  }
  
  #Save file and exit:
  CTRL+o
  CTRL+x

As discussed earlier, these options are okay when analysing total RNA samples. However, depending on the type of sample, changes in the params.config file should be made. Click `here <https://biocorecrg.github.io/MOP2/docs/mop_preprocess.html>`_ to check all parameters accepted by *mop_preprocess*.

.. code-block:: console

  #Run the module in the background, with singularity and in the local computer:
  nextflow run mop_preprocess.nf -with-singularity -bg -profile local > log_preprocess.txt
  
Results
......................

Once the module has finished, these directories should be in your output folder:

- **fast5_files**: Contains the basecalled fast5 files.

- **fastq_files**: Contains one or, in case of demultiplexing, more fastq files.

- **QC_files**: Contains each single QC produced by the pipeline.

- **alignment**: Contains the bam and bai file(s).

- **counts**: Contains read counts per gene / transcript.

- **assigned**: Contains assignment of each read to a given gene / transcript.

- **report**: Contains the final multiqc report.

Now, we would look at the alignments in IGV (genome browser) together with the stats reported in the multiQC html to decide if we have enough quality data to proceed with the polyA tail length estimation and RNA modification detection analysis. Due to time limitations, here you should decide if we can proceed or not only based on the multiQC report.

- **Question 5:** Do we have enough data in all samples to proceed to the downstream analysis? Why? 

PolyA tail length estimation
......................

After preprocessing the data, we can go directly to run the *mop_tail* module which will output polyA tail length estimation at per read level. Please run the code below:

.. code-block:: console

  #Go to the directory:
  cd ./../mop_tail/
  
  #Edit params.config file:
  nano params.config
  
  #Params.config content:
  params {
    
    input_path         = "$baseDir/../mop_preprocess/output/"
    reference           = "/home/andelgado/Documents/software/NanoConsensus/ref/Saccharomyces_cerevisiae.rRNA.fa"

    pars_tools         = "$baseDir/tools_opt.tsv"

    output             = "$baseDir/outputPoly"

    tailfindr          = "YES"
    nanopolish         = "YES"
 
    email              = "username@domain"
  }
  
  #Save file and exit:
  CTRL+o
  CTRL+x

  #Run the module in the background, with singularity and in the local computer:
  nextflow run mop_tail.nf -with-singularity -bg -profile local > log_tail.txt
  
Results
......................

Once the module has finished, these directories should be in your output folder:

- **nanopolish_flow**: Contains nanopolish's results.

- **tailfindr_flow**: Contains tailfindr's results.

- **polya_common**: Contains the text files that include the combined polyA tail length predictions at per read-level. 

Check the generated files and answer these questions below:

- **Question 6:** Which predictions would you trust? Why? 

- **Question 7:** Should we have done this analysis? Why? 

Master Of Pores 2 (MOP2) - RNA modification detection (2)
---------------------

Hands-on 2: *mop_mod* and *mop_consensus*
---------------------
