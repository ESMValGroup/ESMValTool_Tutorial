---
title: "Writing your own diagnostic script"
teaching: 20
exercises: 30
compatibility: ESMValTool v2.14.0

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
languages such as Python, R and NCL but we will focus on understanding
and writing Python diagnostics in this lesson.

In this lesson, we will explain how to find an existing diagnostic and run it
using ESMValTool installed in editable/development mode. For a development
installation, see the instructions in the lesson [Development and
contribution]({{ page.root }}{% link _episodes/07-development-setup.md %}).
Also, we will work with the recipe [recipe_python.yml][recipe] and the
diagnostic script [diagnostic.py][diagnostic] called by this recipe that we have
seen in the lesson [Running your first recipe]({{ page.root }}{% link
_episodes/04-recipe.md %}).

Let's get started!

## Understanding an existing Python diagnostic

If you clone the ESMValTool repository, a folder called ``ESMValTool`` is
created in your home/working directory, see the instructions in the lesson
[Development and contribution]({{ page.root }}{% link
_episodes/07-development-setup.md %}).

The folder ``ESMValTool`` contains the source code of the tool. We can find the
recipe ``recipe_python.yml`` and the python script ``diagnostic.py`` in these
directories:

- *~/ESMValTool/esmvaltool/recipes/examples/recipe_python.yml*
- *~/ESMValTool/esmvaltool/diag_scripts/examples/diagnostic.py*

Let's have look at the code in  ``diagnostic.py``.
For reference, we show the diagnostic code in the dropdown box below.
There are four main sections in the script:

- A description i.e. the ``docstring`` (line 1).
- Import statements (line 2-16).
- Functions that implement our analysis (line 21-102).
- A typical Python top-level script i.e. ``if __name__ == '__main__'`` (line
  105-108).

