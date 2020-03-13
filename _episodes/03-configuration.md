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
Make a copy and rename it to ``config-user.yml``:

~~~
  cp config-user-example.yml config-user.yml
~~~
{: .source}

This file contains the information for:
  * Directory structure for the data from different projects
  * Rootpath to input data
  * Number of available CPUs
  * Destination directory
  * Auxiliary data directory
  * Output settings

## Directory structure for the data from different projects
Input data can be from various models, observations and reanalysis data that adhere to the [CF/CMOR standard](https://cmor.llnl.gov/). ESMValTool uses several categories (in ESMValTool, this is referred to as projects) for input data based on their source.
The current categories in the configuration file are mentioned below. For example, CMIP is used for a dataset from the climate model intercomparison project whereas OBS for an observational dataset. For each category, you can define either one path or several pathes as a list.

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

> ## Attention
>
> * For more information about data from different projects, you can visit ESMValTool [documentation](https://esmvaltool.readthedocs.io/en/latest/getting_started/inputdata.html).
{: .callout}

## Rootpaths to input data
The ``rootpath`` specifies the directories where ESMValTool will look for input data.
In this lesson, you work with data from (CMIP5)[https://esgf-node.llnl.gov/projects/cmip5/].
Add the root path of the folder where you downloaded the data during the (Setup)[https://esmvalgroup.github.io/tutorial/setup.html].

~~~
  rootpath:
  ...
    CMIP5: [~/cmip5_inputpath1, ~/cmip5_inputpath2, ~/tutorial/test_data]
~~~
{: .source}

> ## Attention
>
> * For more information about setting the correct rootpath, you can visit ESMValTool [documentation](https://esmvaltool.readthedocs.io/projects/esmvalcore/en/latest/esmvalcore/datafinder.html).
{: .callout}

## Number of available CPUs
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

