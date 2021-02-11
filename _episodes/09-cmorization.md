---
title: "CMORization: Using observational datasets"
teaching: 15
exercises: 45

questions:
- "What is so challenging about observational data?"
- "How do I use observational datasets in ESMValTool?"
- "How add support for new (observational) datasets?"

objectives:
- "Understand what CMORization is and why it is necessary."
- "Learn how to write a new CMORizer script."

keypoints:
- "CMORizers are dataset-specific scripts that can be run once to generate CMOR-compliant data."
- "ESMValTool comes with a set of CMORizers readily available, but you can also add your own."
---

## Introduction

This episode deals with "CMORization". ESMValTool was designed to work with data
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
CMOR tables. As example, the CMOR tables for the CMIP6 project can be found
[here](https://github.com/PCMDI/cmip6-cmor-tables).
{: .callout}

The Earth System Grid Federation (ESGF) is home to all CMIP data. The data
hosted there typically (mostly) follow the CMIP standards, and therefore
ESMValTool should work with these data without problems.

Datasets that are *not* part of one of the CMIP projects often don't follow the
CMOR standards. In this case, a reformatting script can be used to create a
CMOR-compliant copy of these datasets. CMORizer scripts for several popular
datasets are included in ESMValTool, and ESMValTool also provides a convenient
way to execute them.

Occasionally it happens that there are still minor issue with CMIP datasets. In
those cases, it is possible to fix those issues in ESMValCore before any further
processing is done. The same can be done for non-CMIP data. The advantage is
that you don't need to store an additional, reformatted copy of the data. The
disadvantage is that these fixes should be implemented inside ESMValCore.
Writing a CMORizer script is technically is simpler.

The concepts discussed so far are illustrated in the figure below.
![Data flow with ESMValTool](../fig/data_flow.png)
*Illustration of the data flow in ESMValTool.*

In this lesson, we will re-implement a CMORizer script for the FLUXCOM dataset that
contains observations of the Gross Primary Production (GPP), a variable that is
important for calculating components of the global carbon cycle. We will go through all
the steps and explain relevant topics as we go. If you prefer to implement CMOR
fixes, please read the documentation
[here](https://docs.esmvaltool.org/projects/esmvalcore/en/latest/develop/fixing_data.html#fixing-data).
While fixes are implemented slightly differently, conceptually the process is
the same and the concepts explained in this episode are still useful.

## 1. Check if your variable is following the CMOR standard

The very first step we have to do is to check if your data file follows the CMOR standard. Only data files that fully follow this standard can be read by the ESMValTool.

Most variables that we would want to use with the ESMValTool are defined in the Coupled Model Intercomparison Project (CMIP) data request and can be found in the
CMOR tables in the folder `<https://github.com/ESMValGroup/ESMValCore/tree/master/esmvalcore/cmor/tables/cmip6/Tables>`,
differentiated according to the ``MIP`` they belong to. The tables are a
copy of the `PCMDI <https://github.com/PCMDI>` guidelines.

> ## Find the variable "gpp" in a CMOR table
>
> Check the available CMOR tables to find the variable ``gpp`` with the following characteristics:
> - standard_name: ``gross_primary_productivity_of_biomass_expressed_as_carbon``
> - frequency: ``mon``
> - modeling_realm: ``land``
>
> > ## Answers
> >
> > The variable ``gpp``belongs to the land variables. The temporal resolution that we are looking for is ``monthly``.
> > This information points to the "Lmon" CMIP table. And indeed, the variable ``gpp`` can be found in the file
> > `<https://github.com/ESMValGroup/ESMValCore/blob/master/esmvalcore/cmor/tables/cmip6/Tables/CMIP6_Lmon.json>`.
> >
> {: .solution}
{: .challenge}


If the variable you are interested in is not available in the standard CMOR tables,
you need to write a custom CMOR table entry for the variable. Don't worry! It sounds more complicated than it is!
Examples of custom CMOR table entries are for example the standard error of a specific variable.
For our variable ``gpp`` there is indeed no CMOR definition for the standard error, therefore ``gppStderr`` was defined in the custom CMOR table here:
`<https://github.com/ESMValGroup/ESMValCore/tree/master/esmvalcore/cmor/tables/custom>`, as ``CMOR_gppStderr.dat``.

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

It is easiest to start a new custom CMOR table by using an existing custom table as a template.
You can then edit the content and save it as ``CMOR_<short_name>.dat``.

> ## Does the variable ``cVegStderr`` need a costum CMOR table?
>
> Check the available CMOR tables to find the variable ``cVegStderr`` with the
> following characteristics:
> - standard_name: ``vegetation_carbon_content``
> - frequency: ``mon``
> - modeling_realm: ``land``
>
> If it is not available, create a custom CMOR table following the template of
> the CMIP6 CMOR table of ``cVeg`` and custom CMOR table of ``gppStderr``.
>
> > ## Answers
> >
> > The first step here is to check if the variable ``cVegStderr`` is already
> > listed in a CMOR table. We have the information that the modeling_realm 
> > of the variable is ``land`` and that the frequency is ``mon``. This 
> > means we have to check the CMOR table ``Lmon`` for an entry. We focus
> > our search on the CMIP6 tables since these are the most recent tables
> > available. You can find the lists here: 
> > `<https://github.com/ESMValGroup/ESMValCore/tree/master/esmvalcore/cmor/tables/cmip6/Tables>`
> >
> > We do not find the variable ``cVegStderr`` in the ``Lmon`` table, which
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
{: .challenge}


## 2. Store your dataset in the right place

Now that we have made sure that we have a CMOR definition of our variable
the next step in writing our own cmorizer script is to make sure that the
ESMValTool will be able to find our data file that we want to use. Since
it is not cmorized yet, it cannot be stored in the regular observations
and reanalysis data folders. For that purpose it is great to create a 
``RAWOBS`` folder in which you would store all non-cmorized data files.
This folder needs the same sub-folder structure as the OBS folder we already
know. In our case of the ``FLUXCOM`` data, we need a sub-folder called
``Tier3`` in the folder ``RAWOBS`` and then a sub-folder called ``FLUXCOM``
in the sub-folder ``Tier3``.

> ## What is the deal with those ``tiers``?
>
> Many observations and reanalysis datasets are restricted in their access.
> This is due to the huge amount of work that goes into creating these datasets
> and the fact that the dataset creators feel a responisbility about what their
> dataset is used for. In many cases "restricted access" just means that one 
> has to register with an email address and is adivsed to achnowledge the data
> providers in scientific publications. After that one is free to download the
> data without any other hurdles.
> 
> However, there are also datasets available that do not need a registration
> for access, e.g. the "obs4MIPs" or "ana4MIPs" datasets that are specifically
> produced to facilitate comparisons with model simulations.
> 
> To reflect these different levels of access restriction, the ESMValTool team
> has created a tier-system with which the different observations and 
> reanalysis datasets are described. The definition of the different tiers are 
> as follows:
> - Tier1: obs4MIPs and ana4MIPS datasets (they do not need any additional 
> cmorization before they can be used with the ESMValTool)
> - Tier2: any other freely available datasets (most of them will need some 
> kind of cmorization before they can be used with the ESMValTool)
> - Tier3: any datasets that has an access restriction, meaning someone has to
> register to access the data (most of these datasets will also need some kind
> of cmorization before they can be used with the ESMValTool)
>
> The access restrictions are also the reason why it is not possible to 
> providers the ESMValTool routinely during the installation with all 
> observations and reanalysis data that are used in all example recipes. 
> However, the cmorization scripts for these datasets are provided with the
> ESMValTool so that each user can cmorize their own copy of the access 
> restricted datasets if they need them.
>
{: .callout}


## 3. Edit your configuration file

The next step then is to make sure that the ESMValTool will find the 
``FLUXCOM`` data in the ``RAWOBS`` folder when it is running the cmorizing 
script. For this to happen we have to specify the path to the ``RAWOBS`` 
folder in our configuration file. In the same way as the path to the ``OBS`` 
folder is defined, we define there our path to the ``RAWOBS`` folder:

```yaml
rootpath:
  OBS:      /path/to/my/obs/data
  RAWOBS:   /path/to/my/rawobs/data
```

## 4. Naming convention of the observational data files

For the ESMValTool to be able to read the observations from the NetCDF file,
the file name needs a very specific structure and order of information parts.
The file name will be automatically correctly created if a cmorizing
script has been used to create the netCDF file.

The correct structure of an observational data set is defined in
[config-developer.yml]
(https://github.com/ESMValGroup/ESMValCore/blob/master/esmvalcore/config-developer.yml),
and looks like the following:

```bash
OBS_[dataset]_[type]_[version]_[mip]_[short_name]_YYYYMM-YYYYMM.nc
```

For the example of the ``FLUXCOM`` data set, the correct structure of the
file name looks then like this:

```bash
OBS_FLUXCOM_reanaly_ANN-v1_Lmon_gpp_198001-198012.nc
```

The different parts of the name are explained in more detail here:

- OBS: describes what kind of data can be expected in the file, in this case
  ``observations``;
- FLUXCOM: that is the name of the dataset;
- reanaly: describes the source of the data, here we are looking at reanalysis 
  data (therefore ``reanaly``), could also be ``sat`` for satellite data;
- ANN-v1: describes the version of the dataset:
- Lmon: is the information in which ``mip`` the variable is to be expected, and
  what kind of temporal resolution it has; here we expect ``gpp`` to be part
  of the land realm (``L``) and we have the dataset in a monthly resolution
  (``mon``);
- gpp: Is the name of the variable. Each observational data file is supposed
  to only include one variable per file;
- 198001-198012: Is the period the dataset spans with ``198001`` being the
  start year and month, and ``198012`` being the end year and month;

> ## Note
>
> There is a different naming convention for ``obs4mips`` data (see the exact
> specifications for the obs4mips data file naming convention in the
> ``config-developer.yml`` file).
{: .callout}


## 5. Create a CMORizer for the new dataset

ESMValTool can work with CMORizer scripts written in various programming 
languages, but most of them are written in either Python or NCL. In this 
tutorial we will implement our CMORizer script in Python, but the steps 
that one needs to consider when writing a cmorizer are the same for any 
other language.

As mentioned before, we will re-implement the cmorizing script for the 
``FLUXCOM`` dataset that is available at the MPI for Biogeochemistry in Jena: 
[cmorize_obs_fluxcom.py]
(https://github.com/ESMValGroup/ESMValTool/blob/master/esmvaltool/cmorizers/obs/cmorize_obs_fluxcom.py).

In a first step we need to create a configuration file for the dataset. This
is necessary to allow the ESMValTool to retrieve all necessary information 
about the datasets, and to write the filename of the cmorized variable from 
this dataset correctly. This configutation file needs to be stored in the 
following folder: 
``ESMValTool/esmvaltool/cmorizers/obs/cmor_config/``
It is imporant to note that the name of the configuration file has to be 
identical to the name of our dataset. For our example the configuration file
therefore must be called ``FLUXCOM.yml``, and it can be found here:
[FLUXCOM.yml]
(https://github.com/ESMValGroup/ESMValTool/blob/master/esmvaltool/cmorizers/obs/cmor_config/FLUXCOM.yml)

Let's have a closer look what that configuration file contains:

```yaml
---
# Filename 
filename: 'GPP.ANN.CRUNCEPv6.monthly.*.nc'

# Common global attributes for Cmorizer output
attributes:
  dataset_id: FLUXCOM
  version: 'ANN-v1'
  tier: 3
  modeling_realm: reanaly
  project_id: OBS
  source: 'http://www.bgc-jena.mpg.de/geodb/BGI/Home'
  reference: 'fluxcom'
  comment: ''

# Variables to cmorize
variables:
  gpp:
    mip: Lmon
```

The first part of this configuration file defines the filename of the raw
observations file. The second part defines the common global attributes for
the cmorizer output, e.g. information that is needed to piece together the
final observations file name in the correct structure 
(see Section `6. Naming convention of the observational data files`).
Another global attribute is ``reference`` which includes a ``doi`` related to 
the dataset. Please see the section `adding references
<https://docs.esmvaltool.org/en/latest/community/diagnostic.html#adding-references>`
on how to add reference tags to the ``reference`` section in the configuration 
file. If a single dataset has more than one reference,
it is possible to add tags as a list e.g. ``reference: ['tag1', 'tag2']``.
The third part in the configuration file defines the variables that are 
supposed to be cmorized.

in the directory ``ESMValTool/esmvaltool/cmorizers/obs/cmor_config/``. Note
that the name of this configuration file has to be identical to the name of
your data set. It is recommended that you set ``project`` to ``OBS6`` in the
configuration file. That way, the variables defined in the CMIP6 CMOR table,
augmented with the custom variables described above, are available to your script.

The actual cmorizing script ``cmorize_obs_mte.py`` consists of a header with
information on where and how to download the data, and noting the last access
of the data webpage.

The main body of the CMORizer script must contain a function called

```python
def cmorization(in_dir, out_dir, cfg, config_user):
```

with this exact call signature. Here, ``in_dir`` corresponds to the input
directory of the raw files, ``out_dir`` to the output directory of final
reformatted data set and ``cfg`` to the configuration dictionary given by
the  ``.yml`` configuration file. The return value of this function is ignored. All
the work, i.e. loading of the raw files, processing them and saving the final
output, has to be performed inside its body. To simplify this process, ESMValTool
provides a set of predefined utilities.py_, which can be imported into your CMORizer
by

```python
from . import utilities as utils
```

Apart from a function to easily save data, this module contains different kinds
of small fixes to the data attributes, coordinates, and metadata which are
necessary for the data field to be CMOR-compliant.

Note that this specific CMORizer script contains several subroutines in order
to make the code clearer and more readable (we strongly recommend to follow
that code style). For example, the function ``_get_filepath`` converts the raw
filepath to the correct one and the function ``_extract_variable`` extracts and
saves a single variable from the raw data.

.. utilities.py: https://github.com/ESMValGroup/ESMValTool/blob/master/esmvaltool/cmorizers/obs/utilities.py


## 6. Run the CMORizer script

The cmorizing script for the given dataset can be run with:

```bash
cmorize_obs -c <config-user.yml> -o <dataset-name>
```

> ## Note
>
> The output path given in the configuration file is the path where
> your cmorized dataset will be stored. The ESMValTool will create a folder
> with the correct tier information (see Section `2. Edit your configuration file`) if that tier folder is not
> already available, and then a folder named after the data set. In this
> folder the cmorized data set will be stored as a netCDF file.
{: .callout}

If your run was successful, one or more NetCDF files are produced in your
output directory.

> ## Check that your cmoriziation was successful
>
> Check your output directory for the freshly produced NetCDF file.
>
> > ## Answers
> >
> > Write the answer here.
> >
> {: .solution}
{: .challenge}


## 7. Make a test recipe

Let's start with the end goal: we want to be able to run a recipe that is able
to load our data. So we will make this recipe and try to run it. It will
probably fail, but that's okay. As you will see, the error messages will come in
handy. Moreover, the recipe will serve as a test case. Once we get it to run, we
know we have completed our task.

> ## Create a test recipe
>
> Create a simple recipe that loads the fluxnet data. It should include a
> datasets section with a single entry for the fluxnet dataset with the correct
> dataset keys, and a diagnostics section with two variables: gpp and gppStderr.
> We won't need any preprocessors or scripts (set `scripts: null`), but you will
> have to add a documentation section with a description, authors and
> maintainer, otherwise the recipe will fail.
>
> > ## Answer
> >
> > Here's an example recipe
> >
> > ```yaml
> > documentation:
> >
> >   description: Test recipe for fluxnet data
> >
> >   authors:
> >     - kalverla_peter
> >
> >   maintainer:
> >     - kalverla_peter
> >
> > datasets:
> >   - {project: OBS6, dataset: fluxnet, mip: Lmon, tier: 3, start_year: 2010, end_year: 2015, type: reanaly, version: latestversion}
> >
> > diagnostics:
> >   fluxnet:
> >     description: Check that ESMValTool can load the cmorized fluxnet data without errors.
> >     variables:
> >       gpp:
> >       gppStderr:
> >     scripts: null
> >
> > ```
> >
> > Note: a recipe similar to this one is available under
> > `~/path/to/ESMValTool/esmvaltool/recipes/examples/recipe_check_obs.yml`.
> > That recipe includes checks all datasets for which CMORizers are available.
> >
> {: .solution}
{: .challenge}

Try to run the example recipe with

```bash
esmvaltool run recipe_check_fluxnet.yml --log_level debug
```

The `log_level` flag ensures that all relevant information is included in the
output. We will need this information to find out what needs to be done. Look
carefully through the log messages. You'll probably find something like

```
DEBUG   Retrieving OBS6 configuration
DEBUG   Skipping non-existent /home/peter/default_inputpath/Tier3/fluxnet
DEBUG   Looking for files matching ['OBS6_fluxnet_reanaly_latestversion_Lmon_gppStderr[_.]*nc'] in []
ERROR   No input files found for variable {'variable_group': 'gppStderr' ...
...
ERROR   Missing data for preprocessor fluxnet/gppStderr
...
ERROR   Program terminated abnormally, see stack trace below for more information:
...
```
{: .error}

So the output tells us that it cannot find the data, and also where it has been
looking. This makes sense, as we have not yet created a CMORized copy of the data.
But it is always useful to know where ESMValTool will look for it later on.

## Conclusion


[documentation](https://docs.esmvaltool.org/en/latest/input.html#observations)

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
