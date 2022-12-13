# Gadi FAQs

::::{grid}
:gutter: 1

:::{grid-item-card}
````{dropdown} **Debugging**:
**How to debug issues with gadi jupyter**

The script submitted to PBS and the PBS logs are saved under the directory tmp/runjp directory under the user's /scratch directory for the group used to run the script. These will often have more information if the script was started alright but you can't seem to be able to connect to the Jupyer Notebooks. This usually happens when the job started alright but exited due to an error in the script.

For example, let's say I (ccc561) requests gadi_jupyter to run using the project w35. The information will then be stored under '''/scratch/w35/ccc561/tmp/runjp'''

If I then run gadi_jupyter again but this time running on w40, the information will be stored under '''/scratch/w40/ccc561/tmp/runjp'''

**Why do I get segfaults?**

A segmentation fault means the program you are using has tried to read or write a memory location it does not have access to. This can mean there is an error in the code, or there is a system limit set. If the program runs for others, but not you, it may well be a system limit issue. This seems to be quite common when trying to write netcdf files. In some cases this can be overcome by setting your stack size to unlimited

For tcsh/csh add this to your .cshrc:
```bash
limit stacksize unlimited
```

For bash add this to your .bashrc:
```bash
ulimit -s unlimited
```

Note: you must log out and log back in again for this change to take effect.

**I cannot see a project folder under /g/data and/or /scratch**

You need to be a member of a project to access or even just list the project folder. So if you cannot see a project folder, it doesn't mean that it doesn't exist or it was removed, it simply means that you are not a member of that project. Go to my.nci.org.au if you want to request access.

**Weird errors for a `script that was previously working`**

Make sure you have loaded the right modules and only them! Avoid to load modules directly in your .bash_profile, .bashrc files . They will be loaded every time and occasionally interfere with the modules you are loading for a specific task.

It is useful and safe to add instructions as

module use /g/data/hh5/public/modules

as this is making the modules available without loading any.

Sometimes you need to install a python module in your user space (`$HOME/.local/...`) as it is not available in a managed conda environment. Again, occasionally modules stored in your local user space can interfere with other loaded modules, for example if they have different dependencies. If you find this is causing issues you can exclude the local environment by setting this environment variable

`PYTHONNOUSERSITE=x (=True and =1 should also work as values)`

````
:::

:::{grid-item-card}
````{dropdown} **PBS Jobs**:
**How to run independent jobs in parallel**

If you have to run the same script with a lot of independent sets of inputs, or you have lots of similar scripts to run at NCI. And these scripts run on 1 processor. You could submit each of your jobs independently via PBS but then small jobs are not efficient at NCI. It would probably be better for you to have a master script that dispatches your jobs to available processors. You then submit this master script to PBS and this script requires a lot more processors and hence might be more efficient with the scheduler. There is an example of such a master script Embarrassingly_parallel_job here (no link yet).

**No files found or non-existent path error while running a PBS job**

When submitting a job on gadi you need to declare explicitly what gdata and/or scratch projects your script need to access.

This is done by setting the storage flag in the job you are submitting:

```bash
PBS -l storage=gdata/hh5+gdata/ua8+gdata/e14
```

In the above case I want to use input data from gdata/ua8 save my results to gdata/e14 and use the conda modules which are in gdata/hh5
````
:::

:::{grid-item-card}
````{dropdown} **Installing**:
**Issues installing a R module on gadi**

First of all, you might not need to install the module, as NCI is now managing some R environments which might include already what you need: https://opus.nci.org.au/display/DAE/Getting+Started.

If this approach doesn't work for you and you need to install a module, make sure you have all the necessary modules loaded. As outlined in the [Gadi user guide](https://opus.nci.org.au/display/Help/R) you need to load the same intel module that was used to install the R module you are using. If you still have issues, contacting the NCI Helpdesk, rather than us, is more efficient as they have more experience with R.

**I want to use Matlab on gadi**

You will need to setup your institutional Matlab license. We cannot help you troubleshooting any of this, just follow the [NCI instructions](https://opus.nci.org.au/display/Help/MATLAB) and contact their Helpdesk if you run into any issue.
````
:::

::::