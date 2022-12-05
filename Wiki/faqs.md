# FAQs and Tips

This page lists FAQs/tips for the different models and codes supported by the CMS team, as well as accessing and publishing data.

# Table of Contents
1. [FAQ Model pages](#faq-model-pages)
2. [FAQ Gadi](#faq-gadi)
    
    2.1 [How to debug issues with gadi jupyter](#gadi-jupyter)
    
    2.2 [How to run independent jobs in parallel](#parallel-jobs)

3. [FAQ Python](#faq-python)

    3.1 [How to transform 1D lat/lon arrays into 2D lat/lon arrays with Python](#transform-latlon)
    
    3.2 [Running out of memory when running a relatively small timeseries analysis using xarray/dask](#memory-dask)

4. [FAQ miscellaneous](#faq-miscellaneous)




<a name="faq-model-pages"></a>
## 1. FAQ Model pages
----------------------------------------------------
* UM and ACCESS FAQ 
* UM error messages
* WRF FAQ
* LIS and NUWRF FAQ
* MOM FAQ
* Data_FAQ
* CESM FAQ 

## FAQ miscellaneous


## 3. FAQ Gadi
----------------------------------------------------

<a name="gadi-jupyter"></a>
### 3.1 How to debug issues with gadi jupyter

The script submitted to PBS and the PBS logs are saved under the directory tmp/runjp directory under the user's /scratch directory for the group used to run the script. These will often have more information if the script was started alright but you can't seem to be able to connect to the Jupyer Notebooks. This usually happens when the job started alright but exited due to an error in the script.

For example, let's say I (ccc561) requests gadi_jupyter to run using the project w35. The information will then be stored under '''/scratch/w35/ccc561/tmp/runjp'''

If I then run gadi_jupyter again but this time running on w40, the information will be stored under '''/scratch/w40/ccc561/tmp/runjp'''

<a name="parallel-jobs"></a>
### 3.2 How to run independent jobs in parallel

If you have to run the same script with a lot of independent sets of inputs, or you have lots of similar scripts to run at NCI. And these scripts run on 1 processor. You could submit each of your jobs independently via PBS but then small jobs are not efficient at NCI. It would probably be better for you to have a master script that dispatches your jobs to available processors. You then submit this master script to PBS and this script requires a lot more processors and hence might be more efficient with the scheduler. There is an example of such a master script [[Embarrassingly_parallel_job|on this page]].

## 4. FAQ Python
--------------------------------------

<a name="transform-latlon"></a>
### How to transform 1D lat/lon arrays into 2D lat/lon arrays with Python

If using numpy or masked arrays, one can use the resize() method. Assuming slat and slon are 1D latitude and longitude arrays, do this:

```python
tmp    = numpy.resize(slat, (slon.size, slat.size))
slat2D = numpy.transpose(tmp)
slon2D = numpy.resize(slon, (slat.size, slon.size))
```

If you want masked arrays, replace `numpy` by `numpy.ma`

<a name="memory-dask"></a>
### Running out of memory when running a relatively small timeseries analysis using xarray/dask

Xarray loads data lazily, which means that it will only load the data values when you need them. However, the data is stored in the files in chunks and all values in a chunk will be loaded as a block. The most common pattern for chunks in netcdf files is to have a grid stored in the same chunk. So, if you run a calculation for a specific timestep, you will only load one chunk. This means that often timesteps are on separate chunks, often `time` has a chunk size of 1, where all timesteps for a particular grid point are stored on different chunks. If you don’t change the file chunking and try to run a calculation that needs all the timeseries for a single grid point, you will be effectively loading the entire file in memory.

If you want to rechunk your data to have all timesteps on the same chunks you can run:

```python
array = array.chunk(chunks={‘time’=-1})
```

-1 means that the `time` dimension size will be used as chunk size. If you have 100 timesteps, it would be equivalent to set the new chunks as `{time=100}`.



== Why do I get segfaults? ==

A segmentation fault means the program you are using has tried to read or write a memory location it does not have access to. This can mean there is an error in the code, or there is a system limit set. If the program runs for others, but not you, it may well be a system limit issue. This seems to be quite common when trying to write netcdf files. In some cases this can be overcome by setting your stack size to unlimited

For tcsh/csh add this to your .cshrc:
<syntaxhighlight lang="text">
limit stacksize unlimited
</syntaxhighlight>

For bash add this to your .bashrc:
<syntaxhighlight lang="text">
ulimit -s unlimited
</syntaxhighlight>

Note: you must log out and log back in again for this change to take effect.

&nbsp;

== No files found or non-existent&nbsp;path error while running a PBS job ==

When submitting a job on gadi you need to declare explicitly what gdata and/or scratch projects your script&nbsp;need to access.

This is done by setting the storage flag in the job you are submitting:

PBS -l storage=gdata/hh5+gdata/ua8+gdata/e14

In the above case I want to use input data from gdata/ua8 save my results to gdata/e14 and use the conda modules which are in gdata/hh5

== I cannot see a project folder under /g/data and/or /scratch ==

You need to be a member of a project to access or even just list the project folder. So if you cannot see a project folder, it doesn't mean that it doesn't exist or it was removed, it simply means that you are not a member of that project. Go to my.nci.org.au if you want to request access.

== Weird errors for&nbsp;a script that was previously working&nbsp; ==

Make sure you have loaded the right modules and only them! Avoid to load modules directly in your .bash_profile, .bashrc files . They will be loaded every time and occasionally interfere with the modules you are loading for a specific task.&nbsp;

It is useful and safe to add instructions as

module use /g/data/hh5/public/modules

as this is making the modules available without loading any.

Sometimes you need to install a python module in your user space (&nbsp;$HOME/.local/... ) as it is not available in a managed conda environment. Again, occasionally modules stored in your local user space can interfere with other loaded modules, for example if they have different dependencies. If you find this is causing issues you can exclude the local environment by setting this environment variable

PYTHONNOUSERSITE=x (&nbsp;&nbsp;=True and =1 should also work as values)

&nbsp;




== Issues installing a R module on gadi ==

First of all, you might not need to install the module, as NCI is now managing some R environments which might include already what you need:&nbsp;[https://opus.nci.org.au/display/DAE/Getting+Started [https://opus.nci.org.au/display/DAE/Getting+Started]].

If this approach doesn't work for you and you need to install a module, make sure you have all the necessary modules loaded.&nbsp;As outlined in the [https://opus.nci.org.au/display/Help/R Gadi user guide] you need to load the same intel module that was used to install the R module you are using. If you still have issues, contacting the NCI Helpdesk, rather than us, is more efficient as they have more experience with R.

== I want to use Matlab on gadi ==

You will need to setup your institutional Matlab license. We cannot help you troubleshooting any of this, just follow the [https://opus.nci.org.au/display/Help/MATLAB NCI instructions] and contact their Helpdesk if you run into any issue.

## Obsolete:
-------------------------

## How do I push my local code changes on gadi&nbsp;back to github using ssh keys?

You have an account on [http://github.com | github]. You have cloned a repository to raijin and made changes to it. Now you want to push those changes back to the github repository but you get errors like this (where <username> is your github username):
<syntaxhighlight lang="text">
error: The requested URL returned error: 403 Forbidden while accessing https://github.com/<username>/test.git/info/refs

fatal: HTTP request failed
</syntaxhighlight>
Firstly, you need to [https://help.github.com/articles/generating-ssh-keys/ | make an ssh key and add the public part of the key to your account on github]. Check the url to which you are trying to push your changes: &nbsp; <syntaxhighlight lang="text">
git remote -v
origin https://github.com/<username>/test.git (fetch)
origin https://github.com/<username>/test.git (push)
</syntaxhighlight>

If it looks like the output above you need to alter the url and put in a login name in front of the address. For ssh access the login name is always git. The command to set the remote url is:
<syntaxhighlight lang="text">
git remote set-url origin ssh://git@github.com/<username>/test.git
but replace <username> with your github username. 