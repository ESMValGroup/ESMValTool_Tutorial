---
title: "Introduction"
teaching: 0
exercises: 10
questions:
-  What is ESMValTool?
-  When to use ESMValTool?
-  What are the main parts of ESMValTool that I need to know?
-  How does ESMValTool contribute to making climate research FAIR?
-  What is the role of the ESMValTool community?
-  Where to find help if I'm stuck with ESMValTool?

objectives:
-  Describe the difference between ESMValTool and other tools like CDO or xarray
-  Understand the main parts of ESMValTool (recipe, diagnostic script)
-  Browse the documentation of ESMValTool for help
-  Exlain why ESMValTool is a great way to make climate analysis FAIR
-  List three different ways to get help from to the ESMValTool community (docs, user engagement email, github issues)
-  Know when (not) to use ESMValTool

keypoints:
-  ESMValTool provides a reliable interface to analyse and evaluate climate data
-  By streamlining common preprocessor functions, ESMValTool facilitates comparison
-  ESMValTool is built and maintained by an active community of climate scientists and software developers
-  Using ESMValTool stimulates standardization, collaboration, and reuse
-  ESMValTool is written in Python, but supports diagnostic scripts in multiple languages

---

## What is ESMValTool?
EMSValTool is first and foremost a tool to analyse climate data. But you probably already knew that and we like to think there's more to it than that. So let's start with a quick check to synchronize our expectations.

> ## Question: what is ESMValTool?
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
> Check our answers by unfolding the boxes below.
>
>> ## ESMValTool is ...
>>
>>  - A tool to analyse climate data. It takes care of finding, opening, checking, fixing, concatenating, and preprocessing CMIP data and several other supported datasets.
>>  - A way to make climate science more [FAIR](https://fair-software.eu/about). ESMValTool collects provenance information about the data and code that are used to obtain a result. It comes with a readible recipe format that makes climate analysis consistent, reproducbile, and easy to share.
>>  - A community effort. EMSValTool is developed and maintained by a large team of climate scientists and software engineers. It is an open source project to which anyone can contribute. It's longevity depends on these contributions.
>>  - A command line tool. ESMValTool was originally designed for the command line. But, we are working on a user-friendly python interface as well.
>>  - Free. ESMValTool is licenced under Apache 2.0, which means everyone can use, modify, or share it free of charge. We *do* encourage all users to contribute to the community once they get more comfortable with the tool, though.
>{: .solution}
>
>> ## ESMValTool is not ...
>>
>>  - The easy way out. If you just want to do an exploratory analysis or quickly hack something together, ESMValTool is probably not the way to go. The tool is intended for robust, repeatable and shareable climate analysis. That *does* require a bit more effort.
>>  - Perfect. Although we are continuously working to improve the tool, you may encounter some bugs or missing features. In this lesson, you'll learn how to troubleshoot, find help, and maybe even contribute to the solution yourself.
>>  - Suitable for (Jupyter) notebooks. ESMValTool was designed as a command line tool. But, we are working on a user-friendly Python interface as well.
>{: .solution}
{: .challenge}

To learn more about ESMValTool is, you can look at the [documentation](https://docs.esmvaltool.org/en/latest/introduction.html), the [official website](https://www.esmvaltool.org/about.html), or the [overview paper](https://gmd.copernicus.org/articles/13/1179/2020/) in *Geoscientific Model Development*.

## How does ESMValTool work?
The figure below shows the different components of ESMValTool. Most of the work is done in the 'core', which performs a number of preprocessor steps. Outside of the core we have some configuration files that specify things like *where to find the CMIP data*. The most important file however, is the *recipe* that specifies which preprocessor functions need to be applied to what data. The recipe also points to a diagnostic script that is executed after the preprocessor and performs a more specific analysis on the preprocessed data.

![figure showing ESMValTool architecture]({{ page.root }}/fig/esmvaltool_architecture.png)

>## Discussion: (dis)advantages of this approach?
> Discuss or think about the pro's and cons of this architecture for a moment. Then unfold the box below to see our answers.
>
>
>>## See our thoughts
>>
>>  - Streamlining common preprocessing steps ensures consistency in the algorithms being used and the way they are executed. This facilitates comparison and reproducibility.
>>  - Provenance and citation information can be tracked through the entire workflow.
>>  - The core builds upon the [iris](https://scitools.org.uk/iris/docs/latest/) package, which is quite strict in order to minimize unexpected results.
>>  - Recipes are easy to read and share.
>>  - A collection of recipes and diagnostic scripts is shipped with ESMValTool, ready for re-use. Everyone can add to this collection.
>>  - The recipe format takes some getting used to and may be a bit less flexible then working on the datasets directly.
>>  - Missing features can be more of a limiting factor.
>{: .solution}
{: .discussion}

## Community

some exercises:

- How many people are connected to github?
- How many open issues?
- How many merged pull requests in the last month?

{% include links.md %}
