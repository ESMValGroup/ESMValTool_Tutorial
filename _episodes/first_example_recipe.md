---
title: "Running a recipe (First example)"
teaching: 10
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
- "Log information is useful to How to interpret the first warnings/errors"
- "The dataset section in the recipe relates to Understanding the directory structure of CMIP data on your server/disk and knowing how to use data from different experiments such as CMIP/ScenarioMIP."
---

This episode describes how ESMValTool recipes work, how to run a recipe and how to explore the recipe output. By the end of this episode, you should be able to run your first recipe, look at the recipe output, modify a recipe, explore and run some basic recipe debugging.

## Introduction to Recipes

Recipes are the instructions that you give to ESMValTool that tell it what you want to do. This includes four main section:
- datasets: what datasets you want to use
 - the time range and time resolution,
 - the MIP, ensemble member,
 - the experiment (ie historical, ssp125 etc...)
 - the grid type (CMIP6 only)
 - This section can also be optional, as datasets can no preprocessing is needed.

- proprocessors: one or preprocessors
 - which preprocessor modules to apply
 - the order to apply them
 - and the preprocesor arguments.
 - This section can also be optional, if no preprocessing is needed.

- diagnostics: All the information about diagnostic
 -

- description: a brief description of the recipe
 - who wrote the recipe, and who maintains it
 - which project is responsible for it
 - which publications, reference are linked with the recipe
 - Note that the authors, publications and references and named in the config-references.yml

This information

## How to run ESMValTool

Once you’ve set up your conda environment and installed ESMValTool (See episode #2) and set up your config-user.yml file to correctly match you local environment, (see episode #3), ESMValTool is invoked using a simple command:
~~~
esmvaltool -c configuration recipe
~~~

To try your hand with a basic recipe, please work through this episode.


## Example recipe
This is a basic recipe that takes a simple dataset and produces a simple plot.
Please copy and paste the following recipe into your ESMValTool working area with the name: recipe_example.yml

    # ESMValTool
    # recipe_example.yml
    ---
    documentation:
      description: |
        Demonstrate basic ESMValTool example

      authors:
        - demora_lee
        - Ben
        - Ranjini

      maintainer:
        - demora_lee

      references:
        - demora2018gmd
        # Some plots also appear in ESMValTool paper 2.

      projects:
        - ukesm


    preprocessors:
      # --------------------------------------------------
      # Time series preprocessors
      # --------------------------------------------------
      prep_timeseries: # For 0D fields
        multi_model_statistics:
          span: full
          statistics: [mean ]


    diagnostics:
      # --------------------------------------------------
      # Time series diagnostics
      # --------------------------------------------------
      diag_timeseries_temperature:
        variables:
          thetaoga:
            preprocessor: prep_timeseries
            additional_datasets:
              - {dataset: CESM1-BGC, project: CMIP5, exp: historical, mip: Omon, ensemble: r1i1p1, start_year: 1850, end_year: 2005, }
              - {dataset: CNRM-CM5, project: CMIP5, exp: historical, mip: Omon, ensemble: r1i1p1, start_year: 1850, end_year: 2005, }
              - {dataset: CSIRO-Mk3-6-0, project: CMIP5, exp: historical, mip: Omon, ensemble: r1i1p1, start_year: 1850, end_year: 2005, }
              - {dataset: CanESM2, project: CMIP5, exp: historical, mip: Omon, ensemble: r1i1p1, start_year: 1850, end_year: 2005, }
              - {dataset: GFDL-ESM2M, project: CMIP5, exp: historical, mip: Omon, ensemble: r1i1p1, start_year: 1861, end_year: 2005, }
              - {dataset: HadGEM2-ES, project: CMIP5, exp: historical, mip: Omon, ensemble: r1i1p1, start_year: 1859, end_year: 2005, }
              - {dataset: IPSL-CM5A-LR, project: CMIP5, exp: historical, mip: Omon, ensemble: r1i1p1, start_year: 1850, end_year: 2005, }
              - {dataset: MIROC-ESM, project: CMIP5, exp: historical, mip: Omon, ensemble: r1i1p1, start_year: 1850, end_year: 2005, }
              - {dataset: MPI-ESM-LR, project: CMIP5, exp: historical, mip: Omon, ensemble: r1i1p1, start_year: 1850, end_year: 2005, }
              - {dataset: NorESM1-M, project: CMIP5, exp: historical, mip: Omon, ensemble: r1i1p1, start_year: 1850, end_year: 2005, }
        scripts:
          timeseries_diag:
            script: ocean/diagnostic_timeseries.py

> ## Explore the recipe
>
> Use the command and investigate the sample recipe.
> vim  recipe.yml
>
> Please note the datasets, preprocessors, diagnostic sections.
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
> esmvaltool -c user-config.yml recipe_example.yml
>
> What
{: .challenge}


> ## Inspect the output:
> Now that you have run the esmvaltool command for the first time, please locate your output directory.
> Each time you run ESMValTool, it will produce a new output directory with the format:
> This directory should contain four folders:
>  run work preproc plots
> If you’re missing the preproc directory, then your config-user.yml file has the value remove_preproc_dir set to true.


>
> Please locate and inspect the following files:
>  You output plot(s).
>  Your main output log file
>  Your settings.yml file
>  A metadata.yml file
{: .discussion}


> ## Edit the recipe and run
> So far, the example recipe has used global volume-weighted ocean temperature. Please edit this recipe to investigate one of the following fields:
>  Land surface temperature
>  Atmospheric surface temperature
>  Ocean surface temperature (tos)
> You will need to edit the dataset request, c
{: .challenge}


> ## Common issues & tips
>
> ### ESMValTool can’t locate the data
> User didn’t correctly edit user-config.yml
>
> ### Esmvaltool not found
> User didn’t active or correctly install conda
>
> ### Diagnostic path problems
> explain ESMValTool’s methods to determine diagnostic path, and how it should appear in the recipe
>
> ### FX files not found.
>
> ### The preprocessor works but the diagnostic fails:
> How to tell them appart and how to re-run a failed diagnostic (but not the preprocessor)
>
> ### Your recipe’s name/project/reference isn’t recognised by ESMValTool.
>
{: .callout}
