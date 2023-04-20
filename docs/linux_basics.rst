.. _linux_basics-page:

*******************
Linux Basics
*******************

Introduction to Linux
=================================

* `What is Linux?`_
* `What is a Command Line Interface (GUI)?`_
* `Practical part`_


What is Linux?
---------------------

.. image:: images/Linux-Logo.png
  :width: 400

Linux is an open-source operating system that was developed in the 1990s by `Linus Torvalds <https://en.wikipedia.org/wiki/Linus_Torvalds>`__. Unlike other popular operating systems like Windows and MacOS, Linux is free to download, use, and distribute.

One of the most distinctive features of Linux is that it is highly customizable. Users can choose from a wide variety of "distributions" (or "distros") of Linux, each with its own unique set of pre-installed software and user interface. Some popular distros include `Ubuntu <https://ubuntu.com/>`__, `Debian <https://www.debian.org/>`__, and `Fedora <https://fedoraproject.org/>`__.

Linux is known for being incredibly stable and secure. It is also highly efficient, which means it can run well even on older or less powerful hardware. This has made Linux a popular choice for servers and other industrial applications.

Because it is **open-source**, Linux has a large and active community of developers who are constantly working to improve the operating system. This means that there are always new updates and features being added to Linux, and users can be confident that their operating system is always up-to-date and secure.

Linux is a is widely used in **bioinformatics** due to its ability to handle large data sets and complex computational tasks. Bioinformaticians use Linux to analyze DNA sequences, study gene expression, and develop computational models of biological systems.

One of the most important features of Linux for bioinformatics is its support for powerful command-line tools. These tools allow bioinformaticians to perform complex data processing and analysis tasks with ease. In addition, Linux provides a rich array of programming languages and libraries that are essential for bioinformatics research, such as **Python**, **Java**, **Perl**, **C** and **C++**.

One of the key advantages of Linux for bioinformatics is its ability to run on a wide range of hardware, from powerful servers to small embedded devices. This makes it possible to run bioinformatics analyses on a variety of different systems, including **high-performance computing clusters**, **cloud-based platforms**, and **personal computers**.

Another advantage of Linux for bioinformatics is its strong focus on security and reliability. Because Linux is **open source**, the community is constantly working to improve the operating system and fix any security vulnerabilities that are discovered. This makes Linux a trusted platform for sensitive bioinformatics data.


What is a Command Line Interface (GUI)?
---------------------

.. image:: images/cli.png
  :width: 600
  
A command line interface (**CLI**) is a text-based interface used to interact with a computer's operating system or software by entering commands through a command prompt.

The command prompt usually consists of a text area where the user can enter a command, and the output of the command is displayed in the same area.

**Commands** can be entered using specific keywords or phrases, which are interpreted by the operating system or software.

For example, in the Windows command prompt, the user can type "**dir**" to list the files and directories in the current directory, and in the Unix/Linux command line, the user can type "**ls**" to achieve the same result.

In addition to simple commands, more complex operations can be performed by chaining commands together using special symbols.

Overall, command line interfaces offer a fast and powerful way to interact with a computer or software system, and are often preferred to graphical user interface (**GUI**) by experienced users or developers due to their flexibility and efficiency.

Practical part
----------------


Create directory and practice moving around
*******************

To create file and folders in linux is quite simple. You can use a number of programs for creating an empty file (**touch**) or an empty directory (**mkdir**)

.. code-block:: bash

  touch my_beautiful_file.txt

  mkdir my_beautiful_folder

To display the list of files and folder we can use the command **ls**

.. code-block:: bash

  ls
  my_beautiful_file.txt  my_beautiful_folder


To change the name of a file (or a directory) you can use the command **mv** while for copying the file you can use **cp**. Adding the option **-r** (recursive) to **cp** allows to copy a whole folder and its content. 

.. code-block:: bash

  mv my_beautiful_file.txt my_ugly_file.txt
  mv my_beautiful_folder my_ugly_folder

  cp my_ugly_file.txt my_beautiful_file.txt
  cp my_ugly_folder -r my_beautiful_folder

