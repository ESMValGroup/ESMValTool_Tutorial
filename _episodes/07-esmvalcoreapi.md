---
title: "Advanced Jupyter notebook"
teaching: 20
exercises: 30
compatibility: ESMValTool v2.11.0

questions:
- "How to find data for ESMValTool in a Jupyter Notebook?"
- "How to use preprocessor functions?"
objectives:
- "Introduce and use the Dataset object"
- "Import and use included preprocessor functions"
- "View and check the data"
keypoints:
- "API can be used as a helper to develop recipes"
- "Preprocessors can be used in a Jupyter Notebook to check the output"
---

In this episode we will introduce the ESMValCore API in a jupyter notebook. This is reformatted from material from
this [blog post](https://blog.esciencecenter.nl/easy-ipcc-powered-by-esmvalcore-19a0b6366ea7){:target="_blank"}
by Peter Kalverla. There's also material from the [example notebooks][docs-notebooks]{:target="_blank"} and the
[API reference documentation][api-reference]{:target="_blank"}.

## Start ARE session
Log in to [ARE][are]{:target="_blank"} with your NCI account to start a JupyterLab session.
Refer to this [ARE setup guide]({{ page.root }}{% link _extras/02-aresetup.md %}) for more details.
Open the folder to your hackathon folder in `nf33` where you can create a new notebook or use the 
example notebook.

## Find Datasets with facets
We have seen from running available recipes that ESMValTool is able to find data from facets that were given in
the recipe. We can use this in a Notebook, including filling out the facets for data definition. 
To do this we will use the `Dataset` object from the API. Let's look at this example. 

```python
from esmvalcore.dataset import Dataset

dataset = Dataset(
    short_name='tos',
    mip='Omon',
    project='CMIP6',
    exp='historical',
    dataset='CESM2',
    ensemble='r4i1p1f1',
    grid='gn',
)
dataset.augment_facets()
print(dataset)
```
> ### Tip: 
> when running a recipe there is a `_filled` recipe in the output which augments the facets.
{: .callout}

> Search from files locally with wildcard functionality `'*'` to get the available datasets. 
> How can you search for all available ensembles?
> 
> > ## Solution
> > ```python
> > CFG['search_esgf'] = 'always'
> > dataset_search = Dataset(
> >     short_name='tos',
> >     mip='Omon',
> >     project='CMIP6',
> >     exp='historical',
> >     dataset='CESM2',
> >     ensemble='*',
> >     grid='gn',
> > )
> > ensemble_datasets = list(dataset_search.from_files())
> > 
> > print([ds['ensemble'] for dataset in ensemble_datasets])
> > ```
> {: .solution}
{: .challenge}

> ## Add supplementary variables
> Supplementary variables can be added to the `Dataset` object which can be used for certain 
> preprocessors such as area statistics and weighting. Add the area file to this Dataset.
>
> > ## Solution
> > ```python
> > # Discard augmented facets as they will be different for areacello
> > dataset = Dataset(**dataset.minimal_facets)
> > 
> > # Add areacello as supplementary dataset
> > dataset.add_supplementary(short_name='areacello', mip='Ofx')
> > 
> > # Autocomplete and inspect
> > dataset.augment_facets()
> > print(dataset.summary())
> > ```
> {: .solution}
{: .challenge}


> ## Loading the data and inspect
> 
> ```python
> # Before load
> print(dataset.files)
> 
> cube = dataset.load()
> cube
> ```
> > ## Output
> > ```output
> > ```
> {: .solution}
{: .challenge}

## Preprocessors
As mentioned in previous lessons, the idea of preprocessors are that they are a set of 
functions that can be applied in a centralised, documented and efficient way. There 
are a broad range of operations that are commonly done to input data before diagnostics 
or metrics are applied and can be done to all the datasets in a recipe consistently. 
See the [documentation][recipe-section-preprocessors]{:target="_blank"}.

> ## Exercise: apply preprocessors using the API 
> See [API reference][api-preprocessors]{:target="_blank"} to check the 
> arguments for preprocessor functions. For this exercise, find the global mean, then
> anomalies which we can get monthly, then aggregate to annually for plotting.
> 
> > ## Solution
> > ```python
> > from esmvalcore.preprocessor import annual_statistics, anomalies, area_statistics
> > 
> > # Set the reference period for anomalies 
> > reference_period = {
> >     "start_year": 1950, "start_month": 1, "start_day": 1,
> >     "end_year": 1979, "end_month": 12, "end_day": 31,
> > }
> > 
> > cube = area_statistics(cube, operator='mean')
> > cube = anomalies(cube, reference=reference_period, period='month')
> > cube = annual_statistics(cube, operator='mean')
> > cube.convert_units('degrees_C')
> > cube
> > ```
> {: .solution}
{: .challenge}

## Custom code
Continue with other libraries and make custom plots
```python
import xarray as xr
da = xr.DataArray.from_iris(cube)
da.plot()
print(da)
```
## Build workflow and diagnostic

> ## Solution
> ```python
> import cf_units
> import matplotlib.pyplot as plt
> from iris import quickplot
> 
> from esmvalcore.config import CFG
> from esmvalcore.dataset import Dataset
> from esmvalcore.preprocessor import annual_statistics, anomalies, area_statistics
> 
> 
> # Settings for automatic ESGF search
> CFG['search_esgf'] = 'when_missing'
> 
> # Declare common dataset facets
> template = Dataset(
>     short_name='tos',
>     mip='Omon',
>     project='CMIP6',
>     exp= '*', # We'll fill this below
>     dataset='*',  # We'll fill this below
>     ensemble='r4i1p1f1',
>     grid='gn',
> )
> 
> # Substitute data sources and experiments
> datasets = []
> for dataset_id in ["CESM2", "MPI-ESM1-2-LR", "IPSL-CM6A-LR"]:
>     for experiment_id in ['ssp126', 'ssp585']:
>         dataset = template.copy(dataset=dataset_id, exp=['historical', experiment_id])
>         dataset.add_supplementary(short_name='areacello', mip='Ofx', exp='historical')
>         dataset.augment_facets()
>         datasets.append(dataset)
> 
> # Set the reference period for anomalies 
> reference_period = {
>     "start_year": 1950, "start_month": 1, "start_day": 1,
>     "end_year": 1979, "end_month": 12, "end_day": 31,
> }
> 
> # (Down)load, pre-process, and plot the cubes
> for dataset in datasets: 
>     cube = dataset.load()
>     cube = area_statistics(cube, operator='mean')
>     cube = anomalies(cube, reference=reference_period, period='month')  # notice 'month'
>     cube = annual_statistics(cube, operator='mean')
>     cube.convert_units('degrees_C')
> 
>     # Make sure all datasets use the same calendar for plotting
>     tcoord = cube.coord('time')
>     tcoord.units = cf_units.Unit(tcoord.units.origin, calendar='gregorian')
> 
>     # Plot
>     quickplot.plot(cube, label=f"{dataset['dataset']} - {dataset['exp']}")
> 
> # Show the plot
> plt.legend()
> plt.show()
> ```
{: .solution}

## Convert to recipe
```python
from esmvalcore.dataset import datasets_to_recipe
import yaml

for dataset in ensemble_datasets:
    dataset.facets['diagnostic'] = 'easy_ipcc'
print(yaml.safe_dump(datasets_to_recipe(datasets)))
```

> ## Output
> ```yaml
> datasets:
> - dataset: CESM2
>   ensemble: r4i1p1f1
>   institute: NCAR
> - dataset: IPSL-CM6A-LR
>   ensemble: r4i1p1f1
>   institute: IPSL
> - dataset: MPI-ESM1-2-LR
>   ensemble: r4i1p1f1
>   institute: MPI-M
> - dataset: TaiESM1
>   ensemble: r1i1p1f1
>   institute: AS-RCEC
> - dataset: AWI-CM-1-1-MR
>   ensemble: r(1:5)i1p1f1
>   institute: AWI
> - dataset: AWI-ESM-1-1-LR
>   ensemble: r1i1p1f1
>   institute: AWI
> - dataset: BCC-CSM2-MR
>   ensemble: r(1:3)i1p1f1
>   institute: BCC
> ...
> 
> diagnostics:
>   easy_ipcc:
>     variables:
>       tos:
>         exp:
>         - historical
>         - ssp585
>         grid: gn
>         mip: Omon
>         project: CMIP6
> ```
{: .solution}

{% include links.md %}
