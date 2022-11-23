# CMS help

The CMS team supports CLEX members with computational issues: 

+ **Running Models**: we support several models (see navigation bar on the left) and assist with others, subject to limitations of scope and team resources. 
+ **Data Analysis**: we assist with analysis and general computing tasks on NCI machines, and offer only limited support on other sites or home machines as we possess neither control over nor familiarity with such systems.
+ **Data Access and publishing**: we offer assistance in publishing data and accessing climate data available at NCI.
The CMS team can be contacted for support through: 

+ The Climate Weather Science (CWS) helpdesk cws_help@nci.org.au
+ One-on-one videocall
+ In person meetings with local CMS team members
+ CodeBreak

The CWS help desk is the preferred contact method as it allows the CMS team to prioritize queries so as to set aside time for other CMS duties, provides transparency of open requests to all CMS team members , and gives the CMS team flexibility to cover for team absences. Simply outline your issue in an email to cws_help@nci.org.au to open a ticket on our helpdesk.

## Guidelines for help requests

The following are tips on how to make your request more effective and easier for us to answer quickly.

### Before asking

+ Follow the troubleshooting guide
+ Read relevant documentation on our wiki or blog
+ Google it !

### When creating the request 

+ Reduce your query if possible to a minimal reproducible example ([Stack Overflow has a nice description of what this means](https://stackoverflow.com/help/minimal-reproducible-example))
+ Submit separate help requests for significantly new queries instead of appending to existing tickets
+ Ensure required directories and/or files have [group permissions](http://climate-cms.wikis.unsw.edu.au/Tips:_Custom_file_permissions_at_creation) that make them accessible to us. For example, NCI /home directories by default are not, so you will need to change their permissions, but typically we can access the NCI /scratch and /g/data for your group
+ Include all relevant information in your help request:
    + If a script/command generates an error, provide an exact reproduction of the script/command typed and error message
    + What have you tried so far to troubleshoot
    + Background info about your task (as we might be able to suggest instead a different approach) with links to any references invoked in your request
    + Platform used for your task (e.g., your machine/OS, NCI Gadi, OOD, accessdev). If NCI, supply your username, and if Gadi indicate if the issue was in a login session or the PBS compute nodes
    + Specify any environment variables set and modules loaded, and if loaded in your .bashrc or .bash_profile files (not recommended). For errors with the conda environment we support, include the environment version. 
    + Upload any required code to [gist.github.com](https://gist.github.com/) or create a [GitHub repository](https://docs.github.com/en/free-pro-team@latest/github/getting-started-with-github/create-a-repo) for more complex examples. If barred by licensing issues, put it on NCI and provide the path. Never send copies of your code.
    + Do not send code or error messages as screenshots or poorly formatted text

### After

+ You should receive a reply to your query within 24 hours during normal business hours.
+ We will provide a solution to your problem (if within scope), or a work-around, or an explanation for why it is currently unsolvable
+ You should expect updates required for current tickets to take at most a week during non-holiday periods
+ Consider if you need some [training](../training/training-intro.md) to improve your skills

If any of the above expectations are not being met, please contact the CMS team.
