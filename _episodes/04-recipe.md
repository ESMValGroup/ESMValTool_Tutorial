---
title: "Running your first recipe"
teaching: 15
exercises: 15
compatibility: ESMValTool v2.12.0

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

The recipe format has briefly been introduced in the 
[Introduction][lesson-introduction] episode. To see all the
recipes that are shipped with ESMValTool, type

```bash
esmvaltool recipes list
```

We will start by running [examples/recipe_python.yml](https://docs.esmvaltool.
org/en/latest/recipes/recipe_examples.html)

```
esmvaltool run examples/recipe_python.yml
```


or if you have the user configuration file in your current directory then

```
esmvaltool run --config_dir . examples/recipe_python.yml
```

If everything is okay, you should see that ESMValTool is printing a lot of
output to the command line. The final message should be "Run was successful".
The exact output varies depending on your machine, but it should look something
like the example log output on terminal below.

{% include example_output.txt %}

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
> >    something like `/home/<username>/.config/esmvaltool/config-user.yml` or
 `~/esmvaltool_tutorial/config-user.yml`.
> > 1. ESMValTool found the recipe in its installation directory, 
>> something like
> >    `/home/users/username/mambaforge/envs/esmvaltool/bin/esmvaltool/recipes/examples/`
>> or if you are using a pre-installed module on a server, something like
>> `/apps/jasmin/community/esmvaltool/ESMValTool_<version>
>>/esmvaltool/recipes/examples/recipe_python.yml`, 
>> where `<version>` is the latest release.
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
> > Just after all the 'creating tasks' and before 'executing tasks', we find the
> > following line in the output:
> >
> > ```
> >INFO    [3966381] These tasks will be executed: timeseries/script1,
> > timeseries/tas_amsterdam, timeseries/tas_global, map/tas, map/script1
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
># ESMValTool
># recipe_python.yml
>#
># See https://docs.esmvaltool.org/en/latest/recipes/recipe_examples.html
># for a description of this recipe.
>#
># See https://docs.esmvaltool.org/projects/esmvalcore/en/latest/recipe/overview.html
># for a description of the recipe format.
>---
>documentation:
>  description: |
>    Example recipe that plots a map and timeseries of temperature.
>
>  title: Recipe that runs an example diagnostic written in Python.
>
>  authors:
>    - andela_bouwe
>    - righi_mattia
>
>  maintainer:
>    - schlund_manuel
>
>  references:
>    - acknow_project
>
>  projects:
>    - esmval
>    - c3s-magic
>
>datasets:
>  - {dataset: BCC-ESM1, project: CMIP6, exp: historical, ensemble: r1i1p1f1, grid: gn}
>  - {dataset: bcc-csm1-1, project: CMIP5, exp: historical, ensemble: r1i1p1}
>
>preprocessors:
>  # See https://docs.esmvaltool.org/projects/esmvalcore/en/latest/recipe/preprocessor.html
>  # for a description of the preprocessor functions.
>
>  to_degrees_c:
>    convert_units:
>      units: degrees_C
>
>  annual_mean_amsterdam:
>    extract_location:
>      location: Amsterdam
>      scheme: linear
>    annual_statistics:
>      operator: mean
>    multi_model_statistics:
>      statistics:
>        - mean
>      span: overlap
>    convert_units:
>      units: degrees_C
>
>  annual_mean_global:
>    area_statistics:
>      operator: mean
>    annual_statistics:
>      operator: mean
>    convert_units:
>      units: degrees_C
>
>diagnostics:
>
>  map:
>    description: Global map of temperature in January 2000.
>    themes:
>      - phys
>    realms:
>      - atmos
>    variables:
>      tas:
>        mip: Amon
>        preprocessor: to_degrees_c
>        timerange: 2000/P1M
>        caption: |
>          Global map of {long_name} in January 2000 according to {dataset}.
>    scripts:
>      script1:
>        script: examples/diagnostic.py
>        quickplot:
>          plot_type: pcolormesh
>          cmap: Reds
>
>  timeseries:
>    description: Annual mean temperature in Amsterdam and global mean since 1850.
>    themes:
>      - phys
>    realms:
>      - atmos
>    variables:
>      tas_amsterdam:
>        short_name: tas
>        mip: Amon
>        preprocessor: annual_mean_amsterdam
>        timerange: 1850/2000
>        caption: Annual mean {long_name} in Amsterdam according to {dataset}.
>      tas_global:
>        short_name: tas
>        mip: Amon
>        preprocessor: annual_mean_global
>        timerange: 1850/2000
>        caption: Annual global mean {long_name} according to {dataset}.
>    scripts:
>      script1:
>        script: examples/diagnostic.py
>        quickplot:
>          plot_type: plot
>
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
> 1. How is the location of the city specified?
> 1. How is the temporal range of the data specified?
>
> > ## Answers
> >
> >   1. The example recipe is written by Bouwe Andela and Mattia Righi.
> >   1. Manuel Schlund is listed as the maintainer of this recipe.
> >   1. Two datasets are analysed:
> >      - CMIP6 data from the model BCC-ESM1
> >      - CMIP5 data from the model bcc-csm1-1
> >   1. The preprocessor `annual_mean_global` computes an area mean as well as
> >      annual means
> >   1. The diagnostic called `map` executes a script referred to as `script1`.
> >      This is a python script named `examples/diagnostic.py`
> >   1. There are two diagnostics: `map` and `timeseries`. Under the diagnostic
> >      `map` we find two tasks:
> >      - a preprocessor task called `tas`, applying the preprocessor called
> >        `to_degrees_c` to the variable `tas`.
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
>>
>>  1. The `extract_location` preprocessor is used to get data for a specific location
>>      here. ESMValTool interpolates to the location based on the chosen scheme.
>>      Can you tell the scheme used here? For more ways to extract areas, see the
>>      [Area operations][preproc-area-manipulation] page.
>>  1. The `timerange` tag is used to extract data from a specific time period here.
>>      The start time is `01/01/2000` and the span of time to calculate means is 
>>      `1 Month` given by `P1M`. For more options on how to specify time ranges,
>>      see the [timerange documentation][timeranges].
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
> >   for each of the input datasets, a file called `metadata.yml`
> >   describing the contents of these datasets and provenance information in the 
>>    form of `.xml` files.
> > - **timeseries/tas_global**: creates `/preproc/timeseries/tas_global`, which
> >   contains preprocessed data for each of the input datasets, a
> >   `metadata.yml` file and provenance information in the 
>>    form of `.xml` files.
> > - **timeseries/tas_amsterdam**: creates `/preproc/timeseries/tas_amsterdam`,
> >   which contains preprocessed data for each of the input datasets, plus a
> >   combined `MultiModelMean`, a `metadata.yml` file and provenance files.
> > - **map/script1**: creates `/run/map/script1` with general information and a
> >   log of the diagnostic script run. It also creates `/plots/map/script1/` and
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
> When you run ESMValTool, any log messages from the diagnostic script are not 
> printed on the terminal.
> But they are written to the `log.txt` files in the folder `/run/<diag_name>/log.txt`.
>
> ESMValTool *does* print a command that can be used to re-run a diagnostic 
> script. When you use this the output *will* be printed to the command line.
{: .callout}

## Modifying the example recipe

Let's make a small modification to the example recipe. Notice that now that
you have copied and edited the recipe, you can use

```
esmvaltool run recipe_python.yml
```

to refer to your local file rather than the default version shipped with
ESMValTool.

> ## Change your location
>
> Modify and run the recipe to analyse the temperature for your own location.
>
> > ## Solution
> >
> > In principle, you only have to modify the location
> > in the preprocessor called `annual_mean_amsterdam`. However, it is good
> > practice to also replace all instances of `amsterdam` with the correct name
> > of your location. Otherwise the log messages and output will be confusing.
> > You are free to modify the names of preprocessors or diagnostics.
> >
> > In the `diff` file below you will see the changes we have made to the file.
> > The top 2 lines are the filenames and the lines like `@@ -39,9 +39,9 @@`
> > represent the line numbers in the original and modified file, respectively.
> > For more info on this format, see
> > [here](https://en.wikipedia.org/wiki/Diff#Unified_format).
> >
> > ```diff
>>--- recipe_python.yml	
>>+++ recipe_python_london.yml	
>>@@ -39,9 +39,9 @@
>>     convert_units:
>>       units: degrees_C
>> 
>>-  annual_mean_amsterdam:
>>+  annual_mean_london:
>>     extract_location:
>>-      location: Amsterdam
>>+      location: London
>>       scheme: linear
>>     annual_statistics:
>>       operator: mean
>>@@ -83,7 +83,7 @@
>>           cmap: Reds
>> 
>>   timeseries:
>>-    description: Annual mean temperature in Amsterdam and global mean since 1850.
>>+    description: Annual mean temperature in London and global mean since 1850.
>>     themes:
>>       - phys
>>     realms:
>>@@ -92,9 +92,9 @@
>>       tas_amsterdam:
>>         short_name: tas
>>         mip: Amon
>>-        preprocessor: annual_mean_amsterdam
>>+        preprocessor: annual_mean_london
>>         timerange: 1850/2000
>>-        caption: Annual mean {long_name} in Amsterdam according to {dataset}.
>>+        caption: Annual mean {long_name} in London according to {dataset}.
>>       tas_global:
>>         short_name: tas
>>         mip: Amon
> > ```
> >
> {: .solution}
{: .challenge}

{% include links.md %}
