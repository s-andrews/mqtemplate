MaxQuant Automation for the capstone cluster
============================================

This repository contains scripts which can be used to automate the running of maxquant on a cluster.  It uses a series of template files generated in the maxquant interface to automatically create suitable ```mqpar.xml``` files for running in the cluster batch queue.

Accessing the software
----------------------

The scripts from this repository are accessed by running

```module load mqtemplate```

This will load the latest version of the modules, and will also load the appropriate version of maxquant


Running a search
----------------

Searches start from creating a directory and putting all of the RAW files to analyse in it.

The structure of the launch command is:

```
run_mq_template --template lfq --proteome mouse *raw
```

Where the template is the pre-prepared maxquant template XML file and the proteome is the fasta file of protein sequences against which to search.

We have provided some built-in templates and proteomes.  You can view these with:

```
$ run_mq_template --list_templates
Available templates
===================

lfq
```

Currently the only template is lfq (label free quantitation), but we expect to add one or more TMT templates in the near future

For proteomes you can do:

```
$ run_mq_template --list_proteomes
Available proteomes
===================

celegans       celegans_UP000001940_2024_04_10
human          human_UP000005640_2024_08_23
mouse          mouse_UP000000589_2024_08_23
scerevisiae    scerevisiae_UP000002311_2024_08_20
```

When specifying the proteome you can use either the short or long form of the name.  If you want to use a different proteome then you can also specify the path to a fasta file directly.  The file IDs in any custom file will need to be in uniprot format for the accession, name and description extraction to work correctly.

Search output
-------------

Running the ```run_mq_template``` command does not actually initiate a search.  Instead it writes out an ```mqpar.xml``` file into the directory containing the RAW files and will show the ```ssub``` command you should use to actually launch the search on the cluster.

```
$ run_mq_template --template lfq --proteome mouse *raw
Proteome file is /bi/apps/mqtemplate/latest/proteomes/mouse_UP000000589_2024_08_23.fa
Template file is /bi/apps/mqtemplate/latest/templates/lfq.xml
Writing mqpar to /bi/home/andrewss/MaxQuantTest/example/mqpar.xml
Command to start searching:

ssub -o mqcmd.log --cores=12 --mem=24G maxquant_cmd mqpar.xml
```

Copying and pasting the ssub command into your shell will start the search.

Cleaning up search results
--------------------------

After the search is complete MaxQuant will have generated a lot of files, most of which you won't use or need.  We therefore provide a script called ```clean_mq_results``` to remove the unwanted search results files.

To use this script just specify the directory containing the run you want to clean up (the directory containing the raw files)

```clean_mq_results /bi/home/andrewss/MaxQuantTest/example/```

If you're already in the directory you can just do

```clean_mq_results .```







