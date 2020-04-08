# Contributing to ESMValTool tutorial

[ESMValTool](https://www.esmvaltool.org/) tutorial is an open-source project and we greatly value contributions of all kinds: fixes to this tutorial, bug reports, reviews of pull requests, infrastructure improvements, community help, and outreach. We value the time you invest in contributing and strive to make the process as easy as possible. If you have suggestions for improving the process of contributing, please open an [issue][issues] by choosing the ``New feature`` template in the ``issues`` tab of this repository.

## Acknowledgement

The [eScience academy][ea-site] initiated this tutorial.
The format of this tutorial is based on the [Software Carpentry][swc-site], which is an open-source project.

## Agreement

By contributing, you agree that we may redistribute your work under [our license](LICENSE.md).
In exchange, we will address your issues and/or assess your change proposal as promptly as we can, and help you become a member of our community.
Everyone involved in this [tutorial](site) agrees to abide by our [code of conduct](CODE_OF_CONDUCT.md).

## How to Contribute

There are many ways to contribute:

* If you do not have a [GitHub][github] account,
you can [send us comments by email][email].
However,
we will be able to respond more quickly if you use one of the other methods described below.

* If you have a [GitHub][github] account,
or are willing to [create one][github-join],
please work in this [repository][site],
which can be viewed at <https://esmvalgroup.github.io/tutorial>.
You can report problems or suggest improvements by [creating an issue][issues].
This is the easiest way to tell us about your ideas,
and a good way to introduce yourself
and to meet some of our community members.
This allows us to assign the item to someone
and to respond to it in a threaded discussion.
There are three templates to make an issue:
  * for reporting a bug, please use [bug reports][issues]
  * for developing lesson material, please use [New lesson material][issues]
  * for adding a feature to the repository, please use [New feature][issues]

* If you would like to add what is already discussed in an issue,
you can submit a [pull request][PR].
To do so, you can make use of the [pull request checklist][PR].
The reviewers are community volunteers who provide feedback.
The maintainers have final say over what gets merged into the tutorial.

## Tutorial content guides

This section demonstrates all the features that can be used when developing a lesson in [RMarkdown] (https://rmarkdown.rstudio.com/).
TODO level

### Lesson development

The content of this tutorial is developed based on the [Carpentries Curriculum Development Handbook][swc-handbook].

### Lesson organization

https://carpentries.github.io/lesson-example/03-organization/index.html


### Lesson formatting

https://carpentries.github.io/lesson-example/04-formatting/index.html

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
[site]: https://esmvalgroup.github.io/tutorial/
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
[ea-site]: https://github.com/escience-academy
[c-site]: https://carpentries.org/
[lc-site]: https://librarycarpentry.org/
[lc-issues]: https://github.com/issues?q=user%3Alibrarycarpentry
