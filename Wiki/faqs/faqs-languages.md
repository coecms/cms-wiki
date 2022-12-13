# Coding Language's FAQs


`````{tab-set}
````{tab-item} Python

**1.How to transform 1D lat/lon arrays into 2D lat/lon arrays with Python**

If using numpy or masked arrays, one can use the resize() method. Assuming slat and slon are 1D latitude and longitude arrays, do this:

```python
tmp    = numpy.resize(slat, (slon.size, slat.size))
slat2D = numpy.transpose(tmp)
slon2D = numpy.resize(slon, (slat.size, slon.size))
```

If you want masked arrays, replace `numpy` by `numpy.ma`

**2.Running out of memory when running a relatively small timeseries analysis using xarray/dask**

Xarray loads data lazily, which means that it will only load the data values when you need them. However, the data is stored in the files in chunks and all values in a chunk will be loaded as a block. The most common pattern for chunks in netcdf files is to have a grid stored in the same chunk. So, if you run a calculation for a specific timestep, you will only load one chunk. This means that often timesteps are on separate chunks, often `time` has a chunk size of 1, where all timesteps for a particular grid point are stored on different chunks. If you don’t change the file chunking and try to run a calculation that needs all the timeseries for a single grid point, you will be effectively loading the entire file in memory.

If you want to rechunk your data to have all timesteps on the same chunks you can run:

```python
array = array.chunk(chunks={"time"=-1})
```

-1 means that the `time` dimension size will be used as chunk size. If you have 100 timesteps, it would be equivalent to set the new chunks as `{time=100}`.

````

````{tab-item} Fortan
stuff
````

````{tab-item} C++
stuff
````

````{tab-item} Bash
stuff
````

`````
