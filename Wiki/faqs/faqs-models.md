# Model's FAQs

::::{grid}
:gutter: 1

:::{grid-item-card}
```{dropdown} **UM and ACCESS FAQ**:
**Who can gain access to the UM and the ACCESS models?** 

The full ACCESS model is made up of a number of sub-models for different processes - UM for the atmosphere, MOM for the ocean and CABLE for the land surface. Each of these components was created by different groups with different conditions of use.

The Met Office Unified Model is proprietary software released under a commercial licence. The Australian University community has a sub-licence to use the UM from CAWCR, managed by ARCCSS. Australian researchers outside of ARCCSS may request access to the model by emailing access_help@nf.nci.org.au. Note that our licence is restricted to running the model at NCI and does not permit sharing of model code outside of the UM partnership.

GFDL's Modular Ocean Model is open-source software available under the GPL. Any researcher is free to use, modify and share the model code.

CSIRO's CABLE model is proprietary software. See the [licence agreement](http://cawcr.gov.au/projects/access/cable/cable_licence_agreement.pdf) for conditions of use.

**What to do to gain access to the UM and the ACCESS models?**

ARCCSS researchers can get access to the UM and ACCESS models by contacting the CMS team at climate_help@nf.nci.org.au. Researchers outside of ARCCSS should contact the general ACCESS helpdesk at access_help@nf.nci.org.au. Using the models requires an account at [NCI](http://nci.org.au/), the Australian national supercomputing centre.

**What to do to use the software in /projects/access/bin?**

Several tools useful for working with ACCESS are available at NCI under the path `/projects/access/bin`. These tools include output viewers and converters between the UM file format and standard formats like NetCDF and GRIB. To access these tools you must be registered as an ACCESS user (see above).

**How to convert UM hybrid vertical coordinates to pressure levels**

The formula for the vertical coordinate is http://climate-cms.wikis.unsw.edu.au/Vertical_Coordinates

Iris will compute the height variable directly if the file you read in has the orography in.

If you have the pressure field in your file as well, Metpy might then help you to interpolate your fields to specific pressure levels: https://unidata.github.io/MetPy/latest/examples/sigma_to_pressure_interpolation.html#sphx-glr-examples-sigma-to-pressure-interpolation-py
```
:::

:::{grid-item-card}
``````{dropdown} **UM Errors**:
`````{tab-set}
````{tab-item} Extracting
When compiling the UM, the first step is for the UMUI(x) to extract the source code. If an error occurs, check for these things:

1.  Did the error occur during the base, model, or reconfiguration extraction?
2. Check for the extraction log, it is either in
`~/UM_OUTPUT/<JOBID>/<umbase or ummodel or umrecon>/ext.out`
or
`/scratch/users/$USER/<JOBID>/<umbase or ummodel or umrecon>/ext.out`

The final lines typically show at which command the error occurred.

**SVN errors**

If you get a permission denied error from an SVN, it might be because svn needed your password, and didn't get it. The solution is to run

```bash
svn ls https://access-svn.nci.org.au/svn/um/branches/
```

If you get asked for a password, you need to supply your NCI password. And then it will ask you whether it should store it in plain text.
As much as I hate to say it: you need to answer 'yes'.

And immediately afterwards, you have to run the command:

```bash
chmod -R go-rwx $HOME/.subversion
```

**SCP and RSYNC errors**

If you get an error like this:

```bash
mkdir: cannot create directory '/abc123': Permission denied
```

(where abc123 is your username), the most likely explanation is that you haven't set the DATAOUTPUT environment variable on accessdev.
If you are using bash, add this line to your `$HOME/.bashrc` file:

```bash
export DATAOUTPUT=/short/${PROJECT}/${USER}/UM_ROUTDIR
```

If you are using tcsh or csh, add this line to your $HOME/.login

```bash
setenv DATAOUTPUT /short/${PROJECT}/${USER}/UM_ROUTDIR
```

Then log out of accessdev and log back in, open up umuix again, and check whether the error went away.
````

````{tab-item} Submiting
My second tab
````

````{tab-item} Compiling
My third tab
````

````{tab-item} Running
**REPLANCA: PP HEADERS ON ANCILLARY FILE DO NOT MATCH**

The model is trying to update an ancillary field, but it is looking for a date that is past the end of the ancillary file. For instance, say an ancillary file is valid for dates between 1850-01-01 and 2001-01-01 and the model is trying to update fields for the date 2001-02-01. The model can't find this data in the file, so it crashes.

Near the end of the `.leave` file there will be a block that looks like

```text
  STASH code in dataset                    122   STASH code requested
                    58
 'Start' position of lookup tables for dataset in overall lookup array
                   481
                   122                    58                    39
  UP_ANCIL : Error in REPLANCA.

 ''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''*
 UM ERROR (Model aborting) :
 Routine generating error: UP_ANCIL
 Error code:                    239
 Error message:
