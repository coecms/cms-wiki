# Tips and Tricks


## Unstable WRF simulation

If you notice that the WRF model has become unstable you should consider making the following changes in the namelist.input file:

  * reduce the time-step; and/or 
  * set w_damping to 1; and/or 
  * set epssm (0.1 by default) to a higher value (e.g. 0.3) 
  
The changes above indeed improved the stabilities in our case.{br} 
You can also explore the following ideas:

  * reduce the total number of processors 
  * change the decomposition through nproc_x and nproc_y options 
  * reduce the level of optimisation via re-compilation 
  
## Runtime segmentation error

WRF will always crash with a segmentation error when compiled without debugging information, but you should make sure you always run WRF with an unlimited stacksize. This should be set in your run script:

  * For tcsh `limit stacksize unlimited`
  * For bash `ulimit -s unlimited`

```{note}
If you have some tricks that can help WRF users in our community please email cws_help@nci.org.au.
```
