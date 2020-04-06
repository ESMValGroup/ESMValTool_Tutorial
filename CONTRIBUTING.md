# Contributing to ESMValTool tutorial

[ESMValTool](https://www.esmvaltool.org/) tutorial is an open-source project and we greatly value contributions of all kinds: fixes to this tutorial, bug reports, reviews of pull requests, infrastructure improvements, community help, and outreach. We value the time you invest in contributing and strive to make the process as easy as possible. If you have suggestions for improving the process of contributing, please open an [issue][issues] by choosing the ``new feature`` template.

## Acknowledgement

The format of this tutorial is based on the [Software Carpentry][swc-site], which is an open-source project.
TODO escience academy

## Agreement

By contributing, you agree that we may redistribute your work under [our license](LICENSE.md).
In exchange, we will address your issues and/or assess your change proposal as promptly as we can, and help you become a member of our community.
Everyone involved in this [tutorial](site) agrees to abide by our [code of conduct](CODE_OF_CONDUCT.md).

## How to Contribute

There are many ways to contribute:

If you wish to change this tutorial,
please work in <https://github.com/esmvalgroup/tutorial>,
which can be viewed at <https://esmvalgroup.github.io/tutorial>.
There are many ways to contribute,
from writing new exercises and improving existing ones
to updating or filling in the documentation
and submitting [bug reports][issues]
about things that don't work, aren't clear, or are missing.
If you are looking for ideas, please see the 'Issues' tab for
a list of issues associated with this repository,



* If you do not have a [GitHub][github] account,
you can [send us comments by email][email].
However,
we will be able to respond more quickly if you use one of the other methods described below.

* If you have a [GitHub][github] account,
or are willing to [create one][github-join],
you can report problems or suggest improvements by [creating an issue][issues].
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
The reviewers are community volunteers to provide feedback.
The maintainers have final say over what gets merged into the tutorial.

## Lesson development guides

### Lesson development

### Lesson organization

### Lesson formatting

### Preview your changes locally



## Using GitHub

If you choose to contribute via GitHub, you may want to look at
[How to Contribute to an Open Source Project on GitHub][how-contribute].
To manage changes, we follow [GitHub flow][github-flow].


1.  Fork the originating repository to your GitHub profile.
2.  Within your version of the forked repository, move to the `gh-pages` branch and
create a new branch for each significant change being made.
3.  Navigate to the file(s) you wish to change within the new branches and make revisions as required.
4.  Commit all changed files within the appropriate branches.
5.  Create individual pull requests from each of your changed branches
to the `gh-pages` branch within the originating repository.


When starting work, please make sure your clone of the originating `gh-pages` branch is up-to-date
before creating your own revision-specific branch(es) from there.
Additionally, please only work from your newly-created branch(es) and *not*
your clone of the originating `gh-pages` branch.
Lastly, published copies of all the lessons are available in the `gh-pages` branch of the originating
repository for reference while revising.

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
```
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
[swc-issues]: https://github.com/issues?q=user%3Aswcarpentry
[swc-lessons]: https://software-carpentry.org/lessons/
[swc-site]: https://software-carpentry.org/
[c-site]: https://carpentries.org/
[lc-site]: https://librarycarpentry.org/
[lc-issues]: https://github.com/issues?q=user%3Alibrarycarpentry