REPLANCA: PP HEADERS ON ANCILLARY FILE DO NOT MATCH
 ''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''*
```

The important bit here is `STASH code requested`, which is in this case was `58`. Internally the UM refers to each model field (U, V, temperature, etc.) by a numeric STASH code. You can check field is represented by a code by going to https://accessdev.nci.org.au/umdocs/7.3/stash_browse. The STASH codes are divided into different sections, for errors like this you'll find the variable in section `0) Prognostic Variables`. Scroll down the item list to find the name for the error code - the code `58` corresponds to `SULPHUR DIOXIDE EMISSIONS`.

To find out which file this corresponds to you'll need to go back to the UMUI, and check the screen `Atmosphere -> Ancillary and Input data files -> Climatologies`. It should be fairly simple to work out the entry to use (in our example you'd look at `Sulphur-Cycle Emissions`) but if you have trouble check with the helpdesk. You'll need to replace the file here with one that covers the next part of your model run.

Its common for this error to occur when running an AMIP-type simulation and going past 2001, where the data changes from Historical to the RCP scenarios. If this is the case you'll need to change Ozone (files starting with SPARCO3), 2d Sulphur Cycle (sulp) Soot (BC_hi), Biomass (Bio), and OCFF (OCFF). You can find ancillary files for the different scenarios under `/projects/access/data/ancil/CMIP5`, or ask the CMS team at climate_help@nf.nci.org.au and we'll help you locate the right ancillary file.

**INANCILA: Insufficient space for LOOKUP headers**

This section is currently tested. Ask Holger if you still see this message after 1/6/2018.

Error message is something along these lines:

```text
Field:                    128 OCFFEMIS
 Opening Anc File: /projects/access/data/ancil/CMIP5/OCFF_RCP85_1850_2100.N96
 No room in LOOKUP table for Ancillary File                     47
 INANCCTL: Error return from INANCILA                     14
 INANCILA: Insufficient space for LOOKUP headers                               

 Failure in call to INANCCTL
 ''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''*
 UM ERROR (Model aborting) :
 Routine generating error: INITIAL
 Error code:                     14
 Error message:
INANCILA: Insufficient space for LOOKUP headers
 ''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''*
```

The UM needs an initial guess on how big the ancillary files are. It needs to reserve a little bit of memory for every level at every time step of every field. This number can be set at

Model Selection -> Atmosphere -> Ancillary and input data files -> In file related options >> Header record sizes.

**DRLANDF1 : Error in FILE_OPEN**

Symptom: The UM aborts with the error message:
```text
 ''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''*
 UM ERROR (Model aborting) :
 Routine generating error: UM_SHELL
 Error code:                      1
 Error message:
 DRLANDF1 : Error in FILE_OPEN.
 ''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''*
