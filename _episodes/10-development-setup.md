---
title: "Development Setup"
teaching: 0
exercises: 0
questions:
- "What are the prerequisites for installing ESMValTool?"
- "How do I confirm that the installation was succesful?"
objectives:
- "Execute a succesful ESMValTool installation"
keypoints:
- "ESMValTool is installed from source code that lives in the [GitHub repository](https://github.com/ESMValGroup/ESMValTool)"
- "All the required packages can be installed using conda and the [environment.yml file](https://github.com/ESMValGroup/ESMValTool/blob/master/environment.yml)"
- "You can find more information about installation in the [documentation](https://esmvaltool.readthedocs.io/en/latest/getting_started/install.html)"
---

So you want to contribute code to ESMValTool, what follows describes a development installation to help you get going.

> ## Attention
>
> -   This episode is based on the ESMValTool installation instructions, for more information and advanced cases you can visit the  ESMValTool [documentation](https://esmvaltool.readthedocs.io/en/latest/getting_started/install.html). 
> -   For this episode it is assumed you have knowledge of [git](https://git-scm.com/) you can refresh your knowledge in the corresponding [git carpentries course](http://swcarpentry.github.io/git-novice/). 
{: .callout}

## Obtaining the source code

The ESMValTool source code is available on a public GitHub repository:
https://github.com/ESMValGroup/ESMValTool

The easiest way to obtain it is to clone the repository using git. 
To clone the public repository open a terminal window and type:

~~~
    git clone https://github.com/ESMValGroup/ESMValTool.git
~~~
{: .source}

By default, this command will create a folder called ESMValTool containing the
source code of the tool. 

Move into the directory to continue installation. 

> ## Attention
> 
> Make sure that the master branch is checked out before continuing intallation:
> ~~~
> git checkout master
> ~~~ 
> {: .source}
{: .callout}

## Prerequisites

It is recommended to use conda to manage ESMValTool dependencies.
For a minimal conda installation go to https://conda.io/miniconda.html. To
simplify the installation process, an environment definition file is provided
in the repository (``environment.yml`` in the root folder).

> ## Attention
> It is preferable to use a local, fully user-controlled conda installation.
> If you have conda installed already, make sure it is up to date by running ``conda update -n base conda``.
{: .callout}

From now on, we will assume that the installation is going to be done through
conda.

Ideally, you should create a conda environment for ESMValTool, so it is
independent from any other Python tools present in the system.

To create an environment, go to the directory containing the ESMValTool source
code (called ESMValTool if you did not choose a different name) and run

~~~
    conda env create --name esmvaltool --file environment.yml
~~~
{: .source}

The environment is called ``esmvaltool`` by default, but it is possible to use
the option ``--name ENVIRONMENT_NAME`` to define a custom name. You can activate
the environment using the command:

~~~
    conda activate esmvaltool
~~~
{: .source}

If you run into trouble, please try recreating the environment. More information can be found in the (conda documentation)[https://docs.conda.io/en/latest/].


## Software installation

Once all prerequisites are fulfilled, ESMValTool can be installed by running
the following commands in the directory containing the ESMValTool source code:

~~~
    pip install .
~~~
{: .source}

## Test the installation

The next step is to check that the installation works properly.
To do this, run the tool with:

~~~
    esmvaltool --help
~~~
{: .source}

If everything was installed properly, ESMValTool should have printed a
help message to the console.

For a more complete installation verification, run the automated tests and
confirm that no errors are reported:

~~~
    python setup.py test --installation
~~~
{: .source}

{% include links.md %}

