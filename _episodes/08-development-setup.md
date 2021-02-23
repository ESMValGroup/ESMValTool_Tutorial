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

If everything is installed properly, ESMValTool prints a help message to the console.

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

### Review process

We first discuss our idea in an
**[issue](https://github.com/ESMValGroup/ESMValTool/issues)** in ESMValTool repository.
Then, we create a new ``branch`` locally and start developing new codes.
Once our development is finished, we can initiate a ``pull request``.
For a full description of the GitHub workflow, please see ESMValTool documentation on
[GitHub Workflow](https://docs.esmvaltool.org/en/latest/community/repository.html#github-workflow).

The pull request will be tested, discussed and merged. This is called "**review process**".
The process will take some effort and time to learn.
However, a few “tools” i.e. command lines gets you a long way,
and we’ll cover those essentials in this lesson.

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
> > ## Solution
> >
> >
> {: .solution}
{: .challenge}

### Run unit tests

Previous section introduced some tools to check code style and quality.
What it hasen’t done is show us how to tell whether our code is getting the right answer.
To achieve that, we can run tests using ``pytest`` locally:

> ~~~bash
> conda activate esmvaltool
> cd ESMValTool
> git checkout your_branch_name
> pytest
> ~~~

Tests will also be run automatically by [CircleCI](https://circleci.com/), when you submit a pull request.

### Build documentation

Documentations are available on [docs.esmvaltool.org](https://docs.esmvaltool.org/en/latest/index.html).
The source files are located in ``ESMValTool/doc/sphinx/source/``.

To build documentations locally, we run:

> ~~~bash
> conda activate esmvaltool
> cd ESMValTool
> git checkout your_branch_name
> python setup.py build_sphinx -Ea
> ~~~

Similar to codes, documentations should be well written and adhere to standards.
If documentations are buit properly, the previous command prints a message to the console:

> ~~~bash
> build succeeded.
>
> The HTML pages are in doc/sphinx/build/html.
> ~~~

The main page of the documentation as been built into ``index.html``
in ``doc/sphinx/build/html`` directory.
To preview this page locally, we open the file in a web browser:

> ~~~bash
> xdg-open doc/sphinx/build/html/index.html
> ~~~

> ## Creating a documenation
>
> Let's checkout our local branch and add the recipe
> [recipe_warming_stripes.yml](../files/recipe_warming_stripes.yml)
> to the the ``example`` directory:
>
> ~~~bash
> cd ESMValTool
> git checkout your_branch_name
> cp path_of_recipe_warming_stripes.yml esmvaltool/recipes/examples/
> ~~~
>
> Then, we create a documentation file ``recipe_warming_stripes.rst`` for this recipe:
>
> ~~~bash
> nano doc/sphinx/source/recipes/recipe_warming_stripes.rst
> ~~~
>
> Add a reference i.e. ``.. _recipe_warming_stripes:``, a section title
> and some text about the recipe:
>
> ~~~rst
> .. _recipe_warming_stripes:
>
> Reproducing Ed Hawkins' warming stripes visualization
> ======================================================
>
> This recipe produces warming stripes plots.
> ~~~
>
> We can think of this file as one page of a book.
> Then, we need to decide where this page should be located inside the book.
> The table of content is defined by ``index.rst``:
>
> ~~~bash
> nano doc/sphinx/source/recipes/index.rst
> ~~~
>
> Add the name of the recipe ``recipe_warming_stripes`` to
> the section ``Other`` in this file and
> preview the recipe documentation page locally.
>
>> ## Solution
>>
>> First, add the name of the recipe ``recipe_warming_stripes`` to
>> the section ``Other``:
>>
>> ~~~rst
>> Other
>> ^^^^^
>> .. toctree::
>>   :maxdepth: 1
>>   ...
>>   ...
>>   recipe_warming_stripes
>> ~~~
>>
>> Then, build and preview the documentation page:
>>
>> ~~~bash
>> python setup.py build_sphinx -Ea
>> xdg-open doc/sphinx/build/html/recipes/recipe_warming_stripes.html
>> ~~~
>>
> {: .solution}
{: .challenge}

{% include links.md %}
