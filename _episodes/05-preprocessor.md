---
title: "Writing your own recipe"
teaching: 15
exercises: 30
questions:
- "How do I create a new recipe?"
- "Can I use different preprocessors for different variables?"
- "Can I use different datasets for different variables?"
- "How can I combine different preprocessor functions?"
objectives:
- "Create a recipe with multiple preprocessors"
- "Use different preprocessors for different variables"
- "Run a recipe with variables from different datasets"
keypoints:
- "A recipe can work with different preprocessors at the same time."
- "The setting `additional_datasets` can be used to add a different dataset."
- "Variable groups are useful for defining different settings for different
  variables."
---

## Introduction

One of the key strenghts of ESMValTool is in making complex analyses reusable
and reproducible. But that doesn't mean everything in ESMValTool needs to be
complex. Sometimes, the biggest challenge is in making things simpler. You
probably know the 'warming stripes' visualization by Professor Ed Hawkins. On
the site <https://showyourstripes.info> you can find the same visualization for
many regions in the world.

![Warming stripes](../fig/warming_stripes.png) *Shared by Ed Hawkins under a
Creative Commons 4.0 Attribution International licence. Source:
<https://showyourstripes.info>*

In this episode, we will reproduce and extend this functionality with
ESMValTool. We have prepared a small Python script that takes a netcdf file with
timeseries data, and visualizes it in the form of our desired warming stripes
figure.

TODO/FIXME: add this script

Download the file

```bash
wget ......
```

and verify that it is now in your working directory. If you want, you may also have a look at the contents, but it is not necessary to follow along.

We will write an ESMValTool recipe that takes some data, performs the necessary preprocessing, and then runs our little Python script.

> ## Drawing up a plan
>
> In the previous episode, we have seen that ESMValTool executed a number of
> tasks. Write down which tasks we will need to do in this episode. And what does
> each of these tasks do?
>
> > ## Answer
> >
> > In this episode, we will need to do 2 tasks:
> >
> > - A preprocessing task that converts the gridded temperature data to a timeseries
> >   of global temperature anomalies
> > - A diagnostic tasks that calls our Python script, taking our preprocessed
> >   timeseries data as input.
> >
> {: .solution}
{: .challenge}

## Building a recipe from scratch

The easiest way to make a new recipe is to start from an existing one, and
modify it until it does exactly what you need. However, in this episode we will
start from scratch. This forces us to think about all the steps. We will deal
with common errors as they occur throughout the development.

