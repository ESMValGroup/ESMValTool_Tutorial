---
title: "Development and contribution"
teaching: 10
exercises: 20
questions:
- "What is a development installation?"
- "How can I test new or improved code?"
- "How can I incorporate my contributions into ESMValTool?"

objectives:
- "Execute a successful ESMValTool installation from the source code."
- "Contribute to ESMValTool development."

keypoints:
- "A development installation is needed if you want to incorporate your code into ESMValTool."
- "Contributions include adding a new or improved script or helping with a review process."
- "There are several tools to help improve the quality of your code."
- "It is possible to run tests on your machine."
- "You can preview documentation pages locally."
---

We now know how ESMValTool works, but how do we develop it?
ESMValTool is an open-source project in ESMValGroup. We can contribute to its development by:

- a new or updated recipe script, see lesson on
[Writing your own recipe]({{ page.root }}{% link _episodes/06-preprocessor.md %})
- a new or updated diagnostics script, see lesson on
[Writing your own diagnostic script]({{ page.root }}{% link _episodes/08-diagnostics.md %})
- a new or updated cmorizer script, see lesson on
[CMORization: Using observational datasets]({{ page.root }}{% link _episodes/09-cmorization.md %})
- helping  with reviewing process of pull requests, see ESMValTool documentation on
[Review of pull requests](https://docs.esmvaltool.org/en/latest/community/review.html)

In this lesson, we first show how to set up a development installation of ESMValTool so you can
make changes or additions. We then explain how you can contribute these changes to the community.

> ## Git knowledge
>
> For this episode, you need some knowledge of
> [Git](https://git-scm.com/). You can refresh your knowledge in the
> corresponding [Git carpentries course](http://swcarpentry.github.io/git-novice/).
{: .callout}

## Development installation

We’ll explore how ESMValTool can be installed it in a ``develop`` mode.
Even if you aren’t collaborating with the community, this installation is needed
to run your new codes with ESMValTool.
Let’s get started.

### 1 Source code

The ESMValTool source code is available on a public GitHub repository:
[https://github.com/ESMValGroup/ESMValTool](https://github.com/ESMValGroup/ESMValTool).
To obtain the code, there are two options:

1. download the code from the repository. A ZIP file called
   ``ESMValTool-master.zip`` is downloaded. To continue the installation, unzip
   the file, move to the ``ESMValTool-master`` directory and then follow the
   sequence of steps starting from **2 ESMValTool dependencies**.
2. clone the repository if you want to contribute to the ESMValTool development:

~~~bash
git clone https://github.com/ESMValGroup/ESMValTool.git
~~~

This command will ask your GitHub username and a personal **token** as password.
Please follow instructions on
[GitHub token authentication requirements][token-authentication-requirements]
to create a personal access token. After the authentication,
the output might look like:

~~~
Cloning into 'ESMValTool'...
remote: Enumerating objects: 163, done.
remote: Counting objects: 100% (163/163), done.
remote: Compressing objects: 100% (125/125), done.
remote: Total 95049 (delta 84), reused 76 (delta 30), pack-reused 94886
Receiving objects: 100% (95049/95049), 175.16 MiB | 5.48 MiB/s, done.
Resolving deltas: 100% (68808/68808), done.
~~~
{: .output}

Now, a folder called ``ESMValTool`` has been created in your working directory.
This folder contains the source code of the tool.
To continue the installation, we move into the ``ESMValTool`` directory:

~~~bash
cd ESMValTool
~~~

Note that the ``master`` branch is checked out by default.
We can see this if we run:

~~~bash
git status
~~~

~~~
On branch master
Your branch is up to date with 'origin/master'.

nothing to commit, working tree clean
~~~
{: .output}

### 2 ESMValTool dependencies

It is recommended to use conda to manage ESMValTool dependencies. For a minimal
conda installation, see section **Install Conda** in lesson
[Installation]({{ page.root }}{% link _episodes/02-installation.md %}).
To simplify the installation process, an environment file ``environment.yml`` is
provided in the ESMValTool directory. We create an environment by running:

~~~bash
conda env create --file environment.yml
~~~

The environment is called ``esmvaltool`` by default.
If an ``esmvaltool`` environment is already created following the lesson
[Installation]({{ page.root }}{% link _episodes/02-installation.md %}),
we should choose another name for the new environment in this lesson by:

~~~bash
conda env create -n a_new_name --file environment.yml
~~~

For more information see [conda managing environments][manage-environments].
Now, we should activate the environment:

~~~bash
conda activate esmvaltool
~~~

### 3 ESMValTool installation

ESMValTool can be installed in a ``develop`` mode by running:

~~~bash
pip install --editable '.[develop]'
~~~

This will add the ``esmvaltool`` directory to the Python path in editable mode and
install the development dependencies. We should check if the installation
works properly. To do this, run the tool with:

~~~bash
esmvaltool --help
~~~

If the installation is successful, ESMValTool prints a help message to the console.

> ## Checking the development installation
>
> We can use the command ``conda list`` to list installed packages in
> the ``esmvaltool`` environment.
> Use this command to check that ESMValTool is installed in a ``develop`` mode.
>
> **Tip**: see the
> [documentation on conda list](https://docs.conda.io/projects/conda/en/latest/commands/list.html).
>
>> ## Solution
>>
>> Run:
>>
>> ~~~bash
>> conda list esmvaltool
>>~~~
>>
>> ~~~bash
>> # Name                    Version                   Build  Channel
>> esmvaltool                2.1.1                     dev_0    <develop>
>>~~~
>>
>> As can be seen, it is mentioned ``<develop>`` under the ``Channel``.
> {: .solution}
{: .challenge}

### 4 Updating ESMValTool

The ``master`` branch has the latest features of ESMValTool. Please make sure
that the source code on your machine is up-to-date. If you obtain the source
code using git clone as explained in step **1 Source code**, you can run ``git pull``
to update the source code. Then ESMValTool installation will be updated
with changes from the ``master`` branch.

## Contribution

We have seen how to install ESMValTool in a ``develop`` mode.
Now, we try to contribute to its development. Let's see how we can achieve this.

### Review process

We first discuss our ideas in an
**[issue](https://github.com/ESMValGroup/ESMValTool/issues)** in ESMValTool repository.
This can avoid disappointment at a later stage, for example,
if more people are doing the same thing.
It also gives other people an early opportunity to provide input and suggestions,
which results in more valuable contributions.

Then, we create a new ``branch`` locally and start developing new codes.
Once our development is finished, we can initiate a ``pull request``.
For a full description of the GitHub workflow, please see ESMValTool documentation on
[GitHub Workflow](https://docs.esmvaltool.org/en/latest/community/repository.html#github-workflow).

The pull request will be tested, discussed and merged. This is called "**review process**".
The process will take some effort and time to learn.
However, a few (command line) tools can get you a long way,
and we’ll cover those essentials in the next sections.

**Tip**: we encourage you to keep the pull requests small.
Reviewing small incremental changes are more efficient.

### Background

We saw 'warming stripes' in lesson
[Writing your own recipe]({{ page.root }}{% link _episodes/06-preprocessor.md %}).
Imagine the following task: you want to contribute warming stripes recipe and diagnostics
to ESMValTool. You have to add the diagnostics
[warming_stripes.py](../files/warming_stripes.py) and the recipe
[recipe_warming_stripes.yml](../files/recipe_warming_stripes.yml)
to their locations in ESMValTool directory.
After these changes, you should also check if everthing works fine.
This is where we take advantage of the tools that are introduced later.

Let’s get started.

### Check code quality

We aim to adhere to best practices and coding standards. There are
several tools that check our code against those standards like:

- [flake8](https://flake8.pycqa.org/en/latest/#) for checking against the PEP8 style guide
- [yapf](https://pypi.org/project/yapf/) to ensure consistent formatting for the whole project
- [isort](https://pypi.org/project/isort/) to consistently sort the import statements
- [yamllint](https://yamllint.readthedocs.io/en/stable/#) to ensure there are no syntax errors in our recipes and config files
- [lintr](https://github.com/jimhester/lintr) for diagnostic scripts written in R
- [codespell](https://pypi.org/project/codespell/) to check grammar

The good news is that ``pre-commit`` has been already installed
when we chose development installation.
``pre-commit`` is a command line and runs all of those tools. It also fixes some of those errors.
To explore other tools, have a look at ESMValTool documentation on
[Code style](https://docs.esmvaltool.org/en/latest/community/introduction.html#code-style).

> ## Using pre-commit
>
> Let's checkout our local branch and add the script
> [warming_stripes.py](../files/warming_stripes.py) to the
> ``esmvaltool/diag_scripts`` directory.
>
> ~~~bash
> cd ESMValTool
> git checkout your_branch_name
> cp path_of_warming_stripes.py esmvaltool/diag_scripts/
> ~~~
>
> By default, ``pre-commit`` only runs on the files that have been staged in git:
>
> ~~~bash
> git status
> git add esmvaltool/diag_scripts/warming_stripes.py
> pre-commit run --files esmvaltool/diag_scripts/warming_stripes.py
> ~~~
>
> Inspect the output of ``pre-commit`` and fix the remaining errors.
>
>> ## Solution
>>
>> The output of  ``pre-commit``:
>>
>> ~~~ bash
>> Check for added large files..............................................Passed
>> Check python ast.........................................................Passed
>> Check for case conflicts.................................................Passed
>> Check for merge conflicts................................................Passed
>> Debug Statements (Python)................................................Passed
>> Fix End of Files.........................................................Passed
>> Trim Trailing Whitespace.................................................Passed
>> yamllint.............................................(no files to check)Skipped
>> nclcodestyle.........................................(no files to check)Skipped
>> style-files..........................................(no files to check)Skipped
>> lintr................................................(no files to check)Skipped
>> codespell................................................................Passed
>> isort....................................................................Passed
>> yapf.....................................................................Passed
>> docformatter.............................................................Failed
>> - hook id: docformatter
>> - files were modified by this hook
>> flake8...................................................................Failed
>> - hook id: flake8
>> - exit code: 1
>>
>> esmvaltool/diag_scripts/warming_stripes.py:20:5:
>> F841 local variable 'nx' is assigned to but never used
>> ~~~
>>
>> As can be seen above, there are two ``Failed`` check:
>>
>> 1. ``docformatter``: it is mentioned that "files were modified by this hook".
>> We run ``git diff`` to see the modifications.
>> The syntax ``"""`` at the end of docstring is moved by one line.
>> 2. ``flake8``: the error message is about an unused local variable ``nx``.
>> We should check our codes regarding the usage of ``nx``.
>> For now, let's assume that it is added by mistake and remove it.
> {: .solution}
{: .challenge}

### Run unit tests

Previous section introduced some tools to check code style and quality.
What it hasn’t done is show us how to tell whether our code is getting the right answer.
To achieve that, we need to write and run tests for widely-used functions.
ESMValTool comes with a lot of tests that are in the folder ``tests``.

To run tests, first we make sure that the working directory is ``ESMValTool``
and our local branch is checked out. Then, we can run tests using ``pytest`` locally:

~~~bash
pytest
~~~

Tests will also be run automatically by [CircleCI](https://circleci.com/),
when you submit a pull request.

> ## Running tests
>
> Let's checkout our local branch and add the recipe
> [recipe_warming_stripes.yml](../files/recipe_warming_stripes.yml)
> to the the ``esmvaltool/recipes`` directory:
>
> ~~~bash
> cp path_of_recipe_warming_stripes.yml esmvaltool/recipes/
> ~~~
>
> Run ``pytest`` and inspect the results. If a test is failed, try to fix it.
>
>> ## Solution
>>
>> Run:
>>
>> ~~~bash
>> pytest
>> ~~~
>>
>> When ``pytest`` run is complete, you can inspect the test reports
>> that are printed in the console. Have a look at the second section of the report
>> ``FAILURES``:
>>
>> ~~~ bash
>> ================================ FAILURES ==========================================
>> ______________ test_recipe_valid[recipe_warming_stripes.yml] ______________
>> ~~~
>>
>> The test message shows that the recipe ``recipe_warming_stripes.yml`` is not a valid recipe.
>> Look for a line that starts with an ``E`` in the rest of the message:
>>
>> ~~~ bash
>>
>> E           esmvalcore._task.DiagnosticError: Cannot execute script
>> '~/esmvaltool_tutorial/warming_stripes.py' (~/esmvaltool_tutorial/warming_stripes.py):
>> file does not exist.
>> ~~~
>>
>> To fix the recipe, we need to edit the path of the diagnostic script
>> as ``warming_stripes.py``:
>>
>> ~~~yml
>>    scripts:
>>      warming_stripes_script:
>>        script: warming_stripes.py
>> ~~~
>>
>> For details, see lesson
>> [Writing your own diagnostic script]({{ page.root }}{% link _episodes/08-diagnostics.md %}).
>>
> {: .solution}
{: .challenge}

### Build documentation

When we add or update a code, we also update its corresponding documentation.
The ESMValTool documentation is available on
[docs.esmvaltool.org](https://docs.esmvaltool.org/en/latest/index.html).
The source files are located in ``ESMValTool/doc/sphinx/source/``.

To build documentation locally, first we make sure that the working directory is ``ESMValTool``
and our local branch is checked out. Then, we run:

~~~bash
python setup.py build_sphinx -Ea
~~~

Similar to code, documentation should be well written and adhere to standards.
If the documentation is built properly, the previous command prints a message to the console:

~~~
build succeeded.

The HTML pages are in doc/sphinx/build/html.
~~~
{: .output}

The main page of the documentation has been built into ``index.html``
in ``doc/sphinx/build/html`` directory.
To preview this page locally, we open the file in a web browser:

~~~bash
xdg-open doc/sphinx/build/html/index.html
~~~

> ## Creating a documenation
>
> In previous exercises, we added the recipe
> [recipe_warming_stripes.yml](../files/recipe_warming_stripes.yml) to ESMValTool.
> Now, we create a documentation file ``recipe_warming_stripes.rst`` for this recipe:
>
> ~~~bash
> nano doc/sphinx/source/recipes/recipe_warming_stripes.rst
> ~~~
>
> Add a reference i.e. ``.. _recipe_warming_stripes:``, a section title
> and some text about the recipe like:
>
> ```
> .. _recipe_warming_stripes:
>
> Reproducing Ed Hawkins' warming stripes visualization
> ======================================================
>
> This recipe produces warming stripes plots.
> ```
>
> Save and close the file. We can think of this file as one page of a book.
> Then, we need to decide where this page should be located inside the book.
> The table of content is defined by ``index.rst``. Let's have a look at the content:
>
> ~~~bash
> nano doc/sphinx/source/recipes/index.rst
> ~~~
>
> Add the recipe name i.e. ``recipe_warming_stripes`` to the section ``Other`` in this file
> and preview the recipe documentation page locally.
>
>> ## Solution
>>
>> First, we add the recipe name ``recipe_warming_stripes`` to the section ``Other``:
>>
>> ```
>> Other
>> ^^^^^
>> .. toctree::
>>   :maxdepth: 1
>>   ...
>>   ...
>>   recipe_warming_stripes
>> ```
>>
>> Then, we build and preview the documentation page:
>>
>> ~~~bash
>> python setup.py build_sphinx -Ea
>> xdg-open doc/sphinx/build/html/recipes/recipe_warming_stripes.html
>> ~~~
>>
> {: .solution}
{: .challenge}

Congratulations! You are now ready to make a pull request.

{% include links.md %}

[token-authentication-requirements]: https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations/#what-you-need-to-do-today
[manage-environments]: https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html
[install-from-source]: https://docs.esmvaltool.org/en/latest/quickstart/installation.html#install-from-source
