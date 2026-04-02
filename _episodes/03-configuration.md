---
title: "Configuration"
teaching: 10
exercises: 15
compatibility: ESMValTool v2.14.0

questions:
- What is the user configuration file and how should I use it?

objectives:
- Understand the contents of the ``config-user.yml`` file
- Prepare a personalized ``config-user.yml`` file
- Configure ESMValTool to use stored climate data and to download climate data

keypoints:
- The ``config-user.yml`` sets basic configurations for ESMValTool.
- There are specific configuration files to find stored climate data.
- ESMvalTool can download automatically climate data from ESGF, therefore specific configurations are needed.

---

## The configuration file 

First, for the purposes of this tutorial, we will create a directory in our home directory
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
  esmvaltool config copy defaults/config-user.yml
~~~
The default configuration file will be downloaded to the default location: 
`~/.config/esmvaltool/config-user.yml`, where `~` is the
path to your home directory. Note that files and directories starting with a
period are "hidden", to see the `.config` directory in the terminal use
`ls -la ~`. 
Note, if a configuration file by that name already exists in the default 
location, the `config copy` command will not update the file as ESMValTool will not 
overwrite the file. You will have to move the file first if you want an updated copy of the 
default user configuration file.



We run a text editor called ``nano`` to have a look inside the configuration file
and then modify it if needed:

~~~bash
  nano ~/.config/esmvaltool/config-user.yml
~~~

If ``nano`` does not work on your system, or if you prefer a different editor, 
any other editor can be used, e.g. ``vim``.

This file contains the information for:

- Output settings
- Destination directory
- Auxiliary data directory
- Number of tasks that can be run in parallel
- ...

> ## Text editor side note
>
> No matter what editor you use, you will need to know where it searches
> for and saves files. If you start it from the shell, it will (probably)
> use your current working directory as its default location. We use ``nano``
> in examples here because it is one of the least complex text editors.
> Press <kbd>ctrl</kbd> + <kbd>O</kbd> to save the file,
> and then <kbd>ctrl</kbd> + <kbd>X</kbd> to exit ``nano``.
{: .callout}

## Destination directory

The configuration file starts with setting the destination directory, which is 
the rootpath where ESMValTool will store its output folders containing
e.g. figures, data, logs, etc. With every run, ESMValTool automatically
generates a new output folder determined by recipe name, and date and time
using the format: YYYYMMDD_HHMMSS.

> ## Set the destination directory
>
> Let's name our destination directory ``esmvaltool_output`` in the current directory.
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

## Output settings

