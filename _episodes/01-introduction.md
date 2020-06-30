---
title: "Introduction"
teaching: 0
exercises: 25
questions:
- What is ESMValTool and when is it useful?
- What are the main components of ESMValTool?
- How does ESMValTool contribute to FAIR climate research?
- What is the role of the ESMValTool community?

objectives:
- Describe key strengths and weaknesses of ESMValTool
- Know the different components of ESMValTool
- Understand the concept of a community-driven software project

keypoints:
- ESMValTool provides a reliable interface to analyse and evaluate climate data
- Using ESMValTool promotes standardization, collaboration, and reuse
- ESMValTool is built and maintained by an active community of scientists and developers
- ESMValTool is written in Python, but supports diagnostic scripts in multiple languages

---

## What is ESMValTool

EMSValTool is first and foremost a tool to analyse climate data. But you probably already knew that and we like to think there's more to it than that. So let's start with a quick check to synchronize our expectations.

> ## Question: what is ESMValTool
>
> Which of the following items would you say apply to ESMValTool?
>
> - A tool to analyse climate data
> - The easy way out
> - A community effort
> - Free
> - A command line tool
> - A way to make climate science more [FAIR](https://fair-software.eu/about)
> - Perfect
> - Suitable for (Jupyter) notebooks
>
> Check our answers by unfolding the box below.
>
> > ## ESMValTool is
> >
> > &#10003; **A tool to analyse climate data.**  It takes care of finding, opening, checking, fixing, concatenating, and preprocessing CMIP data and several other supported datasets.
> >
> > &#10005;  **The easy way out.** If you just want to do an exploratory analysis or quickly hack something together, ESMValTool is probably not the way to go. The tool is intended for robust, repeatable and shareable climate analysis. That *does* require a bit more effort.
> >
> > &#10003; **A community effort.** EMSValTool is developed and maintained by a large team of climate scientists and software engineers. It is an open source project to which anyone can contribute. Its longevity depends on these contributions.
> >
> > &#10003; **Free.** ESMValTool is licenced under Apache 2.0, which means everyone can use, modify, or share it free of charge. However, we *do* encourage all users to contribute to the community once they get more comfortable with the tool.
> >
> > &#10003; **A command line tool.** ESMValTool was originally designed for the command line. But, we are working on a user-friendly python interface as well.
> >
> > &#10003; **A way to make climate science more [FAIR](https://fair-software.eu/about).** ESMValTool collects provenance information about the data and code that are used to obtain a result. It comes with a readable recipe format that makes climate analysis consistent, reproducible, and easy to share.
> >
> > &#10005;  **Perfect.** Although we are continuously working to improve the tool, you may encounter some bugs or missing features. In this lesson, you will learn how to troubleshoot, find help, and maybe even contribute to the solution yourself.
> >
> > &#10005;  **Suitable for (Jupyter) notebooks.** ESMValTool was designed as a command line tool. But, we are working on a user-friendly Python interface as well.
> {: .solution}
{: .challenge}

To learn more about ESMValTool, you can look at the [documentation](https://docs.esmvaltool.org/en/latest/introduction.html), the [official website](https://www.esmvaltool.org/about.html), or the [overview paper](https://gmd.copernicus.org/articles/13/1179/2020/) in *Geoscientific Model Development*.

## How does ESMValTool work

The figure below shows the different components of ESMValTool. Most of the work is done in the 'core', which performs a number of preprocessor steps. Outside of the core we have some configuration files that specify things like *where to find the CMIP data*. The most important file however, is the *recipe* that specifies which preprocessor functions need to be applied to what data. The recipe also points to a diagnostic script that is executed after the preprocessor and performs a more specific analysis on the preprocessed data.

![figure showing ESMValTool architecture]({{ page.root }}/fig/esmvaltool_architecture.png)

> ## Discussion: (dis)advantages of this approach
>
> Discuss or think about the pros and cons of this architecture for a moment. Then unfold the box below to see our answers.
>
>
> > ## See our thoughts
> >
> > - Streamlining common preprocessing steps ensures consistency in the algorithms being used and the way they are executed. This facilitates comparison and reproducibility.
> > - Provenance and citation information can be tracked through the entire workflow.
> > - The core builds upon the [iris](https://scitools.org.uk/iris/docs/latest/) package, which is quite strict in order to comply with CF conventions and minimize unexpected results.
> > - Recipes are easy to read and share.
> > - A collection of recipes and diagnostic scripts is shipped with ESMValTool, ready for re-use. Everyone can add to this collection.
> > - The recipe format takes some getting used to and may be a bit less flexible then working on the datasets directly.
> > - Features or dataset support that are missing from ESMValCore can be a limiting factor.
> {: .solution}
{: .discussion}

## Community

ESMValTool is built and maintained by an active community of scientists and engineers. Many of the interactions take place on GitHub. Here, we briefly introduce you to some of the most important pages.

> ## Challenge: meet ESMValGroup
>
> Browse to [github.com/ESMValGroup](https://github.com/ESMValGroup). This is our 'organization' GitHub page. Have a look around. How many collaborators are there? Do you know any of them? You should see 2 main repositories: ESMValTool and ESMValCore. Clicking on them will take you to there. How many people have contributed to each of them? Can you also find out how many people have contributed to this tutorial?
>
> > ## Solution
> >
> > At the time of making this lesson, there were 138 members of ESMValGroup. 55 of them contributed to ESMValTool, 48 to ESMValCore. 52 (!) people contributed to this tutorial.
> {: .solution}
{: .challenge}

> ## Challenge: issues and pull requests
>
> Go back to the repository pages of [ESMValTool](https://github.com/ESMValGroup/ESMValTool) or [ESMValCore](https://github.com/ESMValGroup/ESMValCore). You should see tabs for 'issues' and 'pull requests'. You can use the labels to navigate them a bit more. How many open issues are about enhancements of ESMValTool? And how many bugs have been fixed in ESMValCore? There is also an 'insights' tab, where you can see a summary of recent activity. How many issues have been opened and closed in the past month?
>
> > ## Solution
> >
> > At the time of making this tutorial, there were 50 ESMValTool issues (the majority) about enhancement and 31 bugs had been fixed in the Core. 13 ESMValTool issues had been closed in the past month, versus 8 opened: overall good progress.
> {: .solution}
{: .challenge}

## Conclusion

This concludes the introduction of the tutorial. You should now have a basic knowledge of ESMValTool and our community. The following episodes will walk you through the installation, configuration and running your first recipes.

{% include links.md %}
