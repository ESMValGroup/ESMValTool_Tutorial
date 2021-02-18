---
title: "Development and contribution"
teaching: 10
exercises: 20
questions:
- "What is a development installation?"
- "How can I test new or improved codes?"
- "How can I incorporate new or improved codes to esmvaltool?"

objectives:
- "Execute a successful ESMValTool installation from the source code."
- "Contribute to esmvaltool development."

keypoints:
- "A development installation is needed if you want latest features or to incorporate your codes."
- "Contributions include adding a new or improved script or helping with a review process."
- "There are several tools to check te quality of your code."
- "It is possible to run tests on your machine."
- "You can build documentations locally."
---

So you want to contribute code to ESMValTool. What follows describes a
development installation to help you get going.

> ## Attention
>
> -   This episode is based on the ESMValTool installation instructions, for
>     more information and advanced cases you can visit the  ESMValTool
>     [documentation](https://esmvaltool.readthedocs.io/en/latest/getting_started/install.html).
>
> -   For this episode it is assumed you have knowledge of
>     [git](https://git-scm.com/). You can refresh your knowledge in the
>     corresponding [git carpentries
>     course](http://swcarpentry.github.io/git-novice/).
{: .callout}

## Obtaining the source code

The ESMValTool source code is available on a public GitHub repository:
[https://github.com/ESMValGroup/ESMValTool](https://github.com/ESMValGroup/ESMValTool)

The easiest way to obtain it is to clone the repository using git. To clone the
public repository open a terminal window and type:

~~~bash
git clone https://github.com/ESMValGroup/ESMValTool.git
~~~

By default, this command will create a folder called ESMValTool containing the
source code of the tool.

Move into the directory to continue the installation.

> ## Attention
>
> Make sure that the master branch is checked out before continuing installation:
> ~~~bash
> git checkout master
> ~~~
{: .callout}

## Prerequisites

It is recommended to use conda to manage ESMValTool dependencies. For a minimal
conda installation go to
[https://conda.io/miniconda.html](https://conda.io/miniconda.html). To simplify
the installation process, an environment definition file is provided in the
repository (``environment.yml`` in the root folder).

> ## Attention
> It is preferable to use a local, fully user-controlled conda installation. If
> you have conda installed already, make sure it is up to date by running
> ``conda update -n base conda``.
{: .callout}

From now on, we will assume that the installation is going to be done through
conda.

Ideally, you should create a dedicated conda environment for ESMValTool, so it
is independent from any other Python tools present in the system.

To create an environment, go to the directory containing the ESMValTool source
code (called ESMValTool if you did not choose a different name) and run

~~~bash
conda env create --name esmvaltool --file environment.yml
~~~

The environment is called ``esmvaltool`` by default, but it is possible to use
the option ``--name ENVIRONMENT_NAME`` to define a custom name. You can activate
the environment using the command:

~~~bash
conda activate esmvaltool
~~~

If you run into trouble, please try recreating the environment. More information
can be found in the [conda documentation](https://docs.conda.io/en/latest/).

## Software installation

Once all prerequisites are fulfilled, ESMValTool can be installed by running
the following commands in the directory containing the ESMValTool source code:

~~~bash
pip install --editable '.[develop]'
~~~

This will add the `esmvaltool` directory to the Python path in editable mode and
install the development dependencies.

## Test the installation

The next step is to check that the installation works properly.
To do this, run the tool with:

~~~bash
esmvaltool --help
~~~

If everything was installed properly, ESMValTool should have printed a
help message to the console.

For a more complete installation verification, run the automated tests and
confirm that no errors are reported:

~~~bash
python setup.py test --installation
~~~

> ## ESMValCore
>
> Most core functionality for the ESMValTool is available in the
> [ESMValCore](https://github.com/ESMValGroup/ESMValCore) Python package. If
> your contribution in ESMValTool needs changes to ESMValCore please follow [its
> contribution
> guide](https://github.com/ESMValGroup/ESMValCore/blob/master/CONTRIBUTING.md).
{: .callout}

{% include links.md %}
