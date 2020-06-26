---
title: "Configuration"
teaching: 0
exercises: 0
questions:
- "What is the user configuration file and how can I use it?"
objectives:
- "Understand the contents of the user-config.yml file"
- "Prepare the user-config.yml file for use"
- "Configure ESMValTool to ignore some settings"
keypoints:
- "The ``config-user.yml`` tells ESMValTool where to find input data."
- " ``rootpath`` determines root directory for input data."
- " ``output_dir`` is the destination directory."
---

## The configuration file

The ``config-user.yml`` configuration file contains all the global level information needed by ESMValTool to run.
This is an [YAML file](https://yaml.org/spec/1.2/spec.html). An example configuration file can be found in the root directory of the ESMValTool repository: [config-user-example.yml](https://github.com/ESMValGroup/ESMValTool/blob/master/config-user-example.yml).

Let's download it to our working directory ``esmvaltool_tutorial`` that is made during the [Setup](https://esmvalgroup.github.io/tutorial/setup.html).
To do that, click on [this link](https://raw.githubusercontent.com/ESMValGroup/ESMValTool/master/config-user-example.yml) to see a raw version of the file, right-click and press ``save as``, then you can rename it to ``config-user.yml`` and save it into the working directory ``esmvaltool_tutorial``.

Now in a terminal, let's change our working directory to ``esmvaltool_tutorial``. Then, we run a text editor called Nano to open the configuration file:

~~~bash
  cd esmvaltool_tutorial
  nano config-user.yml
~~~

This file contains the information for:

* Rootpath to input data
* Directory structure for the data from different projects
* Number of parallel tasks
* Destination directory
* Auxiliary data directory
* Output settings

> ## Which text editor
>
> No matter what editor you use, you will need to know where it searches for and saves files. If you start it from the shell, it will (probably) use your current working directory as its default location. We use ``nano`` in examples because it is one of the least complex text editors. Press <kbd>ctrl</kbd> + <kbd>O</kbd> to save the file, and then <kbd>ctrl</kbd> + <kbd>X</kbd> to exit ``nano``.
{: .callout}

## Rootpath to input data

ESMValTool uses several categories (in ESMValTool, this is referred to as projects) for input data based on their source.
The current categories in the configuration file are mentioned below. For example, CMIP is used for a dataset from the climate model intercomparison project whereas OBS for an observational dataset. The ``rootpath`` specifies the directories where ESMValTool will look for input data. For each category, you can define either one path or several paths as a list.

~~~YAML
rootpath:
  CMIP3: [~/cmip3_inputpath1, ~/cmip3_inputpath2]
  CMIP5: [~/cmip5_inputpath1, ~/cmip5_inputpath2]
  CMIP6: [~/cmip6_inputpath1, ~/cmip6_inputpath2]
  OBS: ~/obs_inputpath
  OBS6: ~/obs6_inputpath
  obs4mips: ~/obs4mips_inputpath
  ana4mips: ~/ana4mips_inputpath
  native6:  ~/native6_inputpath
  RAWOBS: ~/rawobs_inputpath
  default: ~/default_inputpath
~~~

In this lesson, we will work with data from [CMIP5](https://esgf-node.llnl.gov/projects/cmip5/).
We add the root path of the folder where data is available.

~~~YAML
  rootpath:
  ...
    CMIP5: [~/cmip5_inputpath1, ~/cmip5_inputpath2, ~/esmvaltool_tutorial/data]
~~~

> ## Setting the correct rootpath
>
> * To get the data (or its correct rootpath), check instruction in [Setup](https://esmvalgroup.github.io/tutorial/setup.html).
> * For more information about setting the rootpath, you can visit ESMValTool [documentation](https://esmvaltool.readthedocs.io/projects/esmvalcore/en/latest/esmvalcore/datafinder.html).
{: .callout}

## Directory structure for the data from different projects

Input data can be from various models, observations and reanalysis data that adhere to the [CF/CMOR standard](https://cmor.llnl.gov/). To set a directory, you can use one of the values of ``default``, ``BADC``, ``DKRZ``, ``ETHZ``, .... Let's use ``default`` in our example:

~~~YAML
drs:
  CMIP5: default
~~~

> ## Available drs
>
> For more information about directories, you can visit ESMValTool [documentation](https://esmvaltool.readthedocs.io/projects/esmvalcore/en/latest/esmvalcore/config.html#developer-configuration-file).
{: .callout}

## Number of parallel tasks

This option enables you to perform parallel processing. You can choose the number of tasks in parallel as 1/2/3/4/... or you can set it to ``null`` that tells ESMValTool to use the number of available CPUs:

~~~YAML

max_parallel_tasks: null
~~~

> ## Set the number of tasks
>
> If you run out of memory, try setting ``max_parallel_tasks`` to 1. Then, check the amount of memory you need for that by inspecting the file ``run/resource_usage.txt`` in the output directory. Using the number there you can increase the number of parallel tasks again to a reasonable number for the amount of memory available in your system.
{: .callout}

## Destination directory

The destination directory is the rootpath where ESMValTool will store its output, i.e. figures, data, logs, etc. With every run, ESMValTool automatically generates a new output folder determined by the recipe name, and date and time using the format: YYYYMMDD_HHMMSS. This folder contains four further subfolders: ``plots``, ``preproc``, ``run``, ``work``.

Let's name our destination directory as ``esmvaltool_output`` in the working directory:

~~~YAML
output_dir: ./esmvaltool_output
~~~

> ## Content of subfolders
>
> * ``plots``: the location for all plots, split by individual diagnostics and fields.
> * ``preproc``: this folder contains all the preprocessed data and metadata.yml interface files. Note that by default this directory will be deleted after each run because most users will only need the results from the diagnostic scripts.
> * ``run``: this folder includes all log files, a copy of the recipe, a summary of the resource usage, and the settings.yml interface files, resource_usage.txt and temporary files created by the diagnostic scripts.
> * ``work``: a place for any diagnostic script results that are not plots, e.g. files in NetCDF format (depends on the diagnostic script).
>
> We explain more about output in the next [lesson](https://esmvalgroup.github.io/tutorial/04-toy-example/index.html)
{: .callout}

## Auxiliary data directory

The ``auxiliary_data_dir`` setting is the path to place any required additional auxiliary data files. This location allows us to tell the diagnostic script where to find the files if they can not be downloaded at runtime. This option is not for model or observational datasets, rather it is for data files used in plotting such as coastline descriptions and so on.

~~~YAML
auxiliary_data_dir: ~/auxiliary_data
~~~

## Output settings

These settings are used to inform ESMValTool about your preference. You can turn on or off the setting by ``true`` or ``false`` values. Most of these settings are fairly self-explanatory, ie:

~~~YAML
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
# Use netCDF compression true/[false]
compress_netcdf: false
# Save intermediary cubes in the preprocessor true/[false]
save_intermediary_cubes: false
# Remove the preproc dir if all fine
remove_preproc_dir: true
# Path to custom config-developer file, to customise project configurations.
# See config-developer.yml for an example. Set to [null] to use the default
# config_developer_file: null
# Get profiling information for diagnostics
# Only available for Python diagnostics
profile_diagnostic: false
~~~

> ## Make your own configuration file
>
> It is possible to have several configuration files with different purposes, for example: config-user_formalised_runs.yml, config-user_debugging.yml
{: .callout}

> ## Different settings
>
> In the configuration file, which settings are useful to store preprocessed data?
>
>> ## Solution
>>
>> If the option ``save_intermediary_cubes`` is set to true in the config-user.yml file, then the intermediary cubes will also be saved in the folder ``preproc``. Also, if the option ``remove_preproc_dir`` is set to ``false``, then the ``preproc/`` directory contains all the preprocessed data and the metadata interface files.
> {: .solution}
{: .challenge}

{% include links.md %}
