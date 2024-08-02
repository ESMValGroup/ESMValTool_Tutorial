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
by Peter Kalverla

## Start ARE session
Log in to [ARE][are]{:target="_blank"} with your NCI account to start a JupyterLab session.
Refer to this [ARE setup guide]({{ page.root }}{% link _extras/02-aresetup.md %}) for more details.
Open the folder to your hackathon folder in `nf33` where you can create a new notebook or use the 
example notebook.

## Find Datasets with facets

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
search from files (locally and ESGF) with wildcard functionality `'*'`

```python
CFG['search_esgf'] = 'always'
dataset_search = Dataset(
    short_name='tos',
    mip='Omon',
    project='CMIP6',
    exp='historical',
    dataset='CESM2',
    ensemble='*',
    grid='gn',
)
ensemble_datasets = list(dataset_search.from_files())

print([ds['ensemble'] for dataset in ensemble_datasets])
```

## Add supplementary variables

```python
# Discard augmented facets as they will be different for areacello
dataset = Dataset(**dataset.minimal_facets)

# Add areacello as supplementary dataset
dataset.add_supplementary(short_name='areacello', mip='Ofx')

# Autocomplete and inspect
dataset.augment_facets()
print(dataset.summary())
```

Loading the data can be done with a method.

```python
# Before load
print(dataset.files)

cube = dataset.load()
cube
```


## Preprocessors

```python
from esmvalcore.preprocessor import annual_statistics, anomalies, area_statistics

cube = area_statistics(cube, operator='mean')
cube = anomalies(cube, reference=reference_period, period='month')
cube = annual_statistics(cube, operator='mean')
cube.convert_units('degrees_C')
cube
```
- arguments for functions 

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
