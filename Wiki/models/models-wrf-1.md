# How to run WRF

NCAR provides a detailed [tutorial](http://www2.mmm.ucar.edu/wrf/OnLineTutorial/index.php) on how to run WRF, but note:

  * Skip the configure and compile steps of the NCAR tutorial and go straight to the Basics tab. You should have followed the NCI-specific [installation instructions](./models-wrf.md) for building the model at NCI **before** attempting to run the model
  * Skip the geogrid, ungrib, and metgrid steps as these are included and compiled during the installation step
  * To run the January 2000 tutorial case do not download the metgrid data; it is available at NCI, see below
  * When the tutorial says to run the model see below for local run scripts 
  
 
## Data for WRF

To access the WRF input data you need to [request to join the **sx70** project](https://my.nci.org.au/mancini/project/sx70).

### Geographical data

The geographical data has been re-downloaded to include all the data available from [NCAR](http://www2.mmm.ucar.edu/wrf/users/download/get_sources_wps_geog.html) except the NMM data; this includes data only available over the US.

The new complete datasets are under:

  * For V3: /g/data/sx70/data/WPS_GEOG_v3
  * For V4: /g/data/sx70/data/WPS_GEOG_v4 
  
The previous incomplete datasets are also present to ensure you can reproduce current inputs:

  * For V3: /g/data/sx70/data/WPS_GEOG_20180313 
  * For V4: /g/data/sx70/data/WPS_GEOG_20190418 

### Meteorological data for the tutorial

The data needed to run the January 2000 tutorial case is stored under:

  * For V3 of WRF: /g/data/sx70/data/JAN00_v3 
  * For V4 of WRF: /g/data/sx70/data/JAN00_v4 

## Run scripts for WPS, real.exe and wrf.exe

Example scripts to submit WPS, real.exe and wrf.exe to the queues are provided under `WPS` and `WRFV3/run`. The files are named run_WPS.sh, run_real and run_mpi. Those scripts allow you to run WRF from any filesystem independently from the location of the source code.
In particular, you can copy `WRFV3/run` on `/scratch` as well as the inputs needed for WPS and then run from `/scratch`. Those files are good to use as-is for the tutorial. For other configurations, feel free to configure those to your needs.

```{warning}
Do not try to run the executable from the login nodes, use the queue system (PBS). Our recommendation is to run from `/scratch` as detailed above.
```
## Running with OpenMP

For some configurations, it might be advantageous to run with OpenMP in addition to OpenMPI or OpenMP alone for some
idealised cases. It is impossible to know beforehand whether OpenMP will improve or deteriorate performances. You will need to try your
simulation on a short time to see the effect.

By default, the compilation is done for OpenMPI and OpenMP so you
don't need to recompile. Below is an example of a mpirun command with
OpenMP in bash:

```shell
export OMP_NUM_THREADS=2
NCORE=$PBS_NCPUS/$OMP_NUM_THREADS
mpirun -np $NCORE --map-by node:PE=$OMP_NUM_THREADS --rank-by core ./wrf.exe
```

`OMP_NUM_THREADS` indicates the number of OpenMP threads you want to use:

  * If using OpenMPI + OpenMP, then 2 OpenMP threads are usually enough
  * If using OpenMP only, you'll probably want more threads but the number depends on your setup. You can use up to a node (48 threads) 

For `NCORE` you need to understand each OpenMP thread needs its own CPU. This means you need to "make room" for your threads in your CPU request in PBS. So if you are using 2 threads, you only use half the CPU requested for MPI and the other half for the OpenMP parts of the code.{br}
Calculating `NCORE` as such allows you to simply change the number of threads and automatically request the correct number of CPUs for OpenMPI.