```

This indicates that the start dump (NRUN) or restart dump (CRUN) could not be opened. Search the leave file for the string "Unit 21". You should find a line like this:

```bash
OPEN:  File <somefile> to be Opened on Unit 21 does not Exist
```

`somefile` might be empty, in which case there will be 2 spaces between `File`and `to be Opened`.
Otherwise, check that the file exists and that you have read permissions.

If this happens during an NRUN, check the settings in
`Model Selection -> Atmosphere -> Ancillary and input data files --> Start dump`.
Check the settings under "Specify initial dump"

If the file name is an empty string, and you are running a CRUN, open
`Model Selection -> Atmosphere -> Control -> Post processing, Dumping & Meaning --> Dumping and meaning`
and ensure that the restart dump frequency is consistent with your per-submission run length.
(That is, if your run length is 1 year, you may have monthly or yearly restart dumps, but not 5-yearly restart dumps.)

**Unable to WGDOS pack to this accuracy**

Symptom: The UM aborts with the error message:
```text
????????????????????????????????????????????????????????????????????????????????
???!!!???!!!???!!!???!!!???!!!       ERROR        ???!!!???!!!???!!!???!!!???!!!
?  Error code: 2
?  Error from routine: COEX (cmps_all)
?  Error message: Unable to WGDOS pack to this accuracy
?  Error from processor: 0
?  Error number: 85
????????????????????????????????????????????????????????????????????????????????
```

There has been an error compressing one of the output files. Check the model output streams and 'Dumping and Meaning' sections, setting the 'packing profile' to 0 to turn off compression.
````
`````
``````
:::

:::{grid-item-card}
````{dropdown} **WRF FAQ**:

**Tips to reduce WRF outputs size**

* `netCDF4 compression`: you are probably using netCDF v4.0 or newer. In that case, you can enable compression directly in WRF code so the output is compressed at creation time. If you ever want an output file in classic format, you can then use the namelist option `use_netcdf_classic=.true.` in the `&time_control `section. The choice of netcdf happens at the configuration step so if you already have compiled the code, clean it up with
```bash
clean -a 
```

Then follow these steps:

Define the NETCDF4 environment variable:
```csh
#for csh
setenv NETCDF4 1
```
```
#for bash
export NETCDF4=1
```

Configure and compile as usual.

