# Single-Cell RNA Sequencing Workshop: Analysis of Ileum Tissue and Organoids

Welcome to the Single-Cell RNA Sequencing (scRNA-seq) Workshop repository. This workshop is designed to provide students with a brief handson experience in analyzing and interpreting single-cell data using tools such as **Seurat** and **Monocle3** in R. It represents a snippet of the workflow that I learned, applied and validated during my thesis. The workshop focuses on data integration, clustering, differential gene expression analysis, and trajectory analysis to compare ileum tissue and organoid samples. It also includes some theory behind the methodology, as I believe it is important to understand what you are doing and why using a particular tool makes sense. After all, there is no artistry in simply pressing a button and getting results.

## Table of Contents
- [Introduction](#introduction)
- [Workshop Objectives](#workshop-objectives)
- [Requirements and Installation](#Requirements-and-Installation)
- [Folder Structure and Files](#folder-structure-and-files)
- [Setting Up the Conda Environment](#setting-up-the-conda-environment)
- [Setting Up the R Kernel](#setting-up-the-r-kernel)
- [Setting Up Jupyter Notebook](#setting-up-jupyter-notebook)
- [How to Use This Repository](#how-to-use-this-repository)
- [Exercises](#exercises)
- [Output Management](#output-management)

## Introduction

This workshop will guide participants through the steps needed to analyze single-cell RNA sequencing data, starting from data integration and clustering to advanced analyses like trajectory inference. By the end of the workshop, participants will have an basic understanding of how to process and interpret scRNA-seq data, specifically focusing on ileum tissue and organoid samples.

## Workshop Objectives
Participants will learn how to integrate scRNA-seq data from multiple replicates to reduce batch effects. They will perform cell clustering and annotate cell types based on transcriptional profiles, understand and apply differential gene expression analysis to identify cell-specific markers, and use **Monocle3** to infer developmental trajectories and study gene expression patterns over pseudotime. Additionally, participants will compare the transcriptional landscape of ileum tissue and organoid samples to identify key similarities and differences. Emphasis will be placed on the absorptive lineage throughout this entire workshop.

## Requirements and Installation
The specific libraries for this workshop are explained within the Jupyter Notebook. The necessary packages are listed in the `workshop_requirements.txt` file, which should be used to set up a **Conda environment** on the cluster. Once the environment is set up, all software, packages, kernels, and Jupyter Notebook dependencies will be installed.

To begin, clone this repository by running the following commands:
```sh
git clone 
```
Or copy the **for_Ole** folder from `/lustre/shared/for_Ole`, which contains all the files that you need. All relevant workshop files are located in the following directory on the cluster as follows:
```text
/lustre/shared/for_Ole
├── rscript
│   ├── data_workshop
│   │   ├── ileum_tissue
│   │   ├── organoid.seurat
│   │   ├── organoid.cds
│   │   └── (additional data files)
│   ├── FAANG_RMD.Rmd
│   ├── FAANG.ipynb
│   ├── Figures
│   │   └── (markdown figures for notebook explanations)
│   ├── generated_Figures
│   │   └── (temporary folder to render figures)
│   └── R_session
│       └── (backup R session files)
├── workshop_requirements.txt
```
	
	•	rscript/data_workshop: Contains raw data files, including ileum_tissue, organoid.seurat, and organoid.cds objects for analysis.
	•	FAANG_RMD.Rmd: Backup R Notebook for local use.
	•	FAANG.ipynb: The main Jupyter Notebook for the workshop.
	•	Figures: Contains markdown figures used in notebook explanations.
	•	generated_Figures: A temporary folder for rendering figures during the workshop.
	•	R_session: Stores backup R session files to help recover the environment if the kernel crashes.
	•	workshop_requirements.txt: A file listing all required packages for setting up the Conda environment.

**Please do not change anything within this folder.**

## Setup Conda environment

This workshop is made to run on a Conda environment containing all the crucial packages for R, kernels and Jupyter. Here, i assume that you already have Anaconda or Miniconda installed. In case you don’t have it yet, then follow the steps below:

**Anaconda/Miniconda3 installation:**

[Download the Anaconda installer form the website](https://repo.anaconda.com/archive/Anaconda3-2024.02-1-Linux-x86_64.sh) or get the latest version using the line below.
```sh
wget "https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh"
```

**Setting up the environment for the pipeline:**

**1.** Now with Conda installed and update it is time to setup the environment that is required for the R packages.
To install the essential packages and tools, it is important to set your Conda channels in the following order:
```sh
conda config --add channels pwwang
conda config --add channels dnachun
conda config --add channels defaults
conda config --add channels GenomeDK
conda config --add channels r
conda config --add channels bioconda
conda config --add channels conda-forge
```

**2.** Note that the channels you added first end up in the bottom for lower priority. This is intentional!
Make sure that the these channels are the only ones active. If you have any other channels, you should first remove them as stated below. But first check channels you currently have:
```sh
conda config --show channels
```
If there channels we do not want, then remove them:
```sh
conda config --remove channels <name of channel>
```
Then add the required channels as stated before (in the correct order!)

**3.** The requirements are provided in the **"workshop_requirements.txt"** that you downloaded form the Github page or the `/lustre/shared/for_Ole` folder. Make a new environment and install all the packages:
```sh
conda create --name sc_FAANG --file workshop_requirements.txt
```
```sh
conda create --name sc_FAANG --file workshop_requirements.txt
```

