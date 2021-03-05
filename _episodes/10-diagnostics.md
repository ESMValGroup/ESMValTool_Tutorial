---
title: "Writing your own diagnostic script"
teaching: TBD
exercises: TBD

questions:
- "How do I write a new diagnostic in ESMValTool?"
- "How do I use the preprocessor output in a Python diagnostic?"

objectives:
- "Write a new Python diagnostic script."
- "Explain how a diagnostic script reads the preprocessor output."

keypoints:
- "ESMValTool provides helper functions to interface a Python diagnostic script with preprocessor output."
- "Existing diagnostics can be used as templates and modified to write new diagnostics."
---

## Introduction

The diagnostic script is an important component of ESMValTool where the
scientific analysis or performance metric is implemented. With ESMValTool, you
can adapt an existing diagnostic or write a new script from scratch.
Diagnostics can be written in a number of open source
languages such as Python, R, Julia and NCL but we will focus on understanding
and writing Python diagnostics in this lesson.
There are two approches to run your own diagnostics:

1. using ESMValTool installed in a stable mode
2. using ESMValTool installed in a editable/development mode

In this lesson, we will explain how to find an existing diagnostic and run it
using the second approch. For a development installation, see the instructions
in the lesson
[Development and contribution]({{ page.root }}{% link _episodes/08-development-setup.md %}).
Also, we will work with the recipe [recipe_python.yml][recipe] and the diagnostic script
[diagnostic.py][diagnostic] called by this recipe that we have seen in the lesson
[Running your first recipe]({{ page.root }}{% link _episodes/04-recipe.md %}).

Let's get started.

## Understanding an existing Python diagnostic

After development installation, a folder called ``ESMValTool`` has been created
in your working directory. This folder contains the source code of the tool. We
can find the recipe ``recipe_python.yml`` and the python script
``diagnostic.py`` in these directories:

- *path_to_ESMValTool/esmvaltool/recipes/examples/recipe_python.yml*
- *path_to_ESMValTool/esmvaltool/diag_scripts/examples/diagnostic.py*

Let's have look into the codes of the ``diagnostic.py``.
For reference, we show the diagnostic code in the dropdown box below.
There are four main sections in the script:

- A description i.e. the ``docstring`` (line 1).
- Import statements (line 2-12).
- Functions that implement our analysis (line 17-99).
- A typical python top-level script (line 102-105).

