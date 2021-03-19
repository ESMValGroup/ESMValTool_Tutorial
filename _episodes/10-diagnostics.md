---
title: "Writing your own diagnostic script"
teaching: 20
exercises: 30

questions:
- "How do I write a new diagnostic in ESMValTool?"
- "How do I use the preprocessor output in a Python diagnostic?"

objectives:
- "Write a new Python diagnostic script."
- "Explain how a diagnostic script reads the preprocessor output."

keypoints:
- "ESMValTool provides helper functions to interface a Python diagnostic script
  with preprocessor output."
- "Existing diagnostics can be used as templates and modified to write new
  diagnostics."
- "Helper functions can be imported from ``esmvaltool.diag_scripts.shared`` and
  used in your own diagnostic script."
---

## Introduction

The diagnostic script is an important component of ESMValTool and it is where the
scientific analysis or performance metric is implemented. With ESMValTool, you
can adapt an existing diagnostic or write a new script from scratch.
Diagnostics can be written in a number of open source
languages such as Python, R, Julia and NCL but we will focus on understanding
and writing Python diagnostics in this lesson.

In this lesson, we will explain how to find an existing diagnostic and run it
using ESMValTool installed in editable/development mode. For a development
installation, see the instructions in the lesson [Development and
contribution]({{ page.root }}{% link _episodes/08-development-setup.md %}).
Also, we will work with the recipe [recipe_python.yml][recipe] and the
diagnostic script [diagnostic.py][diagnostic] called by this recipe that we have
seen in the lesson [Running your first recipe]({{ page.root }}{% link
_episodes/04-recipe.md %}).

Let's get started!

## Understanding an existing Python diagnostic

After a development mode installation, a folder called ``ESMValTool`` is created
in your working directory. This folder contains the source code of the tool. We
can find the recipe ``recipe_python.yml`` and the python script
``diagnostic.py`` in these directories:

- *path_to_ESMValTool/esmvaltool/recipes/examples/recipe_python.yml*
- *path_to_ESMValTool/esmvaltool/diag_scripts/examples/diagnostic.py*

Let's have look at the code in  ``diagnostic.py``.
For reference, we show the diagnostic code in the dropdown box below.
There are four main sections in the script:

- A description i.e. the ``docstring`` (line 1).
- Import statements (line 2-16).
- Functions that implement our analysis (line 21-101).
- A typical Python top-level script i.e. ``if __name__ == '__main__'`` (line
  102-107).

> ## diagnostic.py
>
>~~~python
>  1:  """Python example diagnostic."""
>  2:  import logging
>  3:  from pathlib import Path
>  4:  from pprint import pformat
>  5:
>  6:  import iris
>  7:
>  8:  from esmvaltool.diag_scripts.shared import (
>  9:      group_metadata,
> 10:      run_diagnostic,
> 11:      save_data,
> 12:      save_figure,
> 13:      select_metadata,
> 14:      sorted_metadata,
> 15:  )
> 16:  from esmvaltool.diag_scripts.shared.plot import quickplot
> 17:
> 18:  logger = logging.getLogger(Path(__file__).stem)
> 19:
> 20:
> 21:  def get_provenance_record(attributes, ancestor_files):
> 22:      """Create a provenance record describing the diagnostic data and plot."""
> 23:      caption = ("Average {long_name} between {start_year} and {end_year} "
> 24:                 "according to {dataset}.".format(**attributes))
> 25:
> 26:      record = {
> 27:          'caption': caption,
> 28:          'statistics': ['mean'],
> 29:          'domains': ['global'],
> 30:          'plot_types': ['zonal'],
> 31:          'authors': [
> 32:              'andela_bouwe',
> 33:              'righi_mattia',
> 34:          ],
> 35:          'references': [
> 36:              'acknow_project',
> 37:          ],
> 38:          'ancestors': ancestor_files,
> 39:      }
> 40:      return record
> 41:
> 42:
> 43:  def compute_diagnostic(filename):
> 44:      """Compute an example diagnostic."""
> 45:      logger.debug("Loading %s", filename)
> 46:      cube = iris.load_cube(filename)
> 47:
> 48:      logger.debug("Running example computation")
> 49:      cube = iris.util.squeeze(cube)
> 50:      return cube
> 51:
> 52:
> 53:  def plot_diagnostic(cube, basename, provenance_record, cfg):
> 54:      """Create diagnostic data and plot it."""
> 55:
> 56:      # Save the data used for the plot
> 57:      save_data(basename, provenance_record, cfg, cube)
> 58:
> 59:      if cfg.get('quickplot'):
> 60:          # Create the plot
> 61:          quickplot(cube, **cfg['quickplot'])
> 62:          # And save the plot
> 63:          save_figure(basename, provenance_record, cfg)
> 64:
> 65:
> 66:  def main(cfg):
> 67:      """Compute the time average for each input dataset."""
> 68:      # Get a description of the preprocessed data that we will use as input.
> 69:      input_data = cfg['input_data'].values()
> 70:
> 71:      # Demonstrate use of metadata access convenience functions.
> 72:      selection = select_metadata(input_data, short_name='tas', project='CMIP5')
> 73:      logger.info("Example of how to select only CMIP5 temperature data:\n%s",
> 74:                  pformat(selection))
> 75:
> 76:      selection = sorted_metadata(selection, sort='dataset')
> 77:      logger.info("Example of how to sort this selection by dataset:\n%s",
> 78:                  pformat(selection))
> 79:
> 80:      grouped_input_data = group_metadata(input_data,
> 81:                                          'variable_group',
> 82:                                          sort='dataset')
> 83:      logger.info(
> 84:          "Example of how to group and sort input data by variable groups from "
> 85:          "the recipe:\n%s", pformat(grouped_input_data))
> 86:
> 87:      # Example of how to loop over variables/datasets in alphabetical order
> 88:      groups = group_metadata(input_data, 'variable_group', sort='dataset')
> 89:      for group_name in groups:
> 90:          logger.info("Processing variable %s", group_name)
> 91:          for attributes in groups[group_name]:
> 92:              logger.info("Processing dataset %s", attributes['dataset'])
> 93:              input_file = attributes['filename']
> 94:              cube = compute_diagnostic(input_file)
> 95:
> 96:              output_basename = Path(input_file).stem
> 97:              if group_name != attributes['short_name']:
> 98:                  output_basename = group_name + '_' + output_basename
> 99:              provenance_record = get_provenance_record(
>100:                  attributes, ancestor_files=[input_file])
>101:              plot_diagnostic(cube, output_basename, provenance_record, cfg)
>102:
>103:
>104:  if __name__ == '__main__':
>105:
>106:      with run_diagnostic() as config:
>107:          main(config)
>108:
>~~~
>
{:.solution}