> ## diagnostic.py
>
>~~~python
> 1:  """Python example diagnostic."""
> 2:  
> 3:  import logging
> 4:  from pathlib import Path
> 5:  from pprint import pformat
> 6:  
> 7:  import iris
> 8:  
> 9:  from esmvaltool.diag_scripts.shared import (
> 10:      group_metadata,
> 11:      run_diagnostic,
> 12:      save_data,
> 13:      save_figure,
> 14:      select_metadata,
> 15:      sorted_metadata,
> 16:  )
> 17:  from esmvaltool.diag_scripts.shared.plot import quickplot
> 18:  
> 19:  logger = logging.getLogger(Path(__file__).stem)
> 20:  
> 21:  
> 22:  def get_provenance_record(attributes, ancestor_files):
> 23:      """Create a provenance record describing the diagnostic data and plot."""
> 24:      # Associated recipe uses contains a caption string with placeholders
> 25:      # like {long_name} that are now populated from attributes dictionary.
> 26:      # Note that for simple recipes, caption can be set here as a simple string
> 27:      caption = attributes["caption"].format(**attributes)
> 28:  
> 29:      record = {
> 30:          "caption": caption,
> 31:          "statistics": ["mean"],
> 32:          "domains": ["global"],
> 33:          "plot_types": ["zonal"],
> 34:          "authors": [
> 35:              "andela_bouwe",
> 36:              "righi_mattia",
> 37:          ],
> 38:          "references": [
> 39:              "acknow_project",
> 40:          ],
> 41:          "ancestors": ancestor_files,
> 42:      }
> 43:      return record
> 44:
> 45:
> 46:  def compute_diagnostic(filename):
> 47:      """Compute an example diagnostic."""
> 48:      logger.debug("Loading %s", filename)
> 49:      cube = iris.load_cube(filename)
> 50:  
> 51:      logger.debug("Running example computation")
> 52:      cube = iris.util.squeeze(cube)
> 53:      return cube
> 54:
> 55:
> 56:  def plot_diagnostic(cube, basename, provenance_record, cfg):
> 57:      """Create diagnostic data and plot it."""
> 58:      # Save the data used for the plot
> 59:      save_data(basename, provenance_record, cfg, cube)
> 60: 
> 61:      if cfg.get("quickplot"):
> 62:          # Create the plot
> 63:          quickplot(cube, **cfg["quickplot"])
> 64:          # And save the plot
> 65:          save_figure(basename, provenance_record, cfg)
> 66: 
> 67:
> 68:  def main(cfg):
> 69:      """Compute the time average for each input dataset."""
> 70:      # Get a description of the preprocessed data that we will use as input.
> 71:      input_data = cfg["input_data"].values()
> 72:  
> 73:      # Demonstrate use of metadata access convenience functions.
> 74:      selection = select_metadata(input_data, short_name="tas", project="CMIP5")
> 75:      logger.info(
> 76:          "Example of how to select only CMIP5 temperature data:\n%s",
> 77:          pformat(selection),
> 78:      )
> 79:  
> 80:      selection = sorted_metadata(selection, sort="dataset")
> 81:      logger.info(
> 82:          "Example of how to sort this selection by dataset:\n%s",
> 83:          pformat(selection),
> 84:      )
> 85:  
> 86:      grouped_input_data = group_metadata(
> 87:          input_data,
> 88:          "variable_group",
> 89:          sort="dataset",
> 90:      )
> 91:      logger.info(
> 92:          "Example of how to group and sort input data by variable groups from "
> 93:          "the recipe:\n%s",
> 94:          pformat(grouped_input_data),
> 95:      )
> 96: 
> 97:      # Example of how to loop over variables/datasets in alphabetical order
> 98:      groups = group_metadata(input_data, "variable_group", sort="dataset")
> 99:      for group_name in groups:
> 100:          logger.info("Processing variable %s", group_name)
> 101:          for attributes in groups[group_name]:
> 102:              logger.info("Processing dataset %s", attributes["dataset"])
> 103:              input_file = attributes["filename"]
> 104:              cube = compute_diagnostic(input_file)
> 105:  
> 106:              output_basename = Path(input_file).stem
> 107:              if group_name != attributes["short_name"]:
> 108:                  output_basename = group_name + "_" + output_basename
> 109:              if "caption" not in attributes:
> 110:                  attributes["caption"] = input_file
> 111:              provenance_record = get_provenance_record(
> 112:                  attributes,
> 113:                  ancestor_files=[input_file],
> 114:             )
> 115:             plot_diagnostic(cube, output_basename, provenance_record, cfg)
> 116: 
> 117: 
> 118:  if __name__ == "__main__":
> 119:      with run_diagnostic() as config:
> 120:          main(config)
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
>> 1. The ``main`` function is defined in line 68 as ``main(cfg)``.
>> 2. The input argument to this function is the variable ``cfg``, a Python dictionary
>> that holds all the necessary
>> information needed to run the diagnostic script such as the location of input
>> data and various settings. We will next parse this ``cfg`` variable
>> in the  ``main`` function and extract information as needed
>> to do our analyses (e.g. in line 71).
>> 3. The ``main`` function is called near the very end on line 120. So, it is mentioned
>> twice in our code - once where it is called by the top-level Python script and
>> second where it is defined.
> {: .solution}
{: .challenge}

> ## The function run_diagnostic
>
> The function ``run_diagnostic`` (line 119) is called a context manager
> provided with ESMValTool and is the main entry point for most Python
> diagnostics.
>
{: .callout}

## Preprocessor-diagnostic interface

In the previous exercise, we have seen that the variable ``cfg`` is the input
argument of the ``main`` function. The first argument passed to the diagnostic
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
> esmvaltool run examples/recipe_python.yml
> ~~~
>
> 1. Find one example of the file ``settings.yml`` in the ``run`` directory?
> 2. Open the file ``settings.yml`` and look at the ``input_files`` list.
>    It contains paths to some files ``metadata.yml``. What information do you
>    think is saved in those files?
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
read from the ``cfg`` dictionary (line 71). Now we can group the ``input_data``
according to some criteria such as the model or experiment. To do so,
ESMValTool provides many functions such as ``select_metadata`` (line 74),
``sorted_metadata`` (line 80), and ``group_metadata`` (line 86). As you can see
in line 9, these functions are imported from ``esmvaltool.diag_scripts.shared``
that means these are shared across several diagnostics scripts. A list of
available functions and their description can be found in
[The ESMValTool Diagnostic API reference][shared].


