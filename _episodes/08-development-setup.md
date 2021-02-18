---
title: "Development and contribution"
teaching: 10
exercises: 20
questions:
- "What is a development installation?"
- "How can I test new or improved codes?"
- "How can I incorporate new or improved codes to ESMValTool?"

objectives:
- "Execute a successful ESMValTool installation from the source code."
- "Contribute to ESMValTool development."

keypoints:
- "A development installation is needed if you want latest features or to incorporate your codes."
- "Contributions include adding a new or improved script or helping with a review process."
- "There are several tools to check te quality of your code."
- "It is possible to run tests on your machine."
- "You can build documentations locally."
---

We now know how ESMValTool works, but how do we develop it?
This lesson helps to run your own codes with ESMValTool and
illustrates how to incorporate your codes into the tool.

> ## Git knowledge
>
> For this episode you need some knowledge of
> [Git](https://git-scm.com/). You can refresh your knowledge in the
> corresponding [Git carpentries course](http://swcarpentry.github.io/git-novice/).
{: .callout}

## Development installation

To run new codes by ESMValTool, we need to install it in a ``develop`` mode.
Let’s get started.

### 1 Source code

The ESMValTool source code is available on a public GitHub repository:
[https://github.com/ESMValGroup/ESMValTool](https://github.com/ESMValGroup/ESMValTool)
To obtain the code, the easiest way is to clone the repository:

~~~bash
git clone https://github.com/ESMValGroup/ESMValTool.git
~~~

This command will create a folder called ESMValTool containing
the source code of the tool. To continue the installation, move into the directory
and check out the ``master`` branch:

~~~bash
cd ESMValTool
git checkout master
~~~

### 2 ESMValTool dependencies

It is recommended to use conda to manage ESMValTool dependencies. For a minimal
conda installation, see section **Install Conda**, in lesson
[Installation]({{ page.root }}{% link _episodes/02-installation.md %}).
To simplify the installation process, an environment file ``environment.yml`` is
provided in the ESMValTool direcotry. To create an environment run:

~~~bash
conda env create --file environment.yml
~~~

The environment is called ``esmvaltool`` by default. Let's activate the environment:

~~~bash
conda activate esmvaltool
~~~

### 3 ESMValTool installation

ESMValTool can be installed in a ``develop`` mode by running:

~~~bash
pip install --editable '.[develop]'
~~~

This will add the `esmvaltool` directory to the Python path in editable mode and
install the development dependencies. We should check if the installation
works properly. To do this, run the tool with:

~~~bash
esmvaltool --help
~~~

If everything was installed properly, ESMValTool should have printed a
help message to the console. For a more complete installation verification,
we can run the automated tests:

~~~bash
python setup.py test --installation
~~~

## Contribution

ESMValTool is an open-source project in ESMValGroup.
You can contribute to its development by:

- a new or updated recipe script, see lesson on
[Writing your own recipe]
- a new or updated diagnostics script, see lesson on
[Writing your own diagnostic script]
- a new or updated cmorizer script, see lesson on
[CMORization: Using observational datasets]
- helping  with reviewing process of pull requests, see documentation on
[Review of pull requests](https://docs.esmvaltool.org/en/latest/community/review.html)

If you would like to add what is already discussed in an
[issue](https://github.com/ESMValGroup/ESMValTool/issues) in ESMValTool repository,
you need to create a new branch locally and checkout it. To do this, follow the commands:

~~~bash
cd ESMValTool
git checkout master
git pull
git checkout -b your_branch_name
~~~

## Run tests

## Build documentation



{% include links.md %}
