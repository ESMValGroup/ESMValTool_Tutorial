---
title: "Working with preprocessors"
teaching: 15
exercises: 45
questions:
- "How do I set up a preprocessor?"
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
> page](>https://docs.esmvaltool.org/projects/ESMValCore/en/latest/api/esmvalcore.preprocessor.html).
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
> Edit the [example recipe](https://github.com/ESMValGroup/ESMValTool_Tutorial/blob/master/data/recipe_example.yml) to first change the variable to
> `thetao`, then add preprocessors to average over the latitude and longitude
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
>> Complete recipe can be downloaded as [recipe_example_thetao.yml](https://github.com/ESMValGroup/ESMValTool_Tutorial/blob/master/data/recipe_example_thetao.yml)
>{: .solution}
{: .challenge}

## Using different preprocessors for different variables

You can also define different preprocessors with several preprocessor sections
(setting different preprocessor names). In the variable section you call the
specific preprocessor which should be applied.

> ## Example
>
> ```yaml
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
>to add new datasets to the same recipe. Placeholders for the different
>components are provided below:
>
>> ## Recipe
>>
>> ```yaml
>>
>> datasets:
>>   - {dataset: UKESM1-0-LL, project: CMIP6, exp: historical,
>>      ensemble: r1i1p1f2}  # single dataset as an example
>>
>> preprocessors:
>>   prep_map:  # preprocessor to just regrid data
>>     # fill preprocessor details here
>>
>>   prep_map_land:  # preprocessor to mask grid cells and then regrid
>>     # fill preprocessor details here including ordering
>>
>> diagnostics:
>>   # --------------------------------------------------
>>   # Two Simple diagnostics that illustrate the use of
>>   # different preprocessors
>>   # --------------------------------------------------
>>  diag_simple_plot:
>>    description:  # preprocess a variable for a simple 2D plot
>>    variables:
>>      # put your variable of choice here
>>      # apply the first preprocessor i.e. name your preprocessor
>>      # edit the following 4 lines for mip, grid and time
>>      # based on your variable choice
>>      mip: Amon
>>      grid: gn  # can change for variables from the same model
>>      start_year: 1970
>>      end_year: 2000
>>    scripts: null  # no scripts called
>>  diag_land_only_plot: # second diagnostic
>>    description:  # preprocess a variable for a 2D land only plot
>>    variables:
>>      # include  a variable and information
>>      # as in the previous diagnostic and
>>      # include your second preprocessor (masking and regridding)
>>    scripts: null  # no scripts
>> ```
>>
>{: .solution}
>
>> ## Solution:
>>
>> Here is one solution to complete the challenge above using  two different
>> preprocessors:
>>
>>```yaml
>>
>> datasets:
>>   - {dataset: UKESM1-0-LL, project: CMIP6, exp: historical,
>>      ensemble: r1i1p1f2}  # single dataset as an example
>>
>> preprocessors:
>>   prep_map:
>>     regrid:  # apply the preprocessor to regrid
>>       target_grid: 1x1  # target resolution
>>       scheme: linear  # how to interpolate for regridding
>>
>>   prep_map_land:
>>     custom_order: true  # ensure that given order of preprocessing is followed
>>     mask_landsea:    # apply a mask
>>       mask_out: sea   # mask out sea grid cells
>>     regrid:  # now apply the preprocessor to regrid
>>       target_grid: 1x1  # target resolution
>>       scheme: linear  # how to interpolate for regridding
>>
>> diagnostics:
>>   # --------------------------------------------------
>>   # Two Simple diagnostics that illustrate the use of
>>   # different preprocessors
>>   # --------------------------------------------------
>>   diag_simple_plot:
>>     description:  # preprocess a variable for a simple 2D plot
>>     variables:
>>       tas:  # surface temperature
>>         preprocessor: prep_map
>>         mip: Amon
>>         grid: gn  # can change for variables from the same model
>>         start_year: 1970
>>         end_year: 2000
>>     scripts: null
>>
>>   diag_land_only_plot:
>>     description:  # preprocess a variable for a 2D land only plot
>>     variables:
>>       tas:  # surface temperature
>>         preprocessor: prep_map_land
>>         mip: Amon
>>         grid: gn  # can change for variables from the same model
>>         start_year: 1970
>>         end_year: 2000
>>     scripts: null
>> ```
>>
>> Complete recipe can be downloaded [here](https://github.com/ESMValGroup/ESMValTool_Tutorial/blob/master/data/recipe_example_multi_preprocessors.yml).
> {: .solution}
{: .challenge}

## Adding different datasets for different variables

Sometimes, we may want to include specific datasets only for certain variables.
An example is when we use observations for two different variables in a
diagnostic. While the CMIP dataset details for the two variables may be common,
the observations will likely not be so. It would be useful to know how to
include different datasets for different variables. Here is an example of a
simple preprocessor and diagnostic setup for that:

> ## Example
>
> ```yaml
>
> datasets:
>   - {dataset: UKESM1-0-LL, project: CMIP6, exp: historical,
>      ensemble: r1i1p1f2}  # common to both variables discussed below
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
>         preprocessor: prep_regrid
>         mip: Amon
>         grid: gn  # can change for variables from the same model
>         start_year: 1970
>         end_year: 2000 # start and end years for a 30 year period,
>                        # we assume this is common and exists for all
>                        # model and obs data
>         additional_datasets:
>           - {dataset: GPCP-SG, project: obs4mips, level: L3,
>              version: v2.2, tier: 1}  # dataset specific to this variable
>       tas:  # second variable is surface temperature
>         preprocessor: prep_regrid
>         mip: Amon
>         grid: gn  # can change for variables from the same model
>         start_year: 1970  # some 30 year period
>         end_year: 2000
>         additional_datasets:
>           - {dataset: HadCRUT4, project: OBS, type: ground,
>              version: 1, tier: 2}  # dataset specific to the temperature variable
>     scripts: null
> ```
>
{: .solution}

## Creating variable groups

Variable grouping can be used to preprocess different clusters of data for the
same variable. For instance, the example below illustrates how we can compute
separate multimodel means for CMIP5 and CMIP6 data given the same variable.
Additionally we can also preprocess observed data for evaluation.

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
>cmip5_datasets: &cmip5_datasets
>  - {dataset: CanESM2, ensemble: "r(1:4)i1p1", project: CMIP5}
>  - {dataset: MPI-ESM-LR, ensemble: "r(1:2)i1p1", project: CMIP5}
>
>cmip6_datasets: &cmip6_datasets
>  - {dataset: UKESM1-0-LL, ensemble: "r(1:4)i1p1f2", grid: gn, project: CMIP6}
>  - {dataset: CanESM5, ensemble: "r(1:4)i1p2f1", grid: gn, project: CMIP6}
>
>diagnostics:
>
>  diag_variable_groups:
>    description: Demonstrate the use of variable groups.
>    variables:
>      tas_cmip5: &variable_settings  # need a key name for the grouping
>        short_name: tas  # specify variable to look for
>        preprocessor: prep_mmm
>        mip: Amon
>        exp: historical
>        start_year: 2000
>        end_year: 2005
>        tag: TAS_CMIP5  # tag is optional if you are using these settings just once
>        additional_datasets: *cmip5_datasets
>      tas_obs:
>        <<: *variable_settings
>        preprocessor: prep_obs
>        tag: TAS_OBS
>        additional_datasets:
>          - {dataset: HadCRUT4, project: OBS, type: ground, version: 1, tier: 2}
>      tas_cmip6:
>        <<: *variable_settings
>        tag: TAS_CMIP6
>        additional_datasets: *cmip6_datasets  # nothing changes from cmip5 except the data set
>    scripts: null
>```
>
{: .solution}

You should be able to see the variables grouped under different subdirectories
under your output preproc directory. The different groupings can be accessed in
your diagnostic by selecting the key name of the field `variable_group`  such as `tas_cmip5`, `tas_cmip6` or `tas_obs`.

> ## How to find what CMIP data is available?
>
> [CMIP5](https://pcmdi.llnl.gov/mips/cmip5/index.html) and
> [CMIP6](https://pcmdi.llnl.gov/CMIP6/Guide/dataUsers.html) data obey the
> [CF-conventions](http://cfconventions.org/). Available variables can be found
> under the [CMIP5 data
> request](https://pcmdi.llnl.gov/mips/cmip5/docs/standard_output.pdf?id=28) and
> the [CMIP6 Data Request](http://clipc-services.ceda.ac.uk/dreq/index.html).
>
> CMIP data is widely available via the Earth System Grid Federation
> ([ESGF](https://esgf.llnl.gov/)) and is accessible to users either via
> download from the ESGF portal or through the ESGF data nodes hosted by large
> computing facilities (like [CEDA-Jasmin](https://esgf-index1.ceda.ac.uk/),
> [DKRZ](https://esgf-data.dkrz.de/), etc). The ESGF also hosts observations for
> Model Intercomparison Projects (obs4MIPs) and reanalyses data (ana4MIPs).
>
> A full list of all CMIP named variables is available here:
> [http://clipc-services.ceda.ac.uk/dreq/index/CMORvar.html](http://clipc-services.ceda.ac.uk/dreq/index/CMORvar.html).
{: .callout}
