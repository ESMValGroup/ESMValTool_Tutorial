---
title: "Running your first recipe"
teaching: 25
exercises: 30
questions:
- "How to run a recipe?"
- "What output is created when I run a recipe?"
objectives:
- "Run an existing ESMValTool recipe"
- "Examine the log information"
- "Make small adjustments to an existing recipe"
- "Navigate the output created by ESMValTool"
keypoints:
- "ESMValTool recipes work 'out of the box' (if input data is available)"
- "Recipes can easily be modified to apply them to your own use case"
- "The log messages are useful to understand what the recipe is (not) doing"
---

This episode describes how ESMValTool recipes work, how to run a recipe and how
to explore the recipe output. By the end of this episode, you should be able to
run your first recipe, look at the recipe output, modify a recipe, explore and
run some basic recipe debugging.

## Running an existing recipe

The recipe format has briefly been introduced in episode 1. To see all the
recipes that are shipped with ESMValTool, type

```bash
esmvaltool recipes list
```

We will start by running [examples/recipe_python.yml](TODO provide RTD link)
FIXME:

```bash
esmvaltool run examples/recipe_python.yml
```

If everything is okay, you should see that ESMValTool is printing a lot of
output to the command line. The final message should be "Run was successful".
The exact output varies depending on your machine, but it should look something
like the example output below.

