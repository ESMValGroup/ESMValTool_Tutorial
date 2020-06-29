---
title: "Running a recipe (First example)"
teaching: 20
exercises: 25
questions:
- "What is a recipe?"
- "How can I do the same preprocessing on many different datasets?"
- "What are the files and directories after running a recipe?"
- "What happens when I run a recipe?"
objectives:
- "Run an ESMValTool recipe"
- "Understand the purpose of different settings in the recipe"
- "Inspect the output directories"
- "Examine the log information"
keypoints:
- "A recipe does not break by fiddling with it"
- "Log information is useful when interpreting the first warnings/errors"
- "The dataset section in the recipe relates to Understanding the directory structure of CMIP data on your server/disk and knowing how to use data from different experiments such as CMIP/ScenarioMIP"
---

This episode describes how ESMValTool recipes work, how to run a recipe and how to explore the recipe output. By the end of this episode, you should be able to run your first recipe, look at the recipe output, modify a recipe, explore and run some basic recipe debugging.

## Introduction to Recipes

Recipes are the instructions that you give to ESMValTool that tell it what you want to do. This includes four main sections: datasets, preprocessors, diagnostics and description.

- datasets: what datasets you want to use, including
 - the time range and time resolution,
 - the MIP, ensemble member,
 - the experiment (i.e. historical, ssp125 etc...),
 - and the grid type (CMIP6 only)

 This section can also be optional, as datasets can no preprocessing is needed.

- preprocessors: general operations applied to a dataset before handling it in a diagnostic, listing
 - which preprocessor modules to apply,
 - the order to apply them,
 - and the preprocessor arguments.

 This section can also be optional, if no preprocessing is needed.

- diagnostics: all the information about the diagnostic, including
 - list of variables to evaluate (with their respective configurations),
 - the desired diagnostic script to use,
 - and additional diagnostic script options or arguments, if needed.

 Also Include additional datasets beyond those included in the datasets section mentioned above, for instance variable specific observational data.

- description: a brief description of the recipe, including
 - who wrote the recipe and who maintains it,
 - which project is responsible for it,
 - and which publications, references are linked with the recipe.

 Note that the authors, publications and references are to be named in the config-references.yml

This information ...

## How to run ESMValTool

