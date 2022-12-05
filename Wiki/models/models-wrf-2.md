# How to run WRF with ERA-Interim data #

-------------------------------------------------------------------------------

-------------------------------------------------------------------------------

# Running WRF from ERA-Interim #

-------------------------------------------------------------------------------

The ARCCSS CMS team helps to maintain a mirror of the ERA-Interim
reanalysis data set at NCI, which can be used as the boundary
conditions for a WRF run. The GRIB files may be found under
`/g/data1/ub4/erai/grib/`, you will need to request access to the
dataset first in order to access it.

You can use `Vtable.ERA-interim.pl` to ungrib the ERA-Interim data,
using the grib files under directories `oper_an_pl`. This Vtable file
can be found in the WPS distribution under `ungrib/Variable_Tables`. You
will also need to ungrib the files in directory `oper_an_sfc` to get the
surface fields.

## Automatic WPS ##

-------------------------------------------------------------------------------

We have a script available at [wps-era on Github](https://github.com/coecms/wps-era) that will run all WPS
steps automatically for you using the pressure level dataset. You can download the script with git:

``` shell
git clone https://github.com/coecms/wps-era
cd wps-era
```

Edit the `namelist.wps` file as appropriate for your model, then run WPS with

``` shell
make WPSDIR=/path/to/WPS
```

WPSDIR should be the path where you compiled WPS. The script will read
the run dates from the namelist, and link in only the files required
for the period you're using.

## Manual WPS ##

-------------------------------------------------------------------------------

### Time dependent data ###

To run ungrib on both the surface and vertical data you can either
create a temporary directory with both types of files, and run
`link_grib.csh` on that directory:

``` shell
mkdir era-tmp
ln -s /g/data1/ub4/erai/grib/oper_an_pl/fullres/2006/ei_oper_an_pl_075x075_90N0E90S35925E_200603* era-tmp
ln -s /g/data1/ub4/erai/grib/oper_an_sfc/fullres/ei_oper_an_sfc_075x075_90N0E90S35925E_200603* era-tmp
./link_grib.csh era-tmp/*
./ungrib.exe
```

OR you can use different ungrib prefixes for the surface and vertical
fields, combining the files with metgrid:

``` shell
./link_grib.csh /g/data1/ub4/erai/grib/oper_an_pl/fullres/2006/ei_oper_an_pl_075x075_90N0E90S35925E_200603*
nano namelist.wps # Set ungrib/prefix to 'PL'
./ungrib.exe

./link_grib.csh /g/data1/ub4/erai/grib/oper_an_sfc/fullres/ei_oper_an_sfc_075x075_90N0E90S35925E_200603*
nano namelist.wps # Set ungrib/prefix to 'SFC'
./ungrib.exe
```

### Invariant Data ###

You will also need to generate the invariant data, which includes the
land-sea mask used for interpolating the ERA Interim source dataset to
your model's target resolution The invariant data is only defined at
19890101T1200Z, so you need to set the start and end dates in
`namelist.wps` to this value (if you're using multiple nests set all
of the dates)

``` shell
&share
    ! Keep other settings the same, change dates to
    start_date = '1989-01-01_12:00:00',
    end_date = '1989-01-01_12:00:00',
/
&ungrib
     out_format = 'WPS'
     prefix = 'INV'
/
```

``` shell
./link_grib.csh /g/data1/ub4/erai/grib/invariant/ei_oper_an_sfc_075x075_90N0E90S3585E_invariant
./ungrib.exe
```

### Remaining processing ###

Before running Metgrid you need to tell it about all of the input GRIB
datasets. In `namelist.wps` set `fg_name` to a list of the time
dependent datasets, and set `constants_name` to `INV:1989-01-01_12`

``` shell
&metgrid
 fg_name = 'SFC', 'PL'
 io_form_metgrid = 2,
 constants_name = 'INV:1989-01-01_12'
/
```

You can then run geogrid and metgrid as normal to generate the input
files (don't forget to change back the run dates if you changed them
for the invariant ungrib).


