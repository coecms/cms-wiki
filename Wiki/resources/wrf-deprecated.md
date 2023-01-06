# WRF porting to NCI machine (Deprecated)

These are the steps to follow to install new WRF versions on an NCI machine. Only for admins within the wrf group at NCI. If starting from scratch:

  * Under your own project, clone the [WRF github repo](https://github.com/coecms/WRF) and create a new branch for the new version (named Vx.x.x), starting from the most recent HEAD (not master)
  * Add `https://github.com/wrf-model/WPS.git` and `https://github.com/wrf-model/WRF.git`
	as remotes (named NCAR_WPS and NCAS_WRF if you want to copy-paste the commands) and fetch
  * In the new branch, run the command: 
  
``` shell  
git merge -s recursive -Xsubtree=WRFV3 --allow-unrelated-histories NCAR_WRF/master
```
	
You may need another branch than master to pick up a specific release.

  * You may need to update the crtm coeffs. You'll probably get a conflict on the file `WRFV3/var/run/crtm_coeffs`. This is a link to `/projects/WRF/data/WRFDA_files/crtm_coeff_X`. Check the version the remote points to. If different from local, [download the updates](http://www2.mmm.ucar.edu/wrf/users/wrfda/download/crtm_coeffs.html).
  * After resolving conflicts, run WRFV3/configure. See what options are correct for the machine architecture. Update WRFV3/run_compile appropriately. 
  * Clone into /projects/WRF with wrf group ownership. Make sure the master branch there points to the new version branch.

Follow same steps for WPS, but remote is called NCAR_WPS and -Xsubtree=WPS.

## Jenkins setup

  * Add a new parameter for the version. Just the numbers, no "V".
  * Create a branch for that version in wrf-testing repo. Exact same name as the previous step parameter.