If you omit the **-r** option the system will complain

.. code-block:: bash

  cp my_ugly_folder my_other_folder


You can use **mv** also for moving a file (or a directory) inside a folder. Also **cp** will allow you to make a copy inside a folder.

.. code-block:: bash

  mv my_beautiful_file.txt my_beautiful_folder
  cp my_ugly_file.txt my_ugly_folder

  ls

  my_beautiful_folder  my_ugly_file.txt  my_ugly_folder


For entering in a folder we can use the tool **cd**

.. code-block:: bash

  cd my_ugly_folder

  ls

  my_ugly_file.txt



For going out we can move one level out 

.. code-block:: bash

  cd ../

  ls

  my_beautiful_folder  my_ugly_file.txt  my_ugly_folder


Sometimes we get lost and would like to know where we are. 


.. image:: images/lost.png
  :width: 600
  
We can use the command **pwd**.

We can write to a file using the character **>**, that means output redirection.

.. code-block:: bash

  echo "ATGTACTGACTGCATGCATGCCATGCA" > my_dna.txt


And display the content of the file using the program **cat**

.. code-block:: bash

  cat my_dna.txt

  ATGTACTGACTGCATGCATGCCATGCA


To convert this sequence to a RNA one we can just replace the **T** base with **U** by using the program **sed**. The sintax of this program is the following **s/<TO BE REPLACED>/<TO REPLACE>/**.


You can add a **g** at the end if you want to replace every character found **s/<TO BE REPLACED>/<TO REPLACE>/g**.

.. code-block:: bash

  sed s/T/U/g my_dna.txt > my_rna.txt

  cat my_rna.txt

  AUGUACUGACUGCAUGCAUGCCAUGCA


Every command has a manual, you can read it by using the program **man** with the name of the tool.

.. code-block:: bash

	man ls
	
	LS(1)                                                                   User Commands                                                                   LS(1)
	
	NAME
	      ls - list directory contents
	
	SYNOPSIS
	       ls [OPTION]... [FILE]...
	
	DESCRIPTION
	       List information about the FILEs (the current directory by default).  Sort entries alphabetically if none of -cftuvSUX nor --sort is specified.
	
	       Mandatory arguments to long options are mandatory for short options too.
	
	       -a, --all
	              do not ignore entries starting with .
	
	      -A, --almost-all
	              do not list implied . and ..
	
	      --author
	            with -l, print the author of each file
	
	     -b, --escape
	            print C-style escapes for nongraphic characters
	Manual page ls(1) line 1 (press h for help or q to quit)


**Recap**
*************

* **touch** writes empty files **mkdir** empty directories
* **mv** move files (or directory) or change their name
* **ls** list files and directories
* **cp** copy files and direcotries
* **cd** change the directory
* **echo** print values to standard output
* **cat** print the content of a file to standard output
* **sed** replace a string with another
* **man** print the manual for a function

Download files from repositories
--------------------------------

Several institutions host different kind of genomics data.


For example the genome browser `Ensembl <https://www.ensembl.org/index.html>`__ is also a public repository of genomes and annotation that can be freely downloaded and used for any kind of analysis

The resource `Ensembl Bacteria <https://bacteria.ensembl.org/index.html>`__ contains a large number of bacterial genomes and their annotation. As an example we can browse the page corresponding to `*Escherichia coli 'BL21-Gold(DE3)pLysS AG'* <https://bacteria.ensembl.org/Escherichia_coli_bl21_gold_de3_plyss_ag_/Info/Index/>`__

<img src="images/ensembl_escherichia.png" width="800"/>

We can click on "Download genes, cDNAs, ncRNA, proteins **FASTA**"

<img src="images/list_ensembl_escherichia.png" width="800"/>

And then on **DNA**

<img src="images/file_list_escherichia.png" width="800"/>

Then as an example we can use the copy the link address of the **README** file using the mouse right button.

<img src="images/right_click.png" width="800"/>

Then we can go back to our command line and use the program **wget** to download that file and using **CTRL+C** to paste the address:

.. code-block:: bash

	wget ftp://ftp.ensemblgenomes.org/pub/bacteria/release-42/fasta/bacteria_22_collection/escherichia_coli_bl21_gold_de3_plyss_ag_/dna/README

	--2019-03-06 18:59:13--  ftp://ftp.ensemblgenomes.org/pub/bacteria/release-42/fasta/bacteria_22_collection/escherichia_coli_bl21_gold_de3_plyss_ag_/dna/README
		   => ‘README’
	Resolving ftp.ensemblgenomes.org (ftp.ensemblgenomes.org)... 193.62.197.94
	Connecting to ftp.ensemblgenomes.org (ftp.ensemblgenomes.org)|193.62.197.94|:21... connected.
	Logging in as anonymous ... Logged in!
	==> SYST ... done.    ==> PWD ... done.
	==> TYPE I ... done.  ==> CWD (1) /pub/bacteria/release-42/fasta/bacteria_22_collection/escherichia_coli_bl21_gold_de3_plyss_ag_/dna ... done.
	==> SIZE README ... 4923
	==> PASV ... done.    ==> RETR README ... done.
	Length: 4923 (4.8K) (unauthoritative)

	100%[======================================================================================================================>] 4,923       --.-K/s   in 0s      
	
	2019-03-06 18:59:14 (295 MB/s) - ‘README’ saved [4923]



we can then use the program **more** to display part of the content of the file:

```{bash}
	more README


	#### README ####

	IMPORTANT: Please note you can download correlation data tables,
	supported by Ensembl, via the highly customisable BioMart and
	EnsMart data mining tools. See http://www.ensembl.org/biomart/martview or
	http://www.ebi.ac.uk/biomart/ for more information.

	The genome assembly represented here corresponds to  
	GCA_000023665.1

	#######################
	Fasta DNA dumps
	#######################

	-----------
	FILE NAMES
	------------
	The files are consistently named following this pattern:
	   <species>.<assembly>.<sequence type>.<id type>.<id>.fa.gz

	<species>:   The systematic name of the species.
	<assembly>:  The assembly build name.
	<sequence type>:
	 * 'dna' - unmasked genomic DNA sequences.
	--More--(14%)


Pressing the bar allows us to scroll down the file, while for exiting you just click **CTRL+C**.<br>
After reading the README we can download the file named **toplevel** that contains chromosomes, regions not assembled into chromosomes and N padded haplotype/patch regions:

.. code-block:: bash

	wget ftp://ftp.ensemblgenomes.org/pub/bacteria/release-42/fasta/bacteria_22_collection/escherichia_coli_bl21_gold_de3_plyss_ag_/dna/Escherichia_coli_bl21_gold_de3_plyss_ag_.ASM2366v1.dna.toplevel.fa.gz
			```

We can use the options **-lh** of the program **ls** to list attributes of the files and show in human readable format the size fo the files

.. code-block:: bash

	ls -lh

	total 2.0M
	drwxr-xr-x 5 lcozzuto Bioinformatics_Unit  209 Mar  7 11:48 advanced_linux_2019
	-rw-r--r-- 1 lcozzuto Bioinformatics_Unit 1.4M Mar  7 13:06 Escherichia_coli_bl21_gold_de3_plyss_ag_.ASM2366v1.dna.toplevel.fa.gz
	drwxr-xr-x 2 lcozzuto Bioinformatics_Unit   39 Mar  6 18:17 my_beautiful_folder
	-rw-r--r-- 1 lcozzuto Bioinformatics_Unit    0 Mar  6 18:15 my_ugly_file.txt
	drwxr-xr-x 2 lcozzuto Bioinformatics_Unit   34 Mar  6 18:17 my_ugly_folder
	-rw-r--r-- 1 lcozzuto Bioinformatics_Unit 4.9K Mar  6 18:59 README


For unzipping the file we can use the program **gunzip**. <br>The uncompressed file is now **4.5M**. 

Let's see the content of the file.

.. code-block:: bash

	more Escherichia_coli_bl21_gold_de3_plyss_ag_.ASM2366v1.dna.toplevel.fa 

	>CP001665 dna:supercontig supercontig:ASM2366v1:CP001665:1:4570938:1 REF
	CGTCCTGGATCTTTATTAGATCGATTAAGCCAATTTTTGTCTATGGTCATTAAATTTTCC
	AATATGCGGCGTAAATCGTGCCCGCCTCGCGGCAGGATCGTTTACACTTAGCGAGTTCTG
	GAAAGTCCTGTGGATAAATCGGGAAAATCTGTGAGAAACAGAAGATCTCTTGCGCAGTTT
	AGGCTATGATCCGCGGTCCCGATCGTTTTGCAGGATCTTGATCGGGCATATAACCGCAGA
	CAGCGGTTCGTGCGTCACCCTCAAGCAGGGTCTTTTCGACGTACGTCAACAATCATGAAT
	GTTTCAGCCTTAGTCATTATCGACTTTTGTTCGAGTGGAGTCCGCCGTGTCACTTTCGCT
	TTGGCAGCAGTGTCTTGCCCGATTGCAGGATGAGTTACCAGCCACAGAATTCAGTATGTG
	GATACGCCCATTGCAGGCGGAACTGAGCGATAACACGCTGGCCCTGTACGCGCCAAACCG
	TTTTGTCCTCGATTGGGTACGGGACAAGTACCTTAATAATATCAATGGACTGCTAACCAG
	TTTCTGCGGAGCGGATGCCCCACAGCTGCGTTTTGAAGTCGGCACCAAACCGGTGACGCA
	AACGCCACAAGCGGCAGTGACGAGCAACGTCGCGGCCCCTGCACAGGTGGCGCAAACGCA
	GCCGCAACGTGCTGCGCCTTCTACGCGCTCAGGTTGGGATAACGTCCCGGCCCCGGCAGA
	ACCGACCTATCGTTCTAACGTAAACGTCAAACACACGTTTGATAACTTCGTTGAAGGTAA
	ATCTAACCAACTGGCGCGCGCGGCGGCTCGCCAGGTGGCGGATAACCCTGGCGGTGCCTA
	TAACCCGTTGTTCCTTTATGGCGGCACGGGTCTGGGTAAAACTCACCTGCTGCATGCGGT
	GGGTAACGGCATTATGGCGCGCAAGCCGAATGCCAAAGTGGTTTATATGCACTCCGAGCG
	CTTTGTTCAGGACATGGTTAAAGCCCTGCAAAACAACGCGATCGAAGAGTTTAAACGCTA
	CTACCGTTCCGTAGATGCACTGCTGATCGACGATATTCAGTTTTTTGCTAATAAAGAACG
	ATCTCAGGAAGAGTTTTTCCACACCTTCAACGCCCTGCTGGAAGGTAATCAACAGATCAT
	TCTCACCTCGGATCGCTATCCGAAAGAGATCAACGGCGTTGAGGATCGTTTGAAATCCCG
	CTTCGGTTGGGGACTGACTGTGGCGATCGAACCGCCAGAGCTGGAAACCCGTGTGGCGAT
	CCTGATGAAAAAGGCCGACGAAAACGACATTCGTTTGCCGGGCGAAGTGGCGTTCTTTAT
	CGCCAAGCGTCTACGATCTAACGTACGTGAGCTGGAAGGGGCGCTGAACCGCGTCATTGC


The file contains the whole genome of the bacteria.


The first line contains the character **>** and the name of the molecule / genome.


This format is called `**FASTA** <https://en.wikipedia.org/wiki/FASTA_format>`__ format and is universally used for storing one or multiple DNA/RNA/Protein sequences.

We can now download in the same ways the proteins:

<img src="images/righ_click_2.png" width="800"/>

and after unzipping the file we can have a look at it.

.. code-block:: bash

	more Escherichia_coli_bl21_gold_de3_plyss_ag_.ASM2366v1.pep.all.fa

	>ACT27082 pep supercontig:ASM2366v1:CP001665:347:1750:1 gene:ECBD_0001 transcript:ACT27082 gene_biotype:protein_coding transcript_biotype:protein_coding descrip
	tion:chromosomal replication initiator protein DnaA
	MSLSLWQQCLARLQDELPATEFSMWIRPLQAELSDNTLALYAPNRFVLDWVRDKYLNNIN
	GLLTSFCGADAPQLRFEVGTKPVTQTPQAAVTSNVAAPAQVAQTQPQRAAPSTRSGWDNV
	PAPAEPTYRSNVNVKHTFDNFVEGKSNQLARAAARQVADNPGGAYNPLFLYGGTGLGKTH
	LLHAVGNGIMARKPNAKVVYMHSERFVQDMVKALQNNAIEEFKRYYRSVDALLIDDIQFF
	ANKERSQEEFFHTFNALLEGNQQIILTSDRYPKEINGVEDRLKSRFGWGLTVAIEPPELE
	TRVAILMKKADENDIRLPGEVAFFIAKRLRSNVRELEGALNRVIANANFTGRAITIDFVR
	EALRDLLALQEKLVTIDNIQKTVAEYYKIKVADLLSKRRSRSVARPRQMAMALAKELTNH
	SLPEIGDAFGGRDHTTVLHACRKIEQLREESHDIKEDFSNLIRTLSS
	>ACT27083 pep supercontig:ASM2366v1:CP001665:1755:2855:1 gene:ECBD_0002 transcript:ACT27083 gene_biotype:protein_coding transcript_biotype:protein_coding descri
	ption:DNA polymerase III, beta subunit
	MKFTVEREHLLKPLQQVSGPLGGRPTLPILGNLLLQVADGTLSLTGTDLEMEMVARVALV
	QPHEPGATTVPARKFFDICRGLPEGAEIAVQLEGERMLVRSGRSRFSLSTLPAADFPNLD
	DWQSEVEFTLPQATMKRLIEATQFSMAHQDVRYYLNGMLFETEGEELRTVATDGHRLAVC
	SMPIGQSLPSHSVIVPRKGVIELMRMLDGGDNPLRVQIGSNNIRAHVGDFIFTSKLVDGR
	FPDYRRVLPKNPDKHLEAGCDLLKQAFARAAILSNEKFRGVRLYVSENQLKITANNPEQE
	EAEEILDVTYSGAEMEIGFNVSYVLDVLNALKCENVRMMLTDSVSSVQIEDAASQSAAYV
	VMPMRL
	>ACT27084 pep supercontig:ASM2366v1:CP001665:2855:3928:1 gene:ECBD_0003 transcript:ACT27084 gene_biotype:protein_coding transcript_biotype:protein_coding descri
	ption:DNA replication and repair protein RecF
	MSLTRLLIRDFRNIETADLALSPGFNFLVGANGSGKTSVLEAIYTLGHGRAFRSLQIGRV
	IRHEQEAFVLHGRLQGEERETAIGLTKDKQGDSKVRIDGTDGHKVAELAHLMPMQLITPE
	GFTLLNGGPKYRRAFLDWGCFHNEPGFFTAWSNLKRLLKQRNAALRQVTRYEQLRPWDKE
	--More--(0%)


We see that many protein sequences are embedded in the files and separated by their name, always preceded by the character **">"**.

To know how many sequences are in the files we can use the program **grep** with the option **-c** for counting the number of rows containg the character **">"**:


.. code-block:: bash

	grep ">" -c Escherichia_coli_bl21_gold_de3_plyss_ag_.ASM2366v1.pep.all.fa
	4228


**Recap**
***********

* **wget** downloads file from a URL
* **more** prints a part of the content of a file in interactive way
* **grep** extract the rows containing a particular character / pattern.


<h3>Next Session</h3>

[Manipulate files, piping, parsing, reformatting](https://biocorecrg.github.io/advanced_linux_2019/parsing)