* `Output a subset of variables`: WRF comes with a built-in mechanism to choose which variables to output or not. So you don't have to output all the default variables (and you can add some non-default ones). The details can be found in the [WRF User's guide](http://www2.mmm.ucar.edu/wrf/users/docs/user_guide_V3.8/users_guide_chap5.htm#runtimeio)

* `clean up the output file`: After creation, it can be useful to clean up the output file. For example, all variables have a Time dimension even if they are constant in time (latitudes and longitudes are usually constants unless you use a moving nest). So it can save storage space to remove this dimension. For example by using NCO:

```bash
ncwa -a Time -v XLONG wrfout_d01_2000-01-24_12\:00\:00 time0.nc
ncks -x -v XLONG wrfout_d01_2000-01-24_12\:00\:00 no_longitude.nc
ncks -A -v XLONG time0.nc no_longitude.nc
mv no_longitude.nc wrfout_d01_2000-01-24_12\:00\:00 
```
Obviously you can list more than 1 variable at once in the commands.

**What to do if WRF does not compile on Raijin?**
You need to make sure that you are using the WRF version that is stored on /projects/WRF on Raijin. Then you should first try to compile using the `dmpar` option. For this, clean (`./clean -a`) and run configure again and choose option #3 (`dmpar`). Then WRF should compile the code without problems. WRF has not yet been successfully compiled on Raijin using the other options.

**Which processor crashed in my WRF simulation?**
WRF output the standard output in rsl.out and the error in rsl.error. There is a pair of files for each processor. If you are running on a large number of processors, checking each file is very time-consuming.
The first thing to do when your simulation stops is to check the output file for your script. If your script to launch WRF is called `script.pbs`, then at the end of the job PBS will create a `script.pbs.o1234567` where `1234567` is to be replaced by your job ID. Open this file and check the end. If you see a message like:
```text
--------------------------------------------------------------------------
mpirun has exited due to process rank 50 with PID 32659 on
node r2059 exiting improperly. There are two reasons this could occur:

1. this process did not call "init" before exiting, but others in
the job did. This can cause a job to hang indefinitely while it waits
for all processes to call "init". By rule, if one process calls "init",
then ALL processes must call "init" prior to termination.

2. this process called "init", but exited without calling "finalize".
By rule, all processes that call "init" MUST call "finalize" prior to
exiting or it will be considered an "abnormal termination"

This may have caused other processes in the application to be
terminated by signals sent by mpirun (as reported here).
--------------------------------------------------------------------------
```

Then it means the simulation did not finish normally. The first line of the message gives the processor number that finished abnormally: "due to `process rank 50`". So in this case, the problem happened on the processor 50 and the error message should then be located in `rsl.error.0050`. Note you might want to check `rsl.out.0050` as well just in case a message was written to the output first.

**What to do if I get a segmentation fault error with WRF?**
If in `rsl.error` you get a message like this one:

```text
 <nowiki>forrtl: severe (174): SIGSEGV, segmentation fault occurred
Image PC Routine Line Source
wrf.exe 00000000017F59A1 Unknown Unknown Unknown
wrf.exe 00000000017F3655 Unknown Unknown Unknown
wrf.exe 0000000001ACFA07 Unknown Unknown Unknown
wrf.exe 0000000001387C8A Unknown Unknown Unknown
wrf.exe 0000000000EC4CDD Unknown Unknown Unknown
wrf.exe 0000000000DCCAB7 Unknown Unknown Unknown</nowiki>
```

you have a segmentation fault error. These can have multiple causes, the best way to find out what is causing the error is to recompile WRF using the debugging options and re-run.
1. Clean the previous compilation in `WRF/` with .`/clean -a`
2. Re-run the compilation with the -`d` option: `./run_compile -d`
````
:::

:::{grid-item-card}
````{dropdown} **LIS FAQ**:
**My LIS outputs are empty**

Please check what filename you have for the following option in `lis.config`:
```text
Model output attributes file
```
Then make sure you have this file in your run directory. And run again.
````
:::

:::{grid-item-card}
````{dropdown} **MOM FAQ**:

**Problem with collating outputs with payu** 

Your simulation runs correctly but the collating of the outputs does not work and you have this error message in the log file:

```bash
Traceback (most recent call last):
  File "/jobfs/local/pbs/mom_priv/jobs/268316.r-man2.SC", line 9, in <module>
    collate_cmd.runscript()
  File "/apps/payu/0.6/lib/payu/subcommands/collate_cmd.py", line 93, in runscript
    expt.collate()
  File "/apps/payu/0.6/lib/payu/experiment.py", line 606, in collate
    model.collate()
  File "/apps/payu/0.6/lib/payu/models/fms.py", line 78, in collate
    assert mppnc_path
AssertionError
```

This probably means you don't have a `mppnccombine` executable in you `mom/bin` directory. If you need such a file please contact `climate_help@nci.org.au` to know where to find it. Include a copy of the error message with your email please.

**Locating log files**

Log files from a Payu run will be in:

* `$EXPERIMENT` (Directory containing the `config.yaml` file):
    * `$RUNID.out`, `$RUNID.err`: Stdout and stderr of model run
    * `$RUNID.o123456`, `$RUNID.e123456`: PBS logs
 
* `$WORK` ('work' symlink in `$EXPERIMENT`, or found under the laboratory path if that's not accessible)
    * Any log files output by the component models

* `$ARCHIVE` ('archive' symlink in `$EXPERIMENT`, or found under the laboratory path)
    * Log files from previous submissions

The laboratory path is configurable in config.yaml, normally it will be `/scratch/$PROJECT/$USER/$MODEL`
````
:::

:::{grid-item-card}
````{dropdown} **CESM FAQ**:
CMS doesn't generally support CESM, but these are some things that may be helpful for users of the model

**CESM2 configuration**

A basic configuration for running CESM2 on Gadi is available at https://github.com/coecms/cesm2-gadi

**Installing XML::LibXML**

CESM requires the Perl module `XML::LibXML`. You'll need to install this yourself, which you can do on Gadi by running

```bash
cpan XML::LibXML
```

If you've not used Perl before it will ask you some questions on where to install it, the defaults are fine and will install it into your home directory at `~/perl5`.

You may need to run the command twice to properly install the library

````
:::

:::: 
