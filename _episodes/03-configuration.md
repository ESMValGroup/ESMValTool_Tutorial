---
title: "Configuration"
teaching: 10
exercises: 10
questions:
- What is the user configuration file and how should I use it?

objectives:
- Understand the contents of the user-config.yml file
- Prepare a personalized user-config.yml file
- Configure ESMValTool to use some settings

keypoints:
- The ``config-user.yml`` tells ESMValTool where to find input data.
- "``rootpath`` defines the root directory for the input data."
- "``output_dir`` defines the destination directory."

---

## The configuration file

The ``config-user.yml`` configuration file contains all the global level
information needed by ESMValTool to run. This is an [YAML
file](https://yaml.org/spec/1.2/spec.html). An example configuration file can be
found in the ESMValCore repository:
[config-user-example.yml](https://github.com/ESMValGroup/ESMValCore/blob/master/esmvalcore/config-user.yml).

You can generate the default configuration file by running:

~~~bash
  esmvaltool config get_config_user
~~~

It will save the file to: `~/.esmvaltool/config-user.yml`, where `~` is the
path to your home directory. Note that files and directories starting with a
period are "hidden", to see the `.esmvaltool` directory in the terminal use
`ls -la ~`.

We run a text editor called Nano to have a look inside the configuration file:

~~~bash
  nano ~/.esmvaltool/config-user.yml
~~~

This file contains the information for:

- Output settings
- Destination directory
- Auxiliary data directory
- Number of tasks that can be run in parallel
- Rootpath to input data
- Directory structure for the data from different projects

> ## Text editor side note
>
> No matter what editor you use, you will need to know where it searches
> for and saves files. If you start it from the shell, it will (probably)
> use your current working directory as its default location. We use ``nano``
> in examples here because it is one of the least complex text editors.
> Press <kbd>ctrl</kbd> + <kbd>O</kbd> to save the file,
> and then <kbd>ctrl</kbd> + <kbd>X</kbd> to exit ``nano``.
{: .callout}

## Output settings

These settings are used to inform ESMValTool about your preference about
specific actions. You can turn on or off the setting by ``true`` or ``false``
values. Most of these settings are fairly self-explanatory, ie:

```yaml
# Diagnostics create plots? [true]/false
write_plots: true
# Diagnositcs write NetCDF files? [true]/false
write_netcdf: true
# Set the console log level debug, [info], warning, error
log_level: info
# Exit on warning (only for NCL diagnostic scripts)? true/[false]
exit_on_warning: false
# Plot file format? [png]/pdf/ps/eps/epsi
output_file_type: png

...

# Use netCDF compression true/[false]
compress_netcdf: false
# Save intermediary cubes in the preprocessor true/[false]
save_intermediary_cubes: false
# Remove the preproc dir if all fine
remove_preproc_dir: true

...

# Path to custom config-developer file, to customise project configurations.
# See config-developer.yml for an example. Set to [null] to use the default
config_developer_file: null
# Get profiling information for diagnostics
# Only available for Python diagnostics
profile_diagnostic: false
```

In general there is no need to change the settings listed above.

## Destination directory

The destination directory is the rootpath where ESMValTool will store its output folders containing
i.e. figures, data, logs, etc. With every run, ESMValTool automatically generates
a new output folder determined by recipe name, and date and time using
the format: YYYYMMDD_HHMMSS.
This folder contains four further subfolders: ``plots``, ``preproc``, ``run``, ``work``.

Let's name our destination directory ``esmvaltool_output`` in the working directory:

```yaml
output_dir: ./esmvaltool_output
```

> ## Content of subfolders
>
> - ``plots``: the location for all plots, split by individual diagnostics and fields.
> - ``preproc``: this folder contains all the preprocessed data and metadata.yml
interface files. Note that by default this directory will be deleted after
each run because most users will only need the results from the diagnostic scripts.
> - ``run``: this folder includes all log files, a copy of the recipe,
a summary of the resource usage, and the settings.yml interface files,
resource_usage.txt and temporary files created by the diagnostic scripts.
> - ``work``: this folder is a place for any diagnostic script results that
are not plots, e.g. files in NetCDF format (depends on the diagnostic script).
>
> We explain more about output in the next
[lesson]({{ page.root }}{% link _episodes/04-recipe.md %})
{: .callout}

## Auxiliary data directory

The ``auxiliary_data_dir`` setting is the path where any required additional
auxiliary data files are stored. This location allows us to tell the diagnostic
script where to find the files if they can not be downloaded at runtime. This
option should not be used for model or observational datasets, but for data
files  (e.g. shape files) used in plotting such as coastline descriptions and so
on.

```yaml
auxiliary_data_dir: ~/auxiliary_data
```

## Number of parallel tasks

This option enables you to perform parallel processing.
You can choose the number of tasks in parallel as
1/2/3/4/... or you can set it to ``null``. That tells
ESMValTool to use the maximum number of available CPUs:

```yaml

max_parallel_tasks: null
```

> ## Set the number of tasks
>
> If you run out of memory, try setting ``max_parallel_tasks`` to 1.
Then, check the amount of memory you need for that by inspecting
the file ``run/resource_usage.txt`` in the output directory.
Using the number there you can increase the number of parallel tasks
again to a reasonable number for the amount of memory available in your system.
{: .callout}


## Rootpath to input data

ESMValTool uses several categories (in ESMValTool, this is referred to as projects)
for input data based on their source. The current categories in the configuration
file are mentioned below. For example, CMIP is used for a dataset from
the climate model intercomparison project whereas OBS is used for an observational dataset.
We can find more information about the projects in the ESMValTool
[documentation](https://docs.esmvaltool.org/projects/esmvalcore/en/latest/quickstart/find_data.html).
The ``rootpath`` specifies the directories where ESMValTool will look for input data.
For each category, you can define either one path or several paths as a list.

```yaml
rootpath:
  CMIP5: [~/cmip5_inputpath1, ~/cmip5_inputpath2]
  OBS: ~/obs_inputpath
  RAWOBS: ~/rawobs_inputpath
  default: ~/default_inputpath
  CORDEX: ~/default_inputpath
```
Site-specific entries for Jasmin, DKRZ and ETHZ are listed at the end of the example configuration file.

In this lesson, we will work with data from
[CMIP5](https://esgf-node.llnl.gov/projects/cmip5/).
We add the root path of the folder where  our/your data is available.

```yaml
  rootpath:
  ...
    CMIP5: [~/cmip5_inputpath1, ~/cmip5_inputpath2, ~/esmvaltool_tutorial/data]
```

> ## Setting the correct rootpath
>
> - To get the data (or its correct rootpath), check instruction in
>   [Setup]({{ page.root }}{% link setup.md %}).
> - For more information about setting the rootpath, see also the ESMValTool
>   [documentation](https://docs.esmvaltool.org/projects/esmvalcore/en/latest/esmvalcore/datafinder.html).
{: .callout}

## Directory structure for the data from different projects

Input data can be from various models, observations and reanalysis data that
adhere to the [CF/CMOR standard](https://cmor.llnl.gov/). The ``drs`` setting
describes the file structure. Let's use ``default`` for ``CMIP5`` in our example
here:

```yaml
drs:
  CMIP5: default
```

> ## Available drs
>
> The ``drs`` setting describes the file structure for several projects (e.g.
> ``CMIP6``, ``CMIP5``, ``obs4mips``, ``OBS6``, ``OBS``) on several key machines
> (e.g. ``BADC``, ``CP4CDS``, ``DKRZ``, ``ETHZ``, ``SMHI``, ``BSC``). For more
> information about ``drs``, you can visit the ESMValTool
> [documentation](https://docs.esmvaltool.org/projects/esmvalcore/en/latest/quickstart/find_data.html#cmor-drs).
{: .callout}

> ## Make your own configuration file
>
> It is possible to have several configuration files with different purposes,
> for example: config-user_formalised_runs.yml, config-user_debugging.yml
{: .callout}

> ## Saving preprocessed data
>
> In the configuration file, which settings are useful to make sure preprocessed
> data is stored when ESMValTool is run?
>
>> ## Solution
>>
> > If the option ``remove_preproc_dir`` is set to ``false``, then the
> > ``preproc/`` directory contains all the pre-processed data and the
> > metadata interface files.
> > If the option ``save_intermediary_cubes`` is set to ``true``
> > then data will also be saved after each preprocessor step in the folder
> > ``preproc``. Note that saving all intermediate results to file will result
> > in a considerable slowdown.
> {: .solution}
{: .challenge}

{% include links.md %}
