# Conda Python Environments

CMS maintain an Anaconda Python environment at NCI, with a wide variety of climate and weather related libraries.

You can find the most recent list of libraries at our github repository, or run conda list with an environment loaded.

To use any of the conda environments:

* Request access to hh5 (to do once)
* You must first run (to do at each session, a qsub job is a new session)
* module use /g/data/hh5/public/modules

You can safely put this in your ~/.bashrc file. It can go in the if in_interactive_shell; then section of the file.

```{admonition} Using hh5 envs in a PBS job
:class: tip
If you need to use the conda environment in a PBS job you will need to add the hh5 project to your storage flags, e.g.

#PBS -l storage=gdata/hh5
Note that these conda environments will work on gadi login and compute nodes, and in the OOD cloud environment. They do not work on accessdev, as this has a very old system version that is no longer compatible.
```
## Stable Environment

We update the stable environment once a quarter, around when NCI do their quarterly maintenance of Gadi. Otherwise everything in the environment stays fixed, we don't update packages or install new packages unless something is very broken.

```{code}
module load conda/analysis3
```

## Unstable Environment

The unstable environment gets updated more often, as we install new packages or apply updates to existing ones. If you ask for a new package it will be installed here.

```{code}
module load conda/analysis3-unstable
```

When we do our quarterly update the unstable environment becomes the new stable environment.

### Removed Environments

Normally after three quarters have passed old environments are removed, to reduce disk space and support burden. Conda environment.yml descriptions of past environments are available at https://github.com/coecms/conda-history.


## Interactive Analysis / Jupyter

Jupyter provides a 'notebook' interface for working with Python - you can combine Python code, text, latex equations and plots in a web interface.

The preferred method of running Jupyter at NCI is through the [`Analysis Research Environment`(ARE) service](https://are.nci.org.au). This runs a Jupyter instance in NCI's cloud that you can access directly from your browser. To use CLEX Conda in ARE, start Jupyter with the advanced options:

* Module Directories: /g/data/hh5/public/modules
* Modules: conda/analysis3

The Centre has developed a script (gadi_jupyter) that can run Jupyter on gadi compute nodes and display the notebook interface on your local computer. These scripts are available at https://github.com/coecms/nci_scripts, see the instructions there for usage.

```{warning}
As ARE gives you access to the same filesystem as Gadi and to several options of memory/cpus combinations, so includes all the extra features `gadi_jupyter` has compared to the older OOD service. For this reason we are not going to maintain gadi_jupyter anymore.
```

You can also run Jupyter directly on VDI, by loading the Conda environment and running 'jupyter lab'.

Note that on Windows the Jupyter scripts must be run through a Bash terminal (From WSL or Cygwin).

## Requesting new packages

You can ask for a new package to be installed or for an existing package to be updated by emailing cws_help@nci.org.au. Please include a link to the package documentation to your request.

It would be appreciated if you can check the package isn't already installed before putting in a request. To do so, please load the unstable environment and use conda list to list the packages included in that environment

As a general rule we will only install packages from the 'conda-forge' channel. Newly installed packages will be available in the conda/analysis3-unstable environment.

## Creating personal environments

You can create your own environment if needed, but please be cautious of both the size on disk and number of files that Conda environments can create.

Make a file ~/.condarc like:

```{code}
auto_activate_base: false
envs_dirs:
  - /scratch/$PROJECT/$USER/conda/envs
  - /g/data/hh5/public/apps/miniconda3/envs
pkgs_dirs:
  - /scratch/$PROJECT/$USER/conda/pkgs
conda-build:
  root-dir: /scratch/$PROJECT/$USER/conda/bld
```

This will set up Conda to create environments in /scratch, by default it puts them in your home directory which will rapidly use up your disk quota.


To create the conda environment, load the conda module, then deactivate it with

```{code}
conda deactivate
conda env create ...
conda activate ...
```

Create an environment file: environment.yml. Files on scratch are deleted if no longer used, so this file allows you to re-create your environment if some of the files are deleted. You can use any name for the environment file. Keep it secure.

```{code}
conda env export > environment.yml
```
To recreate the environment from the environment file:

```{code}
conda env create -f environment.yml
```

### Current available environments

--------------------------------- /g/data/hh5/public/modules ---------------------------------
conda/analysis3-21.10                              conda/cws-radar         
conda/analysis3-22.01                              conda/esmvaltool-21.08  
conda/analysis3-22.04(analysis:analysis3:default)  conda/gdal27            
conda/analysis3-22.07(analysis3-unstable)          conda/python3           
conda/analysis27-18.10(analysis27)                 conda/python27 
