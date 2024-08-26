---
title: "ILAMB support for RAW ACCESS-ESM outputs"
teaching: 15
exercises: 15
compatibility: ILAMB v2.7.1

questions:
- "What do we mean by CMORising?"
- "How to use ilamb-tree-generator to CMORise Raw Access data"
objectives:
- "Analyse raw (non-CMORised) ACCESS outputs with the ILAMB"
keypoints:
- "The ILAMB-Workflow only support RAW ACCESS data"
- "Running the ILAMB-Workflow on RAW ACCESS data can take some time. Consider if it is appropriate for your work"
- "Only a limited number of CMIP variables are supported"
---

In this episode we will introduce how to use `ilamb-tree-generator` as a CMORiser to help you use `ILAMB` to evaluate Access raw output. But before that, we will introduce what is 'CMORise' first.

## What is CMORisation?

"CMORise" refers to the process of converting climate model output data into a standardized format that conforms to the Climate and Forecast (CF) metadata conventions. This process involves using the Climate Model Output Rewriter (CMOR) tool, which ensures that the data adheres to specific requirements for structure, metadata, and units, making it easier to compare and share across different climate models.

## Use `ilamb-tree-generator` to CMORise Access raw output

### Load the ILAMB-Workflow module

The`ilamb-tree-generator` is available in the **ILAMB-Workflow** module that can be loaded as follow:

```bash
module use /g/data/xp65/public/modules
module load ilamb-workflow
```
or
```bash
module use /g/data/xp65/public/modules
module load conda/access-med
```

## Configuring Dataset Inputs for `ilamb-tree-generator`: CMIP and Non-CMIP Examples"

As mentioned earlier, the `ilamb-tree-generator` utilizes a `.yml` file for all input configurations. This format is consistent for different datasets. Below is an example configuration for both CMIP and non-CMIP datasets:

```yaml
datasets:
    - {mip: CMIP, institute: CSIRO, dataset: ACCESS-ESM1-5, project: CMIP6, exp: historical, ensemble: r1i1p1f1}
    - {mip: non-CMIP, institute: CSIRO, dataset: ACCESS-ESM1-5, project: CMIP6, exp: HI-CN-05}
```

The first entry represents a CMIP dataset, which is the standard usage for `ilamb-tree-generator`. The second entry corresponds to an ACCESS raw output, which is a non-CMIP dataset. Although most parameters are similar, there are specific settings for non-CMIP datasets. Here are the details of each parameter:

- `mip`: Set to `non-CMIP` to activate the CMORiser for non-CMIP data.
- `path`: For users working with their own ACCESS raw data, specify the root directory here. If not provided, the tool will default to using data in the `p73` directory.

### run `ilamb-tree-generator`

After setting up the `config.yml` file, run the `ilamb-tree-generator`. This will generate the CMORized data within the `ILAMB-ROOT` directory, making it accessible for ILAMB to read and use:

```bash
ilamb-tree-generator --datasets {your-config.yml-file} --ilamb_root $ILAMB_ROOT
```
Once it finish, you will get your CMORised data been stored by variable names in this format:

```bash
.
├── DATA
└── MODELS
    └── ACCESS-ESM1-5
        └── HI-CN-05
            ├── cSoil.nc
            ├── cVeg.nc
            ├── evspsbl.nc
            ├── gpp.nc
            ├── hfls.nc
            ├── hfss.nc
            ├── hurs.nc
            ├── lai.nc
            ├── nbp.nc
            ├── pr.nc
            ├── ra.nc
            ├── rh.nc
            ├── rlds.nc
            ├── rlus.nc
            ├── rsds.nc
            ├── rsus.nc
            ├── tasmax.nc
            ├── tasmin.nc
            ├── tas.nc
            └── tsl.nc
```

> ## Limitations
> `ilamb-tree-generator` doesn't support all variable in `ACCESS-ESM1-5`, only 19 variables which is required in `ilamb.cfg`. Will try to add more variables in the next version.
>
{: .callout}
