---
title: "Writing your own diagnostic script"
teaching: TBD
exercises: TBD

questions:
- "How do I write a new diagnostic in ESMValTool?"
- "How do I read preprocessor output in a Python diagnostic?"


objectives:
- "Writing a new Python diagnostic"
- "Understanding the interface between the ESMValCore preprocessor and a diagnostic script."

keypoints:
- "Take home message 1"
- "etc ..."
---

## Introduction

The diagnostic script is an important component of ESMValTool where the
scientific analysis or performance metric is implemented. With ESMValTool, you 
can reuse an existing diagnostic, adapt and existing one for your needs or
write your own new diagnostic.  Diagnostics can be written in a number of open 
source languages such as Python, R, Julia or NCL but we will focus on understanding 
and writing Python diagnostics in this lesson. In order to access existing diagnostics or
to write your own, please install ESMValTool in the development mode on your 
machine using the instructions from [here](https://esmvalgroup.github.io/ESMValTool_Tutorial/08-development-setup/index.html).

## Understanding an existing Python diagnostic
We revisit a recipe we have seen before, [recipe_python.yml](https://github.com/ESMValGroup/ESMValTool/blob/master/esmvaltool/recipes/examples/recipe_python.yml) and the diagnostic script called by this recipe -- [diag_scripts/examples/diagnostic.py](https://github.com/ESMValGroup/ESMValTool/blob/master/esmvaltool/diag_scripts/examples/diagnostic.py). For reference, we have the diagnostic file in the dropdown box below.

> ## diagnostic.py
>
>~~~
>"""Python example diagnostic."""
>import logging
>from pathlib import Path
>from pprint import pformat
>
>import iris
>
>from esmvaltool.diag_scripts.shared import (
>    group_metadata,
>    run_diagnostic,
>    save_data,
>    save_figure,
>    select_metadata,
>    sorted_metadata,
>)
>from esmvaltool.diag_scripts.shared.plot import quickplot
>
>logger = logging.getLogger(Path(__file__).stem)
>
>
>def get_provenance_record(attributes, ancestor_files):
>    """Create a provenance record describing the diagnostic data and plot."""
>    caption = ("Average {long_name} between {start_year} and {end_year} "
>               "according to {dataset}.".format(**attributes))
>
>    record = {
>       'caption': caption,
>        'statistics': ['mean'],
>        'domains': ['global'],
>        'plot_types': ['zonal'],
>        'authors': [
>            'andela_bouwe',
>            'righi_mattia',
>
>
>       ],
>        'references': [
>           'acknow_project',
>        ],
>        'ancestors': ancestor_files,
>    }
>    return record
>
>
>def compute_diagnostic(filename):
>    """Compute an example diagnostic."""
>    logger.debug("Loading %s", filename)
>    cube = iris.load_cube(filename)
>
>    logger.debug("Running example computation")
>    cube = iris.util.squeeze(cube)
>    return cube
>
>
>def plot_diagnostic(cube, basename, provenance_record, cfg):
>    """Create diagnostic data and plot it."""
>
>    # Save the data used for the plot
>    save_data(basename, provenance_record, cfg, cube)
>
>    if cfg.get('quickplot'):
>        # Create the plot
>        quickplot(cube, **cfg['quickplot'])
>        # And save the plot
>        save_figure(basename, provenance_record, cfg)
>
>
>def main(cfg):
>    """Compute the time average for each input dataset."""
>    # Get a description of the preprocessed data that we will use as input.
>    input_data = cfg['input_data'].values()
>
>    # Demonstrate use of metadata access convenience functions.
>    selection = select_metadata(input_data, short_name='tas', project='CMIP5')
>    logger.info("Example of how to select only CMIP5 temperature data:\n%s",
>                pformat(selection))
>
>    selection = sorted_metadata(selection, sort='dataset')
>    logger.info("Example of how to sort this selection by dataset:\n%s",
>                pformat(selection))
>
>    grouped_input_data = group_metadata(input_data,
>                                        'variable_group',
>                                        sort='dataset')
>    logger.info(
>        "Example of how to group and sort input data by variable groups from "
>        "the recipe:\n%s", pformat(grouped_input_data))
>
>    # Example of how to loop over variables/datasets in alphabetical order
>    groups = group_metadata(input_data, 'variable_group', sort='dataset')
>    for group_name in groups:
>        logger.info("Processing variable %s", group_name)
>        for attributes in groups[group_name]:
>            logger.info("Processing dataset %s", attributes['dataset'])
>            input_file = attributes['filename']
>            cube = compute_diagnostic(input_file)
>
>            output_basename = Path(input_file).stem
>            if group_name != attributes['short_name']:
>                output_basename = group_name + '_' + output_basename
>            provenance_record = get_provenance_record(
>                attributes, ancestor_files=[input_file])
>            plot_diagnostic(cube, output_basename, provenance_record, cfg)
>
>
>if __name__ == '__main__':
>
>    with run_diagnostic() as config:
>        main(config)
>
>~~~
>{: .language-python}
>
{:.solution}

> ## What is the starting point of the diagnostic?
>
> Can you spot the main function in the Python code above? How many references 
>do you see to this function?
>
> > ## Answer
> >
> > The main function is defined in the middle of this script and is called near the very
>>end.  The function *run_diagnostic* which you also see at the end of the file is what 
>>is called a context manager provided with ESMValTool and is the 
>>main entry point for most Python diagnostics. The variable *cfg* is a Python dictionary 
>>loaded with all the
>>necessary information needed to run the diagnostic script including location of 
>> input data and various settings.
>>In the *main* function, we will next parse this *cfg* variable and extract 
>>information as needed to do our analyses.
> >
> {: .solution}
{: .challenge}

## What information do I need for my analyses?
The very first thing passed to the diagnostic via the *cfg* dictionary is a path to a file
called *settings.yml*.  It is found at the lowest level of your directory structure under 
the *run* directory. An example path would be */path_to_recipe_output/run/diag_name/script_name/settings.yml*. 

> ## What is in the settings.yml file?
> The ESMValTool documentation page provides a generic overview of what is in the 
>settings.yml file [here](https://docs.esmvaltool.org/projects/esmvalcore/en/latest/interfaces.html).
> 
{: .callout}

> ## Challenge : digging in deeper to understand the preprocesor-diagnostic interface
>
> Can you find one example of the settings.yml file when you run this recipe?
> Take a look at the *input_files* list in your settings.yml file. Do you see a mention of 
> a second yml file called *metadat.yml*? 
> What information do you think is saved in metadat.yml?
>
>
> > ## Answer
> >Congratulations on finding an example each of the *settings.yml* and *metadata.yml* 
>>files! You will have noticed that metadat.yml has information on your preprocessed 
>>data. There is one file for each variable and it has detailed information on your data
>> including project (e.g., CMIP6, OBS), dataset names (e.g., MIROC-6, UKESM-0-1-LL), 
>>variable attributes (e.g., standard_name, units), preprocessor applied and time range
>> of the data. You are now ready to access all of this information for your evaluation!
> > 
> >
> {: .solution}
{: .challenge}

## Extracting information needed for analyses
In the *main* function of the diagnostic, you will see that *input_data* values are  read 
from the *cfg* Python dictionary. Typically, users will now need to group this input data 
according to some criteria such as by model or experiment and select specifics to analyse.
ESMValTool prvides a whole host of convenience functions that can do this for you. 
A list of available functions and their description is provided [here](https://docs.esmvaltool.org/en/latest/api/esmvaltool.diag_scripts.shared.html). In our example, you will see several 
of these imported right at the beginning of the file and used after input data is read.

> ## ESMValTool Diagnostic Interface Functions
>
> Can you spot the functions used for selecting and grouping data in the example?
>After running this example, how can you tell what the functions do?
>
> > ## Answer
> >
> > If you look carefully, you can see that there is a statement after each use of the 
>> select and group functions that starts with *logger.info*. These lines print output to
>> the log files. If you looked at the content of your log files under the run directory, you
>> should see the selected and grouped output. This is how you access preprocessed
>> information within your diagnostic.
> >
> {: .solution}
{: .challenge}

Finally, after grouping we read individual attributes such as the filename which gives us 
the specific name of the preprocessed data file we want to read and analyse. 
Following this, we see the call to a function called *compute_diagnostic*. 
In this example, this is where the analyses on the data is done. 
If you were writing your own diagnostic, this is the function you would put your own 
analyses code in.




## Another section

and some more text, etc.

## Conclusion


## For development purposes

Here are some of the elements that we can add

> ## Example exercise
>
> This is just a reminder on how to implement exercises
>
> > ## Example answer
> >
> > And this is where to add the answer.
> > This box will be collapsed in the page is first loaded.
> >
> {: .solution}
{: .challenge}

```bash
example command line instruction
```

```
example error message
```
{: .error}

> ## Example callout box
>
> This is how to create a callout box
{: .callout}
