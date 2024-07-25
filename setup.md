---
title: Pre-requisites
---

This page includes some information on how to prepare for participating in this Hackathon.

> ## Prerequisites
>
> *Minimal requirements:*
>  - Basic understanding of your preferred command line interface (ie a bash terminal)
>  - Access to Gadi
>  - Access to CMIP data
>
> *Optional, but useful:*
>  - Basic understanding of git
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

### NCI-Gadi


#### Gadi-login


#### Access to data on Gadi


#### Test your Setup


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
