---
title: "Configuration"
teaching: 0
exercises: 0
questions:
- "What is the user configuration file and how can I use it?"
objectives:
- "Understand the data directories structure"
- "Configure ESMValTool to ignore some settings"
keypoints:
- "The ``config-user.yml`` file tells ESMValTool what data are input"
- "The ``config-user.yml`` file tells ESMValTool what directory is the destination"
---

## The configuration file

The ``config-user.yml`` configuration file contains all the global level information needed by ESMValTool to run.
This is an (YAML file) [https://yaml.org/spec/1.2/spec.html]. An example configuration file can be found in the root directory of the ESMValTool repository.
Let's change our working directory to ESMValTool, then make a copy of the file and rename it to ``config-user.yml``, then run a text editor called Nano to open it:

~~~
  cd ESMValTool
  cp config-user-example.yml config-user.yml
  nano config-user.yml
~~~
{: .source}

This file contains the information for:
  * Rootpath to input data
  * Directory structure for the data from different projects
  * Number of parallel tasks
  * Destination directory
  * Auxiliary data directory
  * Output settings

> ## Which text editor
>
> No matter what editor you use, you will need to know where it searches for and saves files. If you start it from the shell, it will (probably) use your current working directory as its default location. We use ``nano`` in examples because it is one of the least complex text editors. Press <kbd>ctrl</kbd> + <kbd>O</kbd> to save the file, and then <kbd>ctrl</kbd> + <kbd>X</kbd> to exit nano.
{: .callout}

## Rootpaths to input data
ESMValTool uses several categories (in ESMValTool, this is referred to as projects) for input data based on their source.
The current categories in the configuration file are mentioned below. For example, CMIP is used for a dataset from the climate model intercomparison project whereas OBS for an observational dataset. The ``rootpath`` specifies the directories where ESMValTool will look for input data. For each category, you can define either one path or several paths as a list.

~~~
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
{: .source}

In this lesson, you work with data from [CMIP5](https://esgf-node.llnl.gov/projects/cmip5/).
Add the root path of the folder where you downloaded the data during the [Setup](https://esmvalgroup.github.io/tutorial/setup.html).

~~~
  rootpath:
  ...
    CMIP5: [~/cmip5_inputpath1, ~/cmip5_inputpath2, ~/tutorial/test_data]
~~~
{: .source}

> ## Setting the correct rootpath
>
> For more information about setting the rootpath, you can visit ESMValTool [documentation](https://esmvaltool.readthedocs.io/projects/esmvalcore/en/latest/esmvalcore/datafinder.html).
{: .callout}

## Directory structure for the data from different projects
Input data can be from various models, observations and reanalysis data that adhere to the [CF/CMOR standard](https://cmor.llnl.gov/). To set a directory, you can use one of the values of ``default``, ``BADC``, ``DKRZ``, ``ETHZ``, ... . Let's use ``default`` in our example:

~~~
drs:
  CMIP5: default
~~~
{: .source}

> ## Available drs
>
> For more information about directories, you can visit ESMValTool [documentation](https://esmvaltool.readthedocs.io/projects/esmvalcore/en/latest/esmvalcore/config.html#developer-configuration-file).
{: .callout}

## Number of parallel tasks
This option enables you to perform parallel processing. You can choose the number of tasks in parallel as 1/2/3/4/... or you can set it to ``null`` that tells ESMValTool to use the number of available CPUs:

~~~
max_parallel_tasks: null
~~~
{: .source}

> ## Set the number of tasks
>
> If you run out of memory, try setting ``max_parallel_tasks`` to 1. Then, check the amount of memory you need for that by inspecting the file ``run/resource_usage.txt`` in the output directory. Using the number there you can increase the number of parallel tasks again to a reasonable number for the amount of memory available in your system.
{: .callout}

## Destination directory
The destination directory is the rootpath where ESMValTool will store its output, i.e. figures, data, logs, etc. With every run, ESMValTool automatically generates a new output folder determined by the recipe name, and date and time using the format: YYYYMMDD_HHMMSS. This folder contains four further subfolders: ``plots``, ``preproc``, ``run``, ``work``.

Let's name our destination directory as ``esmvaltool_output`` in the working directory:

~~~
output_dir: ./esmvaltool_output
~~~
{: .source}

> ## Content of subfolders
>
> * ``plots``: the location for all plots, split by individual diagnostics and fields.
> * ``preproc``: this folder contains all the preprocessed data and metadata.yml interface files. Note that by default this directory will be deleted after each run because most users will only need the results from the diagnostic scripts.
> * ``run``: this folder includes all log files, a copy of the recipe, a summary of the resource usage, and the settings.yml interface files, resource_usage.txt and temporary files created by the diagnostic scripts.
> * ``work``: a place for any diagnostic script results that are not plots, e.g. files in NetCDF format (depends on the diagnostic script).
>
> We explain more about output in the next [lesson](https://esmvalgroup.github.io/tutorial/04-toy-example/index.html)
{: .callout}


## Auxiliary data directory (used for some additional datasets)
  auxiliary_data_dir: ~/auxiliary_data

The ``auxiliary_data_dir`` setting is the path to place any required
additional auxiliary data files. This method was necessary because certain
Python toolkits such as cartopy will attempt to download data files at run
time, typically geographic data files such as coastlines or land surface maps.
This can fail if the machine does not have access to the wider internet. This
location allows us to tell cartopy (and other similar tools) where to find the
files if they can not be downloaded at runtime. To reiterate, this setting is
not for model or observational datasets, rather it is for data files used in
plotting such as coastline descriptions and so on.


## Output settings

.. code-block:: yaml

  # Diagnostics create plots? [true]/false
  write_plots: true
  # Diagnositcs write NetCDF files? [true]/false
  write_netcdf: true

The ``write_plots`` setting is used to inform ESMValTool about your preference
for saving figures. Similarly, the ``write_netcdf`` setting is a boolean which
turns on or off the writing of netCDF files.

The ```rootpath`` specifies the directories where ESMValTool will look for input
data. Similarly, ``output_dir`` specifies where ESMValTool will store its
output, i.e. figures, data, logs, etc. Make sure to set appropriate paths.

.. code-block:: yaml




You can tailor it for your system using the explanation below.

.. note::

   The ``config-user.yml`` file is specified as argument at run time, so it is
   possible to have several available with different purposes: one for
   formalised runs, one for debugging, etc...


{% include links.md %}

