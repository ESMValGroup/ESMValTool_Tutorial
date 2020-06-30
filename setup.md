---
title: Preparations for participating in the tutorial 
---

This page includes some information on how to prepare for participating in this tutorial. 

> ## Prerequisites
> The prerequisites for the tutorial are listed on the 
> [tutorials home page]({{ page.root}}[% index.md %})
>  and are also eproduced here:
> - Basic understanding of git (optional)
>
> - Basic understanding of your preferred command line interface (ie a bash terminal)
>
> - Access to CMIP data
>
> - Access to a suitable computing system (eg Jasmin, DLR machine)
>
> - Github account (optional)
{: .prereq}


## Optional tutorials

Where available, we include links to other software carpentry courses. 

### Command line

We typically use the command line to interact with ESMValTool. 
Here’s a software carpentry tutorial on the unix shell:
[https://swcarpentry.github.io/shell-novice/](https://swcarpentry.github.io/shell-novice/)

### Basic understanding of git
 
Git is a distributed version-control system for tracking changes in source code during software development. 
It’s how we distribute, share, and manage the ESMValTool code. 

There are many basic introductions to git available.
 
Here’s a software carpentry tutorial on git:
[https://swcarpentry.github.io/git-novice/](https://swcarpentry.github.io/git-novice/)



## Access to CMIP and Observational data and a suitable compute cluster

To complete this tutorial and use ESMValTool, you will need access to data in a reasonable format.
Some data will be provided, but there is simply too much data available
for your tutors to make it all available directly. 

ESMValTool may be run on multiple platforms, from your local machine to 
large computing clusters. 
The best option is to use a computing cluster with an
[Earth System Grid Federation (ESGF) node](https://esgf.llnl.gov/nodes.html). 
The benefit of using a compute cluster with an ESGF node is that the 
[Coupled Model Intercomparison Project (CMIP)](https://en.wikipedia.org/wiki/Coupled_Model_Intercomparison_Project)
is locally stored on disk and accessible directly by the tool.
Similarly, observational data would also be available at these sites.

Here are a few options
for compute clusters with ESGF nodes:

- CEDA-Jasmin (UK):
- DKRZ (Germany):
- ETHZ (Switzerland):

A full list of all ESGF nodes is available [here](https://esgf.llnl.gov/nodes.html).


If you're running on a computing cluster without an ESGF node, such as 
your local machine, or your institue machine, you will most likely 
have to make a local copy of the data that you need.

If neccesairy, data can be downloaded using the 
[synda tool](https://prodiguer.github.io/synda/index.html).
 


### CEDA-Jasmin

If you do not already have an account on JASMIN, then request an account as
soon as possible. 
Please follow [these instructions on how to create a Jasmin account](https://help.jasmin.ac.uk/article/4435-get-a-jasmin-account-portal)

During the account creation, you will need an SSH key, which can be generated following 
[these instructions](https://help.jasmin.ac.uk/article/185-generate-ssh-key-pair)

Here are some more [instructions on how to get started with jasmin](https://help.jasmin.ac.uk/article/189-get-started-with-jasmin).

Also note that if you are working from home, JASMIN may not be directly
accessible from your home. You may need to use ssh to connect to a machine
in your institute and then on to JASMIN. Please test your connection before
the tutorial. 

#### Jasmin-login

Note that you have only created an account for the web-interface. 
To log into the jasmin machine and do work, you'll need to create a 
login account too using [this page](https://help.jasmin.ac.uk/article/161-get-login-account).


#### Access to data on JASMIN

Please request access to the working groups:
- [esmeval working group](https://accounts.jasmin.ac.uk/services/group_workspaces/esmeval)
- [CMIP5 data](https://services.ceda.ac.uk/cedasite/resreg/application?attributeid=cmip5_research)

Once you have access to the data archive on CEDA, make sure to link your
CEDA and JASMIN accounts. 
This can be done by checking the link to CEDA box on 
[your JASMIN profile page](https://accounts.jasmin.ac.uk/account/profile/).

The linking may take a few hours to take effect and is necessary for you to
access the BADC archives via JASMIN. Some CMIP5 data sets such as MIROC
are not accessible by default and special permission has to be requested to
access them via [the CEDA catalogue page](https://catalogue.ceda.ac.uk/).

#### Test your Setup

Log into jasmin-login:

~~~
ssh -X JASMIN-USERNAME@jasmin-login1.ceda.ac.uk
~~~
{: .language-bash}

Then log into the sci1 machine:

~~~
ssh -X jasmin-sci1
~~~
{: .language-bash}

Can you see the following locations:
~~~
ls /group_workspaces/jasmin4/esmeval/obsdata/Tier2
ls /badc/cmip5/data/cmip5/output1/MOHC/HadGEM2-ES
ls /badc/cmip6/data/CMIP6/CMIP/*/*/historical/r1i1p1f?/Omon/[ts]os/gn/latest/*.nc
~~~
{: .language-bash}

Note that the JASMIN is only open to certain locations (mostly universities, and research centres). 
You may need a VPN if you wish to connect from your home network.

### DLR 
FIXME

### Other computing systems
FIXME

### Your own machine

If you are planning on running ESMValTool on your own machine, please make sure that you are able to download CMIP data and that you have several GB of space available to install conda & ESMVAlTool, but also enough to make a copy of some data. 

You will also need to able to use:
- git
- conda
- synda

#### Linux/unix

#### Mac OSx

#### Windows

ESMValTool does not directly support Windows, 
but successful usage has been reported through the 
[Windows Subsystem for Linux(WSL)](https://docs.microsoft.com/en-us/windows/wsl/),
available in Windows 10.



## Github account (Advanced)

You don’t need a github account to participate in the tutorial. 
However, if you want to raise an issue, contribute to the discussions, 
or share your code, please [create a github account](https://github.com/).

To learn how to use github, please have a look at this [introduction to github]
(https://lab.github.com/githubtraining/introduction-to-github).

You may hear a few of the following phrases during the tutorial. Don’t be alarmed, they will make sense eventually. 

### github issues
Issues are github’s ticketing system. 
They allow users and developers to discuss problems, 
identify bugs, or to make suggestions. 
Each issue is assigned a number and will have it’s own page on github. 

Here’s an explanation of the [github issues](https://guides.github.com/features/issues/).

Raising an issue is the act of creating a new issue. 
If you are asked to raise an issue, please follow any instructions that you are given,
and also make sure that you read the default issue text. 


### pull request 

A github pull request is the act of requesting that a branch is merged with another branch.

This is an advanced feature of GitHub, and will generally be performed by the ESMValTool development team. 



## Install conda

The python package manager Conda (anaconda or miniconda)
needs to be installed on your system before the tutorial starts. 
In some cases, your system may have a central version installed already. 

More details on this process are available in the 
[Installation episode]({{ page.root}}[% link _episodes/02-installation.md  %}). 



{% include links.md %}
