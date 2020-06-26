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

ESMValTool is distributed using [Conda](https://conda.io/). We will be using the Miniconda minimal installer for conda. We suggest a Python 3 based installer, though if you happen to already have Conda installed it should also work with Python 2. For more information about installing conda, see [the conda installation documentation](https://docs.conda.io/projects/conda/en/latest/user-guide/install/index.html).

### Linux

1.  Please download Miniconda3 for Linux at [the miniconda page](https://docs.conda.io/en/latest/miniconda.html). The 64 bit version should work on any recent system. If you have problems in the next step(s) you can alternatively try a 32 bit version.

2.  Next, run the installer

    ~~~
    bash Miniconda3-latest-Linux-x86_64.sh
    ~~~
    {:.language-bash}

3.  Follow the instructions in the installer. The defaults should normally suffice.

4.  You will need to restart your terminal for the changes to have effect.

5.  Verify you have a working conda installation by listing all installed packages

    ~~~
    conda list
    ~~~
    {:.language-bash}

### MacOSX

1.  Please download Miniconda3 for MacOSX at [the miniconda page](https://docs.conda.io/en/latest/miniconda.html). 

2.  Next, run the installer

    ~~~
    bash Miniconda3-latest-MacOSX-x86_64.sh
    ~~~
    {:.language-bash}

3.  Follow the instructions in the installer. The defaults should normally suffice.

4.  You will need to restart your terminal for the changes to have effect.

5.  Verify you have a working conda installation by listing all installed packages

    ~~~
    conda list
    ~~~
    {:.language-bash}

### Windows

ESMValTool does not directly support Windows, But succesful usage has been reported through the [Windows Subsystem for Linux(WSL)](https://docs.microsoft.com/en-us/windows/wsl/), available in Windows 10.

To install the WSL please follow the instructions [on the Windows Documentation page](https://docs.microsoft.com/en-us/windows/wsl/install-win10).

After installing the WSL, installation can be done using the Linux installation instructions.

## Install Julia

Complete instructions for installing Julia can be found on the
[Julia installation page](https://julialang.org/downloads/platform/#linux_and_freebsd).
In this tutorial, we will use the following steps.
First, open a bash terminal and create a directory to install Julia in
```bash
mkdir ~/julia
```
Next, download the file
[`julia-1.0.5-linux-x86_64.tar.gz`](https://julialang-s3.julialang.org/bin/linux/x64/1.0/julia-1.0.5-linux-x86_64.tar.gz)
by clicking the link or going to the [Julia downloads page](https://julialang.org/downloads/).
To extract the file, you can use the following command:
```bash
tar -xvzf ~/Downloads/julia-1.0.5-linux-x86\_64.tar.gz -C ~/julia
```
This will extract the files to a folder named `~/julia/julia-1.0.5`.
To run Julia, you need to add the full path of Julia's `bin` folder to PATH environment variable.
To do this, you can edit the `~/.bashrc` (or `~/.bash_profile`) file.
Open the file in your favorite editor and add a new line as follows at the bottom of the file:
```bash
export PATH="$PATH:$HOME/julia/julia-1.0.5/bin"
```
Finally, for the settings to take effect, either reload your bash profile
```bash
source ~/.bashrc
```
(or `source ~/.bash_profile`), or close the bash terminal window and open a new one.

To check that the Julia executable can be found, run
```bash
which julia
```
to display the path to the Julia executable, it should be `~/julia/julia-1.0.5/bin/julia`.
To test that Julia is installed correctly, run
```bash
julia
```
to start the interactive Julia interpreter. Press `Ctrl+D` to exit.

## Install the ESMValTool package

To install the ESMValTool package, run
```bash
conda create -n esmvaltool -c conda-forge -c esmvalgroup esmvaltool
```
This will create a new
[Conda environment](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html)
called `esmvaltool`, with the ESMValTool package and all of its dependencies installed in it.

## Test that the installation was successful

To test that the installation was successful, run
```bash
conda activate esmvaltool
```
to activate the conda environment called `esmvaltool`.
Next, run
```bash
esmvaltool --help
```
to display the command line help.
To find the location where the ESMValTool package was installed on your system, run
```bash
python -c 'import esmvaltool; print(esmvaltool.__path__[0])'
```

{% include links.md %}
