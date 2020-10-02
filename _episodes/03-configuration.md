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
- "``output_dir`` defines the destination directory."
- "``rootpath`` defines the root path of the data."
- "``drs`` defines the directory structure of the data."

---

## The configuration file

The ``config-user.yml`` configuration file contains all the global level
information needed by ESMValTool to run.
This is a [YAML file](https://yaml.org/spec/1.2/spec.html).
An example configuration file can be found in the ESMValCore repository:
[config-user-example.yml](https://github.com/ESMValGroup/ESMValCore/blob/master/esmvalcore/config-user.yml).

You can generate the default configuration file by running:

~~~bash
  esmvaltool config get_config_user
~~~

It will save the file to: `~/.esmvaltool/config-user.yml`, where `~` is the
path to your home directory. Note that files and directories starting with a
period are "hidden", to see the `.esmvaltool` directory in the terminal use
`ls -la ~`.

We run a text editor called ``nano`` to have a look inside the configuration file:

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

> ## Saving preprocessed data
>
> In the configuration file, which settings are useful to make sure preprocessed
> data is stored when ESMValTool is run?
>
>> ## Solution
>>
>> If the option ``remove_preproc_dir`` is set to ``false``, then the
>> ``preproc/`` directory contains all the pre-processed data and the
>> metadata interface files.
>> If the option ``save_intermediary_cubes`` is set to ``true``
>> then data will also be saved after each preprocessor step in the folder
>> ``preproc``. Note that saving all intermediate results to file will result
>> in a considerable slowdown, and can quickly fill your disk.
> {: .solution}
{: .challenge}

## Destination directory

The destination directory is the rootpath where ESMValTool will store its output folders containing
e.g. figures, data, logs, etc. With every run, ESMValTool automatically generates
a new output folder determined by recipe name, and date and time using
the format: YYYYMMDD_HHMMSS.
This folder contains four further subfolders: ``plots``, ``preproc``, ``run``, ``work``.

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

> ## Set the destination directory
>
> Let's name our destination directory ``esmvaltool_output`` in the working directory.
> How to tell ESMValTool about our destination directory?
>
>> ## Solution
>>
>> We use `output_dir` entry in the `config-user.yml` file as:
>>```yaml
>> output_dir: ./esmvaltool_output
>>```
>>
>> If the `esmvaltool_output` does not exist, ESMValTool will generate it for you.
> {: .solution}
{: .challenge}

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
For example:

```yaml
rootpath:
  CMIP5: [~/cmip5_inputpath1, ~/cmip5_inputpath2]
  OBS: ~/obs_inputpath
  RAWOBS: ~/rawobs_inputpath
  default: ~/default_inputpath
  CORDEX: ~/default_inputpath
```

Site-specific entries for Jasmin and DKRZ are listed at the end of the
example configuration file.

> ## Set the correct rootpath
>
> In this tutorial, we will work with data from
> [CMIP5](https://esgf-node.llnl.gov/projects/cmip5/)
> and [obs4mips](https://esgf-node.llnl.gov/projects/obs4mips/).
> How does ESMValTool find the data?
>
> Note:
> to get the data, check instruction in
> [Setup]({{ page.root }}{% link setup.md %}).
>
>> ## Solution
>>
>> - Are you working on your own local machine?
>>   You need to add the root path of the folder where the data is available
>> to the `config-user.yml` file as:
>>```yaml
>>  rootpath:
>>  ...
>>    CMIP5: ~/esmvaltool_tutorial/data
>>    obs4mips: ~/esmvaltool_tutorial/data
>>```
>>
>> - Are you working with on a computer cluster like Jasmin or DKRZ?
>>   Site-specific path to the data are already listed at the end of the
>> `config-user.yml` file. You need to uncomment the related lines.
>> For example, on Jasmin:
>>```yaml
>>  # Site-specific entries: Jasmin
>>  # Uncomment the lines below to locate data on JASMIN
>>  rootpath:
>>  #  CMIP6: /badc/cmip6/data/CMIP6
>>    CMIP5: /badc/cmip5/data/cmip5/output1
>>  #  CMIP3: /badc/cmip3_drs/data/cmip3/output
>>  #  OBS: /group_workspaces/jasmin4/esmeval/obsdata-v2
>>  #  OBS6: /group_workspaces/jasmin4/esmeval/obsdata-v2
>>    obs4mips: /group_workspaces/jasmin4/esmeval/obsdata-v2
>>  #  ana4mips: /group_workspaces/jasmin4/esmeval/obsdata-v2
>>  #  CORDEX: /badc/cordex/data/CORDEX/output
>>```
>>
>> - For more information about setting the rootpath, see also the ESMValTool
>> [documentation](https://docs.esmvaltool.org/projects/esmvalcore/en/latest/quickstart/find_data.html).
> {: .solution}
{: .challenge}

## Directory structure for the data from different projects

Input data can be from various models, observations and reanalysis data that
adhere to the [CF/CMOR standard](https://cmor.llnl.gov/). The ``drs`` setting
describes the file structure.

The ``drs`` setting describes the file structure for several projects (e.g.
CMIP6, CMIP5, obs4mips, OBS6, OBS) on several key machines
(e.g. BADC, CP4CDS, DKRZ, ETHZ, SMHI, BSC). For more
information about ``drs``, you can visit the ESMValTool documentation on
[Data Reference Syntax (DRS)](https://docs.esmvaltool.org/projects/esmvalcore/en/latest/quickstart/find_data.html#cmor-drs).

> ## Set the correct drs
>
> In this lesson, we will work with data from
> [CMIP5](https://esgf-node.llnl.gov/projects/cmip5/)
> and [obs4mips](https://esgf-node.llnl.gov/projects/obs4mips/).
> How does ESMValTool know the data structure?
>
>> ## Solution
>>
>> - Are you working on your own local machine?
>>   You need to set the `drs` of the data
>> in the `config-user.yml` file as:
>>```yaml
>>   drs:
>>     CMIP5: default
>>     obs4mips: default
>>```
>>
>> - Are you working with on a computer cluster like Jasmin or DKRZ?
>>   Site-specific `drs` of the data are already listed at the end of the
>> `config-user.yml` file. You need to uncomment the related lines.
>> For example, on Jasmin:
>>```yaml
>>  # Site-specific entries: Jasmin
>>  # Uncomment the lines below to locate data on JASMIN
>>  drs:
>>  #  CMIP6: BADC
>>    CMIP5: BADC
>>  #  CMIP3: BADC
>>  #  CORDEX: BADC
>>  #  OBS: BADC
>>  #  OBS6: BADC
>>    obs4mips: BADC
>>  #  ana4mips: BADC
>>```
>>
> {: .solution}
{: .challenge}

> ## Explain the default drs (if working on local machine)
>
> 1. In the previous exercise, we set the `drs` of CMIP5 data to `default`.
> Can you explain why?
>
> 2. Have a look at the directory structure of the data.
> There is the folder `Tier1`. What does it mean?
>
>> ## Solution
>>
>> 1. `drs: default` is one way to retrieve data from a ROOT directory that has no DRS-like structure.
>> ``default`` indicates that all the files are in a folder without any structure.
>>
>> 2. Observational data are organized in Tiers depending on their level of public availability.
>> Therefore the default directory must be structured accordingly with sub-directories
>> `TierX` e.g. Tier1, Tier2 or Tier3, even when `drs: default`.
>>
> {: .solution}
{: .challenge}

> ## Make your own configuration file
>
> It is possible to have several configuration files with different purposes,
> for example: config-user_formalised_runs.yml, config-user_debugging.yml. In this case, you have to pass the path to the config file you want to use as a command line option when running the ESMValTool.
{: .callout}

{% include links.md %}
