# Data FAQs


## Where should I publish my data and code?

You usually have several options and there's not a straight answer it depends on what you're publishing and why.

We listed some of them in our [[Publishing_options|Publishing options]] page

If you're confused feel free to ask us, we are always happy to provide advice and support. Whatever you choose to do remember to report the title, citation and doi on Clever. Published datasets are part of the Centre KPIs and something we report to our funders. It will only take a couple of minutes.

## Do CMIP5/CMIP6 variables coming from the same simulations have the same version number?

For CMIP5 there's no way to be 100% sure that two versions come out of the same simulation. The versioning instructions where quite unclear and interpreted differently by different modelling groups. In the last couple of years, it occurred to few groups (for example GFDL) to add a `simulation_id` to their attributes but this is the exception rather than the norm. I'd like to assume that same version means the same simulation, but having a different version really just means that the group of variables has been published later, or maybe that one of them was calculated wrongly in the post-processing and has been re-published under a new version. A completely different simulation with a different configuration, initialization etc should have a different ensemble code so anything with `r1i1p1`, for example, should come from the same run, even if part of it or its post-processing might have been updated. More information might be available directly from the modelling groups. 

In CMIP6 again the way versions ar eapllied is incnsistent but you can now use the tracking_ids to actually trace a file provenance, [ESDOC](https://search.es-doc.org/) gives you full information on model simulations and each file, simualtion, model, experiment has an handle you can use to track down information.

## Using the CLEX Roadmap data management tool is not useful for me as I don't use NCI.

Using Roadmap is really independent of NCI, while it contains some specific information on NCI systems since they are common to many in the Centre, it is more about data management in general, regardless which hardware you use. Even if you are using your laptop it is good to have a data workflow, a plan of what you will be doing at different stages of your research. For example, to publish an RDA record for your data with us, you would now fill one of these plans, and the advantage would be that you can easily export that as a document which you can always adapt and re-use.

DMP will be compulsory for universities and ARC grants, publishing your data is already compulsory for most journals. Plus, at CMS we really want to hear from users that are not using NCI, users we don't normally hear from. So we get a better idea of what everybody in the Centre is doing and we can create new training resources or support all our users in a better way.