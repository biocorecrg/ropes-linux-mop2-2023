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


Recap
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

