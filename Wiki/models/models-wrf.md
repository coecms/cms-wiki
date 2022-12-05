# WRF #

-------------------------------------------------------------------------------

-------------------------------------------------------------------------------

# Overview #

-------------------------------------------------------------------------------

The Weather Research and Forecasting (WRF) model is a next-generation
mesoscale numerical weather prediction system designed to serve both
operational forecasting and atmospheric research needs. It features
multiple dynamical cores, a 3-dimensional variational (3DVAR) data
assimilation system, and a software architecture allowing for
computational parallelism and system extensibility. WRF is suitable
for a broad spectrum of applications and across scales ranging from
meters to thousands of kilometres. For more details check out the [WRF
Home Page](https://www.mmm.ucar.edu/models/wrf).


# Updates #

-------------------------------------------------------------------------------

If you have any corrections, comments or contributions, please email
us on cws_help@nci.org.au so that we can continue to update over
time. UPP hasn't been ported to Gadi yet.

## 20 November 2022: New WRF version ##

-------------------------------------------------------------------------------

**Versions affected**: None

**Description**: Version v4.4.1 is now ready to be used on Gadi. Note, WRF only has been updated.

## 01 September 2020: New WRF version ##

-------------------------------------------------------------------------------

**Versions affected:** None

**Description**: Version v4.2.1 is now ready to be used on Gadi. Note, WRF
only has been updated. As for v4.2, the WRFV3/ directory was renamed
to WRF/.


## 11 May 2020: New WRF version ##

-------------------------------------------------------------------------------

**Versions affected**: None

**Description**: Version v4.2 is now ready to be used on Gadi. Note, WPS
and WRF have been updated. I have also renamed the WRFV3/ directory to
WRF/.

## 05 March 2020: ERA-Interim ##

-------------------------------------------------------------------------------

**Versions affected**:


  * V3.9

  * V3.9.1.1

  * V4.0.2

  * V4.1.1

  * V4.1.2

  * V4.1.3

  * V4.1.4

**Description**: wps-era has now been ported to Gadi for these
versions. Note the code is using a new METGRID.TBL:
METGRID.TBL.ERAI. This file has been corrected in WRF/WPS/metgrid.
This file is not distributed with standard WRF but is in the code base
ported to Gadi. Please clone wps-era and update your WRF code to use this capability.

**Compilation**: No compilation is required after updating the code.

## 03 March 2020: Bug ##

-------------------------------------------------------------------------------

**Versions affected**:

  * V4.1.1

  * V4.1.2

  * V4.1.3

  * V4.1.4

**Issue description**: An error has been found that affect the January
2000 case from the NCAR tutorial. In order to run this case, you will
now need to use the Variable Table: Vtable.GFS.tutorial for
ungrib.exe.

**Compilation**: No compilation is required after updating the code base
from Github.

# Installation at NCI #

-------------------------------------------------------------------------------

The code for all versions is now on [Github](https://github.com/coecms/WRF). Please follow the
instructions in the README file for the branch corresponding to the
version you want to use. The installation (obtaining and compiling the
source code) is NCI specific. The NCAR guides which we link to give
instructions for compilation. DO NOT USE THOSE INSTRUCTIONS. They will
not work. Install WRF using the instructions on [Github](https://github.com/coecms/WRF) and only follow
the NCAR guides for running your simulation.


# Running at NCI #

-------------------------------------------------------------------------------

  * [How to run WRF](./models-wrf-1.md) 
  * [How to run WRF with ERA-Interim data](./models-wrf-2.md) 
  * [How to run WRF with inputs in netcdf format](./models-wrf-3.md)
  * [Tips and Tricks](./models-wrf-4.md)
  * [WRF performance](./models-wrf-5.md)

# Porting WRF to NCI (CMS members only) #

-------------------------------------------------------------------------------

  * [Deprecated porting procedure](./models-wrf-6.md)
  * [Current porting procedure](./models-wrf-7.md)