> ## Extracting information needed for analyses
>
> We have seen the functions used for selecting, sorting and grouping data in the
> script. What do these functions do?
>
>> ## Answer
>>
>> There is a statement after use of ``select_metadata``, ``sorted_metadata``
>> and ``group_metadata`` that starts with ``logger.info`` (lines 75, 81 and
>> 91). These lines print output to the log files. In the previous exercise, we
>> ran the recipe ``recipe_python.yml``. If you look at the log file
>> ``recipe_python_#_#/run/map/script1/log.txt`` in the ``esmvaltool_output``
>> directory, you can see the output from each of these functions. An example 
>> of how the ``group_metadata`` function groups by variable groups:
>>
>>```
>> INFO:diagnostic:Example of how to group and sort input data by variable groups from the recipe:
>> {'tas': [{'alias': 'CMIP5',
>>           'caption': 'Global map of {long_name} in January 2000 according to '
>>                      '{dataset}.\n',
>>           'dataset': 'bcc-csm1-1',
>>           'diagnostic': 'map',
>>           'end_year': 2000,
>>           'ensemble': 'r1i1p1',
>>           'exp': 'historical',
>>           'filename': '/path/recipe_python_#_#/preproc/map/tas/
>> CMIP5_bcc-csm1-1_Amon_historical_r1i1p1_tas_20000101-20000201.nc',
>>           'frequency': 'mon',
>>           'institute': ['BCC'],
>>           'long_name': 'Near-Surface Air Temperature',
>>           'mip': 'Amon',
>>           'modeling_realm': ['atmos'],
>>           'preprocessor': 'to_degrees_c',
>>           'product': ['output1', 'output2'],
>>           'project': 'CMIP5',
>>           'recipe_dataset_index': 1,
>>           'short_name': 'tas',
>>           'standard_name': 'air_temperature',
>>           'start_year': 2000,
>>           'timerange': '2000/P1M',
>>           'units': 'degrees_C',
>>           'variable_group': 'tas',
>>           'version': 'v1'},
>>          {'activity': 'CMIP',
>>           'alias': 'CMIP6',
>>           'caption': 'Global map of {long_name} in January 2000 according to '
>>                      '{dataset}.\n',
>>           'dataset': 'BCC-ESM1',
>>           'diagnostic': 'map',
>>           'end_year': 2000,
>>           'ensemble': 'r1i1p1f1',
>>           'exp': 'historical',
>>           'filename': '/path/recipe_python_#_#/preproc/map/tas/
>> CMIP6_BCC-ESM1_Amon_historical_r1i1p1f1_tas_gn_20000101-20000201.nc',
>>           'frequency': 'mon',
>>           'grid': 'gn',
>>           'institute': ['BCC'],
>>           'long_name': 'Near-Surface Air Temperature',
>>           'mip': 'Amon',
>>           'modeling_realm': ['atmos'],
>>           'preprocessor': 'to_degrees_c',
>>           'project': 'CMIP6',
>>           'recipe_dataset_index': 0,
>>           'short_name': 'tas',
>>           'standard_name': 'air_temperature',
>>           'start_year': 2000,
>>           'timerange': '2000/P1M',
>>           'units': 'degrees_C',
>>           'variable_group': 'tas',
>>           'version': 'v20181214'}]}
>> 
>>```
>>
>> This is how we can access preprocessed data within our diagnostic.
> {: .solution}
{: .challenge}

## Diagnostic computation

After grouping and selecting data, we can read individual attributes (such as filename)
of each item. Here, we have grouped the input data  by ``variables``, 
so we loop over the variables (line 99). Following this is a call to the
function ``compute_diagnostic`` (line 104). Let's look at the
definition of this function in line 46, where the actual analysis of the data is done.

