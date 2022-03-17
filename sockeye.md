# Notes for Sockeye

## Intro

https://arc.ubc.ca/ubc-arc-sockeye

UBC ARC Sockeye is a heterogeneous High-Performance computational cluster, suitable for a variety of workloads. It is named after the Sockeye salmon (Oncorhyncus nerka), native to the Northern Pacific Ocean and the rivers discharging into it.

Here's the documentation: https://confluence.it.ubc.ca/display/UARC/About+Sockeye

## System Specifications

15,872 CPU cores <br>
101 TB system memory <br>
200 general purpose GPUs <br>
EDR (100Gbps) Infiniband <br>
1.3 PB Dell EMC Isilon w/ 192TB all-flash burst buffer <br>
1.5 PB DDN Lustre <br>
452 TB Local HDD <br>
11.4 TB Local NVMe SSD <br>

## Quickstart Guide

https://confluence.it.ubc.ca/display/UARC/Quickstart+Guide

## Login

You will need a CWL login, and access to the VPN for UBC

* Connect to VPN
* SSH into Sockeye using your CWL username

```ssh <cwl>@sockeye.arc.ubc.ca```

Once you've entered your password and gone through the security check (Duo Push or SMS passcode), you can cd to your project space.

For now, I believe ours is:

`cd /arc/project/st-weberam2-1/`

## Transfer Files to Sockeye

If you want to transfer a large directory, it may be wise to first zip the folder:

`zip -r folder_name.zip folder_name`

Then you can send folder_name.zip using rsync in the terminal. For example, the command before will send folder_name.zip to the project space in Sockeye:

`rsync -a folder_name.zip <cwl>@dtn.sockeye.arc.ubc.ca:/arc/project/st-weberam2-1/`

To test the transfer first, use -an instead of -a option

## Storage Workflow

A recommend storage I/O workflow pattern on Sockeye could be summarized as:

* Copy/transfer input data to /arc/project/st-weberam2-1/
* Login to sockeye and create/modify job script(s) in /scratch/st-weberam2-1/
* Submit jobs that read input data from /arc/project/st-weberam2-1/ and writes output to /scratch/st-weberam2-1/ (or $TMDIR)
* Job completes successfully
* Copy/transfer job output from /scratch/st-weberam2-1/ to /arc/project/st-weberam2-1/
* Interactively post-process and/or analyze results in /arc/project/st-weberam2-1/

## Commonly Used Modules:
  
### FSL
  
```  
module load CVMFS_test
module load gcc
module load cuda/11.0
module load fsl
```
  
### ANTs

```
module load CVMFS_test
module load gcc
module load ants
```
  
### AFNI
  
```
module load CVMFS_test
module load gcc
module load afni
```
  
### fmriprep

https://fmriprep.org/en/1.5.1/singularity.html

Install (if it isn't already):

```
cd /arc/project/st-weberam2-1/
module load singularity
mkdir fmriprep
singularity build fmriprep/fmriprep-21.0.1.simg docker://poldracklab/fmriprep:21.0.1
```
  
Run:
  
```
singularity run --cleanenv fmriprep/fmriprep-21.0.1.simg path/to/data/dir path/to/output/dir participant --participant-label label
```
  
Python Environments
https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html

You might have to run these the first time you use conda

```
module load gcc
module load miniconda
conda init bash
conda config --set env_prompt '({name})'
```

Create a new environment

`conda create --prefix /arc/project/st-weberam2-1>/Intallations/Conda/<new-env-name>`

Install a package to it:

`conda install -y numpy --prefix /arc/project/st-weberam2-1>/Installations/Conda/<new-env-name>`

Activate:

`conda activate /arc/project/st-weberam2-1>/Intallations/Conda/<new-env-name>`

Deactivate:

`conda deactivate`

## R

```
module load CVMFS_test
module load r
R
```