Once you’ve set up your conda environment and installed ESMValTool (see episode #2 LINK) and set up your config-user.yml file to correctly match you local environment, (see episode #3 LINK), ESMValTool is invoked using a simple command:
~~~
esmvaltool -c configuration recipe
~~~
{: .source}

To try your hand with a basic recipe, please work through this episode.


## Introduction to the example recipe
Threcipe presented here is a simple, basic recipe that takes a single dataset and produces a time series plot.

Please copy and paste the following recipe into your ESMValTool working area with the name: recipe_example.yml

>## recipe_example.yml
>~~~YAML
>      # ESMValTool
>      # recipe_example.yml
>      ---
>      documentation:
>        description: Demonstrate basic ESMValTool example
>
>        authors:
>          - demora_lee
>          - mueller_benjamin
>          - swaminathan_ranjini
>
>        maintainer:
>          - demora_lee
>
>        references:
>          - demora2018gmd
>          # Some plots also appear in ESMValTool paper 2.
>
>        projects:
>          - ukesm
>
>      datasets:
>        - {dataset: HadGEM2-ES, project: CMIP5, exp: historical, mip: Omon, ensemble: r1i1p1, start_year: 1859, end_year: 2005}
>
>      preprocessors:
>        prep_timeseries: # For 0D fields
>          annual_statistics:
>            operator: mean
>
>      diagnostics:
>        # --------------------------------------------------      
>        # Time series diagnostics
>        # --------------------------------------------------
>        diag_timeseries_temperature:
>          description: simple_time_series
>          variables:
>            timeseries_variable:
>              short_name: thetaoga
>              preprocessor: prep_timeseries
>          scripts:
>            timeseries_diag:
>              script: ocean/diagnostic_timeseries.py
>~~~
{: .solution}

> ## Explore the recipe
> Use the command and investigate the sample recipe.
> ~~~
> vim  recipe_example.yml
> ~~~
> {: .source}
>

Please note the following sections:
- documentation: lines 4-20

  The documentation consists of the following information:
  - description: a short description of the recipe
  - authors: a list of authors (linked to esmvaltool/config-references.yml)
  - maintainer: a list of maintainers (linked to esmvaltool/config-references.yml)
  - references: a list of references (linked to a bibtexfile in esmvaltool/references with the same name)
  - projects: a list of projects (linked to esmvaltool/config-references.yml)


  - datasets: lines 22-23

    The dataset definition consists of a list of python dictionaries with the information on the datasets.
    - dataset name (key: dataset)
    - project (key: project)
    - experiment (key: exp)
    - mip (for CMIP data, key: mip)
    - ensemble member (key: ensemble)
    - time range (e.g. key-value-pair: start_year: 1982, end_year: 1990)
    - model grid (for CMIP6 data only, key: grid)
    - alias (key: alias; use the alias for e.g. a more human readable name)


  - preprocessors: lines 25-28

    The definition for different preprocessors or combinations.
    If no preprocessing is needed, the preprocessor can be set to an empty python dictionary (`{}`).
    Here, we produce annual means. The preprocessor is called with its name (here: prep_timeseries), later in the diagnostic (line 39).
    (See episode #5 LINK for more details.)


- diagnostic section: lines 30-42

  The information of which diagnostic script to run with which variables.
  The diagnostics section has some indents that are free to call.
  - the first indent (here: diag_timeseries_temperature) is the diagnostic’s name (a string without whitespace), used for setting up the respective directories
  - description: a short description of the diagnostic
  - variables: a definition of all variables that are used in this diagnostic
  - the next indent (here: timeseries_variable) is the variables’ names (a string without whitespace) for the diagnostic to use
  - short_name: the variable name as listed in the dataset
  - preprocessor: the preprocessor(s) applied to the variable before running the diagnostic
  - scripts: a definition of all scripts that are used in this diagnostic
  - the next indent (here: timeseries_diag) is the scripts’ names (a string without whitespace) for the script to use
  - script: a executable script with a directory relative to the `esmvaltool/diag_scripts/` directory


> What is the short_name of the variable being analysed?
> What is the diagnostic script being used?
> How many years of data are being analysed?
> What do you think running this recipe will produce?
{: .challenge}

> ## Not all parts of the recipe are mandatory
> Some functionalities of the example recipe are mandatory, while others are not. E.g., if you miss any of the documentation information, the call will break.
{: .callout}

> ## Running ESMValTool
>
> Use the command:
> ~~~
> esmvaltool -c user-config.yml recipe_example.yml
> ~~~
> {: .source}
> What ...
{: .challenge}


> ## Inspect the output:
> Now that you have run the esmvaltool command for the first time, please locate your output directory.
> Each time you run ESMValTool, it will produce a new output directory with the following format:
> This directory should contain four folders:
>  run work preproc plots
> If you’re missing the preproc directory, then your config-user.yml file has the value remove_preproc_dir set to true (this is used to save disk space). Please set this value to false and run the recipe again.
>
> Please locate and inspect the following files:
>  Your output plot(s).
>  Your main output log file
>  Your settings.yml file
>  A metadata.yml file
{: .discussion}


> ## Edit the recipe and run
> So far, the example recipe has used global volume-weighted ocean temperature. Please edit this recipe to investigate one of the following fields:
> - Land surface average temperature (tsland)
> - Atmospheric surface average temperature (tas)
> - Ocean surface average temperature (tos)
>
> You will need to edit:
> - the dataset:
>   - mip, start_year, end_year
> - the preprocessor:
>   - These fields are all 2D fields, but thetaga was a 0D field. This means that we need to take the average over the latitude and longitude dimensions. To do this, add the area_statistics to the preprocessor.
> - the diagnostic
>  - change the short_name value (thetaoga) for another:
>     - Land surface average temperature (tsland)
>     - Atmospheric surface average temperature (tas)
>     - Ocean surface average temperature (tos)
> ### Advanced:
> If you want to add a different field, please have a look here:
> http://clipc-services.ceda.ac.uk/dreq/index/CMORvar.html
{: .challenge}


## Common issues & tips

### Esmvaltool not found
Can you run the command “esmvaltool -h”. If no, then it’s possible that the conda environment isn’t activated. Please return to the installation section, epside [2] LINK.  

### ESMValTool can’t locate the data
The error message is “esmvalcore._recipe_checks.RecipeError: Missing data”
Which computing machine are you using? Does your user-config.yml file reflect your machine's settings? Is the dataset’s name in the correct order?

### Diagnostic path problems
The directory path to your diagnostics code is set relative to the esmvaltool/diag_scripts subdirectory. Is the code placed in this subdirectory? Is it spelled correctly?

### FX files not found.

### The preprocessor works but the diagnostic fails:
If your preprocessor works fine but your diagnostic script fails, congratulations! A failed diagnostic means that you won’t need to re-run the preprocessor. In your “run/main_log.txt” run output, you should see a line that reads: “To re-run this diagnostic script, run:”, followed by a line with a command that will allow you to re-run your diagnostic script only. Append this line with the “-i” option after the python script you call to re-run your diagnostic.

### Your recipe’s name/project/reference isn’t recognised by ESMValTool.
Error message is “ValueError: Tag 'NAME' does not exist in section 'authors' of path/esmvaltool/config-references.yml”
Most likely, you added your own name to the recipe in the description section, but didn’t add it to the esmvaltool/config-references.yml file, where the names are linked to an email address, institute, and ORCID Identity.
