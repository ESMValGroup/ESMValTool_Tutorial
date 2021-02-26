---
title: "CMORization: adding new datasets to ESMValTool"
teaching: 15
exercises: 45

questions:
- "CMORization: what is it and why do we need it?"
- "How to use the existing CMORizer scripts shipped with ESMValTool?"
- "How add support for new (observational) datasets?"

objectives:
- "Understand what CMORization is and why it is necessary."
- "Use existing scripts to CMORize your data."
- "Write a new CMORizer script to support additional data."

keypoints:
- "CMORizers are dataset-specific scripts that can be run once to generate CMOR-compliant data."
- "ESMValTool comes with a set of CMORizers readily available, but you can also add your own."
---

![Data flow with ESMValTool](../fig/data_flow.png)

## Introduction

This episode deals with "CMORization". ESMValTool is designed to work with data
that follow the CMOR standards. Unfortunately, not all datasets follow these
standards. In order to use such datasets in ESMValTool we first need to reformat
the data. This process is called "CMORization".

> ## What are the CMOR standards?
>
> The name "CMOR" originates from a tool: [the Climate Model Output
> Rewriter](https://cmor.llnl.gov/). This tool is used to create "CF-Compliant
> netCDF files for use in the CMIP projects". So CMOR extends the
> [CF-standard](https://cfconventions.org/) with additional requirements for
> the Coupled Model Intercomparison Projects (see e.g.
> [here](https://pcmdi.llnl.gov/CMIP6/Guide/modelers.html#5-model-output-requirements)).
>
>
> Concretely, the CMOR standards dictate e.g. the variable names and units,
coordinate information, how the data should be structured (e.g. 1 variable per
file), additional metadata requirements, but also file naming conventions a.k.a.
the data reference syntax (DRS). All this information is stored in so-called
CMOR tables. As an example, the CMOR tables for the CMIP6 project can be found
[here](https://github.com/PCMDI/cmip6-cmor-tables).
{: .callout}

ESMValTool offers two ways to CMORize data:
1. A reformatting script can be used to create a CMOR-compliant copy. CMORizer
   scripts for several popular datasets are included in ESMValTool, and
   ESMValTool also provides a convenient way to execute them.
2. ESMValCore can execute CMOR fixes '[on the
   fly](https://docs.esmvaltool.org/projects/esmvalcore/en/latest/develop/fixing_data.html#fixing-data)'.
   The advantage is that you don't need to store an additional, reformatted copy
   of the data. The disadvantage is that these fixes should be implemented
   inside ESMValCore, which is beyond the scope of this tutorial.

In this lesson, we will re-implement a CMORizer script for the FLUXCOM dataset
that contains observations of the Gross Primary Production (GPP), a variable
that is important for calculating components of the global carbon cycle.

We will assume that you are using a development installation of ESMValTool as
explained in the [previous episode](/08-development-setup).


## Obtaining the data

The data for this episode is available via the [FluxCom Data
Portal](http://www.bgc-jena.mpg.de/geodb/BGI/Home). First you'll need to
register. After registration, in the dropdown boxes, select FLUXCOM as the data
choice and click download. Three files will be displayed. Click the download
button on the "FLUXCOM (RS+METEO) Global Land Carbon Fluxes using CRUNCEP
climate data". You'll receive an email with the FTP address to access the
server. Connect to the server, follow the path in your email, and look for the
file `raw/monthly/GPP.ANN.CRUNCEPv6.monthly.2000.nc`. Download that file and
save in in a folder called `/RAWOBS/Tier3/FLUXCOM`.

Note: you'll need a user-friendly ftp client. On Linux, `ncftp` works okay.

> ## What is the deal with those "tiers"?
>
> Many datasets come with access restrictions. In this way the data providers
> can keep track of how their data is used. In many cases "restricted access"
> just means that one has to register with an email address and accept the terms
> of use, which typically ask that you acknowledge the data providers.
>
> There are also datasets available that do not need a registration. The
> "obs4MIPs" or "ana4MIPs" datasets, for example, are specifically produced to
> facilitate comparisons with model simulations.
>
> To reflect these different levels of access restriction, the ESMValTool team
> has created a tier-system. The definition of the different tiers are
> as follows:
>
> - **Tier1**: obs4MIPs and ana4MIPS datasets (can be used directly with the ESMValTool)
> - **Tier2**: other freely available datasets (most of them will need some kind of
>   cmorization)
> - **Tier3**: datasets with access restrictions (most of these datasets will also
>   need some kind of cmorization)
>
> These access restrictions are also the reason why the ESMValTool developers
> cannot distribute copies or automate downloading of all observations and
> reanalysis data used in the recipes. As a compromise we provide the
> CMORization scripts so that each user can CMORize their own copy of the access
> restricted datasets if they need them.
>
{: .callout}

## Run the existing CMORizer script

Before we develop our own CMORizer script, let's first see what happens when we
run the existing one. There is a specific command available in the ESMValTool to
run the CMORizer scripts:

```bash
cmorize_obs -c <config-user.yml> -o <dataset-name>
```

The ``config-user-yml`` is the file in which we define the different data
paths, e.g. where the ESMValTool would find the "RAWOBS" folder. The
``dataset-name`` needs to be identical to the folder name that was created
to store the raw observation data files, in our case this would be "FLUXCOM".

If everything is okay, the output should look something like this:

~~~
...
... Starting the CMORization Tool at time: 2021-02-26 14:02:16 UTC
... ----------------------------------------------------------------------
... input_dir  = /home/peter/data/RAWOBS
... output_dir = /home/peter/esmvaltool_output/cmorize_obs_20210226_140216
... ----------------------------------------------------------------------
... Running the CMORization scripts.
... Using cmorizer scripts repository: /home/peter/miniconda3/envs/esmvaltool/lib/python3.8/site-packages/esmvaltool/cmorizers/obs
... Processing datasets {'Tier3': ['FLUXCOM']}
... Input data from: /home/peter/data/RAWOBS/Tier3/FLUXCOM
... Output will be written to: /home/peter/esmvaltool_output/cmorize_obs_20210226_140216/Tier3/FLUXCOM
... Reformat script: /home/peter/miniconda3/envs/esmvaltool/lib/python3.8/site-packages/esmvaltool/cmorizers/obs/cmorize_obs_fluxcom
... CMORizing dataset FLUXCOM using Python script /home/peter/miniconda3/envs/esmvaltool/lib/python3.8/site-packages/esmvaltool/cmorizers/obs/cmorize_obs_fluxcom.py
... Found input file '/home/peter/data/RAWOBS/Tier3/FLUXCOM/GPP.ANN.CRUNCEPv6.monthly.*.nc'
... CMORizing variable 'gpp'
... Lmon
... Var is gpp
... ... UserWarning: Ignoring netCDF variable 'GPP' invalid units 'gC m-2 day-1'
  warnings.warn(msg)
... Fixing time...
... Fixing latitude...
... Fixing longitude...
... Flipping dimensional coordinate latitude...
... Saving file
... Converting data type of data from 'float64' to 'float32'
... Saving: /home/peter/esmvaltool_output/cmorize_obs_20210226_140216/Tier3/FLUXCOM/OBS_FLUXCOM_reanaly_ANN-v1_Lmon_gpp_200001-200012.nc
... Cube has lazy data [lazy is preferred]
... Ending the CMORization Tool at time: 2021-02-26 14:02:16 UTC
... Time for running the CMORization scripts was: 0:00:00.605970
~~~
{: .output}

So you can see that several fixes are applied, and the CMORized file is written
to the ESMValTool output directory. In order to use it, we'll have to copy it
from the output directory to a folder called
`<path_to_your_data>/OBS/Tier3/FLUXCOM` and make sure the path to ``OBS`` is set
correctly in our config-user file.

You can also see the path where ESMValTool stores the reformatting script:
`<path to esmvaltool>/esmvaltool/cmorizers/obs/cmorize_obs_fluxcom.py`. You may
have a look at this file if you want. The script also uses a configuration file:
`<path to esmvaltool>/esmvaltool/cmorizers/obs/cmor_config/FLUXCOM.yml`.

## Make a test recipe

Our end goal is to run a recipe that can load the data and work with it. So we
will start by making a simple recipe that can serve as a test case. Once we get
it to run, we know we have completed our task.

> ## Create a test recipe
>
> Create a simple recipe called `recipe_check_fluxcom.yml` that loads the
> "FLUXCOM" data. It should include a datasets section with a single entry for
> the "FLUXCOM" dataset with the correct dataset keys, and a diagnostics section
> with two variables: gpp. We don't need any preprocessors or
> scripts (set `scripts: null`), but we have to add a documentation section with
> a description, authors and maintainer, otherwise the recipe will fail.
>
> Use the following dataset keys:
>
> - project: OBS
> - dataset: FLUXCOM
> - type: reanaly
> - version: ANN-v1
> - mip: Lmon
> - start_year: 2000
> - end_year: 2000
> - tier: 3
>
> Some of these dataset keys are further explained in the callout boxes in this
> episode.
>
> > ## Answer
> >
> > Here's an example recipe
> >
> > ```yaml
> > documentation:
> >
> >   description: Test recipe for FLUXCOM data
> >
> >   authors:
> >     - kalverla_peter
> >
> >   maintainer:
> >     - kalverla_peter
> >
> > datasets:
> >   - {project: OBS, dataset: FLUXCOM, mip: Lmon, tier: 3, start_year: 2000, end_year: 2000, type: reanaly, version: ANN-v1}
> >
> > diagnostics:
> >   check_fluxcom:
> >     description: Check that ESMValTool can load the cmorized fluxnet data without errors.
> >     variables:
> >       gpp:
> >     scripts: null
> >
> > ```
> >
> > To learn more about writing a recipe, please refer to [Writing your own recipe](/05-preprocessor).
> >
> {: .solution}
{: .challenge}

Try to run the example recipe with

```bash
esmvaltool run recipe_check_fluxcom.yml --log_level debug
```

If everything is okay, the recipe should run without problems.

## Starting from scratch

Now that you've seen how to use an existing CMORizer script, let's think about
adding a new one. We will remove the existing CMORizer script, and re-implement
it from scratch. This exercise allows us to point out all the details of what's
going on. We'll also remove the CMORized data that we've just created, so our
test recipe will not be able to use it anymore.

```bash
rm <path_to_your_data>/OBS/Tier3/FLUXCOM/OBS_FLUXCOM_reanaly_ANN-v1_Lmon_gpp_200001-200012.nc
rm <path_to_esmvaltool>/esmvaltool/cmorizers/obs/cmorize_obs_fluxcom.nc
rm <path to esmvaltool>/esmvaltool/cmorizers/obs/cmor_config/FLUXCOM.yml
```

If you now run the test recipe again it should fail, and somewhere in the output you should find something like:

~~~
No input files found for ...
Looking for files matching ['OBS_FLUXCOM_reanaly_ANN-v1_Lmon_gpp[_.]*nc'] in ['/home/peter/data/OBS/Tier3/FLUXCOM']
~~~
{: .error}

From this we can see that the first thing our CMORizer should do is to rename
the file so that it follows the CMOR filename conventions.

## Create a new CMORizer script

The first step now is to create a new file in the right folder that will contain
our new CMORizer instructions. Create a file called ``cmorize_obs_fluxcom.py``

```bash
nano <path_to_esmvaltool>/esmvaltool/cmorizers/obs/cmorize_obs_fluxcom.py
```

and fill it with the following boilerplate code:

```python
"""ESMValTool CMORizer for FLUXCOM GPP data.

<We will add some useful info here later>
"""
import logging
from . import utilities as utils

logger = logging.getLogger(__name__)

def cmorization(in_dir, out_dir, cfg, _):
    """Cmorize the dataset."""

    # This is where you'll add the cmorization code
    # 1. find the input data
    # 2. apply the necessary fixes
    # 3. store the data with the correct filename
```

Here, ``in_dir`` corresponds to the input directory of the raw files,
``out_dir`` to the output directory of final reformatted data set and ``cfg`` to
a configuration dictionary given by a configuration file that we will get to shortly.

When you type the command ``cmorize_obs`` in the terminal, ESMValTool will call
this function with the settings found in your configuration files.

> ## Note
>
> Always, always, when modifying or creating new code for the ESMValTool
> repositories, work on your *own, local* branch of the ESMValTool. For more
> information see [Development and contribution](/08-development-setup)
>
{: .callout}

### 1. Find the input data and store it under the right name.

Since the original data does not follow CMOR filename conventions, we need to
tell ESMValTool what the filename for this new dataset looks like. Also, we need
to provide the relevant information so ESMValTool can set the correct filename
for the cmorized data. We supply this information via a dataset configuration
file. It is important to note that the name of the configuration file has to be
identical to the name of the dataset. Thus, we will create a file called
`<path_to_esmvaltool>/esmvaltool/cmorizers/obs/cmor_config/FLUXCOM.yml`.

> ## Create the configuration file for the "FLUXCOM" dataset
>
> Here is the skeleton of the "FLUXCOM" configuration file as it exists in
> the ESMValTool framework. Try to fill in all missing pieces of information
> for this configuration file that are marked with ``???``.
>
> ```yaml
> ---
> # Filename
> filename: ???
>
> # Common global attributes for Cmorizer output
> attributes:
>   dataset_id: ???
>   version: ???
>   tier: ???
>   modeling_realm: ???
>   project_id: ???
>   source: ???
>   reference: ???
>   comment: ???
>
> # Variables to cmorize
> variables:
>   ???:
>     mip: ???
> ```
>
> > ## Answers
> >
> > The configuration file for the "FLUXCOM" dataset with all necessary pieces
> > of information looks  like this:
> >
> > ```yaml
> > ---
> > # Filename
> > filename: 'GPP.ANN.CRUNCEPv6.monthly.*.nc'
> >
> > # Common global attributes for Cmorizer output
> > attributes:
> >   dataset_id: FLUXCOM
> >   version: 'ANN-v1'
> >   tier: 3
> >   modeling_realm: reanaly
> >   project_id: OBS
> >   source: 'http://www.bgc-jena.mpg.de/geodb/BGI/Home'
> >   reference: 'fluxcom'
> >   comment: ''
> >
> > # Variables to cmorize
> > variables:
> >   gpp:
> >     mip: Lmon
> > ```
> >
> > *Suggestion: maybe add the reference under step 3 (additional but not strictly necessary steps)*
> > Note the attribute "reference" here: it should include a ``doi`` related to
> > the dataset. For more information on how to add references to the
> > ``reference`` section of the configuration file, see the section in the
> > documentation about this: [adding
> > references](https://docs.esmvaltool.org/en/latest/community/diagnostic.html#adding-references)
> >
> {: .solution}
{: .challenge}

###### Here we need to add python code to the cmorizer script

so that we can run it and see whether it was able to find the correct input and create the right output.



### 2. Implementing additional fixes


> ## Run the test recipe again
>
> Run the test recipe again from earlier where you wanted to check if the
> ESMValTool can already find and read the freshly downloaded FLUXCOM data.
>
> > ## Answers
> >
> > ```bash
> > esmvaltool run recipe_check_fluxcom.yml --log_level debug
> > ```
> >
> > The ESMValTool should now find your FLUXCOM datafile, and the error
> > message that the ESMValTool can't find the data should now not be present
> > anymore. However, there are plenty of other error messages around.
> > Somewhere within the many messages, you should see this:
> >
> > ```bash
> > ...
> > esmvalcore.cmor.check.CMORCheckError: There were errors in variable GPP:
> > Variable GPP units unknown can not be converted to kg m-2 s-1
> >  lon: standard_name should be longitude, not None
> >  lat: standard_name should be latitude, not None
> >  lon longitude coordinate has values < -360 degrees
> >  lon longitude coordinate has values > 720 degrees
> >  lat: has values < valid_min = -90.0
> >  lat: has values > valid_max = 90.0
> >  GPP: does not match coordinate rank
> > in cube:
> > gross_primary_productivity_of_carbon / (unknown) (time: 12; lat: 360; lon: 720)
> >      Dimension coordinates:
> >           time                                        x        -         -
> >           lat                                         -        x         -
> >           lon                                         -        -         x
> >      Attributes:
> >           created_by: Fabian Gans [fgans@bgc-jena.mpg.de], Ulrich Weber [uweber@bgc-jena.mpg...
> >           flux: GPP
> >           forcing: CRUNCEPv6
> >           institution: MPI-BGC-BGI
> >           invalid_units: gC m-2 day-1
> >           method: Artificial Neural Networks
> >           provided_by: Martin Jung [mjung@bgc-jena.mpg.de] on behalf of FLUXCOM team
> >           reference: Jung et al. 2016, Nature; Tramontana et al. 2016, Biogeosciences
> >           source_file: /mnt/lustre02/work/bd0854/b309143/Data/Tier3/FLUXCOM/OBS_FLUXCOM_reana...
> >           temporal_resolution: monthly
> >           title: GPP based on FLUXCOM RS+METEO with CRUNCEPv6 climate
> >           version: v1
> > ...
> > ```
> {: .solution}
{: .challenge}

The error messages that we see tell us that we do have to reformat the dataset
slightly to have it follow the CMOR standard. It seems like the variable "gpp"
does not have the correct unit that is given in the CMOR table where it is
[defined](https://github.com/ESMValGroup/ESMValCore/tree/master/esmvalcore/cmor/tables/cmip6/Tables).
It also seems like the "latitude" and "longitude" coordinates have some
problems. So let's start writing a short python script that will fix these
problems.


To simplify this process, ESMValTool provides some convenience functions in
``utilities.py`` , which we already included in the boilerplate code above.

Apart from a function to easily save data, this module contains different
kinds of small fixes to the data attributes, coordinates, and metadata which
are necessary for the data field to be CMOR-compliant. We will come back to
these functionalities in a bit.


### 3. Finalizing the CMORizer

Once everything works as expected, there's a couple of things that we can still do.

- Add header info
- Make sure the metadata are added to the config file
- Maybe go through a checklist????
- add an entry to config-references?


The header contains information about where to obtain the data, when it was
accessed the last time, which ESMValTool "tier" it is associated with, and more
detailed information about the necessary downloading and processing steps.

> ## Fill out the header for the "FLUXCOM" dataset
>
> Fill out the information that is necessary in the header of a CMORizing
> script for the dataset "FLUXCOM". The different parts that need to be
> present in the header are the following:
> - caption: " """ESMValTool CMORizer for FLUXCOM GPP data."
> - Tier
> - Source
> - Last access
> - Download and processing instructions
>
> > ## Answers
> >
> > The header for the "FLUXCOM" dataset could look something like this:
> >
> > ```python
> > """ESMValTool CMORizer for FLUXCOM GPP data.
> >
> > Tier
> >     Tier 3: restricted dataset.
> >
> > Source
> >     http://www.bgc-jena.mpg.de/geodb/BGI/Home
> >
> > Last access
> >     20190727
> >
> > Download and processing instructions
> >     From the website, select FLUXCOM as the data choice and click download.
> >     Two files will be displayed. One for Land Carbon Fluxes and one for
> >     Land Energy fluxes. The Land Carbon Flux file (RS + METEO) using
> >     CRUNCEP data file has several data files for different variables.
> >     The data for GPP generated using the
> >     Artificial Neural Network Method will be in files with name:
> >     GPP.ANN.CRUNCEPv6.monthly.\*.nc
> >     A registration is required for downloading the data.
> >     Users in the UK with a CEDA-JASMIN account may request access to the jules
> >     workspace and access the data.
> >     Note : This data may require rechunking of the netcdf files.
> >     This constraint will not exist once iris is updated to
> >     version 2.3.0 Aug 2019
> > """
> > ```
> >
> > This is the header of the "FLUXCOM" CMORizer that is available with the
> > ESMValTool already. It is therefore entirely possible that your entries for
> > the section "Last access" and "Download and processing instructions"
> > differ from the example here since the entries for these sections are
> > somewhat subjective.
> >
> {: .solution}
{: .challenge}






> ## Was the CMORization successful so far?!
>
> If you check the folders in your output path, you should see the following
> folder structure: ``/Tier3/FLUXCOM/``
>
> Within the "FLUXCOM" folder there should be a NetCDF file named
> ``OBS_FLUXCOM_reanaly_ANN-v1_Lmon_gpp_xxxx01-xxxx12.nc``.
>
> The "xxxx" represents the start year of the data period you wanted to
> CMORize, and the "yyyy" represents the end year.
>
{: .callout}

Great!
So we have produced a NetCDF file with the CMORizer that follows the naming
convention for ESMValTool datasets. Let's have a look at the NetCDF file as
it was written with the very basic CMORizer from above (note, we are only
looking at the year 1980 in our example).

```bash
netcdf OBS_FLUXCOM_reanaly_ANN-v1_Lmon_gpp_198001-198012 {
dimensions:
        time = UNLIMITED ; // (12 currently)
        lat = 360 ;
        lon = 720 ;
variables:
        float GPP(time, lat, lon) ;
                GPP:_FillValue = 1.e+20f ;
                GPP:long_name = "GPP" ;
        double time(time) ;
                time:axis = "T" ;
                time:units = "days since 1582-10-15 00:00:00" ;
                time:standard_name = "time" ;
                time:calendar = "gregorian" ;
        double lat(lat) ;
        double lon(lon) ;

// global attributes:
                :_NCProperties = "version=2,netcdf=4.7.4,hdf5=1.10.6" ;
                :created_by = "Fabian Gans [fgans@bgc-jena.mpg.de], Ulrich Weber [uweber@bgc-jena.mpg.de]" ;
                :flux = "GPP" ;
                :forcing = "CRUNCEPv6" ;
                :institution = "MPI-BGC-BGI" ;
                :invalid_units = "gC m-2 day-1" ;
                :method = "Artificial Neural Networks" ;
                :provided_by = "Martin Jung [mjung@bgc-jena.mpg.de] on behalf of FLUXCOM team" ;
                :reference = "Jung et al. 2016, Nature; Tramontana et al. 2016, Biogeosciences" ;
                :temporal_resolution = "monthly" ;
                :title = "GPP based on FLUXCOM RS+METEO with CRUNCEPv6 climate " ;
                :version = "v1" ;
                :Conventions = "CF-1.7" ;
}
```

The file contains a variable named "GPP" that contains three dimensions:
"time", "lat", "lon". The units for this variable are not defined yet.
The ESMValTool did not know how to convert from the
original units to the units that are defined in the CMOR table for the
variable "gpp". The original units are therefore listed in the "global
attributes" section as ``invalid_units``. The ESMValTool recognized that the
units given in the original file did not match the units given in the CMOR
table and therefore put the information about the units in the metadata.

If we test this NetCDF file now with our CMOR checker
```bash
esmvaltool run recipe_check_fluxcom.yml --log_level debug
```
we will encounter an error again. The dataset is not correctly CMORized, and
the first thing that the ESMValTool complains about is:
```bash
iris.exceptions.UnitConversionError: Cannot convert from unknown units. The
"units" attribute may be set directly.
```

Ok, so let's fix the units of the "GPP" variable in the CMORizer. For that
we add the following three lines to the code in the section
``_extract_variable``:

```python
# convert data from gc/m2/day to kg/m2/s
cube = cube / (1000 * 86400)
cube.units = 'kg m-2 s-1'
```

The whole section should then look like this:

```python
def _extract_variable(cmor_info, attrs, filepath, out_dir):
    """Extract variable."""
    var = cmor_info.short_name
    logger.info("Var is %s", var)
    cubes = iris.load(filepath)
    for cube in cubes:
        # convert data from gc/m2/day to kg/m2/s
        cube = cube / (1000 * 86400)
        cube.units = 'kg m-2 s-1'

        logger.info("Saving file")
        utils.save_variable(cube,
                            var,
                            out_dir,
                            attrs,
                            unlimited_dimensions=['time'])

```

If we run the CMORizer script now again with the ``cmorize_obs`` call we
get a NetCDF file that has fixed units, but has an "unknown" variable name.

```bash
...
variables:
        float unknown(time, lat, lon) ;
                unknown:_FillValue = 1.e+20f ;
                unknown:units = "kg m-2 s-1" ;
        double time(time) ;
                time:axis = "T" ;
                time:units = "days since 1582-10-15 00:00:00" ;
                time:standard_name = "time" ;
                time:calendar = "gregorian" ;
        double lat(lat) ;
        double lon(lon) ;
...
```

We will have to do some more code tweaking then. But we also have not fixed
the problem with the coordinates ``lat`` and ``lon`` yet. There is no "units"
or "standard_name" given for either of these coordinates which will cause a
problem for the ESMValTool. Such some smaller formatting problems can occur
relatively often for coordinates like ``lat``, ``lon`` or ``time``. This means
that these problems need fixing in many CMORizers.
> ## Finalizing the "FLUXCOM" CMORizer
>
> The task is now to work with the functions in the file "utilities.py" to
> complete the CMORizer for the "FLUXCOM" dataset so that it can be read by
> the ESMValTool. The definitions of the coordinates and the variable "gpp"
> should look like the following after the successful CMORization:
>
> ```bash
> variables:
>         float gpp(time, lat, lon) ;
>                 gpp:_FillValue = 1.e+20f ;
>                 gpp:standard_name = "gross_primary_productivity_of_carbon" ;
>                 gpp:long_name = "Carbon Mass Flux out of Atmosphere due to Gross Primary Production on Land" ;
>                 gpp:units = "kg m-2 s-1" ;
>         double time(time) ;
>                 time:axis = "T" ;
>                 time:bounds = "time_bnds" ;
>                 time:units = "days since 1950-1-1 00:00:00" ;
>                 time:standard_name = "time" ;
>                 time:calendar = "gregorian" ;
>         double time_bnds(time, bnds) ;
>         double lat(lat) ;
>                 lat:axis = "Y" ;
>                 lat:bounds = "lat_bnds" ;
>                 lat:units = "degrees_north" ;
>                 lat:standard_name = "latitude" ;
>                 lat:long_name = "latitude coordinate" ;
>         double lat_bnds(lat, bnds) ;
>         double lon(lon) ;
>                 lon:axis = "X" ;
>                 lon:bounds = "lon_bnds" ;
>                 lon:units = "degrees_east" ;
>                 lon:standard_name = "longitude" ;
>                 lon:long_name = "longitude coordinate" ;
>         double lon_bnds(lon, bnds) ;
> ```
>
> For that to happen, you have to fix the following things:
> - adding standard names to the dimensions ``lat`` and ``lon``
> - fix the metadata for the variable "gpp"
> - change the time units to start in the year 1950
> - make the coordinates CMOR-compliant
> - set global attributes for the data file
>
> > ## Answers
> >
> > To fully CMORize the "FLUXCOM" dataset, you need to add the following
> > lines of code to the ``_extract_variable`` function of your CMORizer
> > script:
> >
> > ```python
> > def _extract_variable(cmor_info, attrs, filepath, out_dir):
> >     """Extract variable."""
> >     var = cmor_info.short_name
> >     logger.info("Var is %s", var)
> >     cubes = iris.load(filepath)
> >     for cube in cubes:
> >         # convert data from gc/m2/day to kg/m2/s
> >         cube = cube / (1000 * 86400)
> >         cube.units = 'kg m-2 s-1'
> >
> >         # The following two lines are needed for iris.util.guess_coord_axis
> >         cube.coord('lat').standard_name = 'latitude'
> >         cube.coord('lon').standard_name = 'longitude'
> >         utils.fix_var_metadata(cube, cmor_info)
> >         utils.convert_timeunits(cube, 1950)
> >         utils.fix_coords(cube)
> >         utils.set_global_atts(cube, attrs)
> >         utils.flip_dim_coord(cube, 'latitude')
> >         coord = cube.coord('latitude')
> >         coord.bounds = np.flip(coord.bounds, axis=1)
> >         logger.info("Saving file")
> >         utils.save_variable(cube,
> >                             var,
> >                             out_dir,
> >                             attrs,
> >                             unlimited_dimensions=['time'])
> >
> > ```
> >
> {: .solution}
{: .challenge}

So now we can finally run our new CMORizing script to produce an ESMValTool
readable datafile of the "FLUXCOM" dataset. As already mentioned, the call
to run a CMORizing script is as follows:

```bash
cmorize_obs -c <config-user.yml> -o <dataset-name>
```

If your run was successful, you should have gotten no error message in the
ESMValTool logging information and one or more NetCDF files should have been
produced in your output directory.

To make sure that the CMORization has really worked as planned, we run the
CMOR checker on this newly produced NetCDF file. If the ESMValTool can read
the data without problems, you should see as one of the last lines of
ESMValTool output:

```bash
INFO    Run was successful
```

## Last steps

Congratulations! You have successfully CMORized a new dataset!
Since you have gone through all the trouble to reformat the dataset so that
the ESMValTool can work with it, it would be great if you could provide the
CMORizer, and ultimately with that the dataset, to the rest of the community.
To do that there are a few more steps you have to do:
1. Check out the previous episode on [Contributing to ESMValTool](/08-development-setup)
1. Make sure that you have added the info of your dataset to the User Guide so
   that people know it is available for the ESMValTool [Obtaining input
   data](https://github.com/ESMValGroup/ESMValTool/blob/master/doc/sphinx/source/input.rst)
1. Make sure that there is a reference file available for the dataset [BibTeX
   info
   file](https://github.com/ESMValGroup/ESMValTool/tree/master/esmvaltool/references)
## Some final comments

Adding a new CMORizer to the ESMValTool is definitely already an advanced task
when working with the ESMValTool. You need to have a basic understanding of
how the ESMValTool works and how it's internal structure looks like. In
addition, you need to have a basic understanding of NetCDF files and a
programming language. In our example we used python for the CMORizing script
since we advocate for focusing the code development on only a few different
programming languages. This helps to maintain the code and to ensure the
compatibility of the code with possible fundamental changes to the structure
of the ESMValTool and ESMValCore.

More information about adding observations to the ESMValTool can be found in the
[documentation](https://docs.esmvaltool.org/en/latest/input.html#observations).




<!--
> ## RAWOBS, OBS, OBS6, native6!?
>
> ESMValTool uses project IDs to find the data on your hard drive, and also to
> find more information about the data. The `RAWOBS` and `OBS` projects were
> created for external data before and after CMORization, respectively. These
> names can be misleading, though, since not all external datasets are
> observations.
>
> Then, in going from CMIP5 to CMIP6, the CMOR standards changed a bit. Some
> variables are named differently in CMIP5 and CMIP6. This posed a dilemma:
> should CMORization reformat to the CMIP5 or CMIP6 definition? To solve this,
> the `OBS6` project was created. So data in the `OBS6` follow the CMIP6
> standards.
>
> `native6` is used to store external datasets for which we have implemented
> fixes in ESMValCore, so they can be CMORized 'on the fly'.
>
{: .callout} -->









<!-- ## Check if your variable is following the CMOR standard / Check if it's in a CMOR table

The very first step we have to do is to check if your data file follows the CMOR standard.
Only data files that fully follow this standard can be read by the ESMValTool.

Most variables that we would want to use with the ESMValTool are defined in the Coupled
Model Intercomparison Project (CMIP) data request and can be found in the
CMOR tables in the folder
[cmip6/Tables](https://github.com/ESMValGroup/ESMValCore/tree/master/esmvalcore/cmor/tables/cmip6/Tables),
differentiated according to the "MIP" they belong to. The tables are a
copy of the [PCMDI](https://github.com/PCMDI) guidelines.

> ## Find the variable "gpp" in a CMOR table
>
> Check the available CMOR tables to find the variable "gpp" with the following characteristics:
> - standard_name: ``gross_primary_productivity_of_biomass_expressed_as_carbon``
> - frequency: ``mon``
> - modeling_realm: ``land``
>
> > ## Answers
> >
> > The variable "gpp" belongs to the land variables. The temporal resolution that we are looking
> > for is "monthly". This information points to the "Lmon" CMIP table. And indeed, the variable
> > "gpp" can be found in the file
> > [here](https://github.com/ESMValGroup/ESMValCore/blob/master/esmvalcore/cmor/tables/cmip6/Tables/CMIP6_Lmon.json).
> >
> {: .solution}
{: .challenge}


If the variable you are interested in is not available in the standard CMOR
tables, you need to write a custom CMOR table entry for the variable. Examples
of custom CMOR table entries are for example the standard error of a specific
variable. For our variable "gpp" there is indeed no CMOR definition for the
standard error, therefore "gppStderr" was defined in the custom CMOR table
[here](https://github.com/ESMValGroup/ESMValCore/tree/master/esmvalcore/cmor/tables/custom),
as ``CMOR_gppStderr.dat``.

To create a new custom CMOR table you need to follow these
guidelines:

- Provide the ``variable_entry``;
- Provide the ``modeling_realm``;
- Provide the variable attributes, but leave ``standard_name`` blank. Necessary
  variable attributes are: ``units``, ``cell_methods``, ``cell_measures``,
  ``long_name``, ``comment``.
- Provide some additional variable attributes. Necessary additional variable
  attributes are: ``dimensions``, ``out_name``, ``type``. There are also
  additional variable attributes that can be defined here (see the already
  available cmorizers).

It is easiest to start a new custom CMOR table by using an existing custom table
as a template.
You can then edit the content and save it as ``CMOR_<short_name>.dat``.

> ## Does the variable "cVegStderr" need a costum CMOR table?
>
> Check the available CMOR tables to find the variable "cVegStderr" with the
> following characteristics:
> - standard_name: ``vegetation_carbon_content``
> - frequency: ``mon``
> - modeling_realm: ``land``
>
> If it is not available, create a custom CMOR table following the template of
> the CMIP6 CMOR table of "cVeg" and custom CMOR table of "gppStderr".
>
> > ## Answers
> >
> > The first step here is to check if the variable "cVegStderr" is already
> > listed in a CMOR table. We have the information that the modeling_realm
> > of the variable is ``land`` and that the frequency is ``mon``. This
> > means we have to check the CMOR table "Lmon" for an entry. We focus
> > our search on the CMIP6 tables since these are the most recent tables
> > available. You can find the lists here:
> > `<https://github.com/ESMValGroup/ESMValCore/tree/master/esmvalcore/cmor/tables/cmip6/Tables>`
> >
> > We do not find the variable "cVegStderr" in the "Lmon" table, which
> > means we will have to write our own custom table for this.
> > There were two examples given which we could use as templates for the
> > new custom table that we have to create. These two examples can be
> > found here: [cVeg](https://github.com/ESMValGroup/ESMValCore/blob/master/esmvalcore/cmor/tables/cmip6/Tables/CMIP6_Lmon.json)
> > and [gppStderr](https://github.com/ESMValGroup/ESMValCore/blob/master/esmvalcore/cmor/tables/custom/CMOR_gppStderr.dat)
> >
> > We have to create a new file with the name ``CMOR_cVegStrerr.dat`` in
> > the custom CMOR table folder (https://github.com/ESMValGroup/ESMValCore/blob/master/esmvalcore/cmor/tables/custom/).
> > The content of the file should then look like this:
> >
> > ```
> > SOURCE: CMIP6
> > !============
> > variable_entry:    cVegStderr
> > !============
> > modeling_realm:    land
> > !----------------------------------
> > ! Variable attributes:
> > !----------------------------------
> > standard_name:
> > units:             kg m-2
> > cell_methods:      area: mean where land time: mean
> > cell_measures:     area: areacella
> > long_name:         Carbon Mass in Vegetation Error
> > !----------------------------------
> > ! Additional variable information:
> > !----------------------------------
> > dimensions:        longitude latitude time
> > out_name:          cVegStderr
> > type:              real
> > !----------------------------------
> > !
> > ```
> >
> > Note that there is no entry for ``standard_name``. This is on purpose.
> > It is a sign for the ESMValTool to not crash although the variable that
> > we are looking for is ok to have no official CMIP6 ``standard_name``.
> >
> {: .solution}
{: .challenge} -->