Additionally you can configure the output settings that
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
[documentation](https://docs.esmvaltool.org/projects/ESMValCore/en/latest/
> quickstart/configure.html?highlight=auxiliary_data#top-level-configuration-options).
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


## Make your own configuration file

Configuration files could live in the user configuration directory, which is 
by default ``~/.config/esmvaltool``. The directory could be also specified
via the command line argument ``--config_dir`` or can be changed with the 
ESMVALTOOL_CONFIG_DIR environment variable.
We will learn how to do this in the
[next lesson]({{ page.root }}{% link _episodes/04-recipe.md %}).

It is possible to have several configuration files with different purposes,
for example: config-user_formalised_runs.yml, config-user_debugging.yml.
In this case, ESMValTool searches for all YAML files within each of the 
configuration directories and merges them together. How this is done is 
explained [here](https://docs.esmvaltool.org/projects/ESMValCore/en/
latest/quickstart/configure.html#yaml-files).


## Rootpath to input data

ESMValTool uses several categories (in ESMValTool, this is referred to as projects)
for input data based on their source (e.g.
CMIP6, CMIP5, obs4mips, OBS6, OBS). For example, CMIP is used for a dataset from
the Climate Model Intercomparison Project whereas OBS may be 
used for an observational dataset.
More information about the projects used in ESMValTool is available in the 
[documentation](https://docs.esmvaltool.org/projects/esmvalcore/en/latest/
quickstart/find_data.html). The ``data`` section for each project in the configuration 
files defines sources of input data. The easiest way to get started with these is to 
copy one of the example configuration files and tailor it to your needs.

When using ESMValTool on your own machine, the recommended setup can 
be obtained by running the command

~~~bash
  esmvaltool config copy data-local-esmvaltool.yml
~~~

After the default file "data-local-esmvaltool.yml" is copied in your configuration
folder `~/.config/esmvaltool/config-user.yml` you can update the `rootpath` and the
`dirname_template` to match your file locations. The ``rootpath`` specifies the 
directories where ESMValTool will look for input data of the specific project. The
`dirname_template` setting describes the file structure for each project.

If you are working on a HPC system there are also  several example configurations 
for popular HPC systems, .e.g. JASMIN/DKRZ/ETH/IPSL. To list the available example 
files, run the command:

~~~bash
  esmvaltool config list data-hpc
~~~

To load the suitable configuration file for the HPC system at DKRZ you can type:
~~~bash
  esmvaltool config copy data-hpc-dkrz.yml
~~~

It is also possible to ask ESMValTool to download climate model data as needed. When
running `ESMValTool` you can automatically download the files required to run a recipe 
from ESGF for the projects CMIP3, CMIP5, CMIP6, CORDEX, and obs4MIPs. Therefore
we need to copy the appropriate configuration file in the default configuration folder:

~~~bash
  esmvaltool config copy data-intake-esgf.yml
~~~

Additionally we need to configure [intake-esgf]
(https://intake-esgf.readthedocs.io/en/stable/configure.html).
This can be done by specifying a download directory in the intake-esgf configure file, 
which is by default `~/.esgf`. Therefor copy the conf.yaml file in the 
`~/.config/intake-esgf` folder and replace the default under `local_cache:` and add 
this download folder also under `esg_dataroot:`. The uodated file should 
look like this:
> ## conf.yml
>
> ```yaml 
> additional_df_cols: []
> break_on_error: true
> confirm_download: false
> download_db: ~/.config/intake-esgf/download.db
> esg_dataroot:
> - <your_download_dir>
> - /p/css03/esgf_publish
> - /eagle/projects/ESGF2/esg_dataroot
> - /global/cfs/projectdirs/m3522/cmip6/
> - /glade/campaign/collections/cmip.mirror
> globus_indices:
>   ESGF2-US-1.5-Catalog: true
>   anl-dev: false
>   ornl-dev: false
> local_cache:
> - <your_download_dir>
> logfile: ~/.config/intake-esgf/esgf.log
> num_threads: 6
> print_log_on_error: false
> requests_cache:
>   cache_name: intake-esgf/requests-cache.sqlite
>   expire_after: 3600
>   use_cache_dir: true
> slow_download_threshold: 0.5
> solr_indices:
>   esg-dn1.nsc.liu.se: false
>   esgf-data.dkrz.de: false
>   esgf-node.ipsl.upmc.fr: false
>   esgf-node.llnl.gov: false
>   esgf-node.ornl.gov: false
>   esgf.ceda.ac.uk: false
>   esgf.nci.org.au: false
> stac_indices:
>   api.stac.ceda.ac.uk: false
> ```
>
{: .solution}


> ## Set the correct rootpaths
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
>>   You need to copy teh `data-local-esmvaltool.yml` into your config directory 
>>   and add the root path of the folder where the data is available (e.g., ``<your_climate_data_dir>``) as:
>>
>>```yaml
>>   projects:
>>   ...
>>     CMIP6:
>>       data:
>>         local:
>>           type: esmvalcore.io.local.LocalDataSource
>>           rootpath: <your_climate_data_dir>
>>           dirname_template: "{project}/{activity}/{institute}/{dataset}/{exp}/{ensemble}/{mip}/{short_name}/{grid}/{version}"
>>           filename_template: "{short_name}_{mip}_{dataset}_{exp}_{ensemble}_{grid}*.nc"
>>     CMIP5:
>>       data:
>>         local:
>>           type: esmvalcore.io.local.LocalDataSource
>>           rootpath: <your_climate_data_dir>
>>           dirname_template: "{project.lower}/{product}/{institute}/{dataset}/{exp}/{frequency}/{modeling_realm}/{mip}/{ensemble}/{version}/{short_name}"
>>           filename_template: "{short_name}_{mip}_{dataset}_{exp}_{ensemble}*.nc"
>>```
>>
>> - Are you working on your local machine and have downloaded data using ESMValTool?
>> You need to add the root path of the folder where the data has been downloaded to as
>> specified in the `esgf-cache`.
>>
>> ```yaml
>>   projects:
>>   ...
>>     CMIP6:
>>       data:
>>         local:
>>           type: esmvalcore.io.local.LocalDataSource
>>           rootpath: <your_climate_data_dir>
>>           dirname_template: "{project}/{activity}/{institute}/{dataset}/{exp}/{ensemble}/{mip}/{short_name}/{grid}/{version}"
>>           filename_template: "{short_name}_{mip}_{dataset}_{exp}_{ensemble}_{grid}*.nc"
>>         esgf-cache:
>>           type: esmvalcore.io.local.LocalDataSource
>>           rootpath: <your_download_dir>
>>           dirname_template: "{project}/{activity}/{institute}/{dataset}/{exp}/{ensemble}/{mip}/{short_name}/{grid}/{version}"
>>           filename_template: "{short_name}_{mip}_{dataset}_{exp}_{ensemble}_{grid}*.nc"
>>     CMIP5:
>>       data:
>>         local:
>>           type: esmvalcore.io.local.LocalDataSource
>>           rootpath: <your_climate_data_dir>
>>           dirname_template: "{project.lower}/{product}/{institute}/{dataset}/{exp}/{frequency}/{modeling_realm}/{mip}/{ensemble}/{version}/{short_name}"
>>           filename_template: "{short_name}_{mip}_{dataset}_{exp}_{ensemble}*.nc"
>>         esgf-cache:
>>           type: esmvalcore.io.local.LocalDataSource
>>           rootpath: <your_download_dir>
>>           dirname_template: "{project.lower}/{product}/{institute}/{dataset}/{exp}/{frequency}/{modeling_realm}/{mip}/{ensemble}/{version}"
>>           filename_template: "{short_name}_{mip}_{dataset}_{exp}_{ensemble}*.nc"
>>```
>>
>> - Are you working on a computer cluster like Jasmin or DKRZ?
>>   Site-specific path to the data for JASMIN/DKRZ/ETH/IPSL 
>>   are already available in specific configuration files. You 
>>   need to copy this file in your configuration directory.
>>   For example, on DKRZ:
>>
>>```bash
>>   esmvaltool config copy data-hpc-dkrz.yml
>>```
>>
>> - For more information about configure the data sources, see also the ESMValTool
>> [documentation](https://docs.esmvaltool.org/projects/ESMValCore/en/latest/quickstart/configure.html#project-specific-configuration).
> {: .solution}
{: .challenge}


{% include links.md %}
