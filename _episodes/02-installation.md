---
title: "Installation"
teaching: 10
exercises: 20
questions:
- "What are the prerequisites for installing ESMValTool?"
- "How do I confirm that the installation was successful?"
objectives:
- "Install ESMValTool"
- "Demonstate that the installation was successful"
keypoints:
- "All the required packages can be installed using conda"
- "You can find more information about installation in the documentation"
---
## Overview

In this tutorial we will be using the [Conda](https://conda.io/projects/conda/en/latest/index.html)
package manager to install the ESMValTool.
Other installation methods are also available, they can be found in the
[documentation](https://docs.esmvaltool.org/en/latest/quickstart/installation.html).
ESMValTool also contains diagnostics written in [Julia](https://julialang.org/).
Because Julia cannot be installed by Conda, we will install Julia separately.
We will first learn how to install Conda, Julia, and finally the ESMValTool.
We end this chapter by testing that the installation was successful.

## Install Conda

ESMValTool is distributed using Conda(link). Most users use Linux or OSX, but succesful usage has also been reported through the Windows Subsystem for Linux(WSL) (link).

We will be using the Miniconda minimal installer for conda.

> ## Python 3 Only
>
> Please make sure to download a Python 3 based installation (Miniconda3), as ESMValTool only supports Python 3
>
>
{: .callout}


### Linux

1. Please download miniconda at [the miniconda page](https://docs.conda.io/en/latest/miniconda.html)

2. Next, run the installer:
~~~
bash Miniconda3-latest-Linux-x86_64.sh
~~~
{:.language-bash}

3. Follow the instructions in the installer. The defaults should normally suffice.

4. You will need to restart your terminal for the changes to have effect.

5. Verify you have a working conda installation by listing all installed packages
~~~
conda list
~~~

### MacOSX

### Windows

After installing the WSL, installation can be done using the Linux installation instructions.



## Install Julia

## Install ESMValTool

## Test that the installation was successful

{% include links.md %}