> ## Example output
>
> ```bash
> 2020-07-01 08:22:58,571 UTC [33433] INFO
> ______________________________________________________________________
>           _____ ____  __  ____     __    _ _____           _
>          | ____/ ___||  \/  \ \   / /_ _| |_   _|__   ___ | |
>          |  _| \___ \| |\/| |\ \ / / _` | | | |/ _ \ / _ \| |
>          | |___ ___) | |  | | \ V / (_| | | | | (_) | (_) | |
>          |_____|____/|_|  |_|  \_/ \__,_|_| |_|\___/ \___/|_|
> ______________________________________________________________________
>
> ESMValTool - Earth System Model Evaluation Tool.
>
> http://www.esmvaltool.org
>
> CORE DEVELOPMENT TEAM AND CONTACTS:
>   Veronika Eyring (PI; DLR, Germany - veronika.eyring@dlr.de)
>   Bouwe Andela (NLESC, Netherlands - b.andela@esciencecenter.nl)
>   Bjoern Broetz (DLR, Germany - bjoern.broetz@dlr.de)
>   Lee de Mora (PML, UK - ledm@pml.ac.uk)
>   Niels Drost (NLESC, Netherlands - n.drost@esciencecenter.nl)
>   Nikolay Koldunov (AWI, Germany - nikolay.koldunov@awi.de)
>   Axel Lauer (DLR, Germany - axel.lauer@dlr.de)
>   Benjamin Mueller (LMU, Germany - b.mueller@iggf.geo.uni-muenchen.de)
>   Valeriu Predoi (URead, UK - valeriu.predoi@ncas.ac.uk)
>   Mattia Righi (DLR, Germany - mattia.righi@dlr.de)
>   Manuel Schlund (DLR, Germany - manuel.schlund@dlr.de)
>   Javier Vegas-Regidor (BSC, Spain - javier.vegas@bsc.es)
>   Klaus Zimmermann (SMHI, Sweden - klaus.zimmermann@smhi.se)
>
> For further help, please read the documentation at
> http://esmvaltool.readthedocs.io. Have fun!
>
> 2020-07-01 08:22:58,573 UTC [33433] INFO    Using config file /pf/b/b380506/work/config-DKRZ.yml
> 2020-07-01 08:22:58,573 UTC [33433] INFO    Writing program log files to:
> /scratch/b/b380506/recipe_example_20200701_082257/run/main_log.txt
> /scratch/b/b380506/recipe_example_20200701_082257/run/main_log_debug.txt
> 2020-07-01 08:22:58,574 UTC [33433] INFO    Starting the Earth System Model Evaluation Tool v2.0.0b9 at time: 2020-07-01 08:22:58 UTC
> 2020-07-01 08:22:58,574 UTC [33433] INFO    ----------------------------------------------------------------------
> 2020-07-01 08:22:58,574 UTC [33433] INFO    RECIPE   = /pf/b/b380506/work/recipes/recipe_example.yml
> 2020-07-01 08:22:58,575 UTC [33433] INFO    RUNDIR     = /scratch/b/b380506/recipe_example_20200701_082257/run
> 2020-07-01 08:22:58,575 UTC [33433] INFO    WORKDIR    = /scratch/b/b380506/recipe_example_20200701_082257/work
> 2020-07-01 08:22:58,575 UTC [33433] INFO    PREPROCDIR = /scratch/b/b380506/recipe_example_20200701_082257/preproc
> 2020-07-01 08:22:58,575 UTC [33433] INFO    PLOTDIR    = /scratch/b/b380506/recipe_example_20200701_082257/plots
> 2020-07-01 08:22:58,575 UTC [33433] INFO    ----------------------------------------------------------------------
> 2020-07-01 08:22:58,575 UTC [33433] INFO    Running tasks using at most 8 processes
> 2020-07-01 08:22:58,575 UTC [33433] INFO    If your system hangs during execution, it may not have enough memory for keeping this number of tasks in memory.
> 2020-07-01 08:22:58,575 UTC [33433] INFO    If you experience memory problems, try reducing 'max_parallel_tasks' in your user configuration file.
> 2020-07-01 08:22:58,607 UTC [33433] INFO    Creating tasks from recipe
> 2020-07-01 08:22:58,607 UTC [33433] INFO    Creating tasks for diagnostic diag_timeseries_temperature
> 2020-07-01 08:22:58,607 UTC [33433] INFO    Creating preprocessor task diag_timeseries_temperature/timeseries_variable
> 2020-07-01 08:22:58,608 UTC [33433] INFO    Creating preprocessor 'prep_timeseries' task for variable 'thetaoga'
> 2020-07-01 08:22:58,610 UTC [33433] INFO    Using input files for variable thetaoga of dataset HadGEM2-ES:
> /mnt/lustre01/work/kd0956/CMIP5/data/cmip5/output1/MOHC/HadGEM2-ES/historical/mon/ocean/Omon/r1i1p1/v20110916/thetaoga/thetaoga_Omon_HadGEM2-ES_historical_r1i1p1_185912-200512.nc
> 2020-07-01 08:22:58,617 UTC [33433] INFO    PreprocessingTask diag_timeseries_temperature/timeseries_variable created. It will create the files:
> /scratch/b/b380506/recipe_example_20200701_082257/preproc/diag_timeseries_temperature/timeseries_variable/CMIP5_HadGEM2-ES_Omon_historical_r1i1p1_thetaoga_1859-2005.nc
> 2020-07-01 08:22:58,617 UTC [33433] INFO    Creating diagnostic task diag_timeseries_temperature/timeseries_diag
> 2020-07-01 08:22:58,618 UTC [33433] INFO    These tasks will be executed: diag_timeseries_temperature/timeseries_variable, diag_timeseries_temperature/timeseries_diag
> 2020-07-01 08:22:58,621 UTC [33433] INFO    Running 2 tasks using 2 processes
> 2020-07-01 08:22:58,641 UTC [39196] INFO    Starting task diag_timeseries_temperature/timeseries_variable in process [39196]
> 2020-07-01 08:22:58,735 UTC [33433] INFO    Progress: 1 tasks running, 1 tasks waiting for ancestors, 0/2 done
> 2020-07-01 08:23:05,884 UTC [39196] INFO    Successfully completed task diag_timeseries_temperature/timeseries_variable (priority 0) in 0:00:07.241490
> 2020-07-01 08:23:05,963 UTC [33433] INFO    Progress: 0 tasks running, 1 tasks waiting for ancestors, 1/2 done
> 2020-07-01 08:23:05,969 UTC [39197] INFO    Starting task diag_timeseries_temperature/timeseries_diag in process [39197]
> 2020-07-01 08:23:05,975 UTC [39197] INFO    Running command ['/pf/b/b380506/miniconda3/envs/esmvaltool_pub/bin/python', '/mnt/lustre01/pf/b/b380506/work/GIT/ESMValTool/esmvaltool/diag_scripts/ocean/diagnostic_timeseries.py', '/scratch/b/b380506/recipe_example_20200701_082257/run/diag_timeseries_temperature/timeseries_diag/settings.yml']
> 2020-07-01 08:23:05,976 UTC [39197] INFO    Writing output to /scratch/b/b380506/recipe_example_20200701_082257/work/diag_timeseries_temperature/timeseries_diag
> 2020-07-01 08:23:05,976 UTC [39197] INFO    Writing plots to /scratch/b/b380506/recipe_example_20200701_082257/plots/diag_timeseries_temperature/timeseries_diag
> 2020-07-01 08:23:05,976 UTC [39197] INFO    Writing log to /scratch/b/b380506/recipe_example_20200701_082257/run/diag_timeseries_temperature/timeseries_diag/log.txt
> 2020-07-01 08:23:05,977 UTC [39197] INFO    To re-run this diagnostic script, run:
> cd /scratch/b/b380506/recipe_example_20200701_082257/run/diag_timeseries_temperature/timeseries_diag; MPLBACKEND="Agg" /pf/b/b380506/miniconda3/envs/esmvaltool_pub/bin/python /mnt/lustre01/pf/b/b380506/work/GIT/ESMValTool/esmvaltool/diag_scripts/ocean/diagnostic_timeseries.py /scratch/b/b380506/recipe_example_20200701_082257/run/diag_timeseries_temperature/timeseries_diag/settings.yml
> 2020-07-01 08:23:06,064 UTC [33433] INFO    Progress: 1 tasks running, 0 tasks waiting for ancestors, 1/2 done
> 2020-07-01 08:23:13,312 UTC [39197] INFO    Maximum memory used (estimate): 0.2 GB
> 2020-07-01 08:23:13,313 UTC [39197] INFO    Sampled every second. It may be inaccurate if short but high spikes in memory consumption occur.
> 2020-07-01 08:23:13,316 UTC [39197] WARNING No provenance information was written to /scratch/b/b380506/recipe_example_20200701_082257/run/diag_timeseries_temperature/timeseries_diag/diagnostic_provenance.yml
> 2020-07-01 08:23:13,316 UTC [39197] INFO    Successfully completed task diag_timeseries_temperature/timeseries_diag (priority 1) in 0:00:07.347165
> 2020-07-01 08:23:13,380 UTC [33433] INFO    Progress: 0 tasks running, 0 tasks waiting for ancestors, 2/2 done
> 2020-07-01 08:23:13,380 UTC [33433] INFO    Successfully completed all tasks.
> 2020-07-01 08:23:13,395 UTC [33433] INFO    Ending the Earth System Model Evaluation Tool v2.0.0b9 at time: 2020-07-01 08:23:13 UTC
> 2020-07-01 08:23:13,395 UTC [33433] INFO    Time for running the recipe was: 0:00:14.820802
> 2020-07-01 08:23:14,283 UTC [33433] INFO    Maximum memory used (estimate): 0.7 GB
> 2020-07-01 08:23:14,284 UTC [33433] INFO    Sampled every second. It may be inaccurate if short but high spikes in memory consumption occur.
> 2020-07-01 08:23:14,286 UTC [33433] INFO    Run was successful
> ```
>
{: .solution}

Let's dissect what's happening here.


> ## Locating the output directory
>
> After the logo and general information, the output starts with some important locations.
>
> - Did ESMValTool use the right config file?
> - What is the main output folder generated by ESMValTool?
> - Can you guess what the different output directories are for?
>
{: .challenge}

> ## No 'preproc' directory?
>
> If you’re missing the preproc directory, then your `config-user.yml` file has
> the value remove_preproc_dir set to true (this is used to save disk space).
> Please set this value to false and run the recipe again.
>
{: .callout}

> ## ESMValTool search paths
>
> You might wonder how ESMValTool was able find the recipe file, even though
> it's not in your working directory. All the recipe paths printed from
> `esmvaltool recipes list` are relative to ESMValTools installation location.
> This is where ESMValTool will look if it cannot find the file by following the
> path from your working directory.
{: .callout}





> ## Inspect specific files
>
> Please locate and inspect the following files:
>
> - Your output plot(s).
> - Your main output log file.
> - Your settings.yml file.
> - A metadata.yml file.
> - The diagnostic log file.
{: .checklist}

Exemplary output (depending on the directory paths and package versions that are
available) can be found below. Note that the timestamps differ.

> ## Your output plot(s)
>
> Plots for the dataset(s) are located in `./recipe_example_#_#/plots/timeseries_diag/`
>
> For a single dataset:
>
> ![single dataset](../fig/diag_CMIP5_HadGEM2-ES_Omon_historical_r1i1p1_thetaoga_prep_timeseries_diag_timeseries_temperature_1900_2000_timeseries_0.png "single dataset")
>
> Or an overlay plot, if multiple datasets are defined:
>
> ![multiple datasets](../fig/MultipleModels__thetaoga_prep_timeseries_diag_timeseries_temperature_1900_2000_timeseries_.png "multiple datasets")
{: .solution}

> ## Your main output log file
>
> The main output file is located at `./recipe_example_#_#/run/main_log.txt/`
> Let's have look at the file:
>
> ~~~bash
> nano ./recipe_example_#_#/run/main_log.txt
> ~~~
>
> Note: This is the same information as the terminal output, above, but without the time stamps.
{: .solution}

> ##  Your settings.yml file
>
> This file is located at `./recipe_example_#_#/run/diag_timeseries_temperature/timeseries_diag/settings.yaml`.
> Let's have look at the file:
>
> ~~~bash
> nano ./recipe_example_#_#/run/diag_timeseries_temperature/timeseries_diag/settings.yaml
> ~~~
>
{: .solution}

> ## A metadata.yml file.
>
> This file is located at `./recipe_example_#_#/preproc/diag_timeseries_temperature/timeseries_variable/metadata.yml`.
> Please note that `preproc` diretctory exist if only `remove_preproc_dir: false`
> in ``config-user.yml`` file.
> Let's have look at the file:
>
> ~~~bash
> nano ./recipe_example_#_#/preproc/diag_timeseries_temperature/timeseries_variable/metadata.yml
> ~~~
>
{: .solution}

> ## The diagnostic log file.
>
> This file is located at `./recipe_example_#_#/run/diag_timeseries_temperature/timeseries_diag/log.txt`
> Let's have look at the file:
>
> ~~~bash
> nano ./recipe_example_#_#/run/diag_timeseries_temperature/timeseries_diag/log.txt
> ~~~
>
{: .solution}





## Introduction to recipes

Recipes are the instructions that you give to ESMValTool that tell it what you
want to do. This includes four main sections: datasets, preprocessors,
diagnostics and description.

  - datasets: what datasets you want to use, including
    - the time period and temporal resolution,
    - the MIP (Model Intercomparison Project, like atmospheric realm of MIP
      monthly data: Amon),
    - ensemble member,
    - the experiment (e.g. historical, ssp125),
    - and the grid type (necessary for CMIP6 only).

  - preprocessors: general operations applied to a dataset before handling it in
    a diagnostic, defining
    - which preprocessor modules to apply,
    - the order in which they are applied,
    - and the preprocessor arguments.

    This section can also be optional, if no preprocessing is needed.

  - diagnostics: all the information about the diagnostic, including
    - list of variables to evaluate (with their respective configurations),
    - the desired diagnostic script to use,
    - and additional diagnostic script options or arguments, if needed.

    It is possible to also include additional datasets beyond those included in
    the datasets section mentioned above, for instance variable specific
    observational data.

  - description: a brief description of the recipe, including
    - who wrote the recipe and who maintains it,
    - which project the recipe was written for,
    - and which publications and references are linked with the recipe.

    Note that the authors, publications and references need to be included in
    the `config-references.yml` for the recipe to run successfully.

The information you provide in the recipe is not only affecting the processes
you are starting, but also the directory names your output will be structured
in.
For additional reads, please have a look at the recipe format description in the
[ESMValTool manual](https://docs.esmvaltool.org/projects/esmvalcore/en/latest/recipe/overview.html#recipe-section-diagnostics).

## How to run ESMValTool

Once you’ve set up your conda environment and installed ESMValTool (see
[episode #2]({{ page.root}}{% link _episodes/02-installation.md %})
) and set up your `config-user.yml` file to correctly match your local
environment, (see
[episode #3]({{ page.root}}{% link _episodes/03-configuration.md %})
), ESMValTool is invoked using the command:

~~~bash
esmvaltool run /path/to/recipe_example.yml
~~~

If the configuration file is not in the default location
`~/.esmvaltool/config-user.yml`, you can pass its path explicitly:

~~~bash
esmvaltool run --config_file /path/to/config-user.yml /path/to/recipe_example.yml
~~~

Note that the path to the recipe can be either a path to a recipe file or it can
be the name of an installed recipe.
To view the list of installed recipes, run
~~~bash
esmvaltool recipes list
~~~

To try your hand with a basic recipe, please work through this episode.

## Introduction to the example recipe
The recipe presented here is a simple, basic recipe that takes a single dataset
and produces a time series plot.

Please download
[recipe_example.yml](https://github.com/ESMValGroup/ESMValTool_Tutorial/blob/master/files/recipe_example.yml)
into your ESMValTool working directory:

~~~bash
wget https://raw.githubusercontent.com/ESMValGroup/ESMValTool_Tutorial/master/files/recipe_example.yml
~~~

> ## recipe_example.yml
> ```YAML
>  1    # ESMValTool
>  2    # recipe_example.yml
>  3    ---
>  4    documentation:
>  5      description: Demonstrate basic ESMValTool example
>  6
>  7      authors:
>  8        - demora_lee
>  9        - mueller_benjamin
> 10        - swaminathan_ranjini
> 11
> 12      maintainer:
> 13        - demora_lee
> 14
> 15      references:
> 16        - demora2018gmd
> 17        # Some plots also appear in ESMValTool paper 2.
> 18
> 19      projects:
> 20        - ukesm
> 21
> 22    datasets:
> 23      - {dataset: HadGEM2-ES, project: CMIP5, exp: historical, mip: Omon, ensemble: r1i1p1, start_year: 1900, end_year: 2000}
> 24
> 25    preprocessors:
> 26      prep_timeseries: # For 0D fields
> 27        annual_statistics:
> 28          operator: mean
> 29
> 30    diagnostics:
> 31      # --------------------------------------------------
> 32      # Time series diagnostics
> 33      # --------------------------------------------------
> 34      diag_timeseries_temperature:
> 35        description: simple_time_series
> 36        variables:
> 37          timeseries_variable:
> 38            short_name: thetaoga
> 39            preprocessor: prep_timeseries
> 40        scripts:
> 41          timeseries_diag:
> 42            script: ocean/diagnostic_timeseries.py
> ```
{: .solution}

> ## Explore the recipe
>
> We use the text editor ``nano`` to investigate the sample recipe.
> ~~~bash
> nano recipe_example.yml
> ~~~
{: .challenge}

> ## Text editor side note
>
> No matter what editor you use, you will need to know where it searches
> for and saves files. If you start it from the shell, it will (probably)
> use your current working directory as its default location. We use ``nano``
> in examples here because it is one of the least complex text editors.
> Press <kbd>ctrl</kbd> + <kbd>O</kbd> to save the file,
> and then <kbd>ctrl</kbd> + <kbd>X</kbd> to exit ``nano``.
> The line numbers can be shown by pressing <kbd>alt</kbd> + <kbd>shift</kbd> + <kbd>3</kbd>.
{: .callout}

Please note the following sections:

  - documentation: lines 4-20

    The documentation consists of the following information:

    - description: a short description of the recipe
    - authors: a list of authors
    - maintainer: a list of maintainers
    - references: a list of references
    - projects: a list of projects

> ## config-references.yml and references
>
> The values for the keys ``author``, ``maintainer``, ``projects`` and
> ``references`` in the recipe should be known by ESMValTool:
>
> - A list of ESMValTool
> author, maintainer, and projects are mentioned in the file ``config-references.yml``
> that can be found in
> [ESMValTool/esmvaltool](https://github.com/ESMValGroup/ESMValTool/blob/master/esmvaltool/config-references.yml).
>
> - ESMValTool references in `BibTeX` format can be found in
> [ESMValTool/esmvaltool/references](https://github.com/ESMValGroup/ESMValTool/tree/master/esmvaltool/references) directory.
{: .callout}

  - datasets: lines 22-23

    The dataset definition consists of a list of python dictionaries with the
    information on the datasets.

    - dataset name (key: dataset)
    - project (key: project)
    - experiment (key: exp)
    - mip (for CMIP data, key: mip)
    - ensemble member (key: ensemble)
    - time range (e.g. key-value-pair: start_year: 1900, end_year: 2000)
    - model grid (for CMIP6 data only, key: grid)
    - alias (key: alias; use the alias for e.g. a more human readable name for
      the dataset)

  - preprocessors: lines 25-28

    The definition for different preprocessors or preprocessor combinations.
    If no preprocessing is needed, the preprocessor can be set to an empty
    map (`{}`). Here, we produce annual means. The preprocessor is called
    with its name (here: prep_timeseries), later in the diagnostic (line 39).
    (See [episode #5]({{ page.root}}{% link _episodes/05-preprocessor.md %})
    for more details.)

  - diagnostic section: lines 30-42

    The information of which diagnostic script to run with which variables. The
    diagnostics section has some indents that are free to be called.

    - the first indent (here: diag_timeseries_temperature) is the diagnostic’s
      name (a string without whitespace), used for setting up the respective
      directories
    - description: a short description of the diagnostic
    - variables: a definition of all variables that are used in this diagnostic
    - the next indent (here: timeseries_variable) is the variables’ names (a
      string without whitespace) for the diagnostic to use
    - short_name: the variable name as listed in the dataset
    - preprocessor: the preprocessor(s) applied to the variable before running
      the diagnostic
    - scripts: a definition of all scripts that are used in this diagnostic
    - the next indent (here: timeseries_diag) is the scripts’ names (a string
      without whitespace) for the script to use
    - script: a script that will produce the plots. The path can be either
      relative the ESMValTool installation with subdirectory
      [`esmvaltool/diag_scripts/`](https://github.com/ESMValGroup/ESMValTool/tree/master/esmvaltool/diag_scripts)
      or an absolute path. In this case, it is a diagnostic script that is
      installed with ESMValTool.

> ## Please answer the following questions:
>
> What is the short_name of the variable being analyzed?
>
> What is the diagnostic script being used?
>
> How many years of data are being analyzed?
>
> What do you think running this recipe will produce?
{: .challenge}

> ## What is the short_name of the variable being analyzed?
>
> thetaoga -  Global Average Sea Water Potential Temperature
{: .solution}

> ## What is the diagnostic script being used?
>
> The installed copy of
> [`ocean/diagnostic_timeseries.py`](https://github.com/ESMValGroup/ESMValTool/blob/master/esmvaltool/diag_scripts/ocean/diagnostic_timeseries.py)
{: .solution}

> ## How many years of data are being analyzed?
>
> 1900 to 2000.
{: .solution}

> ## What do you think running this recipe will produce?
>
> A time series plot of thetaoga with increments of 1 year.
{: .solution}

> ## Not all parts of the recipe are mandatory
>
> Some functionalities of the example recipe are mandatory, while others are
> not. E.g., if you miss any of the documentation information, the call will
> break.
{: .callout}




## Do your first edits

> ## Edit the recipe and run again
>
> So far, the example recipe has used global volume-weighted ocean temperature.
> Please edit this recipe to investigate one of the following fields:
>
> - Land surface temperature (ts) for dataset HadGEM2-ES for 1970-2000
> - Atmospheric surface average temperature (tas) for dataset HadGEM2-ES for 1970-2000
> - Ocean surface average temperature (tos) for datasets HadGEM2-ES and HadGEM2-CC for 1970-2000
>
> You will need to edit:
>
> - the dataset:
>
>   - `mip`, `start_year`, `end_year`
>
> - the preprocessor:
>
>   - These fields are all 2D fields, but thetaoga was a 0D field. This means
>     that we need to take the average over the latitude and longitude
>     dimensions. To do this, add the area_statistics to the preprocessor.
>
> - the diagnostic:
>
>  - change the short_name value (thetaoga) for another:
>
>     - Land surface average temperature (ts)
>     - Atmospheric surface average temperature (tas)
>     - Ocean surface average temperature (tos)
>
{: .challenge}

The snippets for the edits can be found below:

> ## Land surface average temperature
>
> In the `diff` file below you will see the changes we have made to the file. The top 2 lines are the filenames and the lines like @@ -20,12 +20,14 @@ indicate the line numbers in the original and modified file, respectively. For more info on this format, see [here](https://en.wikipedia.org/wiki/Diff#Unified_format).
>
> ```diff
> --- data/recipe_example.yml
> +++ data/recipe_example_ts.yml
> @@ -20,12 +20,14 @@
>      - ukesm
>
>  datasets:
> -  - {dataset: HadGEM2-ES, project: CMIP5, exp: historical, mip: Omon, ensemble: r1i1p1, start_year: 1900, end_year: 2000}
> +  - {dataset: HadGEM2-ES, project: CMIP5, exp: historical, mip: Amon, ensemble: r1i1p1, start_year: 1970, end_year: 2000}
>
>  preprocessors:
>    prep_timeseries: # For 0D fields
>      annual_statistics:
>        operator: mean
> +    area_statistics:
> +      operator: mean
>
>  diagnostics:
>    # --------------------------------------------------
> @@ -35,7 +37,7 @@
>      description: simple_time_series
>      variables:
>        timeseries_variable:
> -        short_name: thetaoga
> +        short_name: ts
>          preprocessor: prep_timeseries
>      scripts:
>        timeseries_diag:
> ```
>
> Note: The x-axis in the plot now shows the years 1970 - 2000.
>
> Complete recipe can be downloaded as [recipe_example_ts.yml](https://github.com/ESMValGroup/ESMValTool_Tutorial/blob/master/files/recipe_example_ts.yml)
{: .solution}

> ## Atmospheric surface average temperature
>
> ```diff
> --- data/recipe_example.yml
> +++ data/recipe_example_tas.yml
> @@ -20,12 +20,14 @@
>      - ukesm
>
>  datasets:
> -  - {dataset: HadGEM2-ES, project: CMIP5, exp: historical, mip: Omon, ensemble: r1i1p1, start_year: 1900, end_year: 2000}
> +  - {dataset: HadGEM2-ES, project: CMIP5, exp: historical, mip: Amon, ensemble: r1i1p1, start_year: 1970, end_year: 2000}
>
>  preprocessors:
>    prep_timeseries: # For 0D fields
>      annual_statistics:
>        operator: mean
> +    area_statistics:
> +      operator: mean
>
>  diagnostics:
>    # --------------------------------------------------
> @@ -35,7 +37,7 @@
>      description: simple_time_series
>      variables:
>        timeseries_variable:
> -        short_name: thetaoga
> +        short_name: tas
>          preprocessor: prep_timeseries
>      scripts:
>        timeseries_diag:
> ```
>
> Complete recipe can be downloaded as [recipe_example_tas.yml](https://github.com/ESMValGroup/ESMValTool_Tutorial/blob/master/files/recipe_example_tas.yml)
{: .solution}

> ## Ocean surface average temperature
>
> ```diff
> --- data/recipe_example.yml
> +++ data/recipe_example_tos.yml
> @@ -20,12 +20,15 @@
>      - ukesm
>
>  datasets:
> -  - {dataset: HadGEM2-ES, project: CMIP5, exp: historical, mip: Omon, ensemble: r1i1p1, start_year: 1900, end_year: 2000}
> +  - {dataset: HadGEM2-ES, project: CMIP5, exp: historical, mip: Omon, ensemble: r1i1p1, start_year: 1970, end_year: 2000}
> +  - {dataset: HadGEM2-CC, project: CMIP5, exp: historical, mip: Omon, ensemble: r1i1p1, start_year: 1970, end_year: 2000}
>
>  preprocessors:
>    prep_timeseries: # For 0D fields
>      annual_statistics:
>        operator: mean
> +    area_statistics:
> +      operator: mean
>
>  diagnostics:
>    # --------------------------------------------------
> @@ -35,7 +38,7 @@
>      description: simple_time_series
>      variables:
>        timeseries_variable:
> -        short_name: thetaoga
> +        short_name: tos
>          preprocessor: prep_timeseries
>      scripts:
>        timeseries_diag:
> ```
>
> Note: The unit in the plots is now degrees celsius! There are now 3 plots in the work directory. One for each dataset and one for the multiple dataset overview.
>
> Complete recipe can be downloaded as [recipe_example_tos.yml](https://github.com/ESMValGroup/ESMValTool_Tutorial/blob/master/files/recipe_example_tos.yml)
{: .solution}

> ## Advanced
>
> If you want to add a different field, please have a look here:
> http://clipc-services.ceda.ac.uk/dreq/index/CMORvar.html
{: .callout}

## Common issues & tips

> ## esmvaltool not found
>
> Can you run the command `esmvaltool -h`. If no, then it’s possible that the
> conda environment isn’t activated. Please return to the in
> [installation section]({{ page.root }}{% link _episodes/02-installation.md %}).
{: .solution}

> ## The error message is `esmvalcore._recipe_checks.RecipeError: Missing data`
>
> ESMValTool can’t locate the data.
>
> Which computing machine are you using? Does your `user-config.yml` file reflect
> your machine's settings? Is the dataset’s name in the correct order?
{: .solution}

> ## Diagnostic path problems
>
> The directory path to your diagnostics code is set relative to the
> `esmvaltool/diag_scripts` subdirectory. Is the code placed in this subdirectory?
> Is it spelled correctly?
{: .solution}

> ## FX files not found
>
> There is no FX file for your corresponding dataset. Are your datasets’ names
> spelled correctly? Is there a FX file for this dataset?
{: .solution}

> ## The preprocessor works but the diagnostic fails
>
> If your preprocessor works fine but your diagnostic script fails,
> congratulations! A failed diagnostic means that you won’t need to re-run the
> preprocessor. In your `run/main_log.txt` run output, you should see a line
> that reads: “To re-run this diagnostic script, run:”, followed by a line with
> a command that will allow you to re-run your diagnostic script only. Append
> this line with the “-i” option after the python script you call to re-run your
> diagnostic.
{: .solution}

> ## `ValueError: Tag 'NAME' does not exist in section 'authors' of path/esmvaltool/config-references.yml`
>
> Your recipe’s name/project/reference isn’t recognised by ESMValTool.
>
> Most likely, you added your own name to the recipe in the description section,
> but didn’t add it to the esmvaltool/config-references.yml file, where the
> names are linked to an email address, institute, and ORCID Identity.
{: .solution}
