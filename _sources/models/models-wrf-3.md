# How to run WRF with inputs in netcdf format


By default, metgrid.exe reads in only what is called intermediate format. WPS only comes with a conversion tool from GRIB to intermediate format. In the climate community, we use the netCDF file format more than the GRIB file format. So some of you might have to write your own interpreter from netCDF to intermediate format.

You can find a good example of an interpreter in this [git repository](https://github.com/coecms/Soil_Moisture_WRF).

```{note}
This case was to create an intermediate file with only 1 variable in it and the configuration has to be done within the source file for now.
```
