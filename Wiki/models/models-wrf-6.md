# Current WRF porting to NCI

These are the steps to install new WRF versions on the NCI machine, intended only for admins within the `wrf` group at NCI. If starting from
scratch and assuming WRF V4.4.1:

  * git clone --recursive git@github.com:coecms/WRF.git 
  * cd WRF 
  * git checkout -b V4.4.1   
    ```{note}
    Name of branch needs to follow the format VX.Y(.Z) 
    ```
  * git remote add ncar-wrf git@github.com:wrf-model/WRF.git 
  * git fetch ncar-wrf 
  * git merge -s recursive -Xsubtree=WRF ncar-wrf/master   
    ```{note}
    This will get the latest version. 
    ```
  * Resolve any conflicts from the merge. For conflicts of scientific nature, ncar changes override. For
	NCI-related conflicts, local branch differences override.
  * cd WRF
  * ./run_compile 
  * git push -u origin V4.4.1
