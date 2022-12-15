# Conda hh5 environment setup

The next-generation of the hh5 conda environment takes advantage of containerisation in order to reduce inode usage and increase performance of the small file operations associated with some actions in python (e.g. `import`-ing packages with many dependencies). Rather than packaging a conda environment within a container, this environment takes advantage of singularity's ability to manage overlay and [squashfs](https://en.wikipedia.org/wiki/SquashFS) filesystems. This allows the 'container' to be flexible and extensible in a way that a pure container-based system cannot be. Each `analysis` conda environment is packaged in a squashfs instead of in the container directly, and one or more of these squashfs environments is mounted into a bare-bones container at runtime.

The concept combines ideas from [Singularity Registry HPC](https://singularity-hpc.readthedocs.io/en/latest/) and [Rioux et al. *PEARC '20: Practice and Experience in Advanced Research Computing, July 2020, Pages 72–76*](https://doi.org/10.1145/3311790.3401776) in order to improve the deployment, maintenance, accessibility and performance of managed conda environments provided for Climate and Weather researchers on NCI systems.

## Overview
The relevant parts of the fundamental layout of the `/g/data/hh5/public` software area will look as follows:
```
$ ls -l /g/data/hh5/public
drwxrwxr-x+ 2 hh5_apps hh5  4096 Dec 14 22:31 apps
drwxrwxr-x+ 2 hh5_apps hh5  4096 Dec 14 22:31 modules
drwxrwxr-x+ 1 hh5_apps hh5  4096 Dec 14 22:31 scripts
```
Note that this is the ideal case, the file ownership displayed is contingent on the availability of a service user to use for software deployment. This caveat will apply every time the `hh5_apps` user appears on this page

The `apps` directory contains the base, uncontainerised conda environment and the squashfs files containing each analysis environment, the `modules` directory contains the environment modules needed to load the conda environments, and the `scripts` directory contains the machinery that enables commands to be executed inside the container as necessary, rather than running all commands from a shell inside the container.

The key difference between a standard conda environment setup and the `hh5` conda is the `envs` directory. In a standard conda environment setup, the envs directory contains a series of directories corresponding to each environment. Each of these directories is a fully self-contained environment, often comprising in excess of 250,000 individual files. Though these files are usually [hardlinked to central package caches](https://www.anaconda.com/blog/understanding-and-improving-condas-performance), this is not reflected in filesystem quotas, and hardlinked files are multiply counted for the purposes of quota enforcement on NCI systems. In the `hh5` environment, the `envs` directory appears as follows:
```
$ ls -l /g/data/hh5/public/apps/miniconda3/envs

```
Note that the symlink targets refer to paths that only exist inside the corresponding `.sqsh` files, and as such, they appear broken unless inspected from inside the container with the correct squashfs mounted. 

On loading an `analysis3-xx.yy` module (after running `module use /g/data/hh5/public/modules`), a `conda activate` command is run from inside the container in order to set the environment outside of the container to what would be expected had a `conda activate` command been run on an uncontainerised environment. The exception is that the `bin` directory inside the environment is translated to the appropriate subdirectory of `scripts`. For example, the path:
```
/g/data/hh5/public/apps/miniconda3/envs/analysis3-23.01/bin
```
becomes
```
/g/data/hh5/public/scripts/analysis3-23.01.d/bin
```
There are also environment variables set that are interpreted by the launcher script required to access commands within the squashfs conda environment. 

There is a dedicated launcher script in `analysis3-23.01.d/bin` directory (herein referred to as `launcher.sh`), and every entry in the in-container `envs/analysis3-23.01/bin` directory is symlinked to this script in the `analysis3-23.01.d/bin` directory. These links are programmatically generated, and invoke singularity with the correct squashfs mounted, and then inspect the link name to determine which command execute the command from within the container. The launcher script also has provisions for command overrides and configuration to alter the behaviour of commands in the container if necessary. The launcher script can also be invoked directly in order to run arbitrary commands inside the container. For example, running `launcher.sh bash` will launch an interactive shell inside the container with all correct bind, overlay and squashfs mounts in place.

## The launcher script
The launcher script is designed to be the interface between the standard Gadi user environment and the contents of a conda environment squashfs file. It performs a number of checks in order to generate the correct `singularity` launcher line from outside the container, or run the correct command directly if it is invoked from inside the container. Its workflow is as follows:

* Source its configuration script - There are some settings that are loaded at runtime, these are derived from a script named `launcher_conf.sh` that resides in the same directory as `launcher.sh`. 
* Parse out its own command line arguments - `launcher.sh` has some dedicated command line arguments to supply information to it in the case of the environment not being able to be set beforehand (e.g. when invoking `ssh`). Since `launcher.sh` can invoke arbitary commands, these arguments must be processed and removed from the list of arguments to launch
* Determine the path to the `singularity` binary - Usually this will be in the conf script or provided by a command line argument. If neither of those things has happened, it will attempt to load the `singularity` module and query the location using the `which` command.
* Determine whether it is running a command, or being invoked directly - if a command linked to `launcher.sh` has been run, the full argument list, minus the `launcher.sh`-specific arguments will be passed to `singularity exec`. If `launcher.sh` has been invoked directly, the program to run and its arguments are assumed to be in the arguments following `launcher.sh`.
* Determine if it is being invoked from within a container - if `launcher.sh` determines that it is already inside a singularity container, it will substitute the real path to the binary in place of its own path, and run the binary directly.
* Determine if there is an override script for the command being run - If there is, run this instead of `singularity exec`
* 