Remember the basic structure of a recipe, and notice that each of them is
extensively described in the documentation under the header ["The recipe
format"](https://docs.esmvaltool.org/projects/esmvalcore/en/latest/recipe/overview.html):

- [documentation](https://docs.esmvaltool.org/projects/esmvalcore/en/latest/recipe/overview.html#recipe-section-documentation)
- [datasets](https://docs.esmvaltool.org/projects/esmvalcore/en/latest/recipe/overview.html#recipe-section-datasets)
- [preprocessors](https://docs.esmvaltool.org/projects/esmvalcore/en/latest/recipe/overview.html#recipe-section-preprocessors)
- [diagnostics](https://docs.esmvaltool.org/projects/esmvalcore/en/latest/recipe/overview.html#recipe-section-diagnostics)

This is the first place to look for help if you get stuck.

Open a new file called `recipe_warming_stripes.yml`:

```bash
nano recipe_warming_stripes.yml
```

Let's add the standard header comments (these don't do anything), and a first description.

```yaml
# ESMValTool
# recipe_warming_stripes.yml
---
documentation:
  description: Reproducing Ed Hawkins' warming stripes visualization
```

Notice that yaml always requires 2 spaces indentation between the different
levels. Pressing `ctrl+o` will save the file. Verify the filename at the bottom
and press enter. Then use `ctrl+x` to exit the editor.

We will try to run the recipe after every modification we make, to see if it (still) works.

```bash
esmvaltool run recipe_warming_stripes.yml
```

In this case, it gives an error. Below you see the last few lines of the error message.
```
...
Error validating data /home/user/esmvaltool_tutorial/recipe_barcodes.yml with schema /home/user/miniconda3/envs/esmvaltool_tutorial/lib/python3.8/site-packages/esmvalcore/recipe_schema.yml
	documentation.authors: Required field missing
2020-10-08 15:23:11,162 UTC [19451] INFO    If you suspect this is a bug or need help, please open an issue on https://github.com/ESMValGroup/ESMValTool/issues and attach the run/recipe_*.yml and run/main_log_debug.txt files from the output directory.
```
{: .error}

Here, ESMValTool is telling us that it's missing a required field, namely the
authors. It is good to know that ESMValTool always tries to validate the recipe
in an early stage. This initial check doesn't catch everything though, so we
should always stay alert.

Let's add some additional information to the recipe. Open the recipe file again,
and add an authors section below the description. ESMValTool expects the authors
as a list, like so:

```yaml
authors:
  - lastname_firstname
```

To bypass a number of similar error messages, add a minimal diagnostics section
below the documentation. The file should now look like:

```yaml
# ESMValTool
# recipe_warming_stripes.yml
---
documentation:
  description: Reproducing Ed Hawkins' warming stripes visualization
  authors:
    - doe_john
diagnostics:
  dummy_diagnostic_1:
    scripts: null
```

This is the minimal recipe layout that is required by ESMValTool.
If we now run the recipe again, you will probably see the following error

```
ValueError: Tag 'doe_john' does not exist in section 'authors' of /home/user/miniconda3/envs/esmvaltool_tutorial/python3.8/site-packages/esmvaltool/config-references.yml
```
{: .error}

> ## Pro tip: config-references.yml
>
> The error message above points to a file named `config-references.yml`. This
> is where ESMValTool stores all its citation information.
{: .callout}

For now, let's just use one of the existing references. Change the author field to
`righi_mattia`, who cannot receive enough credit for all the effort he put into
ESMValTool. If you now run the recipe again, you should see the final message

```
INFO    Run was successful
```
{: .output}

## Adding a dataset entry

Let's add a datasets section. We will reuse the same datasets that we used in the previous episode. The data files are stored in `~/esmvaltool_tutorial/data`.

> ## Filling in the dataset keys
>
> Explore the data directory, and look at the explanation of the dataset entry in the [ESMValTool documentation](https://docs.esmvaltool.org/projects/esmvalcore/en/latest/recipe/overview.html#recipe-section-documentation). For both the datasets, write down the following properties:
>
> - project
> - variable (short name)
> - CMIP table
> - dataset (model name or obs/reanalysis dataset)
> - experiment
> - ensemble member
> - grid
> - start year
> - end year
>
> > ## Answers
> >
> > | **key** | **file 1** | **file 2** |
> > | project | CMIP6 | CMIP5 |
> > | short name | tas | tas |
> > | CMIP table | Amon | Amon |
> > | dataset | BCC-ESM1 | CanESM2 |
> > | experiment | historical | historical |
> > | ensemble | r1i1p1f1 | r1i1p1 |
> > | grid | gn (native grid) | N/A |
> > | start year | 1850 | 1850 |
> > | end year | 2014 | 2005 |
> >
> > Note that the grid key is only required for CMIP6 data, and that the extent
> > of the historical period has changed between CMIP5 and CMIP6.
> >
> {: .solution}
{: .challenge}

We will start with the BCC-ESM1 dataset. Add a datasets section to the recipe, listing a single dataset, like so:

```yaml
datasets:
  - {dataset: BCC-ESM1, project: CMIP6, exp: historical, ensemble: r1i1p1f1, grid: gn, start_year: 1850, end_year: 2014}
```

Verify that the recipe still runs. Note that we have not included the short name of the variable in this dataset section. This allows us to reuse this dataset entry with different variable names later on. This is not really necessary for our simple use case, but it is common practice in ESMValTool.

## Adding the preprocessor section

Above, we already described the preprocessing task that needs to convert the standard, gridded temperature data to a timeseries of temperature anomalies.

> ## Defining the preprocessor
>
> Have a look at the available preprocessors in the [documentation](https://docs.esmvaltool.org/projects/esmvalcore/en/latest/recipe/preprocessor.html). Write down
>
> - Which preprocessor functions do you think we should use?
> - What are the parameters that we can pass to these functions?
> - What do you think should be the order of the preprocessors?
> - A suitable name for the overall preprocessor
>
> > ## Solution
> >
> > We need to calculate anomalies and global means. There is an `anomalies`
> > preprocessor which needs a granularity, a reference period, and whether or
> > not to standardize the data. The global means can be calculated with the
> > `area_statistics` preprocessor, which takes an operator as argument (in our
> > case we want to compute the `mean`).
> >
> > The default order in which these preprocessors are applied can be seen
> > [here](https://docs.esmvaltool.org/projects/esmvalcore/en/latest/api/esmvalcore.preprocessor.html#preprocessor-functions):
> > `area_statistics` comes before `anomalies`. If you want to change this, you
> > can use the `custom_order` preprocessor. We will keep it like this.
> >
> > Let's name our preprocessor `global_anomalies`.
> {: .solution}
{: .challenge}

Add the following block to your recipe file:

```yaml
preprocessors:
  global_anomalies:
    area_statistics:
      operator: mean
    anomalies:
        period: full
        reference:
          start_year: 1981
          start_month: 1
          start_day: 1
          end_year: 2010
          end_month: 12
          end_day: 31
        standardize: false
```

and verify that the recipe still runs.

## Completing the diagnostics section

Now we are ready to finish our diagnostics section. Remember that we want to make two tasks: a preprocessor task, and a diagnostic task. To illustrate that we can also pass settings to the diagnostic script, we add the option to specify a custom colormap.

> ## Fill in the blanks
>
> Extend the diagnostics section in your recipe by filling in the blanks in the
> following template:
>
> ```yaml
> diagnostics:
>   <... (suitable name for our diagnostic)>:
>     description: <...>
>     variables:
>       <... (suitable name for the preprocessed variable)>:
>         short_name: <...>
>         preprocessor: <...>
>     scripts:
>       <... (suitable name for our python script)>:
>         script: <full path to python script>
>         colormap: <... choose from matplotlib colormaps>
> ```
>
> > ## Solution
> >
> > ```yaml
> > diagnostics:
> >   diagnostic_warming_stripes:
> >     description: visualize global temperature anomalies as warming stripes
> >     variables:
> >       temperature_anomalies:
> >         short_name: tas
> >         preprocessor: global_anomalies
> >     scripts:
> >       warming_stripes_script:
> >         script: /home/<user>/esmvaltool_tutorial/warming_stripes.py
> >         colormap: 'bwr'
> > ```
> {: .solution}
{: .challenge}

Now you should be able to run the recipe to get your own warming stripes.

Note: for the purpose of simplicity in this episode, we have not added logging or provenance tracking in the diagnostic script. Once you start to develop your own diagnostic scripts and want to add them to the ESMValTool repositories, this will be required. However, writing your own diagnostic script is beyond the scope of the basic tutorial.

## Bonus exercises

> ## Change the preprocessor from global mean to a specific location
>
>
{:.challenge}

> ## Split the diagnostic in 2: the second one should use a different period
>
>
{:.challenge}

> ## Let the second diagnostic use a different pre-processor (different location)
>
> Pro tip: use yaml anchors to avoid repetition
{:.challenge}

> ## Add a second dataset, but only for one of the diagnostics
>
>
{:.challenge}

> ## Also plot the absolute temperature
>
> Pro-tip: use short name for variable group name as a short-hand.
{:.challenge}





# Old stuff below

## Preprocessors: What are they and how do they work?

Preprocessing is the process of performing a set of modular operations on the
data before applying diagnostics or metrics. In the figure below you see the
preprocessor functions in the light blue box on the right.

![figure showing ESMValTool architecture]({{ page.root
}}/fig/esmvaltool_architecture.png)

Underneath the hood, each preprocessor is a modular python function that
receives an iris cube and sometimes some arguments. The preprocessor applies
some mathematical or computational transformation to the input iris cube, then
returns the processed iris cube.

## Inspect the example preprocessor

Each preprocessor section includes a preprocessor name, a list of preprocessor
steps to be executed and any arguments needed by the preprocessor steps.

```yaml
preprocessors:
  prep_timeseries:
    annual_statistics:
      operator: mean
```

For instance, the `annual_statistics` with the  `operation: mean` argument
preprocessor receives an iris cube, takes the annual average for each year of
data in the cube, and returns the processed cube.

You may use one or more of several preprocessors listed in the
[documentation](https://docs.esmvaltool.org/projects/esmvalcore/en/latest/recipe/preprocessor.html).
The standardised interface between the preprocessors allows them to be used
modularly -- like lego blocks. Almost any conceivable preprocessing order of
operations can be performed using ESMValTool preprocessors.

> ## The 'custom order' command.
>
> If you do not want your preprocessors to be applied in the [default
> order](https://docs.esmvaltool.org/projects/ESMValCore/en/latest/api/esmvalcore.preprocessor.html),
> then add the following line to your preprocessor chain:
>
>```yaml
>    custom_order: true
>```
>
> The default preprocessor order is listed in the [ESMValCore preprocessor API
> page](https://docs.esmvaltool.org/projects/ESMValCore/en/latest/api/esmvalcore.preprocessor.html).
>
> Also note that preprocessor operations aren't always commutative - meaning
> that the order of operations matters. For instance, when you run the two
> preprocessors --  'extract_volume' to extract the top 100m of the ocean
> surface and  'volume_statistics' to calculate the volume-weighted mean of the
> data, your result will differ depending on the order of these two
> preprocessors. In fact, the 'extract_volume' preprocessor will fail if you try
> to run it on a 2D dataset.
>
> Changing the order of preprocessors can also speed up your processing. For
> instance, if you want to extract a two-dimensional layer from a 3D field and
> re-grid it, the layer extraction should be done first. If you did it the other
> way around, then the regridding function would be applied to all the layers of
> your 3D cube and it would take much more time.
{: .callout}

Some preprocessor modules are always applied and do not need to be called. This
includes the preprocessors that load the data, apply any fixes and save the data
file afterwards. These do not need to be explicitly included in recipes.

> ## Exercise: Adding more preprocessor steps
>
> Edit the [example recipe](https://github.com/ESMValGroup/ESMValTool_Tutorial/blob/master/files/recipe_example.yml) to first change the variable to
> `thetao`, using only the years 2000-2005. Then add preprocessors to average over the latitude and longitude
> dimensions and finally average over the depth. Now run the recipe.
>
>> ## Solution
>>
>> In the `diff` file below you will see the changes we have made to the file. The top 2 lines are the filenames and the lines like @@ -20,12 +20,15 @@ indicate the line numbers in the original and modified file, respectively. For more info on this format, see [here](https://en.wikipedia.org/wiki/Diff#Unified_format).
>>
>>```diff
>> --- data/recipe_example.yml
>> --- data/recipe_example_thetao.yml
>> @@ -20,12 +20,15 @@
>>      - ukesm
>>
>>  datasets:
>> -  - {dataset: HadGEM2-ES, project: CMIP5, exp: historical, mip: Omon, ensemble: r1i1p1, start_year: 1859, end_year: 2005}
>> +  - {dataset: HadGEM2-ES, project: CMIP5, exp: historical, mip: Omon, ensemble: r1i1p1, start_year: 2000, end_year: 2005}
>>
>>  preprocessors:
>>    prep_timeseries: # For 0D fields
>>      annual_statistics:
>>        operator: mean
>> +    area_statistics:
>> +      operator: mean
>> +    depth_integration:
>>
>>  diagnostics:
>>    # --------------------------------------------------
>> @@ -35,7 +38,7 @@
>>      description: simple_time_series
>>      variables:
>>        timeseries_variable:
>> -        short_name: thetaoga
>> +        short_name: thetao
>>          preprocessor: prep_timeseries
>>      scripts:
>>        timeseries_diag:
>> ```
>>
>> Complete recipe can be downloaded as [recipe_example_thetao.yml](https://github.com/ESMValGroup/ESMValTool_Tutorial/blob/master/files/recipe_example_thetao.yml)
>{: .solution}
{: .challenge}

## Using different preprocessors for different variables

You can also define different preprocessors with several preprocessor sections
(setting different preprocessor names). In the variable section you call the
specific preprocessor which should be applied.

> ## Example
>
> ```yaml
>datasets:
>  - {dataset: HadGEM2-ES, project: CMIP5, exp: historical, mip: Omon, ensemble: r1i1p1, start_year: 2000, end_year: 2005}
>
> preprocessors:
>   prep_timeseries_1:
>     annual_statistics:
>       operator: mean
>   prep_timeseries_2:
>     annual_statistics:
>       operator: mean
>     area_statistics:
>       operator: mean
>     depth_integration:
> ---
> diagnostics:
>   # --------------------------------------------------
>   # Time series diagnostics
>   # --------------------------------------------------
>   diag_timeseries_temperature_1:
>     description: simple_time_series>
>     variables:
>         timeseries_variable:
>         short_name: thetaoga
>         preprocessor: prep_timeseries_1
>     scripts:
>         timeseries_diag:
>         script: ocean/diagnostic_timeseries.py
>
>   diag_timeseries_temperature_2:
>     description: simple_time_series
>     variables:
>         timeseries_variable:
>         short_name: thetao
>         preprocessor: prep_timeseries_2
>     scripts:
>       timeseries_diag:
>         script: ocean/diagnostic_timeseries.py
> ```
>
> Complete recipe can be downloaded as [recipe_example_thetao_thetaoga.yml](https://github.com/ESMValGroup/ESMValTool_Tutorial/blob/master/files/recipe_example_thetao_thetaoga.yml)
{: .solution}

>## Challenge : How to write a recipe with multiple preprocessors
>
>We now know that a recipe can have more than one diagnostic, variable or
>preprocessor. As we saw in the examples so far, we can group preprocessors with
>a single user defined name and can have more than one such preprocessor group
>in the recipe as well. Write two different preprocessors - one to regrid the
>data to a 1x1 resolution and the second preprocessor to mask out sea and ice
>grid cells before regridding to the same resolution. In the second case, ensure
>you perform the masking first before regridding (hint: custom order your
>operations). Now, use the two preprocessors in different diagnostics within the
>same recipe. You may use any variable(s) of your choice. Once you succeed, try
>to add new datasets to the same recipe.
>
> To complete this exercise, we need to change the following sections in the recipe:
>
> - `preprocessors`:
>   - `prep_map`: add a preprocessor to regrid data
>     - fill preprocessor details here
>
>   - `prep_map_land`: add a preprocessor to mask grid cells and then regrid
>     - fill preprocessor details here including ordering
>
> - `diag_simple_plot`:
>   - `description`: preprocess a variable for a simple 2D plot
>   - `variables`:
>     - put your variable of choice here
>     - edit mip, and time based on your variable choice
>     - apply the first preprocessor i.e. name your preprocessor
>
>   - `scripts`: `null` it means that no scripts called.
>
> - `diag_land_only_plot`: second diagnostic
>   - `description`: preprocess a variable for a 2D land only plot
>   - ``variables``:
>     - include a variable and information as in the previous diagnostic
>     - include your second preprocessor (masking and regridding)
>   - `scripts`: `null` it means that no scripts called.
>
>> ## Solution
>>
>> Here is one solution to complete the challenge above using  two different
>> preprocessors:
>>
>>```diff
>> --- data/recipe_example.yml
>> +++ data/recipe_example_multi_preprocessors.yml
>> @@ -20,23 +20,37 @@
>>      - ukesm
>>
>>  datasets:
>> -  - {dataset: HadGEM2-ES, project: CMIP5, exp: historical, mip: Omon, ensemble: r1i1p1, start_year: 1900, end_year: 2000}
>> +  - {dataset: HadGEM2-ES, project: CMIP5, exp: historical, mip: Amon, ensemble: r1i1p1, start_year: 2000, end_year: 2005}
>>
>>  preprocessors:
>> -  prep_timeseries: # For 0D fields
>> -    annual_statistics:
>> -      operator: mean
>> +  prep_map:
>> +    regrid:    #apply the preprocessor to regrid
>> +      target_grid: 1x1 # target resolution
>> +      scheme: linear  #how to interpolate for regridding
>> +
>> +  prep_map_land:
>> +    custom_order: true #ensure that given order of preprocessing is followed
>> +    mask_landsea:    #apply a mask
>> +      mask_out: sea   #mask out sea grid cells
>> +    regrid:    # now apply the preprocessor to regrid
>> +      target_grid: 1x1 # target resolution
>> +      scheme: linear  #how to interpolate for regridding
>>
>>  diagnostics:
>>    # --------------------------------------------------
>> -  # Time series diagnostics
>> +  # Two Simple diagnostics that illustrate the use of
>> +  # different preprocessors
>>    # --------------------------------------------------
>> -  diag_timeseries_temperature:
>> -    description: simple_time_series
>> +  diag_simple_plot:
>> +    description: # preprocess a variable for a simple 2D plot
>> +    variables:
>> +      tas:  # surface temperature
>> +        preprocessor: prep_map
>> +    scripts: null
>> +
>> +  diag_land_only_plot:
>> +    description: #preprocess a variable for a 2D land only plot
>>      variables:
>> -      timeseries_variable:
>> -        short_name: thetaoga
>> -        preprocessor: prep_timeseries
>> -    scripts:
>> -      timeseries_diag:
>> -        script: ocean/diagnostic_timeseries.py
>> +      tas:  # surface temperature
>> +        preprocessor: prep_map_land
>> +    scripts: null
>> ```
>>
>> Complete recipe can be downloaded as
>> [recipe_example_multi_preprocessors.yml](https://github.com/ESMValGroup/ESMValTool_Tutorial/blob/master/files/recipe_example_multi_preprocessors.yml).
> {: .solution}
{: .challenge}

## Adding different datasets for different variables

Sometimes, we may want to include specific datasets only for certain variables.
An example is when we use observations for variables in a
diagnostic. While the CMIP dataset details for the two variables may be common,
the observations will likely not be so. It would be useful to know how to
include different datasets for different variables. Here is an example of a
simple preprocessor and diagnostic setup for that:

> ## Example
>
> ```yaml
>
> datasets:
>   - {dataset: HadGEM2-ES, project: CMIP5, exp: historical, ensemble: r1i1p1}
>
> preprocessors:
>   prep_regrid: # regrid to get all data to the same resolution
>     regrid:  # apply the preprocessor to regrid
>       target_grid: 2.5x2.5  # target resolution
>       scheme: linear  # how to interpolate for regridding
>
> diagnostics:
>   # --------------------------------------------------
>   # Simple diagnostic to illustrate use of different
>   # datasets for different variables
>   # --------------------------------------------------
>   diag_diff_datasets:
>     description: diff_datasets_for_vars
>     variables:
>       pr:  # first variable is precipitation
>         mip: Amon
>         start_year: 2000
>         end_year: 2005
>         preprocessor: prep_regrid
>         additional_datasets:
>           - {dataset: GPCP-SG, project: obs4mips, level: L3,
>              version: v2.2, tier: 1}  # dataset specific to this variable
>       tas:  # second variable is surface temperature
>         mip: Amon
>         start_year: 2000
>         end_year: 2005
>         preprocessor: prep_regrid
>     scripts: null
> ```
> Complete recipe can be downloaded as [recipe_example_pr_tas.yml](https://github.com/ESMValGroup/ESMValTool_Tutorial/blob/master/files/recipe_example_pr_tas.yml).
>
{: .solution}

## Creating variable groups

Variable grouping can be used to preprocess different clusters of data for the
same variable. For instance, the example below illustrates how we can compute
separate multimodel means for different CMIP5 datasets given the same variable.

> ## Example
>```yaml
>
>preprocessors:
>  prep_mmm:
>    custom_order: true
>    regrid:
>      target_grid: 2.5 x 2.5
>      scheme: linear
>    multi_model_statistics:
>      span: full
>      statistics: [mean, median]
>
>  prep_obs:
>    mask_landsea:
>      mask_out: sea
>    regrid:
>      target_grid: 2.5 x 2.5
>      scheme: linear
>
># note that there is no field called datasets anymore
># note how multiple ensembles are added by using (1:4)
>cmip5_control: &cmip5_control
>  - {dataset: HadGEM2-ES, ensemble: r1i1p1, project: CMIP5}
>  - {dataset: MPI-ESM-LR, ensemble: r1i1p1, project: CMIP5}
>
>cmip5_target: &cmip5_target
>  - {dataset: CanESM2, ensemble: "r(1:2)i1p1", project: CMIP5}
>
>diagnostics:
>
>  diag_variable_groups:
>    description: Demonstrate the use of variable groups.
>    variables:
>      tas_control: &variable_settings  # need a key name for the grouping
>        short_name: tas  # specify variable to look for
>        preprocessor: prep_mmm
>        mip: Amon
>        exp: historical
>        start_year: 2000
>        end_year: 2005
>        tag: TAS_CONTROL  # tag is optional if you are using these settings just once
>        additional_datasets: *cmip5_control
>      tas_target:
>        <<: *variable_settings
>        tag: TAS_TARGET
>        additional_datasets: *cmip5_target  # nothing changes from cmip5 except the data set
>    scripts: null
>```
> There is no field called datasets anymore.
> Also, note how multiple ensembles are added by using (1:2).
> Complete recipe can be downloaded as
> [recipe_example_tas_control_target.yml](https://github.com/ESMValGroup/ESMValTool_Tutorial/blob/master/files/recipe_example_tas_control_target.yml).
>
{: .solution}

You should be able to see the variables grouped under different subdirectories
under your output preproc directory. The different groupings can be accessed in
your diagnostic by selecting the key name of the field
`variable_group`  such as `TAS_CONTROL`, or `TAS_TARGET`.

> ## How to find what CMIP data is available?
>
> Please see section "Access to CMIP and Observational data" in [Setup]({{ page.root }}{% link setup.md %}).
{: .callout}
