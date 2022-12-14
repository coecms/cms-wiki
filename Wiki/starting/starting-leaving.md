# Leaving CLEX guidelines

When leaving the Centre it is important to document and organise the data and code you are leaving behind. This arrangement ensures that unecessary files are removed, storage space is freed up in shared projects, and anything of future value to yourself or others is properly stored and documented. 

These guidelines are provided to cover the most common workflow. 

You should not own any data on disk when you leave or shortly after you leave
Any data you own on tape should be accessible for read and write by other people

## Your Exit Email Notification

About 2 months prior to your departure, you should receive the following email:

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

Although you will receive the above email reminder of what needs to be done, going through all of your files might take longer than anticipated, so it is important to remain organised at every stage of your project. Please consider then performing the following suggested actions at various stages of your project as needed.

You may need to consider the following questions with your supervisor:

* Will you require access to the files after you leave (e.g. for a paper review)?
* What files could be useful to others? Should they be published?
* What files won't be useful to others? Should they be archived or deleted?

```{note}
If you need specific advice on how to actually transfer or archive your files, you can request assistance from our helpdesk: cws_help@nci.org.au.
```

## What to do

While what to keep, remove, or publish, will depend on your specific project and working group, some universal principles are applicable to any situation.
 
### Always Keep

**Codes**

Regardless of your project details, always keep the code you used with a reference to its exact version, and document your usage of codes developed by others (e.g., climate models). Document also any modifications you made to these codes. 

If your modifications are extensive or you wrote the code yourself, you should make the code available via GitHub and publish it (preferably on Zenodo ).

```{note}
It is perfectly acceptable to create a repository per project or paper containing all your codes. You can then use the README file from the GitHub repository to explain how your results can be reproduced. Clearly reference everything needed in addition to this repository. You can also have a repository per code, especially if you anticipate you'll reuse the same code for other work.
```

**Configuration and other files necessary to run the code**

In addition to the codes, keep everything required to run them the same way you did. Usually, the most complicated configurations are for climate models. Some climate models will save your configurations in version control repositories (e.g. UM, ACCESS, ACCESS-OM2), in which case you simply need to keep the information on how to retrieve these configurations. Some models don't save your configurations and you need to do it yourself.

For input files, some are published data, in which case you need to keep the reference to this data (including the version). If you have written several codes, the output of one will be the input to the next, in which case you do not necessarily need to keep that data, but you should describe your workflow.

**Workflow**

Workflows can be tricky as there is no one-size-fits-all format to save such information. You could write a README file and archive it with other project files that need archiving, or have a special GitHub repository just for this README file, or a repository for all your projects with READMEs for each project. Whatever you choose, this information must be publicly available (unless your project was restricted), not a personal note, and clearly describe step by step what someone should do to reproduce your work. This description should clearly describe step by step what someone should do to reproduce your work.

```{warning}
Information on your PhD or published papers must be kept for at least 5 years. The time requirements differ slightly depending on institutions and funding bodies.
```

### Always delete

No need to keep files not necessary to reproduce your work:

* log files
* failed experiments files
* temporary files such as those created from successive cdo/nco commands.

```{tip}
Occasionally log files might contain information relevant to the workflow, but but this is hard to access from the files themselves. Accordingly, in this case extract what is useful, add it to the workflow documentation, and delete the files.
```

### Keep or delete?
Some data is harder to categorise, and deciding whether to keep it or delete it requires careful consideration to avoid deleting useful data or wasting storage space.

**Model outputs**

These are usually reproducible, although bitwise reproducibility may not be possible if underlying libraries change or the machine you have used has been retired. 
The decision to keep or delete model outputs must be weighed against the cost, in money and time, of reproducing your work.
On the other hand, data storage can also be expensive as model outputs are usually large, and useless if not done properly.
Discuss this matter with your supervisor, and possibly other collaborators, as the answer might vary depending on how likely someone else might find this data useful.
As model output includes raw output, restart files, and post-processed output, they should all be treated differently. Usually retained raw output is stored on tape even though it is a bit harder to retrieve, because tape is less expensive than other storage alternatives. Only some of the restart files are kept (if any). Ideally, post-processed output should be published if it has been used for publications or if it's going to be shared with others.

**Output of lengthy processing**

It is tempting to keep those files but usually not worth the storage cost unless it is clear they will be reused soon, as with current modern programming techniques a lot of lengthy processing can be significantly shortened, and the very real storage cost outweighs any hypothetical time savings in the future.


## Publishing data and code

Publish your files if they are frequently used by others, ideally as soon as their usefulness to others is evident, not just before you leave.

If your files are at NCI, there is nothing else to. If your files are held at your institution, you may need to make sure there is a copy accessible to others and owned by someone likely to stay for years to come, all subject to your institution data services.

**Sharing files without publication**

For files that might be useful to a small cluster of people but not worth publishing you simply need to change the ownership of the files: get the files to be owned by someone who is present after you leave.

You cannot change the ownership of files that reside at NCI. You and the Lead CI of the project owning the files need to contact help@nci.org.au so NCI staff can do it for you.

```{warning}
Researchers often share data with others without publishing it even when this data is later on used for a new publication. This practice should be avoided at any cost. It creates an issue for the creator of the data who won't get recognition, and for the user who might struggle to refer to the exact data when publishing a paper. As it is now a requirement to publish data associated with a paper, it is always best to publish before sharing!
```

## Your NCI account

When you leave you can retain your NCI credentials if you keep your contact details up to date through https://my.nci.org.au

Make sure to quit all NCI projects you don't need access to anymore. You can be ruthless as it is quite easy to regain access to those projects later on if needed.

Some projects have strict license terms attached (e.g., ACCESS). Your membership might be revoked any time after you leave the Centre if you haven't negotiated extended access to those projects. Please contact the Lead CI or project manager to discuss your needs.

**Transferring files**

If you need to transfer files to a different machine (for example from NCI to your University or personal computer) use `sftp`, `scp` or `rsync` to do it securely; `rsync` can be resumed if the transfer is interrupted.

If transferring from/to NCI:
- use the dedicated data-mover nodes: `g-dm.nci.org.au`
- use copyq if you want to queue a job

## Institutional account

Most universities will close your university and e-mail account after you leave. Often university data services are accessible only via your university account. In this case, you need to arrange access for yourself by contacting the IT services or the CI of projects that you used to deposit data before you leave. Specifics of university data services and advice on what happens when you leave can be found by following the relevant link in the data services page.


