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
- "A development installation is needed if you want to incorporate your codes."
- "Contributions include adding a new or improved script or helping with a review process."
- "There are several tools to check the quality of your code."
- "It is possible to run tests on your machine."
- "You can build documentations locally."
---

We now know how ESMValTool works, but how do we develop it?
This lesson helps to run your own codes with ESMValTool.
It also illustrates how to incorporate your codes into the tool.

> ## Git knowledge
>
> For this episode, you need some knowledge of
> [Git](https://git-scm.com/). You can refresh your knowledge in the
> corresponding [Git carpentries course](http://swcarpentry.github.io/git-novice/).
{: .callout}

## Development installation

We’ll explore how ESMValTool can be installed it in a ``develop`` mode.
Even if you aren’t collaborating with the community, this installation is needed
to run your new codes by ESMValTool.
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

If everything is installed properly, ESMValTool prints a
help message to the console. For a more complete installation verification,
we can run the automated tests:

~~~bash
python setup.py test --installation
~~~

## Contribution

ESMValTool is an open-source project in ESMValGroup.
We can contribute to its development by:

- a new or updated recipe script, see lesson on
[Writing your own recipe]({{ page.root }}{% link _episodes/05-preprocessor.md %})
- a new or updated diagnostics script, see lesson on
[Writing your own diagnostic script]
- a new or updated cmorizer script, see lesson on
[CMORization: Using observational datasets]
- helping  with reviewing process of pull requests, see documentation on
[Review of pull requests](https://docs.esmvaltool.org/en/latest/community/review.html)

The next sections will explore the ways we can achieve this.

### GitHub workflow

We first discuss our idea in an
**[issue](https://github.com/ESMValGroup/ESMValTool/issues)** in ESMValTool repository.

Then, we create a new ``branch`` locally and start developing new codes.
Once our development is finished, we can initiate a ``pull request``.
The pull request will then be tested, discussed and merged. This is called "**review process**".

Using the workflow will take some effort and time to learn.
However, a few “tools” i.e. command lines gets you a long way,
and we’ll cover those essentials in this lesson.
For a full description of the workflow, please see ESMValTool documentation on
[Contributing to the community » GitHub Workflow](https://docs.esmvaltool.org/en/latest/community/repository.html#github-workflow).

### Check code quality

We aim to adhere to best practices and coding standards. The good news is that there are
several tools that we can use to check our code against those standards.

As an example, ``Pre-commit`` is a tool that checks your code for any invalid syntax and formatting errors.
To explore other tools, have a look at ESMValTool documentation on
[Code style](https://docs.esmvaltool.org/en/latest/community/introduction.html#code-style).

> ## Using Pre-commit
>
> Let's checkout our local branch and add the script
> [warming_stripes.py](../files/warming_stripes.py) to the ``example`` directory.
>
> ~~~bash
> cd ESMValTool
> git checkout your_branch_name
> cp path_of_warming_stripes.py esmvaltool/diag_scripts/examples/
> ~~~
>
> By default, ``pre-commit`` only runs on the files that have been changed, and staged in git:
>
> ~~~bash
> git status
> git add esmvaltool/diag_scripts/examples/warming_stripes.py
> pre-commit run --files esmvaltool/diag_scripts/examples/warming_stripes.py
> ~~~
>
> Inspect the output of ``pre-commit`` and fix the errors.
>
> > ## Answers
> >
> >
> {: .solution}
{: .challenge}


### Run tests

### Build documentation



{% include links.md %}
