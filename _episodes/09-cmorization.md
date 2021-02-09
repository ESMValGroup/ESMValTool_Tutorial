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

Include some theory and a nice explanatory figure. Point to the
[documentation](https://docs.esmvaltool.org/en/latest/input.html#observations)

In this lesson, we will re-implement a CMORizer script for .... (include
interesting datasets). We will go through all the steps and explain relevant topics as we go.

## 1. Check if your variable is CMOR standard

Most variables are defined in the CMIP data request and can be found in the
CMOR tables in the folder `/esmvalcore/cmor/tables/cmip6/Tables/
<https://github.com/ESMValGroup/ESMValCore/tree/master/esmvalcore/cmor/tables/cmip6/Tables>`,
differentiated according to the ``MIP`` they belong to. The tables are a
copy of the `PCMDI <https://github.com/PCMDI>` guidelines. If you find the
variable in one of these tables, you can proceed to the next section.

If your variable is not available in the standard CMOR tables,
you need to write a custom CMOR table entry for the variable
as outlined below and add it to `/esmvalcore/cmor/tables/custom/
<https://github.com/ESMValGroup/ESMValCore/tree/master/esmvalcore/cmor/tables/custom>`.

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

It is recommended to use an existing custom table as a template, to edit the
content and save it as ``CMOR_<short_name>.dat``.

## 2. Edit your configuration file

Make sure that beside the paths to the model simulations and observations, also
the path to raw observational data to be cmorized (``RAWOBS``) is present in
your configuration file.

## 3. Store your dataset in the right place

The folder ``RAWOBS`` needs the subdirectories ``Tier1``, ``Tier2`` and
``Tier3``. The different tiers describe the different levels of restrictions
for downloading (e.g. providing contact information, licence agreements)
and using the observations. The unformatted (raw) observations
should then be stored then in the appropriate of these three folders.

## 4. Create a CMORizer for the dataset

ESMValTool can work with CMORizer script written in various programming languages. In the tutorial we will implement our CMORizer script in Python, but the steps would be the same for any other language.

Our example of a cmorizing script here is written for the ``MTE`` dataset
that is available at the MPI for Biogeochemistry in Jena: `cmorize_obs_mte.py
<https://github.com/ESMValGroup/ESMValTool/blob/master/esmvaltool/cmorizers/obs/cmorize_obs_mte.py>`.
It provides data about the Gross Primary Production (GPP).

All the necessary information about the dataset to write the filename
correctly, and which variable is of interest, is stored in a seperate
configuration file: `MTE.yml
<https://github.com/ESMValGroup/ESMValTool/blob/master/esmvaltool/cmorizers/obs/cmor_config/MTE.yml>`
in the directory ``ESMValTool/esmvaltool/cmorizers/obs/cmor_config/``. Note
that the name of this configuration file has to be identical to the name of
your data set. It is recommended that you set ``project`` to ``OBS6`` in the
configuration file. That way, the variables defined in the CMIP6 CMOR table,
augmented with the custom variables described above, are available to your script.

The first part of this configuration file defines the filename of the raw
observations file. The second part defines the common global attributes for
the cmorizer output, e.g. information that is needed to piece together the
final observations file name in the correct structure (see Section `6. Naming convention of the observational data files`).
Another global attribute is ``reference`` which includes a ``doi`` related to the dataset.
Please see the section `adding references
<https://docs.esmvaltool.org/en/latest/community/diagnostic.html#adding-references>`
on how to add reference tags to the ``reference`` section in the configuration file.
If a single dataset has more than one reference,
it is possible to add tags as a list e.g. ``reference: ['tag1', 'tag2']``.
The third part in the configuration file defines the variables that are supposed to be cmorized.

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


## 5. Run the CMORizer script

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


## 6. Naming convention of the observational data files

This is quite theoretical. Maybe it should come earlier on in the tutorial?

For the ESMValTool to be able to read the observations from the NetCDF file,
the file name needs a very specific structure and order of information parts
(very similar to the naming convention for observations in ESMValTool
v1.0). The file name will be automatically correctly created if a cmorizing
script has been used to create the netCDF file.

The correct structure of an observational data set is defined in
`config-developer.yml
<https://github.com/ESMValGroup/ESMValCore/blob/master/esmvalcore/config-developer.yml>`,
and looks like the following:

```bash
OBS_[dataset]_[type]_[version]_[mip]_[short_name]_YYYYMM-YYYYMM.nc
```

For the example of the ``CDS-XCH4`` data set, the correct structure of the
file name looks then like this:

```bash
OBS_CDS-XCH4_sat_L3_Amon_xch4_200301-201612.nc
```

The different parts of the name are explained in more detail here:

- OBS: describes what kind of data can be expected in the file, in this case
  ``observations``;
- CDS-XCH4: that is the name of the dataset. It has been named this way for
  illustration purposes (so that everybody understands it is the xch4 dataset
  downloaded from the CDS), but a better name would indeed be ``ESACCI-XCH4``
  since it is a ESA-CCI dataset;
- sat: describes the source of the data, here we are looking at satellite data
  (therefore ``sat``), could also be ``reanaly`` for reanalyses;
- L3: describes the version of the dataset:
- Amon: is the information in which ``mip`` the variable is to be expected, and
  what kind of temporal resolution it has; here we expect ``xch4`` to be part
  of the atmosphere (``A``) and we have the dataset in a monthly resolution
  (``mon``);
- xch4: Is the name of the variable. Each observational data file is supposed
  to only include one variable per file;
- 200301-201612: Is the period the dataset spans with ``200301`` being the
  start year and month, and ``201612`` being the end year and month;

> ## Note
>
> There is a different naming convention for ``obs4mips`` data (see the exact
> specifications for the obs4mips data file naming convention in the
> ``config-developer.yml`` file).
{: .callout}

## 7. Test the CMORized dataset

To verify that the cmorized data file is indeed correctly formatted, you can
run a dedicated test recipe, that does not include any diagnostic, but only
reads in the data file and has it processed in the preprocessor. Such a recipe
is called ``recipes/examples/recipe_check_obs.yml``. You just need to add a
diagnostic for your dataset following the existing entries.
Only the diagnostic of interest needs to be run, the others should be commented
out for testing.

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
