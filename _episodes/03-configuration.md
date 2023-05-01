---
title: "Configuration"
teaching: 10
exercises: 10
compatibility: ESMValTool v2.8.0

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

For the purposes of this tutorial, we will create a directory in our home directory
called `esmvaltool_tutorial` and use that as our working directory. The following steps 
should do that:

~~~bash
 mkdir esmvaltool_tutorial
 cd esmvaltool_tutorial
~~~

The ``config-user.yml`` configuration file contains all the global level
information needed by ESMValTool to run.
This is a [YAML file](https://yaml.org/spec/1.2/spec.html).

You can get the default configuration file by running:

~~~bash
  esmvaltool config get_config_user --path=<target_dir>
~~~
The default configuration file will be downloaded to the directory specified with 
the `--path` variable. For instance, you can provide the path to your working directory 
as the `target_dir`. If this option is not used, the file will be saved to the default 
location: `~/.esmvaltool/config-user.yml`, where `~` is the
path to your home directory. Note that files and directories starting with a
period are "hidden", to see the `.esmvaltool` directory in the terminal use
`ls -la ~`. Note that if a configuration file by that name already exists in the default 
location, the `get_config_user` command will not update the file as ESMValTool will not 
overwrite the file. You will have to move the file first if you want an updated copy of the 
user configuration file.



We run a text editor called ``nano`` to have a look inside the configuration file
and then modify it if needed:

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

The configuration file starts with output settings that
inform ESMValTool about your preference for output.
You can turn on or off the setting by ``true`` or ``false``
values. Most of these settings are fairly self-explanatory.


> ## Saving preprocessed data
>
> Later in this tutorial, we will want to look at the contents of the
> `preproc` folder.
> This folder contains preprocessed data and is removed by default when
> ESMValTool is run.
> In the configuration file, which settings can be modified to prevent
> this from happening?
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

The destination directory is the rootpath where ESMValTool will store its
output folders containing
e.g. figures, data, logs, etc. With every run, ESMValTool automatically
generates a new output folder determined by recipe name, and date and time
using the format: YYYYMMDD_HHMMSS.

> ## Set the destination directory
>
> Let's name our destination directory ``esmvaltool_output`` in the working directory.
> ESMValTool should write the output to this path, so make sure you have the disk space
> to write output to this directory.
> How do we set this in the `config-user.yml`?
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

## Rootpath to input data

ESMValTool uses several categories (in ESMValTool, this is referred to as projects)
for input data based on their source. The current categories in the configuration
file are mentioned below. For example, CMIP is used for a dataset from
the Climate Model Intercomparison Project whereas OBS may be 
used for an observational dataset.
More information about the projects used in ESMValTool is available in the 
[documentation](https://docs.esmvaltool.org/projects/esmvalcore/en/latest/
quickstart/find_data.html).
When using ESMValTool on your own machine, you can create a directory to download 
climate model data or observation data sets and let the tool use data from there. 
It is also possible to ask 
ESMValTool to download climate model data as needed. This can be done by specifying a 
download directory and by setting the option to download data as shown below.

```yaml
# Directory for storing downloaded climate data
download_dir: ~/climate_data
offline: false
```
If you are working offline or do not want to download the data then set the 
option above to `true`.

The ``rootpath`` specifies the directories where ESMValTool will look for input data.
For each category, you can define either one path or several paths as a list. For example:

```yaml
rootpath:
  CMIP5: [~/cmip5_inputpath1, ~/cmip5_inputpath2]
  OBS: ~/obs_inputpath
  RAWOBS: ~/rawobs_inputpath
  default: ~/climate_data
```
These are typically available in the default configuration file you downloaded, so simply 
removing the machine specific lines should be sufficient to access input data.

> ## Set the correct rootpath
>
> In this tutorial, we will work with data from
> [CMIP5](https://esgf-node.llnl.gov/projects/cmip5/)
> and [CMIP6](https://esgf-node.llnl.gov/projects/cmip6).
> How can we modify the `rootpath` to make sure the data path is set correctly
> for both CMIP5 and CMIP6? 
> Note:
> to get the data, check the instructions in
> [Setup]({{ page.root }}{% link setup.md %}).
>
>> ## Solution
>>
>> - Are you working on your own local machine?
>>   You need to add the root path of the folder where the data is available
>> to the `config-user.yml` file as:
>>
>>```yaml
>>   rootpath:
>>   ...
>>     CMIP5: ~/esmvaltool_tutorial/data
>>     CMIP6: ~/esmvaltool_tutorial/data
>>```
>>
>> - Are you working on your local machine and have downloaded data using ESMValTool?
>> You need to add the root path of the folder where the data has been downloaded to as
>> specified in the `download_dir`.
>>
>> ```yaml
>> rootpath:
>> ...
>>   CMIP5: ~/climate_data
>>   CMIP6: ~/climate_data
>>```
>>
>> - Are you working on a computer cluster like Jasmin or DKRZ?
>>   Site-specific path to the data for JASMIN/DKRZ/ETH/IPSL 
>>   are already listed at the end of the
>> `config-user.yml` file. You need to uncomment the related lines.
>> For example, on JASMIN:
>>
>>```yaml
>>auxiliary_data_dir: /gws/nopw/j04/esmeval/aux_data/AUX
>>rootpath: 
>>  CMIP6: /badc/cmip6/data/CMIP6
>>  CMIP5: /badc/cmip5/data/cmip5/output1
>>  OBS: /gws/nopw/j04/esmeval/obsdata-v2
>>  OBS6: /gws/nopw/j04/esmeval/obsdata-v2
>>  obs4MIPs: /gws/nopw/j04/esmeval/obsdata-v2
>>  ana4mips: /gws/nopw/j04/esmeval/obsdata-v2
>>  default: /gws/nopw/j04/esmeval/obsdata-v2
>>```
>>
>> - For more information about setting the rootpath, see also the ESMValTool
>> [documentation](https://docs.esmvaltool.org/projects/esmvalcore/en/latest/
quickstart/find_data.html).
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
[Data Reference Syntax (DRS)](https://docs.esmvaltool.org/projects/esmvalcore/
en/latest/quickstart/find_data.html#cmor-drs).

> ## Set the correct drs
>
> In this lesson, we will work with data from
> [CMIP5](https://esgf-node.llnl.gov/projects/cmip5/)
> and [CMIP6](https://esgf-node.llnl.gov/projects/cmip6/).
> How can we set the correct `drs`?
>
>> ## Solution
>>
>> - Are you working on your own local machine?
>>   You need to set the `drs` of the data
>> in the `config-user.yml` file as:
>>```yaml
>>   drs:
>>     CMIP5: default
>>     CMIP6: default
>>```
>> - Are you asking ESMValTool to download the data for use with your diagnostics?
>> You need to set the `drs` of the data in the `config-user.yml` file as:
>> ```yaml
>>    drs:
>>      CMIP5: ESGF
>>      CMIP6: ESGF
>>      CORDEX: ESGF
>>      obs4MIPs: ESGF
>>```
>> - Are you working on a computer cluster like Jasmin or DKRZ?
>>   Site-specific `drs` of the data are already listed at the end of the
>> `config-user.yml` file. You need to uncomment the related lines.
>> For example, on Jasmin:
>>```yaml
>>   # Site-specific entries: Jasmin
>>   # Uncomment the lines below to locate data on JASMIN
>>   drs:
>>     CMIP6: BADC
>>     CMIP5: BADC
>>     OBS: default
>>     OBS6: default
>>     obs4mips: default
>>     ana4mips: default
>>```
>>
> {: .solution}
{: .challenge}

> ## Explain the default drs (if working on local machine)
>
> 1. In the previous exercise, we set the `drs` of CMIP5 data to `default`.
>    Can you explain why?
> 2. Have a look at the directory structure of the `OBS` data.
>    There is a folder called `Tier1`. What does it mean?
>
>> ## Solution
>>
>> 1. `drs: default` is one way to retrieve data from a ROOT directory that has
>>    no DRS-like structure. ``default`` indicates that all the files are in a
>>    folder without any structure.
>>
>> 2. Observational data are organized in Tiers depending on their level of
>>    public availability. Therefore the default directory must be structured
>>    accordingly with sub-directories `TierX` e.g. Tier1, Tier2 or Tier3, even
>>    when `drs: default`. More details can be found in the 
[documentation](https://docs.esmvaltool.org/projects/esmvalcore/en/latest/
quickstart/find_data.html#observational-data).
>>
> {: .solution}
{: .challenge}

## Other settings

> ## Auxiliary data directory
>
> The ``auxiliary_data_dir`` setting is the path where any required additional
auxiliary data files are stored. This location allows us to tell the diagnostic
script where to find the files if they can not be downloaded at runtime. This
option should not be used for model or observational datasets, but for data
files (e.g. shape files) used in plotting such as coastline descriptions and
if you want to feed some additional data (e.g. shape files) to your recipe.
>
>```yaml
> auxiliary_data_dir: ~/auxiliary_data
> ```
> See more information in ESMValTool
[document](https://docs.esmvaltool.org/projects/ESMValCore/en/latest/quickstart
/configure.html?highlight=auxiliary_data#user-configuration-file).
{: .callout}

> ## Number of parallel tasks
>
> This option enables you to perform parallel processing. You can choose the
number of tasks in parallel as 1/2/3/4/... or you can set it to ``null``. That
tells ESMValTool to use the maximum number of available CPUs. For the purpose of
the tutorial, please set ESMValTool use only 1 cpu:
>
>```yaml
> max_parallel_tasks: 1
> ```
>
> In general, if you run out of memory, try setting ``max_parallel_tasks`` to 1.
Then, check the amount of memory you need for that by inspecting the file
``run/resource_usage.txt`` in the output directory. Using the number there you
can increase the number of parallel tasks again to a reasonable number for the
amount of memory available in your system.
{: .callout}

> ## Make your own configuration file
>
> It is possible to have several configuration files with different purposes,
> for example: config-user_formalised_runs.yml, config-user_debugging.yml.
> In this case, you have to pass the path of your own configuration file
> as a command-line option when running the ESMValTool.
> We will learn how to do this in the
> [next lesson]({{ page.root }}{% link _episodes/04-recipe.md %}).
{: .callout}

{% include links.md %}
