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
EMSValTool is first and foremost a tool to analyse climate data. But you probably already knew that, and we like to think there's more to it than that.So let's start with a quick check to synchronize our expectations.

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
> - Quite suitable for (Jupyter) notebooks
>
> Check our answers by unfolding the boxes below.
>
>> ## ESMValTool is ...
>>
>> - A tool to analyse climate data. It takes care of finding, opening, checking, fixing, concatenating, and preprocessing CMIP data and several other supported datasets.
>> - A way to make climate science more [FAIR](https://fair-software.eu/about). ESMValTool collects provenance information about the data and code that are used to obtain a result. It comes with a readible recipe format that makes climate analysis consistent, reproducbile, and easy to share.
>> - A community effort. EMSValTool is developed and maintained by a large team of climate scientists and software engineers. It is an open source project to which anyone can contribute. It's longevity depends on these contributions.
>> - A command line tool. ESMValTool was originally designed for the command line. But, we are working on a user-friendly python interface as well.
>> - Free. ESMValTool is licenced under Apache 2.0, which means everyone can use, modify, or share it free of charge. We *do* encourage all users to contribute to the community once they get more comfortable with the tool, though.
>{: .solution}
>
>> ## ESMValTool is not ...
>>
>> - Perfect. Although we are continuously working to improve the tool, you may encounter some bugs or missing features. In this lesson, you'll learn how to troubleshoot, find help, and maybe even contribute to the solution yourself.
>> - The easy way out. If you just want to do an exploratory analysis or quickly hack something together, ESMValTool is probably not the way to go. The tool is intended for robust, repeatable and shareable climate analysis.
>> - Quite suitable for (Jupyter) notebooks. ESMValTool was designed as a command line tool. But, we are working on a user-friendly Python interface as well.
>{: .solution}
{: .challenge}

To learn more about ESMValTool, you can find more information in the [documentation](https://docs.esmvaltool.org/en/latest/introduction.html), or the [overview paper](https://gmd.copernicus.org/articles/13/1179/2020/) in *Geoscientific Model Development*.

## How does ESMValTool work?

### ESMValTool and ESMValCore

Include figure describing ESMValTool

### Preprocessor

### Recipe (include callout on yml)

### Global settings (user-config.yml)

### Diagnostics

## Community

- How many people are connected to github?
- How many open issues?
- How many merged pull requests in the last month?

{% include links.md %}
