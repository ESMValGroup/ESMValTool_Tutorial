---
title: "Installation"
teaching: 10
exercises: 10
questions:
- "What are the prerequisites for installing ESMValTool?"
- "How do I confirm that the installation was successful?"
objectives:
- "Install ESMValTool"
- "Demonstrate that the installation was successful"
keypoints:
- "All the required packages can be installed using conda."
- "You can find more information about installation in the
[documentation](https://docs.esmvaltool.org/en/latest/quickstart/installation.html)."
---
## Overview

The instructions help with the installation of ESMValTool on operating systems
like Linux/MacOSX/Windows.
We use the [Conda](https://conda.io/projects/conda/en/latest/index.html)
package manager to
install the ESMValTool. Other installation methods are also available; they can
be found in the
[documentation](https://docs.esmvaltool.org/en/latest/quickstart/installation.html).
We will first install Conda, and then ESMValTool. We end this chapter by testing
that the installation was successful.


> ## ESMValTool Installation/Usage  Options - A quick summary
> Before we begin, here are all the possible ways in which you can use ESMValTool
> depending on your level of expertise or involvement with ESMvalTool and 
> associated software such as GitHub and Conda.
>
> 1) If you have access to a server where ESMValTool is already installed 
> as a module, for e.g., the ``CEDA JASMIN`` server, you can simply
> load the module with the following:
> ```bash
> module load esmvaltool
> ```
> 2) If you would like to install ESMValTool as a conda package, then this lesson 
> will tell you how!
> 
> 3) If you would like to start experimenting with existing diagnostics or contributing to ESMvalTool, please see the instructions for 
> source installation in the lesson [Development and
> contribution]({{ page.root }}{% link _episodes/08-development-setup.md %}) and 
> in the [documentation](https://docs.esmvaltool.org/en/latest/quickstart/installation.html).
{: .solution}

> ## Install ESMValTool on Windows
>
> ESMValTool does not directly support Windows, but successful usage has been
> reported through the [Windows Subsystem for
> Linux(WSL)](https://docs.microsoft.com/en-us/windows/wsl/),
> available in Windows 10.
> To install the WSL please follow the instructions [on the Windows Documentation
> page](https://docs.microsoft.com/en-us/windows/wsl/install-win10).
> After installing the WSL, installation can be done using the same instructions for
> [Linux/MacOSX](#install-esmvaltool-on-linuxmacosx).
{: .callout}

## Install ESMValTool on Linux/MacOSX

### Install Conda

ESMValTool is distributed using [Conda](https://conda.io/).
Let's check if we already have Conda installed by running:

```bash
conda list
```

If conda is installed, we will see a list of packages.
We recommend updating conda before esmvaltool installation. To do so, run:

```bash
conda update -n base conda
```

If conda is **not** installed, we can use Miniconda minimal installer for conda.
We recommend a Python 3 based installer. For more information about installing conda,
see [the conda installation documentation](https://docs.conda.io/projects/conda/en/latest/user-guide/install/index.html).

To install conda on ``Linux`` or ``MacOSX``, follow the  instructions below:

1.  Please download Miniconda3 at [the miniconda
    page](https://docs.conda.io/en/latest/miniconda.html).
    If you have problems with the 64 bit version in the next step(s)
    you can alternatively try a 32 bit version.

2.  Next, run the installer from the place where you downloaded it:

    On ``Linux``:

    ```bash
    bash Miniconda3-latest-Linux-x86_64.sh
    ```

    On ``MacOSX``:

    ```bash
    bash Miniconda3-latest-MacOSX-x86_64.sh
    ```

3.  Follow the instructions in the installer. The defaults should normally
    suffice.

4.  You will need to restart your terminal for the changes to have effect.

5.  Verify you have a working conda installation by listing all installed
    packages

    ```bash
    conda list
    ```

### Install Julia

Some ESMValTool diagnostics are written in the Julia programming language.
If you want a full installation of ESMValTool including Julia diagnostics, you need
to make sure Julia is installed before installing ESMValTool.

In this tutorial, we will not use Julia, but for reference, we have listed the steps
to install Julia below.
Complete instructions for installing Julia can be found on the [Julia
installation page](https://julialang.org/downloads/platform/#linux_and_freebsd).

> ## Julia installation instructions
>
> First, open a bash terminal and create a directory to install Julia in and cd
> into it
>
> ```bash
> mkdir ~/julia
> cd ~/julia
> ```
>
> Next, to download and extract the file `julia-1.0.5-linux-x86_64.tar.gz`, you can use the following commands::
>
> ```bash
> wget https://julialang-s3.julialang.org/bin/linux/x64/1.0/julia-1.0.5-linux-x86_64.tar.gz
> tar -xvzf julia-1.0.5-linux-x86\_64.tar.gz
> ```
>
> This will extract the files to a directory named `~/julia/julia-1.0.5`. To run
> Julia, you need to add the full path of Julia's `bin` folder to PATH environment
> variable. To do this, you can edit the `~/.bashrc` (or `~/.bash_profile`) file.
> Open the file in a text editor called ``nano``:
>
> ```bash
> nano ~/.bashrc
> ```
>
> and add a new line as follows at the
> bottom of the file:
>
> ```bash
> export PATH="$PATH:$HOME/julia/julia-1.0.5/bin"
> ```
>
> Finally, for the settings to take effect, either reload your bash profile
>
> ```bash
> source ~/.bashrc
> ```
>
> (or `source ~/.bash_profile`), or close the bash terminal window and open a new
> one.
>
> To check that the Julia executable can be found, run
>
> ```bash
> which julia
> ```
>
> to display the path to the Julia executable, it should be
>
> ```
> ~/julia/julia-1.0.5/bin/julia
> ```
> {: .output}
>
> To test that Julia is installed correctly, run
>
> ```bash
> julia
> ```
>
> to start the interactive Julia interpreter. Press `Ctrl+D` to exit.
{: .solution}

### Install the ESMValTool package

The ESMValTool package contains diagnostics scripts in four languages: R,
Python, Julia and NCL. This introduces a lot of dependencies, and therefore the
installation can take quite long. It is, however, possible to install
'subpackages' for each of the languages. The following (sub)packages are
available:

- `esmvaltool-python`
- `esmvaltool-ncl`
- `esmvaltool-r`
- `esmvaltool-julia`
- `esmvaltool` --> the complete package, i.e. the combination of the above.

For the tutorial, we will use only Python diagnostics. Thus, to install the
ESMValTool-python package, run

```bash
conda create -n esmvaltool -c conda-forge -c esmvalgroup esmvaltool-python
```

This will create a new [Conda
environment](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html)
called `esmvaltool`, with the ESMValTool-Python package and all of its dependencies
installed in it.


> ## Common issues
>
> -   Installation takes a long time
>     -   Downloads and compilations can take several minutes to complete,
>         please be patient.
>     -   You might have bad luck and the dependencies can not be resolved at
>         the moment, please try again later or [raise an
>         issue](https://github.com/ESMValGroup/ESMValTool/issues/new/choose)
>     -   If `Solving environment` takes more than 10 minutes, you may need to update
>         conda: `conda update -n base conda`
>     -   You can help conda solve the environment by specifying
>         the python version:
>         ```bash
>         conda create -n esmvaltool -c conda-forge -c esmvalgroup esmvaltool-python python=3.8
>         ```
>     -   Note that on ``MacOSX``, ``esmvaltool-python`` and
>     ``esmvaltool-ncl`` only work with Python 3.7. Use `python=3.7` instead of `python=3.8`.
> -   If you have an old conda installation you could get a `UnsatisfiableError`
>     message. Please install a newer version of conda and try again
> -   Downloads fail due to company proxy, see [conda
>     docs](https://docs.anaconda.com/anaconda/user-guide/tasks/proxy/) how to
>     resolve.
>
{: .callout}

### Test that the installation was successful

To test that the installation was successful, run

```bash
conda activate esmvaltool
```

to activate the conda environment called `esmvaltool`. In the shell prompt the
active conda environment should have been changed from `(base)` to
`(esmvaltool)`.

Next, run

```bash
esmvaltool --help
```

to display the command line help.

> ## Version of ESMValTool
>
> Can you figure out which version of ESMValTool has been installed?
>
> > ## Solution
> >
> > The `esmvaltool --help` command lists `version` as a
> > command to get the version
> >
> > In my case when I run
> > ~~~
> > esmvaltool version
> > ~~~
> > {: .bash}
> > I get that my installed ESMValTool version is
> > ~~~
> > ESMValCore: 2.0.0
> > ESMValTool: 2.0.0
> > ~~~
> > {: .output}
> {: .solution}
{: .challenge}

{% include links.md %}
