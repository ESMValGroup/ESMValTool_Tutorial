# Contributing to ESMValTool tutorial

[ESMValTool][ESMValTool-site] tutorial is an open-source project and we greatly value contributions of all kinds: fixes to this tutorial, bug reports, reviews of pull requests, infrastructure improvements, community help, and outreach. We value the time you invest in contributing and strive to make the process as easy as possible. If you have suggestions for improving the process of contributing, please open an [issue][issues] by choosing the ``Suggestion`` template in the ``issues`` tab of this repository.

## Acknowledgement

The format of this tutorial is based on the [Software Carpentry][swc-site], which is an open-source project.

## Agreement

By contributing, you agree that we may redistribute your work under [our license](LICENSE.md).
In exchange, we will address your issues and/or assess your change proposal as promptly as we can, and help you become a member of our community.
Everyone involved in this [tutorial](tutorial-repo) agrees to abide by our [code of conduct](CODE_OF_CONDUCT.md).

## How to Contribute

There are many ways to contribute:

* If you do not have a [GitHub][github] account,
you can [send us comments by email][email].
However,
we will be able to respond more quickly if you use one of the other methods described below.

* If you have a [GitHub][github] account,
or are willing to [create one][github-join],
please work in this [repository][tutorial-repo],
which can be viewed at the [tutorial site][tutorial-site].
You can ask your questions, report problems or suggest improvements by [creating an issue][issues].
This is the easiest way to tell us about your ideas,
and a good way to introduce yourself
and to meet some of our community members.
This allows us to assign the item to someone
and to respond to it in a threaded discussion.
There are several templates to make an issue:
  * for asking a question, please use [Question and answer][issues]
  * for reporting a bug, please use [Bug reports][issues]
  * for developing lesson material, please use [New lesson material][issues]
  * for adding a feature to the repository, please use [Suggestion][issues]

* If you would like to add what is already discussed in an issue,
you can submit a [pull request][PR].
To do so, you can make use of the [pull request checklist][PR].
The reviewers are community volunteers who provide feedback.
The [maintainers][tutorial-maintainers] have final say over what gets merged into the tutorial.

## Tutorial guidelines

This section demonstrates all the instructions for developing a lesson in the [ESMValTool tutorial][tutorial-site].
The tutorial is a set of lessons (or episodes) that together teach **basic** skills needed to work with [ESMValTool][ESMValTool-site] in climate-related domains.

### Lesson development

The content of this tutorial is mainly developed based on the [Carpentries Curriculum Development Handbook][swc-handbook]. The handbook explains why we teach the way we do, and why our lessons are designed the way they are.
If you are contributing to existing lesson materials, please make sure the content conforms to the concepts provided in the handbook.

We recommend using software to check spelling or grammatical errors.
The following link will guide you through a list of tools for several editors:
<http://wiki.languagetool.org/software-that-supports-languagetool-as-a-plug-in-or-add-on>

### Lesson organization

Each lesson is made up of episodes, which are focused on a particular topic and include time for both teaching and exercises. If you are making a new episode, please make sure the content conforms to the [Carpentries lesson organization][swc-lesson-organization].

### Lesson formatting

Episodes files are [Markdown](https://en.wikipedia.org/wiki/Markdown) files. If you are making a new episode or contributing to existing ones, please make sure the content conforms to the [Carpentries lesson formatting][swc-lesson-formatting].
We recommend using a linter to check errors in Markdown files.
For example, a [markdownlint](https://marketplace.visualstudio.com/items?itemName=DavidAnson.vscode-markdownlint) can be installed as an extension in ``Visual Studio Code``.
Alternatively, it can installed with:

```bash
gem install mdl
```

and can be used as:

```bash
mdl your_markdown_filename
```

## Previewing your changes locally

Extensive instructions for building and viewing the pages locally can be found [here](https://carpentries.github.io/lesson-example/setup.html). To get started quickly, run

```bash
# apt (Ubuntu/Devian)
sudo apt install ruby-dev ruby-bundler
```

or

```bash
# dnf (Fedora/Redhat)
sudo dnf install rubygem-bundler ruby-devel
```

Alternatively, there's an environment file available which can be installed in conda with:

```bash
conda env create -f environment.yml -n esmvaltool_tutorial
```

To install the required ruby packages, run the following command in the tutorial's
main directory to build and serve the website locally:

```bash
make serve
```

The output on the terminal will contain a line similar to:

```bash
Server address: http://127.0.0.1:4000
```

This address can be opened in your browser to preview the website.


## Other Resources

General discussion of [Software Carpentry][swc-site] and [Data Carpentry][dc-site]
happens on the [discussion mailing list][discuss-list],
which everyone is welcome to join.
You can also [reach us by email][email].

[email]: mailto:admin@software-carpentry.org
[ESMValTool-site]: https://www.esmvaltool.org/
[tutorial-repo]: https://esmvalgroup.github.io/tutorial/
[tutorial-site]: https://esmvalgroup.github.io/tutorial
[tutorial-maintainers]: https://github.com/ESMValGroup/tutorial#maintainers
[discuss-list]: http://lists.software-carpentry.org/listinfo/discuss
[github]: https://github.com
[github-flow]: https://guides.github.com/introduction/flow/
[github-join]: https://github.com/join
[how-contribute]: https://egghead.io/series/how-to-contribute-to-an-open-source-project-on-github
[issues]: https://github.com/ESMValGroup/tutorial/issues
[PR]: https://github.com/ESMValGroup/tutorial/pulls
[swc-issues]: https://github.com/issues?q=user%3Aswcarpentry
[swc-lessons]: https://software-carpentry.org/lessons/
[swc-site]: https://software-carpentry.org/
[swc-handbook]: https://carpentries.github.io/curriculum-development/
[swc-lesson-organization]: https://carpentries.github.io/lesson-example/03-organization/index.html
[swc-lesson-formatting]: https://carpentries.github.io/lesson-example/04-formatting/index.html
[ea-site]: https://github.com/escience-academy
[c-site]: https://carpentries.org/
[lc-site]: https://librarycarpentry.org/
[lc-issues]: https://github.com/issues?q=user%3Alibrarycarpentry
