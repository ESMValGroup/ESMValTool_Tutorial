---
title: "Introduction"
teaching: 5
exercises: 10
questions:
- What is ESMValTool?
- Who are the people behind ESMValTool?

objectives:
- Familiarize with ESMValTool
- Synchronize expectations

keypoints:
- ESMValTool provides a reliable interface to analyse and evaluate climate data
- A large collection of recipes and diagnostic scripts is already available
- ESMValTool is built and maintained by an active community of scientists and
  developers
---

## What is ESMValTool?

This tutorial is a first introduction to ESMValTool. Before diving into the
technical steps, let's talk about what ESMValTool is all about.

> ## What is ESMValTool?
>
> What do you already know about, or expect from ESMValTool?
>
> > ## ESMValTool is...
> >
> > EMSValTool is many things, but in this tutorial we will focus on the
> > following traits:
> >
> > &#10003; **A tool to analyse climate data**
> >
> > &#10003; **A collection of diagnostics for reproducible climate science**
> >
> > &#10003; **A community effort**
> >
> {: .solution}
{: .challenge}

## A tool to analyse climate data

ESMValTool takes care of finding, opening, checking, fixing, concatenating, and
preprocessing CMIP data and several other supported datasets.

The central component of ESMValTool that we will see in this tutorial is the
**recipe**. Any ESMValTool recipe is basically a set of instructions to reproduce
a certain result. The basic structure of a recipe is as follows:

- **Documentation** with relevant (citation) information
- **Datasets** that should be analysed
- **Preprocessor** steps that must be applied
- **Diagnostic** scripts performing more specific evaluation steps

An example recipe could look like this:

```yaml
documentation:
  description: Example recipe
  authors:
    - lastname_firstname

datasets:
  - {dataset: HadGEM2-ES, project: CMIP5, exp: historical, mip: Amon, ensemble: r1i1p1, start_year: 1960, end_year: 2005}

preprocessors:
  global_mean:
    area_statistics:
      operator: mean

diagnostics:
  hockeystick_plot:
    description: plot of global mean temperature change
    variables:
      temperature:
        short_name: tas
        preprocessor: global_mean
    scripts: hockeystick.py
```

> ## Understanding the different section of the recipe
>
> Try to figure out the meaning of the different dataset keys. Hint: they can
> be found in the documentation of ESMValTool.
>
> > ## Solution
> > The keys are explained in the ESMValTool documentation, in the section `The
> recipe format`, under
> [datasets](https://docs.esmvaltool.org/projects/esmvalcore/en/latest/recipe/overview.html#recipe-section-datasets)
> {: .solution}
{: .challenge}

## A collection of diagnostics for reproducible climate science

More than a tool, ESMValTool is a collection of publicly available recipes and
diagnostic scripts. This makes it possible to easily reproduce important
results.

> ## Explore the available recipes
>
> Go to the [documentation of esmvaltool](https://docs.esmvaltool.org/) and
> explore the `available recipes` section. Which recipe(s) would you like to
> try?
{: .challenge}

## A community effort

ESMValTool is built and maintained by an active community of scientists and
software engineers. It is an open source project to which anyone can contribute.
Many of the interactions take place on GitHub. Here, we briefly introduce you to
some of the most important pages.

> ## Meet ESMValGroup
>
> Browse to [github.com/ESMValGroup](https://github.com/ESMValGroup). This is
> our 'organization' GitHub page. Have a look around. How many collaborators are
> there? Do you know any of them?
>
> Near the top of the page there are 2 pinned repositories: ESMValTool and
> ESMValCore. Visit each of the repositories. How many people have contributed
> to each of them? Can you also find out how many people have contributed to
> this tutorial?
{: .challenge}

> ## Issues and pull requests
>
> Go back to the repository pages of
> [ESMValTool](https://github.com/ESMValGroup/ESMValTool) or
> [ESMValCore](https://github.com/ESMValGroup/ESMValCore). There are tabs for
> 'issues' and 'pull requests'. You can use the labels to navigate them a bit
> more. How many open issues are about enhancements of ESMValTool? And how many
> bugs have been fixed in ESMValCore? There is also an 'insights' tab, where you
> can see a summary of recent activity. How many issues have been opened and
> closed in the past month?
{: .challenge}

## Conclusion

This concludes the introduction of the tutorial. You now have a basic knowledge
of ESMValTool and its community. The following episodes will walk you through
the installation, configuration and running your first recipes.

{% include links.md %}
