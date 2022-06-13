---
title: "Running your first recipe (compatible with ESMValTool v2.5)"
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

The recipe format has briefly been introduced in the [Introduction]({{ page.root }}{% link _episodes/01-introduction.md %}) episode. To see all the
recipes that are shipped with ESMValTool, type

```bash
esmvaltool recipes list
```

We will start by running [examples/recipe_python.yml](https://docs.esmvaltool.org/en/latest/recipes/recipe_examples.html)

```
esmvaltool run examples/recipe_python.yml
```


or if you have the user configuration file in your current directory then

```
esmvaltool run --config_file ./config-user.yml examples/recipe_python.yml
```

If everything is okay, you should see that ESMValTool is printing a lot of
output to the command line. The final message should be "Run was successful".
The exact output varies depending on your machine, but it should look something
like the example output below.

> ## Example output
>
> ```
>2022-01-24 17:31:48,745 UTC [190720] INFO    
>______________________________________________________________________
>          _____ ____  __  ____     __    _ _____           _
>         | ____/ ___||  \/  \ \   / /_ _| |_   _|__   ___ | |
>         |  _| \___ \| |\/| |\ \ / / _` | | | |/ _ \ / _ \| |
>         | |___ ___) | |  | | \ V / (_| | | | | (_) | (_) | |
>         |_____|____/|_|  |_|  \_/ \__,_|_| |_|\___/ \___/|_|
>______________________________________________________________________
>
>ESMValTool - Earth System Model Evaluation Tool.
>
>http://www.esmvaltool.org
>
>  CORE DEVELOPMENT TEAM AND CONTACTS:
>  Birgit Hassler (Co-PI; DLR, Germany - birgit.hassler@dlr.de)
>  Alistair Sellar (Co-PI; Met Office, UK - alistair.sellar@metoffice.gov.uk)
>  Bouwe Andela (Netherlands eScience Center, The Netherlands - b.andela@esciencecenter.nl)
>  Lee de Mora (PML, UK - ledm@pml.ac.uk)
>  Niels Drost (Netherlands eScience Center, The Netherlands - n.drost@esciencecenter.nl)
>  Veronika Eyring (DLR, Germany - veronika.eyring@dlr.de)
>  Bettina Gier (UBremen, Germany - gier@uni-bremen.de)
>  Remi Kazeroni (DLR, Germany - remi.kazeroni@dlr.de)
>  Nikolay Koldunov (AWI, Germany - nikolay.koldunov@awi.de)
>  Axel Lauer (DLR, Germany - axel.lauer@dlr.de)
>  Saskia Loosveldt-Tomas (BSC, Spain - saskia.loosveldt@bsc.es)
>  Ruth Lorenz (ETH Zurich, Switzerland - ruth.lorenz@env.ethz.ch)
>  Benjamin Mueller (LMU, Germany - b.mueller@iggf.geo.uni-muenchen.de)
>  Valeriu Predoi (URead, UK - valeriu.predoi@ncas.ac.uk)
>  Mattia Righi (DLR, Germany - mattia.righi@dlr.de)
>  Manuel Schlund (DLR, Germany - manuel.schlund@dlr.de)
>  Breixo Solino Fernandez (DLR, Germany - breixo.solinofernandez@dlr.de)
>  Javier Vegas-Regidor (BSC, Spain - javier.vegas@bsc.es)
>  Klaus Zimmermann (SMHI, Sweden - klaus.zimmermann@smhi.se)
>
>For further help, please read the documentation at
>http://docs.esmvaltool.org. Have fun!
>
>2022-04-24 17:31:48,745 UTC [190720] INFO    Package versions
>2022-04-24 17:31:48,746 UTC [190720] INFO    ----------------
>2022-04-24 17:31:48,746 UTC [190720] INFO    ESMValCore: 2.5.0
>2022-04-24 17:31:48,746 UTC [190720] INFO    ESMValTool: 2.5.0
>2022-04-24 17:31:48,746 UTC [190720] INFO    ----------------
>2022-04-24 17:31:48,746 UTC [190720] INFO    Using config file /home/users/username/esmvaltool-tutorial/config-user.yml
>2022-04-24 17:31:48,746 UTC [190720] INFO    Writing program log files to:
>/home/users/username/esmvaltool-tutorial/esmvaltool_output/recipe_python_20220424_173145/run/main_log.txt
>/home/users/username/esmvaltool-tutorial/esmvaltool_output/recipe_python_20220424_173145/run/main_log_debug.txt
>2022-04-24 17:31:48,747 UTC [190720] INFO    Starting the Earth System Model Evaluation Tool at time: 2022-04-24 17:31:48 UTC
>2022-04-24 17:31:48,747 UTC [190720] INFO    ----------------------------------------------------------------------
>2022-04-24 17:31:48,747 UTC [190720] INFO    RECIPE   = /apps/jasmin/community/esmvaltool/ESMValTool_2.5.0/esmvaltool/recipes/examples/recipe_python.yml
>2022-04-24 17:31:48,747 UTC [190720] INFO    RUNDIR     = /home/users/username/esmvaltool-tutorial/esmvaltool_output/recipe_python_20220424_173145/run
>2022-04-24 17:31:48,747 UTC [190720] INFO    WORKDIR    = /home/users/username/esmvaltool-tutorial/esmvaltool_output/recipe_python_20220424_173145/work
>2022-04-24 17:31:48,747 UTC [190720] INFO    PREPROCDIR = /home/users/username/esmvaltool-tutorial/esmvaltool_output/recipe_python_20220424_173145/preproc
>2022-04-24 17:31:48,747 UTC [190720] INFO    PLOTDIR    = /home/users/username/esmvaltool-tutorial/esmvaltool_output/recipe_python_20220424_173145/plots
>2022-04-24 17:31:48,747 UTC [190720] INFO    ----------------------------------------------------------------------
>2022-04-24 17:31:48,747 UTC [190720] INFO    Running tasks using at most 24 processes
>2022-04-24 17:31:48,747 UTC [190720] INFO    If your system hangs during execution, it may not have enough memory for keeping this number of tasks in memory.
>2022-04-24 17:31:48,747 UTC [190720] INFO    If you experience memory problems, try reducing 'max_parallel_tasks' in your user configuration file.
>2022-04-24 17:31:48,805 UTC [190720] INFO    Creating tasks from recipe
>2022-04-24 17:31:48,805 UTC [190720] INFO    Creating tasks for diagnostic map
>2022-04-24 17:31:48,805 UTC [190720] INFO    Creating preprocessor task map/tas
>2022-04-24 17:31:48,805 UTC [190720] INFO    Creating preprocessor 'select_january' task for variable 'tas'
>2022-04-24 17:31:48,835 UTC [190720] INFO    Found input files for CMIP6
>2022-04-24 17:31:48,900 UTC [190720] INFO    Found input files for CMIP5
>2022-04-24 17:31:48,961 UTC [190720] INFO    PreprocessingTask map/tas created.
>2022-04-24 17:31:48,961 UTC [190720] INFO    Creating diagnostic task map/script1
>2022-04-24 17:31:48,962 UTC [190720] INFO    Creating tasks for diagnostic timeseries
>2022-04-24 17:31:48,962 UTC [190720] INFO    Creating preprocessor task timeseries/tas_amsterdam
>2022-04-24 17:31:48,963 UTC [190720] INFO    Creating preprocessor 'annual_mean_amsterdam' task for variable 'tas'
>2022-04-24 17:31:48,969 UTC [190720] INFO    Found input files for CMIP6
>2022-04-24 17:31:49,019 UTC [190720] INFO    Found input files for CMIP5
>2022-04-24 17:31:49,063 UTC [190720] INFO    PreprocessingTask timeseries/tas_amsterdam created.
>2022-04-24 17:31:49,064 UTC [190720] INFO    Creating preprocessor task timeseries/tas_global
>2022-04-24 17:31:49,064 UTC [190720] INFO    Creating preprocessor 'annual_mean_global' task for variable 'tas'
>2022-04-24 17:31:49,065 UTC [190720] WARNING Missing data for fx variable 'areacella' of dataset CMIP6
>2022-04-24 17:31:49,071 UTC [190720] INFO    Found input files for CMIP6
>2022-04-24 17:31:49,138 UTC [190720] INFO    Found input files for CMIP5
>2022-04-24 17:31:49,183 UTC [190720] INFO    PreprocessingTask timeseries/tas_global created.
>2022-04-24 17:31:49,183 UTC [190720] INFO    Creating diagnostic task timeseries/script1
>2022-04-24 17:31:49,184 UTC [190720] INFO    These tasks will be executed: map/script1, map/tas, timeseries/tas_amsterdam, timeseries/script1, timeseries/tas_global
>2022-04-24 17:31:49,195 UTC [190720] INFO    Running 5 tasks using 5 processes
>2022-04-24 17:31:49,239 UTC [190776] INFO    Starting task map/tas in process [190776]
>2022-04-24 17:31:49,240 UTC [190777] INFO    Starting task timeseries/tas_amsterdam in process [190777]
>2022-04-24 17:31:49,240 UTC [190778] INFO    Starting task timeseries/tas_global in process [190778]
>2022-04-24 17:31:49,335 UTC [190720] INFO    Progress: 3 tasks running, 2 tasks waiting for ancestors, 0/5 done
>2022-04-24 17:31:55,735 UTC [190776] INFO    Successfully completed task map/tas (priority 0) in 0:00:06.494906
>2022-04-24 17:31:55,847 UTC [190720] INFO    Progress: 2 tasks running, 2 tasks waiting for ancestors, 1/5 done
>2022-04-24 17:31:55,852 UTC [190779] INFO    Starting task map/script1 in process [190779]
>2022-04-24 17:31:55,861 UTC [190779] INFO    Running command ['/apps/jasmin/community/esmvaltool/miniconda3/envs/esmvaltool/bin/python', '/apps/jasmin/community/esmvaltool/ESMValTool_2.5.0/esmvaltool/diag_scripts/examples/diagnostic.py', '/home/users/username/esmvaltool-tutorial/esmvaltool_output/recipe_python_20220424_173145/run/map/script1/settings.yml']
>2022-04-24 17:31:55,862 UTC [190779] INFO    Writing output to /home/users/username/esmvaltool-tutorial/esmvaltool_output/recipe_python_20220424_173145/work/map/script1
>2022-04-24 17:31:55,862 UTC [190779] INFO    Writing plots to /home/users/username/esmvaltool-tutorial/esmvaltool_output/recipe_python_20220424_173145/plots/map/script1
>2022-04-24 17:31:55,862 UTC [190779] INFO    Writing log to /home/users/username/esmvaltool-tutorial/esmvaltool_output/recipe_python_20220424_173145/run/map/script1/log.txt
>2022-04-24 17:31:55,862 UTC [190779] INFO    To re-run this diagnostic script, run:
>cd /home/users/username/esmvaltool-tutorial/esmvaltool_output/recipe_python_20220424_173145/run/map/script1; MPLBACKEND="Agg" /apps/jasmin/community/esmvaltool/miniconda3/envs/esmvaltool/bin/python /apps/jasmin/community/esmvaltool/ESMValTool_2.5.0/esmvaltool/diag_scripts/examples/diagnostic.py /home/users/username/esmvaltool-tutorial/esmvaltool_output/recipe_python_20220424_173145/run/map/script1/settings.yml
>2022-04-24 17:31:55,947 UTC [190720] INFO    Progress: 3 tasks running, 1 tasks waiting for ancestors, 1/5 done
>2022-04-24 17:31:58,538 UTC [190777] INFO    Generated PreprocessorFile: /home/users/username/esmvaltool-tutorial/esmvaltool_output/recipe_python_20220424_173145/preproc/timeseries/tas_amsterdam/MultiModelMean_Amon_tas_1850-2000.nc
>2022-04-24 17:31:58,762 UTC [190777] INFO    Successfully completed task timeseries/tas_amsterdam (priority 2) in 0:00:09.521837
>2022-04-24 17:31:58,953 UTC [190720] INFO    Progress: 2 tasks running, 1 tasks waiting for ancestors, 2/5 done
>2022-04-24 17:31:59,700 UTC [190778] INFO    Successfully completed task timeseries/tas_global (priority 3) in 0:00:10.459256
>2022-04-24 17:31:59,855 UTC [190720] INFO    Progress: 1 tasks running, 1 tasks waiting for ancestors, 3/5 done
>2022-04-24 17:31:59,863 UTC [190780] INFO    Starting task timeseries/script1 in process [190780]
>2022-04-24 17:31:59,871 UTC [190780] INFO    Running command ['/apps/jasmin/community/esmvaltool/miniconda3/envs/esmvaltool/bin/python', '/apps/jasmin/community/esmvaltool/ESMValTool_2.5.0/esmvaltool/diag_scripts/examples/diagnostic.py', '/home/users/username/esmvaltool-tutorial/esmvaltool_output/recipe_python_20220424_173145/run/timeseries/script1/settings.yml']
>2022-04-24 17:31:59,872 UTC [190780] INFO    Writing output to /home/users/username/esmvaltool-tutorial/esmvaltool_output/recipe_python_20220424_173145/work/timeseries/script1
>2022-04-24 17:31:59,872 UTC [190780] INFO    Writing plots to /home/users/username/esmvaltool-tutorial/esmvaltool_output/recipe_python_20220424_173145/plots/timeseries/script1
>2022-04-24 17:31:59,872 UTC [190780] INFO    Writing log to /home/users/username/esmvaltool-tutorial/esmvaltool_output/recipe_python_20220424_173145/run/timeseries/script1/log.txt
>2022-04-24 17:31:59,872 UTC [190780] INFO    To re-run this diagnostic script, run:
>cd /home/users/username/esmvaltool-tutorial/esmvaltool_output/recipe_python_20220424_173145/run/timeseries/script1; MPLBACKEND="Agg" /apps/jasmin/community/esmvaltool/miniconda3/envs/esmvaltool/bin/python /apps/jasmin/community/esmvaltool/ESMValTool_2.5.0/esmvaltool/diag_scripts/examples/diagnostic.py /home/users/username/esmvaltool-tutorial/esmvaltool_output/recipe_python_20220424_173145/run/timeseries/script1/settings.yml
>2022-04-24 17:31:59,956 UTC [190720] INFO    Progress: 2 tasks running, 0 tasks waiting for ancestors, 3/5 done
>2022-04-24 17:32:01,586 UTC [190779] INFO    Successfully completed task map/script1 (priority 1) in 0:00:05.733018
>2022-04-24 17:32:01,760 UTC [190720] INFO    Progress: 1 tasks running, 0 tasks waiting for ancestors, 4/5 done
>2022-04-24 17:32:06,079 UTC [190780] INFO    Maximum memory used (estimate): 0.2 GB
>2022-04-24 17:32:06,081 UTC [190780] INFO    Sampled every second. It may be inaccurate if short but high spikes in memory consumption occur.
>2022-04-24 17:32:06,760 UTC [190780] INFO    Successfully completed task timeseries/script1 (priority 4) in 0:00:06.896972
>2022-04-24 17:32:06,771 UTC [190720] INFO    Progress: 0 tasks running, 0 tasks waiting for ancestors, 5/5 done
>2022-04-24 17:32:06,771 UTC [190720] INFO    Successfully completed all tasks.
>2022-04-24 17:32:07,764 UTC [190720] INFO    Wrote recipe output to:
>file:///home/users/username/esmvaltool-tutorial/esmvaltool_output/recipe_python_20220424_173145/index.html
>2022-04-24 17:32:07,764 UTC [190720] INFO    Ending the Earth System Model Evaluation Tool at time: 2022-04-24 17:32:07 UTC
>2022-04-24 17:32:07,764 UTC [190720] INFO    Time for running the recipe was: 0:00:19.017514
>2022-04-24 17:32:08,702 UTC [190720] INFO    Maximum memory used (estimate): 1.3 GB
>2022-04-24 17:32:08,703 UTC [190720] INFO    Sampled every second. It may be inaccurate if short but high spikes in memory consumption occur.
>2022-04-24 17:32:08,704 UTC [190720] INFO    Run was successful
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
> >    something like `/home/<username>/.esmvaltool/config-user.yml` or `~/esmvaltool_tutorial/config-user.yml`.
> > 1. ESMValTool found the recipe in its installation directory, something like
> >    `/home/users/username/miniconda3/envs/esmvaltool/bin/esmvaltool/recipes/examples/`
>> or if you are using a pre-installed module on a server, something like `/apps/jasmin/community/esmvaltool/ESMValTool_2.5.0/esmvaltool/recipes/examples/recipe_python.yml`
> > 1. ESMValTool creates a time-stamped output directory for every run. In this
> >    case, it should be something like `recipe_python_YYYYMMDD_HHMMSS`. This
> >    folder is made inside the output directory specified in the previous
> >    episode: `~/esmvaltool_tutorial/esmvaltool_output`.
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
> >[190720] INFO    These tasks will be executed: map/script1, map/tas, timeseries/tas_amsterdam, timeseries/script1, timeseries/tas_global
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
>   title: Recipe that runs an example diagnostic written in Python.
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
>       fx_variables:
>         areacella:
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
> >   information in the form of `.xml`, `.bibtex` and `.txt` files.
> > - **timeseries/script1**: creates `/run/timeseries/script1` with general
> >   information and a log of the diagnostic script run. It also creates
> >   `/plots/timeseries/script1` and `/work/timeseries/script1`, which contain
> >   output figures and output datasets, respectively. For each output file,
> >   there is also corresponding provenance information in the form of
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
> > The top 2 lines are the filenames and the lines like `@@ -31,10 +31,10 @@`
> > represent the line numbers in the original and modified file, respectively.
> > For more info on this format, see
> > [here](https://en.wikipedia.org/wiki/Diff#Unified_format).
> >
> > ```diff
> > --- recipe_python.yml
> > +++ recipe_python_london.yml
> > @@ -31,10 +31,10 @@
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
> > @@ -73,16 +73,16 @@
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
