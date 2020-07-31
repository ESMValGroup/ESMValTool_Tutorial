# Contributing to ESMValTool tutorial

[ESMValTool][tutorial-site] tutorial is an open-source project in
[ESMValGroup][ESMValTool-site] and we greatly value contributions of all kinds:
fixes to this tutorial, bug reports, reviews of pull requests, infrastructure
improvements, community help, and outreach. We value the time you invest in
contributing and strive to make the process as easy as possible. If you have
suggestions for improving the process of contributing, please open an
[issue][issues]. To do so, click on `New issue` in the `issues` tab of this
repository and choose the `Suggestion` template.

## Acknowledgement

The format of this tutorial is based on the [Software Carpentry][swc-site],
which is an open-source project.

## Agreement

By contributing, you agree that we may redistribute your work under [our
license](LICENSE.md). In exchange, we will address your issues and/or assess
your change proposal as promptly as we can, and help you become a member of our
community. Everyone involved in this [tutorial](tutorial-repo) agrees to abide
by our [code of conduct](CODE_OF_CONDUCT.md).

## How to Contribute

There are many ways to contribute:

* If you do not have a [GitHub][github] account, you can send us comments by
  [email][email]. However, we will be able to respond more quickly when you use
  one of the other methods described below.

* If you have a [GitHub][github] account, work in this
  [repository][tutorial-repo], which can be viewed at the tutorial
  [site][tutorial-site]. You can ask your questions, report problems or suggest
  improvements by [creating an issue][issues]. This is the easiest way to tell
  us about your ideas, and a good way to introduce yourself and to meet some of
  our community members. This allows us to assign the item to someone and to
  respond to it in a threaded discussion. To open an issue, click on `New issue`
  in the `issues` tab of this repository and choose a template from the
  [list](https://github.com/ESMValGroup/tutorial/issues/new/choose) as described
  below:
  * for asking a question, please use `Question and answer`.
  * for reporting a bug, please use `Bug reports`.
  * for developing lesson material, please use `New lesson material`.
  * for adding a feature to the repository, please use `Suggestion`.

* If you would like to add what is already discussed in an issue, you can submit
  a [pull request][PR] and make use of the `pull request checklist`. Each pull
  request is reviewed at least by one reviewer who is a community volunteer. The
  [maintainers][tutorial-maintainers] have final say over what gets merged into
  the tutorial.

## Tutorial guidelines

This section demonstrates all the instructions for developing a lesson in the
[ESMValTool tutorial][tutorial-site]. The tutorial is a set of lessons (or
episodes) that together teach **basic** skills needed to work with
[ESMValTool][ESMValTool-doc] in climate-related domains.

### Lesson development

The content of this tutorial is mainly developed based on the [Carpentries
Curriculum Development Handbook][swc-handbook]. The handbook explains why we
teach the way we do, and why our lessons are designed the way they are. If you
are contributing to existing lesson materials, please make sure the content
conforms to the concepts provided in the handbook.

We recommend using software to check spelling or grammatical errors. The
following link will guide you through a list of tools for several editors:
<http://wiki.languagetool.org/software-that-supports-languagetool-as-a-plug-in-or-add-on>

### Lesson organization

Each lesson is made up of episodes, which are focused on a particular topic and
include time for both teaching and exercises. If you are making a new episode,
please make sure the content conforms to the [Carpentries lesson
organization][swc-lesson-organization].

### Lesson formatting

Episodes are [Markdown](https://en.wikipedia.org/wiki/Markdown) files. If you
are making a new episode or contributing to existing ones, please make sure the
content conforms to the [Carpentries lesson formatting][swc-lesson-formatting].

We also, recommend using a linter to check errors in Markdown files. For
example, a
[markdownlint](https://marketplace.visualstudio.com/items?itemName=DavidAnson.vscode-markdownlint)
can be installed as an extension in [Visual Studio
Code](https://code.visualstudio.com/). Alternatively, a linter can be installed
with:

```bash
gem install mdl
```

and can be used as:

```bash
mdl your_markdown_filename
```

We use [Codacy](https://app.codacy.com/gh/ESMValGroup/ESMValTool_Tutorial)
as an automated code analysis services. To run the analyis
[locally](https://github.com/codacy/codacy-analysis-cli)
on Markdown files with Docker use

```bash
docker run \
  --rm=true \
  --env CODACY_CODE=${PWD} \
  --volume /var/run/docker.sock:/var/run/docker.sock \
  --volume ${PWD}:${PWD} \
  --volume /tmp:/tmp \
  codacy/codacy-analysis-cli \
  analyse --tool remark-int
```

### Previewing your changes locally

If you are making a new episode or contributing to existing ones,
please preview changes on your machine before submitting a
[pull request][PR]. To do so, you need to install
the software described below:

```bash
# apt (Ubuntu/Devian)
sudo apt install ruby-dev ruby-bundler
```

or

```bash
# dnf (Fedora/Redhat)
sudo dnf install rubygem-bundler ruby-devel
```

Alternatively, there is an environment file available which can be installed
with:

```bash
conda env create -f environment.yml -n esmvaltool_tutorial
```

To install the required ruby packages, run the following command in the
tutorial's main directory to build and serve the website locally:

```bash
make serve
```

The output on the terminal will contain a line similar to:

```bash
Server address: http://127.0.0.1:4000
```

This address can be opened in your browser to preview the tutorial website.

## Other Resources

General discussion of the tutorial happens in
ESMValTool [User Engagement Team][user-engagement].
You can reach us by [email][email].

[email]: mailto:esmvaltool@listserv.dfn.de
[ESMValTool-site]: https://www.esmvaltool.org/
[ESMValTool-doc]: https://esmvaltool.readthedocs.io/en/latest/
[tutorial-repo]: https://esmvalgroup.github.io/ESMValTool_Tutorial/
[tutorial-site]: https://esmvalgroup.github.io/ESMValTool_Tutorial
[tutorial-maintainers]: https://github.com/ESMValGroup/ESMValTool_Tutorial#maintainers
[github]: https://github.com
[issues]: https://github.com/ESMValGroup/ESMValTool_Tutorial/issues
[PR]: https://github.com/ESMValGroup/ESMValTool_Tutorial/pulls
[swc-site]: https://software-carpentry.org/
[swc-handbook]: https://carpentries.github.io/curriculum-development/
[swc-lesson-organization]: https://carpentries.github.io/lesson-example/03-organization/index.html
[swc-lesson-formatting]: https://carpentries.github.io/lesson-example/04-formatting/index.html
[user-engagement]: https://github.com/orgs/ESMValGroup/teams/userengagementteam
