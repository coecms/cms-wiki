# NCI `scratch` file management policy

NCI has introduced a file management policy for `/scratch` to make sure this valuable storage space is used efficiently and shared in a fair manner among all users.
Based on this policy NCI will automatically clean up files not accessed for more than 100 days. 

The removal of files happens in 3 steps:

1.  Files are moved to a quarantine space, the file still exists but it is not accessible
2.  Files stay in quarantine for 14 days, during which they can be recovered by the owner.
3.  At the end of the qurantine period files are deleted and cannot be retrieved anymore.

## Managing `scratch`

It is important to be aware of this policy and take steps to actively manage your files on `scratch` to avoid losing them.
Below are some steps we recommend to do regularly:

* Read the [information provided by NCI](https://opus.nci.org.au/pages/viewpage.action?pageId=156434436)
* Gadi shows, on login,  a summary of files which are about to expire or that are quarantined. If there are any you can get more information running `nci-file-expiry`.
* Run 
  ```{code}
  nci-file-expiry list-warnings -p <project> > file_list.txt
  ``` 
  for the listed projects to get the file paths.
  If you identify anything here that is important and at risk of deletion, decide if it should be put on `gdata` or `tape` instead.
* Run
  ```{code}
  nci-file-expiry list-quarantined -p <project> > file_list.txt
  ``` 
  for the listed projects to get the file paths.
  If you identify files you want to recover you can run
  ```{code}
  nci-file-expiry recover <UUID> <output-path>
  ```
  where `UUID` is the file id from the list-quarantined output.
* Rethink your data pipeline: do not leave data you are not using anymore in `scratch`. Decide what you need to do with it when you stop using it: delete, move to `gdata` or outside NCI or archive to tape.
* Manage your data under `gdata` regularly: review your data, archive to tape or outside NCI as necessary.

## Additional information

Below is a list of resources you might find useful to prepare for the automatic file expiry and build a good data workflow for the future.

* Archiving data at NCI

* [Building a sustainable data workflow](https://coecms.github.io/posts/2022-04-26-storage-where-what-why-how.html) blog

* Description of the various filesystems at NCI



## Long term strategy

* Keep using `scratch`. There is not enough disk space if nobody is using `scratch`. Your data is safe on `scratch` as long as you access it, i.e., read, modify, create. Only the data you won't use for some time needs to be managed.
* Managing your data does not mean storing everything on `gdata`. You need to think about what future use you have for that data. If you don't need it anymore and it can be reproduced, delete it. If you will need to publish the data, publish it now. It is a lot easier to publish a new version if you need a small change than to publish the initial version. For information, on what data needs to be published see the [CLEX Data Policy](../resources/resources-policy.md) and Which_data_should_I_publish page. If you can't publish now but won't need it for a long time, put the data on tape. If you won't need this data for a short time but you know you will get back to it soon, put it on `gdata`.
* Learn how to use the tape system. See how to use the MDSS tape system or  our blog.
* Try and avoid your data getting into quarantine. Make managing your data a periodic, frequent task in your calendar. This includes all data: `scratch`, `gdata` and `tape`.
* Be careful when going on leave! Make sure to check your data holdings under `scratch` before your leave.
  For extended leave (e.g. ship cruises, parental leave, sabbatical), do not leave any important data under `scratch`.


!!!COMMENT: I'm not adding links to the old wiki pages yet, as they will have to migrate here or elsewhere!