> ## What is the starting point of a diagnostic?
>
> 1. Can you spot a function called ``main`` in the code above?
> 2. What are its input arguments?
> 3. How many times is this function mentioned?
>
>> ## Answer
>>
>> 1. The ``main`` function is defined in line 66 as ``main(cfg)``.
>> 2. The input argument to this function is the variable ``cfg``, a Python dictionary
>> that holds all the necessary
>> information needed to run the diagnostic script such as the location of input
>> data and various settings. We will next parse this ``cfg`` variable
>> in the  ``main`` function and extract information as needed 
>> to do our analyses (e.g. in line 69).
>> 3. The ``main`` function is called near the very end on line 107. So, it is mentioned
>> twice in our code - once where it is called by the top-level Python script and 
>> second where it is defined.
> {: .solution}
{: .challenge}

> ## The function run_diagnostic
>
> The function ``run_diagnostic`` (line 106) is called a context manager
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
> we saw how to change the configuration settings  before running a recipe.
> First we set the option ``remove_preproc_dir`` to ``false`` in the configuration file,
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
>> can use all of this information in your own diagnostic.
> >
> >
> {: .solution}
{: .challenge}

## Diagnostic shared functions

Looking at the code in  ``diagnostic.py``, we see that ``input_data`` is
read from the ``cfg`` dictionary (line 69). Now we can group the ``input_data``
according to some criteria such as the model or experiment. To do so,
ESMValTool provides many functions such as ``select_metadata`` (line 72),
``sorted_metadata`` (line 76), and ``group_metadata`` (line 80). As you can see
in line 8, these functions are imported from ``esmvaltool.diag_scripts.shared``
that means these are shared across several diagnostics scripts. A list of
available functions and their description can be found in [The ESMValTool Diagnostic API reference][shared].


> ## Extracting information needed for analyses
>
> We have seen the functions used for selecting, sorting and grouping data in the
> script. What do these functions do?
>
>> ## Answer
>>
>> There is a statement after use of ``select_metadata``, ``sorted_metadata``
>> and ``group_metadata`` that starts with ``logger.info`` (lines 73, 77 and
>> 83). These lines print output to the log files. In the previous exercise, we
>> ran the recipe ``recipe_python.yml``. If you look at the log
>> file ``path_to_recipe_output/run/map/script1/log.txt``, you can see the output
>> from each of these functions, for example:
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

After grouping and selecting data, we can read individual attributes (such as filename) 
of each item. Here we have grouped the input data  by ``variables`` 
so we loop over the variables (line 89-93). Following this, is a call to the 
function ``compute_diagnostic`` (line 94). Let's have a look at the
definition of this function in line 43 where the actual analysis on the data is done.

