---
title: "Running your first recipe"
teaching: 15
exercises: 15
questions:
- "How to run a recipe?"
- "What happens when I run a recipe?"
objectives:
- "Run an existing ESMValTool recipe"
- "Examine the log information"
- "Navigate the output created by ESMValTool"
- "Make small adjustments to an existing recipe"
keypoints:
- "ESMValTool recipes work 'out of the box' (if input data is available)"
- "There are strong links between the recipe, log file, and output folders"
- "Recipes can easily be modified to re-use existing code for your own use case"
---

This episode describes how ESMValTool recipes work, how to run a recipe and how
to explore the recipe output. By the end of this episode, you should be able to
run your first recipe, look at the recipe output, and make small modifications.

## Running an existing recipe

The recipe format has briefly been introduced in episode 1. To see all the
recipes that are shipped with ESMValTool, type

```bash
esmvaltool recipes list
```

We will start by running [examples/recipe_python.yml](https://github.com/ESMValGroup/ESMValTool/blob/master/esmvaltool/recipes/examples/recipe_python.yml)

```
esmvaltool run examples/recipe_python.yml
```

If everything is okay, you should see that ESMValTool is printing a lot of
output to the command line. The final message should be "Run was successful".
The exact output varies depending on your machine, but it should look something
like the example output below.

> ## Example output
>
> ```
> INFO    [29586]
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
> http://docs.esmvaltool.org. Have fun!
>
> INFO    [29586] Using config file esmvaltool_config.yml
> INFO    [29586] Writing program log files to:
> /home/user/esmvaltool_tutorial/output/recipe_python_20201007_141734/run/main_log.txt
> /home/user/esmvaltool_tutorial/output/recipe_python_20201007_141734/run/main_log_debug.txt
> INFO    [29586] Starting the Earth System Model Evaluation Tool v2.0.0 at time: 2020-10-07 14:17:34 UTC
> INFO    [29586] ----------------------------------------------------------------------
> INFO    [29586] RECIPE   = /home/user/gh/esmvalgroup/ESMValTool/esmvaltool/recipes/examples/recipe_python.yml
> INFO    [29586] RUNDIR     = /home/user/esmvaltool_tutorial/output/recipe_python_20201007_141734/run
> INFO    [29586] WORKDIR    = /home/user/esmvaltool_tutorial/output/recipe_python_20201007_141734/work
> INFO    [29586] PREPROCDIR = /home/user/esmvaltool_tutorial/output/recipe_python_20201007_141734/preproc
> INFO    [29586] PLOTDIR    = /home/user/esmvaltool_tutorial/output/recipe_python_20201007_141734/plots
> INFO    [29586] ----------------------------------------------------------------------
> INFO    [29586] Running tasks using at most 1 processes
> INFO    [29586] If your system hangs during execution, it may not have enough memory for keeping this number of tasks in memory.
> INFO    [29586] If you experience memory problems, try reducing 'max_parallel_tasks' in your user configuration file.
> INFO    [29586] Creating tasks from recipe
> INFO    [29586] Creating tasks for diagnostic map
> INFO    [29586] Creating preprocessor task map/tas
> INFO    [29586] Creating preprocessor 'select_january' task for variable 'tas'
> INFO    [29586] Using input files for variable tas of dataset BCC-ESM1:
> /home/user/esmvaltool_tutorial/data/cmip6/tas_Amon_BCC-ESM1_historical_r1i1p1f1_gn_185001-201412.nc
> INFO    [29586] Using input files for variable tas of dataset CanESM2:
> /home/user/esmvaltool_tutorial/data/cmip5/tas_Amon_CanESM2_historical_r1i1p1_185001-200512.nc
> INFO    [29586] PreprocessingTask map/tas created. It will create the files:
> /home/user/esmvaltool_tutorial/output/recipe_python_20201007_141734/preproc/map/tas/CMIP5_CanESM2_Amon_historical_r1i1p1_tas_2000-2000.nc
> /home/user/esmvaltool_tutorial/output/recipe_python_20201007_141734/preproc/map/tas/CMIP6_BCC-ESM1_Amon_historical_r1i1p1f1_tas_2000-2000.nc
> INFO    [29586] Creating diagnostic task map/script1
> INFO    [29586] Creating tasks for diagnostic timeseries
> INFO    [29586] Creating preprocessor task timeseries/tas_amsterdam
> INFO    [29586] Creating preprocessor 'annual_mean_amsterdam' task for variable 'tas'
> INFO    [29586] Using input files for variable tas of dataset BCC-ESM1:
> /home/user/esmvaltool_tutorial/data/cmip6/tas_Amon_BCC-ESM1_historical_r1i1p1f1_gn_185001-201412.nc
> INFO    [29586] Using input files for variable tas of dataset CanESM2:
> /home/user/esmvaltool_tutorial/data/cmip5/tas_Amon_CanESM2_historical_r1i1p1_185001-200512.nc
> INFO    [29586] PreprocessingTask timeseries/tas_amsterdam created. It will create the files:
> /home/user/esmvaltool_tutorial/output/recipe_python_20201007_141734/preproc/timeseries/tas_amsterdam/CMIP5_CanESM2_Amon_historical_r1i1p1_tas_1850-2000.> nc
> /home/user/esmvaltool_tutorial/output/recipe_python_20201007_141734/preproc/timeseries/tas_amsterdam/> CMIP6_BCC-ESM1_Amon_historical_r1i1p1f1_tas_1850-2000.nc
> INFO    [29586] Creating preprocessor task timeseries/tas_global
> INFO    [29586] Creating preprocessor 'annual_mean_global' task for variable 'tas'
> INFO    [29586] Using input files for variable tas of dataset BCC-ESM1:
> /home/user/esmvaltool_tutorial/data/cmip6/tas_Amon_BCC-ESM1_historical_r1i1p1f1_gn_185001-201412.nc
> INFO    [29586] Using input files for variable tas of dataset CanESM2:
> /home/user/esmvaltool_tutorial/data/cmip5/tas_Amon_CanESM2_historical_r1i1p1_185001-200512.nc
> INFO    [29586] PreprocessingTask timeseries/tas_global created. It will create the files:
> /home/user/esmvaltool_tutorial/output/recipe_python_20201007_141734/preproc/timeseries/tas_global/CMIP6_BCC-ESM1_Amon_historical_r1i1p1f1_tas_1850-2000.> nc
> /home/user/esmvaltool_tutorial/output/recipe_python_20201007_141734/preproc/timeseries/tas_global/CMIP5_CanESM2_Amon_historical_r1i1p1_tas_1850-2000.nc
> INFO    [29586] Creating diagnostic task timeseries/script1
> INFO    [29586] These tasks will be executed: timeseries/tas_global, timeseries/tas_amsterdam, map/tas, map/script1, timeseries/script1
> INFO    [29586] Running 5 tasks sequentially
> INFO    [29586] Starting task map/tas in process [29586]
> INFO    [29586] Successfully completed task map/tas (priority 0) in 0:00:04.291697
> INFO    [29586] Starting task map/script1 in process [29586]
> INFO    [29586] Running command ['/home/user/miniconda3/envs/esmvaltool_tutorial/bin/python3.8', '/home/user/gh/esmvalgroup/ESMValTool/esmvaltool/diag_scripts/> examples/diagnostic.py', '/home/user/esmvaltool_tutorial/output/recipe_python_20201007_141734/run/map/script1/settings.yml']
> INFO    [29586] Writing output to /home/user/esmvaltool_tutorial/output/recipe_python_20201007_141734/work/map/script1
> INFO    [29586] Writing plots to /home/user/esmvaltool_tutorial/output/recipe_python_20201007_141734/plots/map/script1
> INFO    [29586] Writing log to /home/user/esmvaltool_tutorial/output/recipe_python_20201007_141734/run/map/script1/log.txt
> INFO    [29586] To re-run this diagnostic script, run:
> cd /home/user/esmvaltool_tutorial/output/recipe_python_20201007_141734/run/map/script1; MPLBACKEND="Agg" /home/user/miniconda3/envs/esmvaltool_tutorial/bin/> python3.8 /home/user/gh/esmvalgroup/ESMValTool/esmvaltool/diag_scripts/examples/diagnostic.py /home/user/esmvaltool_tutorial/output/> recipe_python_20201007_141734/run/map/script1/settings.yml
> INFO    [29586] Maximum memory used (estimate): 0.3 GB
> INFO    [29586] Sampled every second. It may be inaccurate if short but high spikes in memory consumption occur.
> INFO    [29586] Successfully completed task map/script1 (priority 1) in 0:00:03.574651
> INFO    [29586] Starting task timeseries/tas_amsterdam in process [29586]
> INFO    [29586] Generated PreprocessorFile: /home/user/esmvaltool_tutorial/output/recipe_python_20201007_141734/preproc/timeseries/tas_amsterdam/> MultiModelMean_Amon_tas_1850-2000.nc
> INFO    [29586] Successfully completed task timeseries/tas_amsterdam (priority 2) in 0:00:09.730443
> INFO    [29586] Starting task timeseries/tas_global in process [29586]
> WARNING [29586] /home/user/miniconda3/envs/esmvaltool_tutorial/lib/python3.8/site-packages/iris/analysis/cartography.py:394: UserWarning: Using > DEFAULT_SPHERICAL_EARTH_RADIUS.
>   warnings.warn("Using DEFAULT_SPHERICAL_EARTH_RADIUS.")
>
> INFO    [29586] Calculated grid area shape: (1812, 64, 128)
> WARNING [29586] /home/user/miniconda3/envs/esmvaltool_tutorial/lib/python3.8/site-packages/iris/analysis/cartography.py:394: UserWarning: Using > DEFAULT_SPHERICAL_EARTH_RADIUS.
>   warnings.warn("Using DEFAULT_SPHERICAL_EARTH_RADIUS.")
>
> INFO    [29586] Calculated grid area shape: (1812, 64, 128)
> INFO    [29586] Successfully completed task timeseries/tas_global (priority 3) in 0:00:06.073527
> INFO    [29586] Starting task timeseries/script1 in process [29586]
> INFO    [29586] Running command ['/home/user/miniconda3/envs/esmvaltool_tutorial/bin/python3.8', '/home/user/gh/esmvalgroup/ESMValTool/esmvaltool/diag_scripts/> examples/diagnostic.py', '/home/user/esmvaltool_tutorial/output/recipe_python_20201007_141734/run/timeseries/script1/settings.yml']
> INFO    [29586] Writing output to /home/user/esmvaltool_tutorial/output/recipe_python_20201007_141734/work/timeseries/script1
> INFO    [29586] Writing plots to /home/user/esmvaltool_tutorial/output/recipe_python_20201007_141734/plots/timeseries/script1
> INFO    [29586] Writing log to /home/user/esmvaltool_tutorial/output/recipe_python_20201007_141734/run/timeseries/script1/log.txt
> INFO    [29586] To re-run this diagnostic script, run:
> cd /home/user/esmvaltool_tutorial/output/recipe_python_20201007_141734/run/timeseries/script1; MPLBACKEND="Agg" /home/user/miniconda3/envs/esmvaltool_tutorial/> bin/python3.8 /home/user/gh/esmvalgroup/ESMValTool/esmvaltool/diag_scripts/examples/diagnostic.py /home/user/esmvaltool_tutorial/output/> recipe_python_20201007_141734/run/timeseries/script1/settings.yml
> INFO    [29586] Maximum memory used (estimate): 0.3 GB
> INFO    [29586] Sampled every second. It may be inaccurate if short but high spikes in memory consumption occur.
> INFO    [29586] Successfully completed task timeseries/script1 (priority 4) in 0:00:03.712112
> INFO    [29586] Ending the Earth System Model Evaluation Tool v2.0.0 at time: 2020-10-07 14:18:02 UTC
> INFO    [29586] Time for running the recipe was: 0:00:27.483308
> INFO    [29586] Maximum memory used (estimate): 0.7 GB
> INFO    [29586] Sampled every second. It may be inaccurate if short but high spikes in memory consumption occur.
> INFO    [29586] Run was successful
> ```
>
{: .solution}

> ## Pro tip: ESMValTool search paths
>
> You might wonder how ESMValTool was able find the recipe file, even though
> it's not in your working directory. All the recipe paths printed from
> `esmvaltool recipes list` are relative to ESMValTool's installation location.
> This is where ESMValTool will look if it cannot find the file by following the
> path from your working directory.
{: .callout}

## Investigating the log messages

Let's dissect what's happening here.


> ## Output files and directories
>
> After the banner and general information, the output starts with some important locations.
>
> 1. Did ESMValTool use the right config file?
> 1. What is the path to the example recipe?
> 1. What is the main output folder generated by ESMValTool?
> 1. Can you guess what the different output directories are for?
> 1. ESMValTool creates two log files. What is the difference?
>
> > ## Answers
> >
> > 1. The config file should be the one we edited in the previous episode,
> >    something like `/home/<user>/.esmvaltool/config-user.yml`.
> > 1. ESMValTool found the recipe in its installation directory, something like
> >    `/home/user/miniconda3/envs/esmvaltool_tutorial/bin/esmvaltool/recipes/examples/`.
> > 1. ESMValTool creates a time-stamped output directory for every run. In this
> >    case, it should be something like `recipe_python_YYYYMMDD_HHMMSS`. This
> >    folder is made inside the output directory specified in the previous
> >    episode: `~/home/<user>/esmvaltool_tutorial/output`.
> > 1. There should be four output folders:
> >    - `plots/`: this is where output figures are stored.
> >    - `preproc/`: this is where pre-processed data are stored.
> >    - `run/`: this is where esmvaltool stores general information about the
> >      run, such as log messages and a copy of the recipe file.
> >    - `work/`: this is where output files (not figures) are stored.
> > 1. The log files are:
> >    - `main_log.txt` is a copy of the command-line output
> >    - `main_log_debug.txt` contains more detailed information that may be
> >      useful for debugging.
> >
> {: .solution}
{: .challenge}

> ## Debugging: No 'preproc' directory?
>
> If you’re missing the preproc directory, then your `config-user.yml` file has
> the value `remove_preproc_dir` set to `true` (this is used to save disk space).
> Please set this value to `false` and run the recipe again.
>
{: .callout}

After the output locations, there are two main sections that can be
distinguished in the log messages:

- Creating tasks
- Executing tasks

> ## Analyse the tasks
>
> List all the tasks that ESMValTool is executing for this recipe. Can you guess
> what this recipe does?
>
> > ## Answer
> >
> > Just after 'creating tasks' and before 'executing tasks', we find the
> > following line in the output:
> >
> > ```
> > INFO    [29586] These tasks will be executed: timeseries/tas_global, timeseries/tas_amsterdam, map/tas, map/script1, timeseries/script1
> > ```
> >
> > So there are three tasks related to timeseries: global temperature,
> > Amsterdam temperature, and a script (*tas*: near-surface air temperature). And
> > then there are two tasks related to a map: something with temperature, and
> > again a script.
> {: .solution}
{: .challenge}

## Examining the recipe file

To get more insight into what is happening, we will have a look at the recipe
file itself. Use the following command to copy the recipe to your working
directory

```bash
esmvaltool recipes get examples/recipe_python.yml
```

Now you should see the recipe file in your working directory (type `ls` to
verify). Use the `nano` editor to open this file:

```bash
nano recipe_python.yml
```

For reference, you can also view the recipe by unfolding the box below.

> ## recipe_python.yml
>
> ```yaml
> # ESMValTool
> # recipe_python.yml
> ---
> documentation:
>   description: |
>     Example recipe that plots a map and timeseries of temperature.
>
>   authors:
>     - andela_bouwe
>     - righi_mattia
>
>   maintainer:
>     - schlund_manuel
>
>   references:
>     - acknow_project
>
>   projects:
>     - esmval
>     - c3s-magic
>
> datasets:
>   - {dataset: BCC-ESM1, project: CMIP6, exp: historical, ensemble: r1i1p1f1, grid: gn}
>   - {dataset: CanESM2, project: CMIP5, exp: historical, ensemble: r1i1p1}
>
> preprocessors:
>
>   select_january:
>     extract_month:
>       month: 1
>
>   annual_mean_amsterdam:
>     extract_point:
>       latitude: 52.379189
>       longitude: 4.899431
>       scheme: linear
>     annual_statistics:
>       operator: mean
>     multi_model_statistics:
>       statistics:
>         - mean
>       span: overlap
>
>   annual_mean_global:
>     area_statistics:
>       operator: mean
>     annual_statistics:
>       operator: mean
>
> diagnostics:
>
>   map:
>     description: Global map of temperature in January 2000.
>     themes:
>       - phys
>     realms:
>       - atmos
>     variables:
>       tas:
>         mip: Amon
>         preprocessor: select_january
>         start_year: 2000
>         end_year: 2000
>     scripts:
>       script1:
>         script: examples/diagnostic.py
>         write_netcdf: true
>         output_file_type: pdf
>         quickplot:
>           plot_type: pcolormesh
>           cmap: Reds
>
>   timeseries:
>     description: Annual mean temperature in Amsterdam and global mean since 1850.
>     themes:
>       - phys
>     realms:
>       - atmos
>     variables:
>       tas_amsterdam:
>         short_name: tas
>         mip: Amon
>         preprocessor: annual_mean_amsterdam
>         start_year: 1850
>         end_year: 2000
>       tas_global:
>         short_name: tas
>         mip: Amon
>         preprocessor: annual_mean_global
>         start_year: 1850
>         end_year: 2000
>     scripts:
>       script1:
>         script: examples/diagnostic.py
>         quickplot:
>           plot_type: plot
> ```
>
{: .solution}

Do you recognize the basic recipe structure that was introduced in episode 1?

- **Documentation** with relevant (citation) information
- **Datasets** that should be analysed
- **Preprocessors** groups of common preprocessing steps
- **Diagnostics** scripts performing more specific evaluation steps

> ## Analyse the recipe
>
> Try to answer the following questions:
>
> 1. Who wrote this recipe?
> 1. Who should be approached if there is a problem with this recipe?
> 1. How many datasets are analyzed?
> 1. What does the preprocessor called `annual_mean_global` do?
> 1. Which script is applied for the diagnostic called `map`?
> 1. Can you link specific lines in the recipe to the tasks that we saw before?
>
> > ## Answers
> >
> >   1. The example recipe is written by Bouwe Andela and Mattia Righi.
> >   1. Manual Schlund is listed as the maintainer of this recipe.
> >   1. Two datasets are analysed:
> >      - CMIP6 data from the model BCC-ESM1
> >      - CMIP5 data from the model CANESM2
> >   1. The preprocessor `annual_mean_global` computes an area mean as well as
> >      annual means
> >   1. The diagnostic called `map` executes a script referred to as `script1`.
> >      This is a python script named `examples/diagnostic.py`
> >   1. There are two diagnostics: `map` and `timeseries`. Under the diagnostic
> >      `map` we find two tasks:
> >      - a preprocessor task called `tas`, applying the preprocessor called
> >        `select_january` to the variable `tas`.
> >      - a diagnostic task called `script1`, applying the script
> >        `examples/diagnostic.py` to the preprocessed data (`map/tas`).
> >
> >      Under the diagnostic `timeseries` we find three tasks:
> >      - a preprocessor task called `tas_amsterdam`, applying the preprocessor
> >        called `annual_mean_amsterdam` to the variable `tas`.
> >      - a preprocessor task called `tas_global`, applying the preprocessor
> >        called `annual_mean_global` to the variable `tas`.
> >      - a diagnostic task called `script1`, applying the script
> >        `examples/diagnostic.py` to the preprocessed data
> >        (`timeseries/tas_global` and `timeseries/tas_amsterdam`).
> >
> {: .solution}
{: .challenge}

> ## Pro tip: short names and variable groups
>
> The preprocessor tasks in ESMValTool are called 'variable groups'. For the
> diagnostic `timeseries`, we have two variable groups: `tas_amsterdam` and
> `tas_global`. Both of them operate on the variable `tas` (as indicated by the
> `short_name`), but they apply different preprocessors. For the diagnostic
> `map` the variable group itself is named `tas`, and you'll notice that we do
> not explicitly provide the `short_name`. This is a shorthand built into
> ESMValTool.
{: .callout}

> ## Output files
>
> Have another look at the output directory created by the ESMValTool run.
>
> Which files/folders are created by each task?
>
> > ## Answer
> >
> > - **map/tas**: creates `/preproc/map/tas`, which contains preprocessed data
> >   for each of the input datasets, and a file called `metadata.yml`
> >   describing the contents of these datasets.
> > - **timeseries/tas_global**: creates `/preproc/timeseries/tas_global`, which
> >   contains preprocessed data for each of the input datasets, and
> >   `metadata.yml`.
> > - **timeseries/tas_amsterdam**: creates `/preproc/timeseries/tas_amsterdam`,
> >   which contains preprocessed data for each of the input datasets, plus a
> >   combined `MultiModelMean`, and `metadata.yml`.
> > - **map/script1**: creates `/run/map/script1` with general information and a
> >   log of the diagnostic script run. It also creates `/plots/map/script1` and
> >   `/work/map/script1`, which contain output figures and output datasets,
> >   respectively. For each output file, there is also corresponding provenance
> >   information in the form of `.svg`, `.xml`, `.bibtex` and `.txt` files.
> > - **timeseries/script1**: creates `/run/timeseries/script1` with general
> >   information and a log of the diagnostic script run. It also creates
> >   `/plots/timeseries/script1` and `/work/timeseries/script1`, which contain
> >   output figures and output datasets, respectively. For each output file,
> >   there is also corresponding provenance information in the form of `.svg`,
> >   `.xml`, `.bibtex` and `.txt` files.
> >
> {: .solution}
{: .challenge}

> ## Pro tip: diagnostic logs
>
> When you run ESMValTool, any log messages from the diagnostic script are not printed on the terminal.
> But they are written to the `log.txt` files in the folder `/run/<diag_name>/log.txt`.
>
> ESMValTool *does* print a command that can be used to re-run a diagnostic script. When you use this
> the output *will* be printed to the command line.
{: .callout}

## Modifying the example recipe

Let's make a small modification to the example recipe. Notice that now that
you have copied and edited the recipe, you can use

```
esmvaltool run recipe_example.yml
```

to refer to your local file rather than the default version shipped with
ESMValTool.

> ## Change your location
>
> Modify and run the recipe to analyse the temperature for your own location.
>
> > ## Solution
> >
> > In principle, you only have to modify the latitude and longitude coordinates
> > in the preprocessor called `annual_mean_amsterdam`. However, it is good
> > practice to also replace all instances of `amsterdam` with the correct name
> > of your location. Otherwise the log messages and output will be confusing.
> > You are free to modify the names of preprocessors or diagnostics.
> >
> > In the `diff` file below you will see the changes we have made to the file.
> > The top 2 lines are the filenames and the lines like `@@ -29,10 +29,10 @@`
> > represent the line numbers in the original and modified file, respectively.
> > For more info on this format, see
> > [here](https://en.wikipedia.org/wiki/Diff#Unified_format).
> >
> > ```diff
> > --- recipe_python.yml
> > +++ recipe_python_london.yml
> > @@ -29,10 +29,10 @@
> >      extract_month:
> >        month: 1
> >
> > -  annual_mean_amsterdam:
> > +  annual_mean_london:
> >      extract_point:
> > -      latitude: 52.379189
> > -      longitude: 4.899431
> > +      latitude: 51.5074
> > +      longitude: 0.1278
> >        scheme: linear
> >      annual_statistics:
> >        operator: mean
> > @@ -71,16 +71,16 @@
> >            cmap: Reds
> >
> >    timeseries:
> > -    description: Annual mean temperature in Amsterdam and global mean since 1850.
> > +    description: Annual mean temperature in London and global mean since 1850.
> >      themes:
> >        - phys
> >      realms:
> >        - atmos
> >      variables:
> > -      tas_amsterdam:
> > +      tas_london:
> >          short_name: tas
> >          mip: Amon
> > -        preprocessor: annual_mean_amsterdam
> > +        preprocessor: annual_mean_london
> >          start_year: 1850
> >          end_year: 2000
> >        tas_global:
> > ```
> >
> {: .solution}
{: .challenge}
