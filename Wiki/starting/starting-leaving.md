# Leaving CLEX guidelines

When leaving the Centre it is important to document and organise the data and code you are leaving beyond. This is both to ensure that unecessary files are removed and storage freed up in shared projects and that anything of value to your future self and/or others is properly stored and documented. 

These guidelines are provided to cover the most common workflow. 

You should not own any data on disk when you leave or shortly after you leave
Any data you own on tape should be accessible for read and write by other people

## The Exit Email

About 2 months before you leave, you should receive the following email:

```{dropdown}
Dear xxxx,

Given that your contract is due to end in 2 months time, we ask that you start considering how data in your accounts will be managed in future. Specifically:
- please delete anything that is not required
- please send an email to the CMS team (copied on this email), detailing the nature of what will remain. This should outline major directories and explain:
	- what they contain 
	- why they’re important to keep
	- what is required to access them - which repository / scripts?
	- if this data is stored anywhere else
	- when can they be deleted (date)
	- where this data should be stored when your account is shut down (please consult with a CLEX CI)
	- who in CLEX should have access to this data?
	- please also let the CMS team if compute access will be required after your contract ends
 - please reply to both the admin and CMS teams with future contact details when you have them

The CMS team can help with tools and techniques to help you manage your data. Please understand that storage is extremely limited, so that a lack of response to the questions above will likely result in your data being archived as soon as your contract ends.

Regards,

The CLEX Admin Team
```

While you will receive the email above to remind you what needs to be done, sorting through your files might take longer than you think and it is important to be organised at any stage of your project. So please consider running the suggested actions at different stages of your project whne appropriate.

You might need to discuss the following questions with your supervisor:

* Will you require access to the files after you leave (e.g. for a paper review)?
* What files will be useful to others? Should they be published?
* What files won't be useful to others? Should they be archived or deleted?

```{note}
If you need specific advice on how to actually transfer or archive your files, you can always ask for assistance to our helpdesk: cws_help@nci.org.au.
```

## What to do

While what exactly should be kept, removed or published will depend on your specific project and working group. There are some principles that can be applied to any situation.
 
### To keep, always

**Codes**

No matter what the details of your projects are you should always keep the code you used. This means keeping a reference to the exact version and document how you used codes developed by others (e.g. climate models). If you modified them in any way this should also be documented. 

If you modfication are extensive or you have written the code yourself then you should make it available via GitHub and publish it (Zenodo is the most indicated for this).

```{note}
It is absolutely fine to create a repository per project or paper with all your codes in. You can then use the README file from the GitHub repository to explain how to reproduce your results. Don't forget to clearly reference everything one might need in addition to this repository. Or you can have a repository per code especially if you envision you'll reuse the same code for other work.
```

**Configuration files for running the codes and some input files**

In addition to the codes themselves, you need to keep everything that enables someone to run the codes in the same way you have done so. Usually, the most complicated configurations are for climate models. Some climate models will save your configurations in version control repositories (e.g. UM, ACCESS, ACCESS-OM2), in which case you simply need to keep the information on how to retrieve these configurations. Some models don't save your configurations and you need to do it yourself.

For the input files, some inputs are published data in which case you need to keep the reference to this data (including the version). If you have written several codes, the output of a piece of code will be the input of the next piece of code, in which case you do not necessarily need to keep that data,  but you need to describe your workflow.

**Workflow**

This can be a tricky one, as there is not a one-size-fits-all format to save this information. It is fine to write a README file and archive it with other files from the project that need archiving. You can also have a special Github repository just for this README file, or a repository for all your projects with READMEs for each project. Whatever format you choose, it is important for this information to be publicly available (unless your project was restricted) and not a personal note.

This description should clearly describe step by step what someone should do to reproduce your work.

```{warning}
Information on your PhD or published paper has to be kept for at least 5 years. The time requirements differ slightly depending on institutions and funding bodies.
```

### To delete, always

You do not need to keep files that are not necessary to reproduce your work:

* log files
* failed experiments
* temporary files such as created from successive cdo/nco commands.

```{tip}
Occasionaly log files might have some information which is relevant to the workflow, still just keeping them makes this information virtually inaccessible to anyone else. The right thing to do in this case is to extract what is useful and add it to the workflow documentation, then getting rid of the files.
```

### To keep or to delete?
Some of the data is harder to categorise, and need some careful consideration to decide if it needs to be kept or deleted. Neglecting to do so might result in the deletion of useful data or waste of storage

**Model outputs**

These are usually reproducible, even if it might not be possible to get bitwise reproducibility if underlying libraries change or the machine you have used is retired. 
Such a decision should be weighed against the cost, in money and time, of reproducing your work.
On the other side storing the data can also be expensive, as the model output are usually large,  and of little use if not done properly.
That is where you need to discuss with your supervisor, and possibly other collaborators, as the answer might vary depending on how likely someone else might find this data useful.
As model output includes raw output, restart files and post-processed output, they should all be treated differently. usually if keeping raw output this is stored on tape, it is a bit harder to retrieve but tape is less expensive than other storage. Only some of the restart files are kept (if any). Ideally, post-processed should be published if it has been used for publication or if it's going to be shared with others. 

**Output of lengthy processing**

 It might feel necessary to keep those files but we would argue it is usually not worth the cost of the storage unless there is a clear indication they will be used again soon. The first reason is that with current modern programming techniques a lot of lengthy processing can be shortened significantly. The second reason is the very real cost of the storage is not worth the very hypothetical time saved in the future.


## Publishing data and code

You should publish your files if the files are frequently used by several other persons. Ideally, that data should be published as soon as its usefulness to others is clear, not just when you leave.

If your files are at NCI, that is all you need to do. If your files are held at your institution, you may need to make sure there is a copy that is accessible by others and owned by someone who is likely to stay for years to come. This might depend on your institution data services.

**Sharing files without publication**

Some files might be useful to a small cluster of people but not be worth publishing. In that case, you simply need to change the ownership of the files: get the files to be owned by someone who stays after you.

If the files are at NCI, you can not change the files' ownership. You and the Lead CI of the project owning the files will need to contact help@nci.org.au so NCI staff can do it for you.

```{warning}
Often researchers shared data with others without publishing it even when this data is used for new publication, this should be avoided at any cost. It does create an issue for the creator of the data who won't get recognition and for the user who might struggle to refer to the exact data when publishing a paper. As it is now a requirement to publish data associated with a paper it is always bets to publish before sharing!
```

## Your NCI account

When you leave, you will keep your NCI credentials IF you keep your contact details up to date through https://my.nci.org.au

Make sure to quit all NCI projects you don't need access to anymore. You can be ruthless as it is quite easy to regain access to those projects later on if needed.

Some projects have strict license terms attached (e.g. access). Your membership might be revoked at any time after you leave the Centre if you haven't negotiated extended access to those projects. Please contact the Lead CI or the project manager to discuss your needs.

**Transferring files**

If you need to transfer files to a different machine (for example from NCI to your University or personal computer):

use sftp, scp or rsync to transfer files securely ( rsync can be resumed )
if transferring from/to NCI:
use the dedicated data-mover nodes if transferring from/to NCI: g-dm.nci.org.au
use copyq if you want to queue a job

## Institutional account

Most universities will close your university and e-mail account. Often university data services are accessible only via your university account. If that is the case, you need to arrange access for yourself by contacting the IT services or the CI of projects that you used to deposit data before you leave. Specifics on university data services and advice on what happens when you leave can be found following the relevant link in the data services page.