Note that output from the ESMValCore preprocessor is in the form of NetCDF files.
Here, ``compute_diagnostic`` uses
[Iris](https://scitools-iris.readthedocs.io/en/latest/index.html) to read data
from a netCDF file and performs an operation ``squeeze`` to remove any dimensions
of length one. We can adapt this function to add our own analysis. As an
example, here we calculate the bias using the average of the data using Iris cubes. 

~~~python
def compute_diagnostic(filename):
    """Compute an example diagnostic."""
    logger.debug("Loading %s", filename)
    cube = iris.load_cube(filename)

    logger.debug("Running example computation")
    cube = iris.util.squeeze(cube)

    # Calculate a bias using the average of data
    cube.data = cube.core_data() - cube.data.mean()
    return cube
~~~

> ## iris cubes
>
> Iris reads data from NetCDF files into data structures called
> [cubes](https://scitools-iris.readthedocs.io/en/latest/userguide/iris_cubes.html).
> The data in these cubes can be modified, combined with other cubes' data or
> plotted.
{: .callout}

> ## Reading data using xarray
>
> Alternately, you can use [xarrays](http://xarray.pydata.org/en/stable/) to read the data
> instead of Iris.
>
>> ## Answer
>>
>> First, import ``xarray`` package at the top of the script as:
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
> Yet another option to read the NetCDF file data is to use 
> the [SciPy's netCDF library][netCDF].
>
>> ## Answer
>>
>> First, import the ``netcdfx`` package at the top of the script as:
>>
>>~~~python
>>from scipy.io import netcdfx
>>~~~
>>
>> Then, change ``compute_diagnostic`` as:
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

## Diagnostic output

### Plotting the output

Often, the end product of a diagnostic script is a plot or figure. The Iris cube
returned from the ``compute_diagnostic`` function (line 94) is passed to the
``plot_diagnostic`` function (line 101). Let's have a look at the definition of
this function in line 53. This is where we would plug in our plotting routine in the
diagnostic script.

More specifically, the ``quickplot`` function (line 61) can be replaced with the
function of our choice. As can be seen, this function uses
``**cfg['quickplot']`` as an input argument. If you look at the diagnostic
section in the recipe ``recipe_python.yml``, you see ``quickplot`` is a key
there:

~~~yaml
     script1:
       script: examples/diagnostic.py
        quickplot:
          plot_type: pcolormesh
          cmap: Reds
~~~

This way, we can pass arguments such as the type of
plot ``pcolormesh`` and the colormap ``cmap:Reds`` from the recipe to the 
``quickplot``  function in the diagnostic.

> ## Passing arguments from the recipe to the diagnostic
>
> Change the type of the plot and its colormap and inspect the output figure.
>
>> ## Answer
>>
>> In the recipe ``recipe_python.yml``, you could change ``plot_type`` and ``cmap``.
>> As an example, we choose ``plot_type: pcolor`` and ``cmap: BuGn``:
>>
>> ~~~yaml
>>     script1:
>>       script: examples/diagnostic.py
>>        quickplot:
>>          plot_type: pcolor
>>          cmap: BuGn
>>~~~
>>
>> The plot can be found at *path_to_recipe_output/plots/map/script1/png*.
> {: .solution}
{: .challenge}

> ## ESMValTool gallery
>
> ESMValTool makes it possible to produce a wide array of plots and figures as seen
> in the [gallery](https://docs.esmvaltool.org/en/latest/gallery.html).
{: .callout}

### Saving the output

In our example, the function ``save_data`` in line 57 is used to save the Iris
cube. The saved files can be found under the ``work`` directory in a ``.nc`` format.
There is also the function ``save_figure`` in line 63 to save the plots under the 
``plot`` directory in a ``.png`` format (or preferred format specified in your 
configuration settings). Again, you may choose your own method
of saving the output. 

### Recording the provenance

When developing a diagnostic script, it is good practice to record
provenance. To do so, we use the function ``get_provenance_record`` (line 99).
Let us have a look at the definition of this function in line 21 where we
describe the diagnostic data and plot. Using the dictionary ``record``, it is
possible to add custom provenance to our diagnostics output. 
Provenance is stored in the *W3C PROV XML*
format and also in an *SVG* file under the ``work`` and ``plot`` directory. For
more information, see [recording provenance][provenance].

## Congratulations!

You now know the basic diagnostic script structure and some available tools for putting 
together your own diagnostics. Have a look at existing recipes and diagnostics in the 
repository for more examples of functions you can use in your diagnostics!
{% include links.md %}

[recipe]: https://github.com/ESMValGroup/ESMValTool/blob/master/esmvaltool/recipes/examples/recipe_python.yml
[diagnostic]: https://github.com/ESMValGroup/ESMValTool/blob/master/esmvaltool/diag_scripts/examples/diagnostic.py
[interface]: https://docs.esmvaltool.org/projects/esmvalcore/en/latest/interfaces.html
[shared]: https://docs.esmvaltool.org/en/latest/api/esmvaltool.diag_scripts.shared.html
[netCDF]: https://docs.scipy.org/doc/scipy-0.14.0/reference/generated/scipy.io.netcdf.netcdf_file.html
[provenance]: https://docs.esmvaltool.org/en/latest/community/diagnostic.html?highlight=provenance#recording-provenance