> ## diagnostic.py
>
>~~~python
>  1   """Python example diagnostic."""
>  2   import logging
>  3   import os
>  4   from pprint import pformat
>  5
>  6   import iris
>  7
>  8   from esmvaltool.diag_scripts.shared import (group_metadata, run_diagnostic,
>  9                                               select_metadata, sorted_metadata)
> 10   from esmvaltool.diag_scripts.shared._base import (
> 11       ProvenanceLogger, get_diagnostic_filename, get_plot_filename)
> 12   from esmvaltool.diag_scripts.shared.plot import quickplot
> 13
> 14   logger = logging.getLogger(os.path.basename(__file__))
> 15
> 16
> 17   def get_provenance_record(attributes, ancestor_files):
> 18       """Create a provenance record describing the diagnostic data and plot."""
> 19       caption = ("Average {long_name} between {start_year} and {end_year} "
> 20                  "according to {dataset}.".format(**attributes))
> 21
> 22       record = {
> 23           'caption': caption,
> 24           'statistics': ['mean'],
> 25           'domains': ['global'],
> 26           'plot_types': ['zonal'],
> 27           'authors': [
> 28               'andela_bouwe',
> 29               'righi_mattia',
> 30           ],
> 31           'references': [
> 32               'acknow_project',
> 33           ],
> 34           'ancestors': ancestor_files,
> 35       }
> 36       return record
> 37
> 38
> 39   def compute_diagnostic(filename):
> 40       """Compute an example diagnostic."""
> 41       logger.debug("Loading %s", filename)
> 42       cube = iris.load_cube(filename)
> 43
> 44       logger.debug("Running example computation")
> 45       return cube.collapsed('time', iris.analysis.MEAN)
> 46
> 47
> 48   def plot_diagnostic(cube, basename, provenance_record, cfg):
> 49       """Create diagnostic data and plot it."""
> 50       diagnostic_file = get_diagnostic_filename(basename, cfg)
> 51
> 52       logger.info("Saving analysis results to %s", diagnostic_file)
> 53       iris.save(cube, target=diagnostic_file)
> 54
> 55       if cfg['write_plots'] and cfg.get('quickplot'):
> 56           plot_file = get_plot_filename(basename, cfg)
> 57           logger.info("Plotting analysis results to %s", plot_file)
> 58           provenance_record['plot_file'] = plot_file
> 59           quickplot(cube, filename=plot_file, **cfg['quickplot'])
> 60
> 61       logger.info("Recording provenance of %s:\n%s", diagnostic_file,
> 62                   pformat(provenance_record))
> 63       with ProvenanceLogger(cfg) as provenance_logger:
> 64           provenance_logger.log(diagnostic_file, provenance_record)
> 65
> 66
> 67   def main(cfg):
> 68       """Compute the time average for each input dataset."""
> 69       # Get a description of the preprocessed data that we will use as input.
> 70       input_data = cfg['input_data'].values()
> 71
> 72       # Demonstrate use of metadata access convenience functions.
> 73       selection = select_metadata(input_data, short_name='pr', project='CMIP5')
> 74       logger.info("Example of how to select only CMIP5 precipitation data:\n%s",
> 75                   pformat(selection))
> 76
> 77       selection = sorted_metadata(selection, sort='dataset')
> 78       logger.info("Example of how to sort this selection by dataset:\n%s",
> 79                   pformat(selection))
> 80
> 81       grouped_input_data = group_metadata(
> 82           input_data, 'standard_name', sort='dataset')
> 83       logger.info(
> 84           "Example of how to group and sort input data by standard_name:"
> 85           "\n%s", pformat(grouped_input_data))
> 86
> 87       # Example of how to loop over variables/datasets in alphabetical order
> 88       for standard_name in grouped_input_data:
> 89           logger.info("Processing variable %s", standard_name)
> 90           for attributes in grouped_input_data[standard_name]:
> 91               logger.info("Processing dataset %s", attributes['dataset'])
> 92               input_file = attributes['filename']
> 93               cube = compute_diagnostic(input_file)
> 94
> 95               output_basename = os.path.splitext(
> 96                   os.path.basename(input_file))[0] + '_mean'
> 97               provenance_record = get_provenance_record(
> 98                   attributes, ancestor_files=[input_file])
> 99               plot_diagnostic(cube, output_basename, provenance_record, cfg)
>100
>101
>102   if __name__ == '__main__':
>103
>104       with run_diagnostic() as config:
>105           main(config)
>
>~~~
>
{:.solution}

> ## What is the starting point of the diagnostic?
>
> 1. Can you spot a function called ``main`` in the code above?
> 2. What is its input arguments?
> 3. How many times is this function mentioned?
>
>> ## Answer
>>
>> 1. The ``main`` function is defined in line 67 as ``main(cfg)``.
>> 2. The variable ``cfg`` is a Python dictionary holding all the necessary
>> information needed to run the diagnostic script like the location of input
>> data and various settings. In the ``main`` function, we will next parse this
>> ``cfg`` variable and extract information as needed to do our analyses (e.g. in line 70).
>> 3. The ``main`` function called near the very end on line 105.
> {: .solution}
{: .challenge}

> ## The function run_diagnostic
>
> The function ``run_diagnostic`` (line 104) is called a context manager
> provided with ESMValTool and is the main entry point for most Python
> diagnostics.
>
{: .callout}

## Preprocesor-diagnostic interface

In the previous exercise, we have seen that the variable ``cfg`` is the input
argument of the ``main`` function. The first thing passed to the diagnostic
via the ``cfg`` dictionary is a path to a file called ``settings.yml``.
The ESMValTool documentation page provides an overview of what is in this file, see
[Diagnostic script interfaces][interface].

> ## What information do I need when writing a diagnostic script?
>
> From the lesson [Configuration]({{ page.root }}{% link _episodes/03-configuration.md %}),
> we saw how to change the configurations before running a recipe.
> Let's first set the option ``remove_preproc_dir`` to ``false`` in the configuration file,
> then run the recipe ``recipe_python.yml``:
>
> ~~~bash
> esmvaltool run recipe_example.yml
> ~~~
>
> 1. Find one example of the file ``settings.yml`` in the ``run`` directory?
> 2. Take a look at the ``input_files`` list. It contains pathes to some files
>    ``metadata.yml``. What information do you think is saved in those files?
>
>> ## Answer
>>
>> 1. One example of ``settings.yml`` can be found in the directory:
>> *path_to_recipe_output/run/map/script1/settings.yml*
>> 2. The ``metadata.yml`` files hold information
>> about the preprocessed data. There is one file for each variable having
>> detailed information on your data including project (e.g., CMIP6, CMIP5),
>> dataset names (e.g., BCC-ESM1, CanESM2), variable attributes (e.g.,
>> standard_name, units), preprocessor applied and time range of the data. You
>> can use all of these information in your own diagnostics.
> >
> >
> {: .solution}
{: .challenge}

