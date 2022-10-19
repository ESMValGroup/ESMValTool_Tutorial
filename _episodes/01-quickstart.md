---
title: "Quickstart guide"
teaching: 2
exercises: 8
compatibility: ESMValTool v.2.6

questions:

- "What is the purpose of the quickstart guide?"
- "How do I load and check the ESMValTool environment?"
- "How do I configure ESMValTool?"
- "How do I run a recipe?"

objectives:

- "Understand the purpose of the quickstart guide"
- "Load and check the ESMValTool environment"
- "Configure ESMValTool"
- "Run a recipe"

keypoints:

- "The purpose of the quickstart guide is to enable a user of ESMValTool
  to run ESMValTool as quickly as possible without having to go through the  
  whole tutorial"
- "Use the `module load` command to load the ESMValTool environment, 
  see the [installation]({{ page.root }}{% link _episodes/02-installation.md %}) 
  episode for more details and use `esmvaltool --help` to check the ESMValTool 
  environment"
- "Use `esmvaltool config get_config_user` to create the ESMValTool user 
  configuration file"
- "Use `esmvaltool run <recipe>.yml` to run a recipe"
---


 {: .callout}

> ## What is the purpose of the quickstart guide?
>
> - The purpose of the quickstart guide is to enable a user of ESMValTool to
>   run ESMValTool as quickly as possible by making the bare minimum number of 
>   changes.
{: .discussion}

> ## How do I load and check the ESMValTool environment?
>
> - For this quickstart guide, an assumption is made that ESMValTool has 
>   already been installed at the site where ESMValTool will be run. If this is
>   not the case, see the [Installation][lesson-installation] episode in this
>   tutorial.
> 
> - Load the ESMValTool environment by following the instructions at
>   [ESMValTool: Pre-installed versions on HPC clusters / other 
>   servers][activate-environment].
>
> - Check the ESMValTool environment by accessing the help for ESMValTool:
>
>     ~~~
>     esmvaltool --help
>     ~~~
>     {: .language-bash}
{: .challenge}

> ## How do I configure ESMValTool?
>
> - Create the ESMValTool user configuration file (the file is written by 
>   default to `~/.esmvaltool/config-user.yml`):
>
>     ~~~
>     esmvaltool config get_config_user
>     ~~~
>     {: .language-bash}
> 
> - Edit the ESMValTool user configuration file using your favourite text editor
>   to uncomment the lines relating to the site where ESMValTool will be run.
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
> - Wait for the recipe to complete. If the recipe completes successfully, the
>   last line printed to screen at the end of the log will look something like: 
>
>    ~~~
>    YYYY-MM-DD HH:mm:SS, NNN UTC [NNNNN] INFO    Run was successful
>    ~~~
>    {: .language-bash}
>
> - View the output of the recipe by opening the HTML file produced by 
>   ESMValTool (the location of this file is printed to screen near the end of
>   the log):
>
>   ~~~
>   YYYY-MM-DD HH:mm:SS, NNN UTC [NNNNN] INFO    Wrote recipe output to:
>   file:///$HOME/esmvaltool_output/recipe_python_<date>_<time>/index.html
>   ~~~
>   {: .language-bash}
>
> - For more details about running recipes see the
>   [Running your first recipe][lesson-recipe] episode in this tutorial.
{: .challenge}

{% include links.md %}