Note that output from the ESMValCore preprocessor is in the form of NetCDF files.
Here, ``compute_diagnostic`` uses
[Iris](https://scitools-iris.readthedocs.io/en/latest/index.html) to read data
from a netCDF file and performs an operation ``squeeze`` to remove any dimensions
of length one. We can adapt this function to add our own analysis. As an example,
here we calculate the bias using the average of the data using Iris cubes. Note that
this is not in the original diagnostic but a suggestion for how to  get 
started adding code to the existing diagnostic.

~~~python
def compute_diagnostic(filename):
    """Compute an example diagnostic."""
    logger.debug("Loading %s", filename)
    cube = iris.load_cube(filename)

    logger.debug("Running example computation")
    cube = iris.util.squeeze(cube)

    # Calculate a bias using the average of data
    cube.data = cube.core_data() - cube.core_data.mean()
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
>> Caution: If you read data using xarray keep in mind to change accordingly
>> the other functions in the diagnostic which are dealing at the moment with
>> Iris cubes.
>>
> {: .solution}
{: .challenge}

> ## Reading data using the netCDF4 package
>
> Yet another option to read the NetCDF file data is to use
> the [netCDF-4 Python interface][netCDF] to the netCDF C library.
>
>> ## Answer
>>
>> First, import the ``netCDF4`` package at the top of the script as:
>>
>>~~~python
>>import netCDF4
>>~~~
>>
>> Then, change ``compute_diagnostic`` as:
>>
>>~~~python
>>def compute_diagnostic(filename):
>>    """Compute an example diagnostic."""
>>    logger.debug("Loading %s", filename)
>>    nc_data = netCDF4.Dataset(filename,'r')
>>
>>    #do your analyses on the data here
>>
>>    return nc_data
>>~~~
>>
>> Caution: If you read data using netCDF4 keep in mind to change accordingly
>> the other functions in the diagnostic which are dealing at the moment with
>> Iris cubes.
>>
> {: .solution}
{: .challenge}

## Diagnostic output

### Plotting the output

Often, the end product of a diagnostic script is a plot or figure. The Iris cube
returned from the ``compute_diagnostic`` function (line 104) is passed to the
``plot_diagnostic`` function (line 115). Let's have a look at the definition of
this function in line 56. This is where we would plug in our plotting routine in the
diagnostic script.

More specifically, the ``quickplot`` function (line 63) can be replaced with the
function of our choice. As can be seen, this function uses
``**cfg['quickplot']`` as an input argument. If you look at the diagnostic
section in the recipe ``recipe_python.yml``, you see ``quickplot`` is a key
there:

```yaml
     script1:
       script: examples/diagnostic.py
        quickplot:
          plot_type: pcolormesh
          cmap: Reds
```

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
>> ```yaml
>>     script1:
>>       script: examples/diagnostic.py
>>        quickplot:
>>          plot_type: pcolor
>>          cmap: BuGn
>>```
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

In our example, the function ``save_data`` in line 59 is used to save the Iris
cube. The saved files can be found under the ``work`` directory in a ``.nc`` format.
There is also the function ``save_figure`` in line 65 to save the plots under the
``plot`` directory in a ``.png`` format (or preferred format specified in your
configuration settings). Again, you may choose your own method
of saving the output.

### Recording the provenance

When developing a diagnostic script, it is good practice to record
provenance. To do so, we use the function ``get_provenance_record`` (line 111).
Let us have a look at the definition of this function in line 22 where we
describe the diagnostic data and plot. Using the dictionary ``record``, it is
possible to add custom provenance to our diagnostics output.
Provenance is stored in the *[W3C PROV XML](https://www.w3.org/TR/prov-xml/)*
format and also in an *SVG* file under the ``work`` and ``plot`` directory. For
more information, see [recording provenance][provenance].

## Congratulations!

You now know the basic diagnostic script structure and some available tools for putting
together your own diagnostics. Have a look at existing recipes and diagnostics in the
repository for more examples of functions you can use in your diagnostics!
{% include links.md %}