## Diagnostic shared functions

Looking at the codes of the ``diagnostic.py``, we see that ``input_data`` is
read from the ``cfg`` dictionary (line 70). Now we can group the ``input_data``
according to some criteria such as the model or experiment. To do so,
ESMValTool provides many functions like ``select_metadata`` (line 73),
``sorted_metadata`` (line 77), and ``group_metadata`` (line 81). As you can see
in line 8, these functions are imported from ``esmvaltool.diag_scripts.shared``
that means these are shared between several diagnostics scripts. A list of
available functions and their description can be found in [Shared diagnostic
script code][shared].

> ## Extracting information needed for analyses
>
> We have seen the functions used for selecting, sorting and grouping data in the
> script. What these functions do?
>
>> ## Answer
>>
>> There is a statement after use of ``select_metadata``, ``sorted_metadata``
>> and ``group_metadata`` that starts with ``logger.info`` (lines 74, 78 and
>> 83). These lines print output to the log files. In the previous exercise, we
>> ran the recipe ``recipe_python.yml``. If you looked at the content of the log
>> file ``path_to_recipe_output/run/map/script1/log.txt``, you can see the some
>> information on how each function works, for example:
>>
>>```
>>2021-03-05 13:19:38,184 [34706] INFO     diagnostic,83  Example of how to group and
>>sort input data by variable groups from the recipe:
>>{'tas': [{'activity': 'CMIP',
>>         'alias': 'CMIP6',
>>         'dataset': 'BCC-ESM1',
>>         'diagnostic': 'map',
>>         'end_year': 2000,
>>         'ensemble': 'r1i1p1f1',
>>         'exp': 'historical',
>>         'filename': '~/recipe_python_20210305_131929/preproc/map/tas/CMIP6_BCC-ESM1_Amon_historical_r1i1p1f1_tas_2000-2000.nc',
>>         'frequency': 'mon',
>>         'grid': 'gn',
>>         'institute': ['BCC'],
>>         'long_name': 'Near-Surface Air Temperature',
>>         'mip': 'Amon',
>>         'modeling_realm': ['atmos'],
>>         'preprocessor': 'select_january',
>>         'project': 'CMIP6',
>>         'recipe_dataset_index': 0,
>>         'short_name': 'tas',
>>         'standard_name': 'air_temperature',
>>         'start_year': 2000,
>>         'units': 'K',
>>         'variable_group': 'tas'},
>>        {'alias': 'CMIP5',
>>         'dataset': 'CanESM2',
>>         'diagnostic': 'map',
>>         'end_year': 2000,
>>         'ensemble': 'r1i1p1',
>>         'exp': 'historical',
>>         'filename': '~/recipe_python_20210305_131929/preproc/map/tas/CMIP5_CanESM2_Amon_historical_r1i1p1_tas_2000-2000.nc',
>>         'frequency': 'mon',
>>         'institute': ['CCCma'],
>>         'long_name': 'Near-Surface Air Temperature',
>>         'mip': 'Amon',
>>         'modeling_realm': ['atmos'],
>>         'preprocessor': 'select_january',
>>         'project': 'CMIP5',
>>         'recipe_dataset_index': 1,
>>         'short_name': 'tas',
>>         'standard_name': 'air_temperature',
>>         'start_year': 2000,
>>         'units': 'K',
>>         'variable_group': 'tas'}]}
>>```
>>
>> This is how we can access preprocessed data within our diagnostic.
> {: .solution}
{: .challenge}

## Diagnostic computation

After grouping and selecting data, we can read individual attributes such as the
filename by looping over variables (line 88-92). Following this, we see the use
of the function ``compute_diagnostic`` (line 93). Let's have a look at the
defenition of this function at line 39 where the analyses on the data is done.

