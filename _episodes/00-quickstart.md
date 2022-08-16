---
title: "Quickstart Guide"
teaching: 1
exercises: 10
questions:

- "What is the purpose of the quickstart guide?"
- "How do I activate and test the ESMValTool environment?"
- "How do I configure ESMValTool?"
- "How do I run a recipe?"

objectives:

- "Understand the purpose of the quickstart guide"
- "Activate and test the ESMValTool environment"
- "Configure ESMValTool"
- "Run a recipe"

keypoints:

- "The purpose of the quickstart guide is to enable a user of ESMValTool
  to run ESMValTool as quickly as possible"
- "Use the `module load` command to activate the ESMValTool environment 
  (see the installation guide for more details) and use `esmvaltool -- --help` to 
  test the ESMValTool environment"
- "Use `esmvaltool config get_config_user` to create the ESMValTool user 
  configuration file"
- "Use `esmvaltool run <recipe>.yml` to run a recipe"
---
> ## What is the purpose of the quickstart guide?
>
> The purpose of the quickstart guide is to enable the user to run the 
> ESMValTool as quickly as possible by making the bare minimum number of 
> changes.
{: .discussion}

> ## How do I activate and test the ESMValTool environment?
>
> - For this quickstart guide, an assumption is made that ESMValTool has 
>   already been installed. If this is not the case, see the 
>   [Installation][lesson-installation] episode in this tutorial.
> 
> - Activate the ESMValTool environment by following the instruction at
>   [ESMValTool: Pre-installed versions on HPC clusters / other 
>   servers][activate-environment].
>
> - Test the ESMValTool environment by accessing the help for ESMValTool:
>
>     ~~~
>     esmvaltool -- --help
>     ~~~
>     {: .language-bash}
{: .challenge}

> ## How do I configure ESMValTool?
>
> - Create the ESMValTool user configuration file (the file is written to the 
>   default directory `~/.esmvaltool`):
>
>     ~~~
>     esmvaltool config get_config_user
>     ~~~
>     {: .language-bash}
> 
> - Edit the ESMValTool user configuration file to uncomment the lines relating
>   to the host you are using to run ESMValTool.
>
> - For more details about the ESMValTool user configuration file see the
>   [Configuration][lesson-configuration] episode in this tutorial.
{: .challenge}

> ## How do I run a recipe?
> 
> - Run the example Python recipe:
> 
>   ~~~
>   esmvaltool run examples/recipe_python.yml 
>   ~~~
>   {: .language-bash}
>
> - If everything has been set up correctly, the last line of code should look
>   something like this: 
>
>    ~~~
>    YYYY-MM-DD HH:mm:SS, NNN UTC [NNNNN] INFO    Run was successful
>    ~~~
>    {: .language-bash}
>
> - View the output of the recipie in the HTML file (the location of this file 
>   is printed to screen near the end of the log):
>
>   ~~~
>   YYYY-MM-DD HH:mm:SS, NNN UTC [NNNNN] INFO    Wrote recipe output to:
>   file:///$HOME/esmvaltool_output/recipe_python_<date>_<time>/index.html
>   ~~~
>   {: .language-bash}
>
> - For more details about running recipes see 
>   [Running your first recipe][lesson-recipe] episode in this tutorial.
{: .challenge}

{% include links.md %}