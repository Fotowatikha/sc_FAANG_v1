# Single-Cell RNA Sequencing Workshop: Analysis of Ileum Tissue and Organoids

Welcome to the Single-Cell RNA Sequencing (scRNA-seq) Workshop repository. This workshop is designed to provide students with a brief handson experience in analyzing and interpreting single-cell data using tools such as **Seurat** and **Monocle3** in R. It represents a snippet of the workflow that I learned, applied and validated during my thesis. The workshop focuses on data integration, clustering, differential gene expression analysis, and trajectory analysis to compare ileum tissue and organoid samples. It also includes some theory behind the methodology, as I believe it is important to understand what you are doing and why using a particular tool makes sense. After all, there is no artistry in simply pressing a button and getting results (in my opinion...).

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
	
- **rscript/data_workshop:** Contains raw data files, including ileum_tissue, organoid.seurat, and organoid.cds objects for analysis.
- **FAANG_RMD.Rmd:** Backup R Notebook for local use.
- **FAANG.ipynb:** The main Jupyter Notebook for the workshop.
- **Figures:** Contains markdown figures used in notebook explanations.
- **generated_Figures:** A temporary folder for rendering figures during the workshop.
- **R_session:** Stores backup R session files to help recover the environment if the kernel crashes.
- **workshop_requirements.txt:** A file listing all required packages for setting up the Conda environment.

**Please do not change anything within this folder!!!.**

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

## Setting Up the R Kernel
Before starting the Jupyter Notebook, you need to set up the R kernel. Follow these steps:

**1.** With your Conda environment set, you should run:
```sh
conda activate sc_FAANG 
```

**2.** Start an R session by typing `R` in the command line:
```sh
R
```

**3.** Install the R kernel by running:
```r
IRkernel::installspec(user = TRUE) 
```
You only need to do this one, as this will register the available R kernels, including the community-made **xeus-R kernel** that supports interactive R sessions.
Note that in this first revision, we will not yet leverage the xeus-R kernel, as this requires PuTTY (Windows) or X11 (Mac) on the user's end. (**Please decide if you want this to be part of the workshop, as it allows for an interactive session where users can select their starting nodes during the pseudotime analysis. If this is too complicated, we can instead display several nodes on the UMAP and allow students to select their nodes of choice by interacting with the code.**)

**4.** Close the R session by typing:
```r
q()
```

The R kernel will now be available for use in Jupyter Notebook, allowing for interactive sessions in **Monocle3** when combined with the following configuration:
```r
options(browser="firefox")
```
This configuration is required for running interactive modes with **Putty (Windows)** or **X11 forwarding (MacOS)** that enables browser-based outputs. Firefox is included in the Conda environment.

## Setting Up Jupyter Notebook

To run the workshop, you need to set up Jupyter Notebook on your HPC (High-Performance Computing) cluster and connect to it from your local machine. Follow these steps for a smooth setup:

**1.** Start Jupyter Notebook on the HPC (You could also submit it as a SLURM to an external cluster with right resources):
```sh
jupyter notebook --no-browser --ip=0.0.0.0 --port=8890
```
The `--no-browser` esnures that Jupyter does not to open any browser on the HPC, and `--ip=0.0.0.0` option allows remote access. The `--port=8890` specifies the port Jupyter will run on. Make sure to choose a port that is not already in use.

**2.** Then Connect to Jupyter Notebook from your local machine using SSH tunneling:
```sh
ssh -NfL localhost:8890:localhost:8890 username@login.anunna.wur.nl
```
The `-NfL` flags create a secure tunnel from your local machine to the HPC without opening an interactive shell. This forwards the port `8890` on your local machine to the same port on the HPC.

**3.** Access Jupyter Notebook in your web browser:

Copy the URL provided by the Jupyter Notebook command (e.g., `http://127.0.0.1:8888/?token=...`) and paste it into your local web browser (see figure below). This will open Jupyter Notebook and allow you to interact with it as if it were running on your local computer.

<img width="1245" alt="jupytr" src="https://github.com/user-attachments/assets/d7e59d55-a85c-4a6c-b297-3193ebbe86e0">

Please make sure that the port `8890` is not already in use locally. If it is, change the port number in both the Jupyter command and the SSH command (e.g `--port=8889` and `localhost:8889:localhost:8889`).

**4.** Open the notebook:

Once you have successfully logged into the notebook environment, navigate to the notebook path and double-click to open it, as demonstrated in the figure below.

<img width="1203" alt="Scherm­afbeelding 2024-11-09 om 08 38 02" src="https://github.com/user-attachments/assets/fafd9777-9822-4320-83b9-f9d1e5667cfc">

**5.** Keep the Jupyter Notebook running:

To ensure that your Jupyter Notebook session remains active even if you disconnect from the HPC, you can use `screen` or `nohup`

**6.** Session duration and port usage:

Your Jupyter Notebook session will remain active for up to **8 hours**. Make sure that the specified port remains reserved for that duration. If you need to start a new session, use a different port number (e.g. `--port=8890`).

**7.** Some tips for troubleshooting:

If you encounter problems connecting or if the port seems blocked, check for active sessions and kill it, or use a different port as mentioned before.

To kill a session, you need to find the Process ID (PID) on your local machine.
To find running SSH processes, use:
```bash
ps aux | grep ssh
```
Then, find the process using a specific port (e.g. our 8890)

To find the PID of of the process is using that specific port (e.g., 889), use:
```bash
lsof -i :8890
```
The output will look like **`ssh     2545 hamid    7u  IPv6 0x38854........      0t0  TCP localhost:ddi-tcp-1 (LISTEN)`**
This show information about the processes using the port, including their PIDs (in the first or second column; 2545).

Once you have identified the PID of the process, you can kill it using the `kill` command:
```bash
kill <PID>
```
or force-kill it with `-9`