Here, ``compute_diagnostic`` uses
[Iris](https://scitools-iris.readthedocs.io/en/latest/index.html) to read data
from a netCDF file and performs a simple computation of averaging over time
dimension. We can adapt this function adding our own analysis.
As an example, here we want to calculate maximum of the data as:

~~~python
def compute_diagnostic(filename):
    """Compute an example diagnostic."""
    logger.debug("Loading %s", filename)
    cube = iris.load_cube(filenam

    logger.debug("Running example computation")
    cube.collapsed('time', iris.analysis.MEAN)
    return cube.data.max()
~~~

> ## iris cubes
>
> Iris reads data into data structures called
> [cubes](https://scitools-iris.readthedocs.io/en/latest/userguide/iris_cubes.html).
> The data in these cubes can be modified, combined with other cubes' data or
> plotted.
{: .callout}

> ## Reading data using xarray
>
> Use the [xarrays](http://xarray.pydata.org/en/stable/) to read the data
> instead of iris.
>
>> ## Answer
>>
>> First, import [xarray] package at the top of the script as:
>>
>>~~~python
>>import xarray as xr
>>~~~
>>
>> Then, change the ``compute_diagnostic`` as:
>>
>>~~~python
>>def compute_diagnostic(filename):
>>    """Compute an example diagnostic."""
>>    logger.debug("Loading %s", filename)
>>    dataset = xr.open_dataset(filename)
>>
>>    #do your analyses on the data here
>>
>>    return dataset
>>~~~
>>
> {: .solution}
{: .challenge}

> ## Reading data using Scipy's netCDF library
>
> Use the [SciPy's netCDF library][netCDF] to read the data instead of iris.
>
>> ## Answer
>>
>> First, import [netcdfx] package at the top of the script as:
>>
>>~~~python
>>from scipy.io import netcdfx
>>~~~
>>
>> Then, change the ``compute_diagnostic`` as:
>>
>>~~~python
>>def compute_diagnostic(filename):
>>    """Compute an example diagnostic."""
>>    logger.debug("Loading %s", filename)
>>    netcdf_file = netcdf.netcdf_file(filename,'r')
>>
>>    #do your analyses on the data here
>>
>>    return netcdf_file
>>~~~
>>
> {: .solution}
{: .challenge}

## Plotting Diagnostic Output

Often, the end product of a diagnostic script is a plot or figure. ESMValTool
makes it possible to produce a wide array of such figures as seen in the
[gallery](https://docs.esmvaltool.org/en/latest/gallery.html). In this example
we use Iris cubes for processing the netCDF data. The Iris cube returned from
the *compute_diagnostic* function (line 93) is passed to the *plot_diagnostic*
function (line 99). You could return an xarray data object for example and pass
that on to the plotting function. The *plot_diagnostic* function is where you
would plug in your plotting routine in this example.

More specifically, the *quickplot* function (line 59) can be replaced with the
function of your choice. The lines preceding this function are to save the Iris
cube object and to save the Provenance of the file. Again, you may choose your
own method of saving your diagnostic object. More information on how to record
Provenance is available
[here](https://docs.esmvaltool.org/en/latest/community/diagnostic.html?highlight=provenance#recording-provenance).

> ## Passing arguments to the diagnostic from the recipe
>
> How can you pass a user defined argument to your diagnostic ? If you wanted to
> plot a colormesh plot with a particular colormap, how would you do so?
>
>> ## Answer
>>
>> ```yaml
>>     script1:
>>       script: examples/diagnostic.py
>>        quickplot:
>>          plot_type: pcolormesh
>>          cmap: Reds
>>```
>>
>> The lines below `script:examples/diagnostic.py` have pairs of arguments and
>> values that are passed on to the diagnostic script. In the case of the
>> *quickplot* argument, we can further pass arguments for *quickplot* such as
>> the type of plot *pcolormesh* and the colormap with keyword *cmap* as
>> `cmap:Reds`. In line 59 of the diagnostic, we access this argument. Look at
>> other recipes and diagnostics for more examples of user defined arguments.
>>
> {: .solution}
{: .challenge}

{% include links.md %}

[recipe]: https://github.com/ESMValGroup/ESMValTool/blob/master/esmvaltool/recipes/examples/recipe_python.yml
[diagnostic]: https://github.com/ESMValGroup/ESMValTool/blob/master/esmvaltool/diag_scripts/examples/diagnostic.py
[interface]: https://docs.esmvaltool.org/projects/esmvalcore/en/latest/interfaces.html
[shared]: https://docs.esmvaltool.org/en/latest/api/esmvaltool.diag_scripts.shared.html
[netCDF]: https://docs.scipy.org/doc/scipy-0.14.0/reference/generated/scipy.io.netcdf.netcdf_file.html
