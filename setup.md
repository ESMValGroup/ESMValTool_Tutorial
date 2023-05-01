---
title: Preparations for participating in the tutorial
---

This page includes some information on how to prepare for participating in this tutorial.

> ## Prerequisites
>
> *Minimal requirements:*
>  - Basic understanding of your preferred command line interface (ie a bash terminal)
>  - Access to CMIP data
>
> *Optional, but useful:*
>  - Basic understanding of git
>  - Access to a suitable computing system (eg CEDA-Jasmin, DKRZ-Mistral)
>  - GitHub account
{: .prereq}


## Command line & git tutorials

We typically use the command line to interact with ESMValTool. While most of us
are likely to have experience with the command line, novices may want to work
through this software carpentry unix shell course.

- Command line:
  [https://swcarpentry.github.io/shell-novice/](https://swcarpentry.github.io/shell-novice/)


Git is a distributed version-control system for tracking changes in source code
during software development. It’s how we distribute, share, and manage the
ESMValTool code.

- git:
  [https://swcarpentry.github.io/git-novice/](https://swcarpentry.github.io/git-novice/)


## Access to CMIP and Observational data and a suitable compute cluster

To complete this tutorial and use ESMValTool, you will need access to data in a
reasonable format. Some data will be provided, but there is simply too much data
available for your tutors to make it all available directly.

ESMValTool may be run on multiple platforms, from your local machine to large
computing clusters. The best option is to use a computing cluster with an [Earth
System Grid Federation (ESGF) node](https://esgf.llnl.gov/nodes.html). The
benefit of using a compute cluster with an ESGF node is that the [Coupled Model
Intercomparison Project
(CMIP)](https://en.wikipedia.org/wiki/Coupled_Model_Intercomparison_Project) is
locally stored on disk and accessible directly by the tool. Similarly,
observational data would also be available at these sites.
The ESGF also hosts observations for Model Intercomparison Projects (obs4MIPs)
 and reanalyses data (ana4MIPs).

Here are a few options for compute clusters with ESGF nodes:

- [CEDA-Jasmin (UK)](#ceda-jasmin)
- [DKRZ (Germany)](#dkrz)

For more information see:

- [CMIP5](https://pcmdi.llnl.gov/mips/cmip5/index.html) and
[CMIP6](https://pcmdi.llnl.gov/CMIP6/Guide/dataUsers.html) data obey the
[CF-conventions](http://cfconventions.org/). Available variables can be found
under the [CMIP5 data request](https://pcmdi.llnl.gov/mips/cmip5/docs/standard_output.pdf?id=28)
and the [CMIP6 Data Request](http://clipc-services.ceda.ac.uk/dreq/index.html).

- List of all
[CMIP named variables](http://clipc-services.ceda.ac.uk/dreq/index/CMORvar.html).

- List of all [ESGF nodes](https://esgf.llnl.gov/nodes.html).

- A good 
[tutorial](https://esgf.github.io/esgf-user-support/user_guide.html#data-search-and-download)
on how to search and download CMIP data from ESGF nodes.

- [Exploring climate model data](https://climate4impact.eu/impactportal/data/esgfsearch.jsp)
on infrastructure for the European network for Earth system modelling.

### CEDA-Jasmin

Please skip this section if you are not going to use JASMIN
and go [here](#github-account-advanced).

If you do not already have an account on JASMIN, then request an account as soon
as possible. Please follow [these instructions on how to create a Jasmin
account](https://help.jasmin.ac.uk/article/4435-get-a-jasmin-account-portal)

During the account creation, you will need an SSH key, which can be generated
following [these
instructions](https://help.jasmin.ac.uk/article/185-generate-ssh-key-pair)

Here are some more [instructions on how to get started with
jasmin](https://help.jasmin.ac.uk/article/189-get-started-with-jasmin).

Also note that if you are working from home, JASMIN may not be directly
accessible from your home. You may need to use ssh to connect to a machine in
your institute and then on to JASMIN. Please test your connection before the
tutorial.

#### Jasmin-login

Note that you have only created an account for the web-interface. To log into
the jasmin machine and do work, you'll need to create a login account too using
[this page](https://help.jasmin.ac.uk/article/161-get-login-account).

#### Access to data on JASMIN

Please request access to the working groups:
- [esmeval working
  group](https://accounts.jasmin.ac.uk/services/group_workspaces/esmeval)
- [CMIP5
  data](https://services.ceda.ac.uk/cedasite/resreg/application?attributeid=cmip5_research)

Once you have access to the data archive on CEDA, make sure to link your CEDA
and JASMIN accounts. This can be done by checking the link to CEDA box on [your
JASMIN profile page](https://accounts.jasmin.ac.uk/account/profile/).

The linking may take a few hours to take effect and is necessary for you to
access the BADC archives via JASMIN. Some CMIP5 data sets such as MIROC are not
accessible by default and special permission has to be requested to access them
via [the CEDA catalogue page](https://catalogue.ceda.ac.uk/).

#### Test your Setup

Log into jasmin-login:

~~~
ssh -X JASMIN-USERNAME@login1.jasmin.ac.uk
~~~
{: .language-bash}

Then log into the sci1 machine:

~~~
ssh -X jasmin-sci1
~~~
{: .language-bash}

Can you see the following locations:
~~~
ls /gws/nopw/j04/esmeval/obsdata-v2/
ls /badc/cmip5/data/cmip5/output1/MOHC/HadGEM2-ES
ls /badc/cmip6/data/CMIP6/CMIP/*/*/historical/r1i1p1f?/Omon/[ts]os/gn/latest/*.nc
~~~
{: .language-bash}

Note that the JASMIN is only open to certain locations (mostly universities, and
research centres). You may need a VPN if you wish to connect from your home
network.

Please request access to the working groups:
- [esmeval working
  group](https://accounts.jasmin.ac.uk/services/group_workspaces/esmeval)
- [CMIP5
  data](https://services.ceda.ac.uk/cedasite/resreg/application?attributeid=cmip5_research)

Once you have access to the data archive on CEDA, make sure to link your CEDA
and JASMIN accounts. This can be done by checking the link to CEDA box on
[your JASMIN profile page](https://accounts.jasmin.ac.uk/account/profile/).

The linking may take a few hours to take effect and is necessary for you to
access the BADC archives via JASMIN. Some CMIP5 data sets such as MIROC are not
accessible by default and special permission has to be requested to access them
via [the CEDA catalogue page](https://catalogue.ceda.ac.uk/).

Congratulations! Please go here [here](#gitHub-account-advanced) next.


### DKRZ

Please skip this section if you are not going to use DKRZ and go [here](#github-account-advanced).

If you do not already have an account at the DKRZ, then
[register](https://luv.dkrz.de/projects/newuser/) as soon as possible. You could
find a short introduction on how to get started at DKRZ
[here](https://docs.dkrz.de/doc/getting_started/index.html).

There is also a [user manual](https://docs.dkrz.de/doc/levante/index.html) for
Levante which is DKRZ's current supercomputer.

#### Join a project

To use the resources on DKRZ you have to join a project. One option is to join
an existing project by logging into [https://luv.dkrz.de/](https://luv.dkrz.de/)
with your account and select 'Join existing project'. Once you are accepted by
the manager of your chosen project, your web account will be turned into a full
LDAP account which will allow you to log into and use the DKRZ's resources. If
you do not have access to an existing project, another option for you would be
to apply for resources at DKRZ. Here are some instructions on [how to apply for
resources](https://www.dkrz.de/services/bereitstellung-von-rechenleistung?set_language=en&cl=en).

#### Access to data on DKRZ

CMIP5 and CMIP6 data are available in these directories:
- CMIP5: /work/kd0956/CMIP5/data/cmip5/output1/
- CMIP6: /work/ik1017/CMIP6/data/CMIP6/CMIP/

#### Test your Setup

Log into Levante (DKRZ)

~~~
ssh -X user-account@levante.dkrz.de
~~~
{: .language-bash}

#### Additional information

Login nodes are for compiling and job submission only! For all other tasks, you
can use the 
[interactive](https://docs.dkrz.de/doc/levante/running-jobs/
partitions-and-limits.html#interactive)
partition or start an 
[interactive session](https://docs.dkrz.de/doc/levante/data-processing-on-levante.html#).

Data storage:
- Personal data: home directory (30GiB)
- Project data: /work/project_id/username
- Temporary data: scratch directory on /scratch/\*/username is
  automatically deleted after 14 days (15TiB) (Please use this directory for all
  your testing! Do not use the work directory for tests.) (see also
  [this](https://docs.dkrz.de/doc/levante/file-systems.html))

Running batch jobs: Info and examples on SLURM job scheduling system at DKRZ can
be found [here](https://docs.dkrz.de/doc/levante/running-jobs/index.html).

Congratulations! Please go here [here](#github-account-advanced) next.

### Using your own machine

Please skip this section if you are not going to use ESMValTool on your
local machine and go [here](#github-account-advanced).

If you are planning on running ESMValTool on your own machine, please make sure
that you are able to download CMIP data and that you have several GB of space
available to install conda and ESMValTool, but also enough to make a copy of some
data (~125MB).

To download the data required by this tutorial, run the following command using
[wget](https://en.wikipedia.org/wiki/Wget):

~~~shell
wget --no-clobber --input-file \
  https://github.com/ESMValGroup/ESMValTool_Tutorial/raw/main/data/dataset.urls \
  --directory-prefix $HOME/esmvaltool_tutorial/data/
~~~
You can also use ESMValTool to automatically download data needed for test recipes. 
Please see the [Configuration][lesson-configuration] episode 
or the [configuration file documentation][config-file] for more information.


## GitHub account (Advanced)

You don’t need a github account to participate in the tutorial. However, if you
want to raise an issue, contribute to the discussions, or share your code,
please [create a github account](https://github.com/).

To learn how to use github, please have a look at this [introduction to
github](https://lab.github.com/githubtraining/introduction-to-github).

You may hear a few of the following phrases during the tutorial. Don’t be
alarmed, they will make sense eventually.

### GitHub issues

Issues are github’s ticketing system. They allow users and developers to discuss
problems, identify bugs, or to make suggestions. Each issue is assigned a number
and will have it’s own page on GitHub.

Here’s an explanation of the [GitHub
issues](https://guides.github.com/features/issues/).

Raising an issue is the act of creating a new issue. If you are asked to raise
an issue, please follow any instructions that you are given, and also make sure
that you read the default issue text.

### GitHub pull requests

A GitHub pull request is the act of requesting that a branch is merged with another branch.

This is an advanced feature of GitHub, and will generally be performed by the
ESMValTool development team.

{% include links.md %}
