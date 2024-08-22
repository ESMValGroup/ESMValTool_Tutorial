---
title: "ILAMB exercise part-two: Support for Raw Access Output"
teaching: 15
exercises: 15
compatibility: ILAMB v2.7

questions:
- "What is CMORise"
- "How to use ilamb-tree-generator to CMORise Raw Access data"
objectives:
- "Explaination of CMORise"
- "use ilamb-tree-generator as a CMORiser to easily CMORise for ILAMB"
keypoints:
- "only support CMIP6 Access raw data"
- "More cpu will increase the speed while running ilamb-tree-generator as a CMORiser"
---

In this episode we will introduce how to use `ilamb-tree-generator` as a CMORiser to help you use `ILAMB` to evaluate Access raw output. But before that, we will introduce what is 'CMORise' first.

## What is CMORise

"CMORise" refers to the process of converting climate model output data into a standardized format that conforms to the Climate and Forecast (CF) metadata conventions. This process involves using the Climate Model Output Rewriter (CMOR) tool, which ensures that the data adheres to specific requirements for structure, metadata, and units, making it easier to compare and share across different climate models.

## Use `ilamb-tree-generator` to CMORise Access raw output

### Load module

Now, new version of `ilamb-tree-generator` is avaiable on module `access-med-0.6`, to load the module:
```bash
module use /g/data/xp65/public/modules/
module load conda/access-med-0.6
```

### config `ilamb-tree-generator`

As we mentioned in the previous section, `ilamb-tree-generator` use a `.yml` file for all necessary input, so the usage are similar, below is an example for cmip dataset and non-cmip dataset.
```yml
datasets:
    - {mip: CMIP, institute: CSIRO, dataset: ACCESS-ESM1-5, project: CMIP6, exp: historical, ensemble: r1i1p1f1}
    
    - {mip: non-CMIP, institute: CSIRO, dataset: ACCESS-ESM1-5, project: CMIP6, exp: HI-CN-05}
```

First one is a cmip dataset, which is the origin way to use `ilamb-tree-generator`. The second dataset is an ACCESS raw output which is a non-cmip dataset, most parts are the same but with some special parameters for non-cmip dataset only, following are the detail of each parameters:

```
mip: 
    need to be non-cmip to trigger the cmoriser for non-cmip data.
path:
    For people who want to use there own ACCESS raw data, you can speciy your data root there, otherwise it will automatically use data in `p73` 
```

### run `ilamb-tree-generator`

After you finish set up the `config.yml` file, trigger the `ilamb-tree-generator`, then you will get your cmorised data in `ILAMB-ROOT` which ilamb can read:

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

### Limited variables

Now `ilamb-tree-generator` doesn't support all variable in `ACCESS-ESM1-5`, only 19 variables which is required in `ilamb.cfg`. Will try to add more variables in the next version.