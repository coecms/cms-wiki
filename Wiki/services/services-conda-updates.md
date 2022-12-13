# Conda environments update history

## 22.04 (Current Stable)

### Notable new packages

**Statistics/ML**
* lmfit Non-Linear Least-Squares Minimization and Curve-Fitting for Python

**Climate specific**
* benchcab CABLE benchmarking
* xclim library of functions to compute climate indices from observations or model simulations. It is built using xarray and can benefit from the parallelization handling provided by dask
* acs-replica-intake Intake-esm catalogue for the Australian Reference Climate Data at NCI collection

**Developer**
* jupyter-resource-usage extension for Jupyter Notebooks and JupyterLab that displays an indication of how much resources your current notebook server and its children (kernels, terminals, etc) are using

## 22.01 (Unsupported)

### Notable new packages

**Statistics/ML**
* pygam build Generalized Additive Models in Python
  spectrum tools to estimate Power Spectral Densities based on Fourier transform, parametric methods or eigenvalues analysis

**Climate specific**
* xmhw xarray compatible Marine Heatwave Detection
* argopy library dedicated to Argo data access, manipulation and visualisation for standard users as well as Argo experts.
* ilamb the International Land Model Benchmarking (ILAMB) project is a model-data intercomparison and integration project designed to improve the performance of land models

**Developer**
* dataclasses-json provides a simple API for encoding and decoding dataclasses to and from JSON

**Geospatial**
* pysal spatial analysis library for open, cross platform geospatial data science

## 21.10

### Notable new packages

**Statistics/ML**
* xarraymannkendall compute linear trends over 2D and 3D arrays
* hdbscan tools to use unsupervised learning to find clusters, or dense regions, of a dataset

**Geospatial**
* timezonefinder looking up the corresponding timezone for given coordinates on earth entirely offline
* iris-grib converting between weather and climate datasets that are stored as GRIB files and Iris cubes
* intake-thredds Intake interface to THREDDS data catalogs

**Developer Tools**
* lftp sophisticated file transfer program supporting a number of network protocols
* watchdog monitor file system events
* numpy_groupies Optimised tools for group-indexing operations: aggregated sum and more
* anytree Simple, lightweight and extensible Tree data structure
* fortran-language-server Fortran implementation of the Language Server Protocol
* jupyter-book building beautiful, publication-quality books and documents from computational material
* kerchunk Cloud-friendly access to archival data
* ujson ultra fast JSON encoder and decoder

**Known Issues**

* MetPy is incompatible with the Matplotlib version in this environment https://github.com/Unidata/MetPy/issues/2281

