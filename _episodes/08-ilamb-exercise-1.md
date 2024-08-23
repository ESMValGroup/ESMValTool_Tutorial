---
title: "The ILAMB-Exercise Part one: Running ILAMB on Gadi "
teaching: 15
exercises: 20
compatibility: ESMValTool v2.11.0

questions:
- "How do I run the ILAMB on NCI GADI?"
objectives:
- "Understand how to load, configure and run the ILAMB using the ACCESS-NRI ILAMB-Workflow"
keypoints:
- "The ACCESS-NRI ILAMB-Workflow facilitates the configuration of the ILAMB on NCI Gadi."
- "Users need to set up a run using a configuration file."
- "The `ilamb-tree-generator` allows to quickly build a data directory srtucture for the ILAMB."
- "The ILAMB can take advantage of the multiple CPUs available on Gadi."
---

> ## What is the purpose of the quickstart guide?
>
> - The purpose of the quickstart guide is to enable a user of GADI to
>   run ILAMB as quickly as possible.
>
> ## How do I load the module with ILAMB envisonment on GADI?
> 
> - On GADI, we have already prepared a module in preoject `xp65`, so you
>   need to be a member of project `xp65` first, and run following command to >   load 
>   the module
>
> >    ~~~
> >    module use /g/data/xp65/public/modules
> >    module load conda/access-med-0.8
> >    ~~~
> {: .language-bash}
{: .callout}

> ## How do I configure ILAMB?
>
> ILAMB has a `config.cfg` file as configuration file to initiate a benchmark study. With this file, you configure comparison sections and define which variables from which dataset will be compared.
>
> An example configure file for ILAMB on <i>Gadi</i> could be called `config. cfg` and look like this for comparing your models with two variables of the radiation and energy cycle as measured by <a href="https://ceres.larc.nasa.gov" target="_blank">Clouds and the Earth’s Radiant Energy System (CERES) project</a>:
> > ### Config File Example 
> > ~~~
> > # This configure file specifies comparison sections, variables and observational data for running ILAMB on Gadi.
> > 
> > # See https://www.ilamb.org/doc/first_steps.html#configure-files for the ILAMB Tutorial that addesses Configure Files
> > # See https://www.ilamb.org/datasets.html for the ILAMB and IOMB collections
> > 
> > # Structure:
> > # [h1:] Sections
> > # [h2:] Variables
> > # []    Observational Datasets
> > 
> > #=======================================================================================
> > 
> > [h1: Hydrology Cycle]
> > bgcolor = "#E6F9FF"
> > 
> > [h2: Evapotranspiration]
> > variable       = "et"
> > alternate_vars = "evspsbl"
> > cmap           = "Blues"
> > weight         = 5
> > mass_weighting = True
> > 
> > [MODIS]
> > source        = "DATA/evspsbl/MODIS/et_0.5x0.5.nc"
> > weight        = 15
> > table_unit    = "mm d-1"
> > plot_unit     = "mm d-1"
> > relationships = "Precipitation/GPCPv2.3","SurfaceAirTemperature/CRU4.02"
> > 
> > [MOD16A2]
> > source        = "DATA/evspsbl/MOD16A2/et.nc"
> > weight        = 15
> > table_unit    = "mm d-1"
> > plot_unit     = "mm d-1"
> > relationships = "Precipitation/GPCPv2.3","SurfaceAirTemperature/CRU4.02"
> > 
> > 
> > #~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
> > 
> > [h2: Latent Heat]
> > variable       = "hfls"
> > alternate_vars = "le"
> > cmap           = "Oranges"
> > weight         = 5
> > mass_weighting = True
> > 
> > [FLUXCOM]
> > source   = "DATA/hfls/FLUXCOM/le.nc"
> > land     = True
> > weight   = 9
> > skip_iav = True
> > 
> > [DOLCE]
> > source   = "DATA/evspsbl/DOLCE/DOLCE.nc"
> > weight   = 15
> > land     = True
> > 
> > #~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
> > 
> > [h2: Sensible Heat]
> > variable       = "hfss"
> > alternate_vars = "sh"
> > weight         = 2
> > mass_weighting = True
> > 
> > [FLUXCOM]
> > source   = "DATA/hfss/FLUXCOM/sh.nc"
> > weight   = 15
> > skip_iav = True
> > 
> > ###########################################################################
> > 
> > [h1: Radiation and Energy Cycle]
> > bgcolor = "#FFECE6"
> > 
> > #~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
> > 
> > [h2: Albedo]
> > variable = "albedo"
> > weight   = 1
> > ctype    = "ConfAlbedo"
> > 
> > [CERESed4.1]
> > source   = "DATA/albedo/CERESed4.1/albedo.nc"
> > weight   = 20
> > 
> > [GEWEX.SRB]
> > source   = "DATA/albedo/GEWEX.SRB/albedo_0.5x0.5.nc"
> > weight   = 20
> > 
> > #~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
> > 
> > [h2: Surface Upward SW Radiation]
> > variable = "rsus"
> > weight   = 1
> > 
> > [FLUXNET2015]
> > source   = "DATA/rsus/FLUXNET2015/rsus.nc"
> > weight   = 12
> > 
> > [GEWEX.SRB]
> > source   = "DATA/rsus/GEWEX.SRB/rsus_0.5x0.5.nc"
> > weight   = 15
> > 
> > [WRMC.BSRN]
> > source   = "DATA/rsus/WRMC.BSRN/rsus.nc"
> > weight   = 12
> > 
> > #~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
> > 
> > [h2: Surface Net SW Radiation]
> > variable = "rsns"
> > derived  = "rsds-rsus"
> > weight   = 1
> > 
> > [CERESed4.1]
> > source   = "DATA/rsns/CERESed4.1/rsns.nc"
> > weight   = 15
> > 
> > [FLUXNET2015]
> > source   = "DATA/rsns/FLUXNET2015/rsns.nc"
> > weight   = 12
> > 
> > [GEWEX.SRB]
> > source   = "DATA/rsns/GEWEX.SRB/rsns_0.5x0.5.nc"
> > weight   = 15
> > 
> > [WRMC.BSRN]
> > source   = "DATA/rsns/WRMC.BSRN/rsns.nc"
> > weight   = 12
> > 
> > #~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
> > 
> > [h2: Surface Upward LW Radiation]
> > variable = "rlus"
> > weight   = 1
> > 
> > [FLUXNET2015]
> > source   = "DATA/rlus/FLUXNET2015/rlus.nc"
> > weight   = 12
> > 
> > [GEWEX.SRB]
> > source   = "DATA/rlus/GEWEX.SRB/rlus_0.5x0.5.nc"
> > weight   = 15
> > 
> > [WRMC.BSRN]
> > source   = "DATA/rlus/WRMC.BSRN/rlus.nc"
> > weight   = 12
> > 
> > #~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
> > 
> > [h2: Surface Net LW Radiation]
> > variable = "rlns"
> > derived  = "rlds-rlus"
> > weight   = 1
> > 
> > [CERESed4.1]
> > source   = "DATA/rlns/CERESed4.1/rlns.nc"
> > weight   = 15 
> > 
> > [FLUXNET2015]
> > source   = "DATA/rlns/FLUXNET2015/rlns.nc"
> > weight   = 12
> > 
> > [GEWEX.SRB]
> > source   = "DATA/rlns/GEWEX.SRB/rlns_0.5x0.5.nc"
> > weight   = 15
> > 
> > [WRMC.BSRN]
> > source   = "DATA/rlns/WRMC.BSRN/rlns.nc"
> > weight   = 12
> > 
> > #~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
> > 
> > [h2: Surface Net Radiation]
> > variable = "rns"
> > derived  = "rlds-rlus+rsds-rsus"
> > weight = 2
> > 
> > [CERESed4.1]
> > source   = "DATA/rns/CERESed4.1/rns.nc"
> > weight   = 15
> > 
> > [FLUXNET2015]
> > source   = "DATA/rns/FLUXNET2015/rns.nc"
> > weight   = 12
> > 
> > [GEWEX.SRB]
> > source   = "DATA/rns/GEWEX.SRB/rns_0.5x0.5.nc"
> > weight   = 15
> > 
> > [WRMC.BSRN]
> > source   = "DATA/rns/WRMC.BSRN/rns.nc"
> > weight   = 12
> > 
> > ###########################################################################
> > 
> > [h1: Forcings]
> > bgcolor = "#EDEDED"
> > 
> > #~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
> > 
> > [h2: Surface Air Temperature]
> > variable = "tas"
> > weight   = 2
> > 
> > [FLUXNET2015]
> > source   = "DATA/tas/FLUXNET2015/tas.nc"
> > weight   = 9
> > 
> > #~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
> > 
> > [h2: Diurnal Temperature Range]
> > variable = "dtr"
> > weight   = 2
> > derived  = "tasmax-tasmin"
> > 
> > [CRU4.02]
> > source   = "DATA/dtr/CRU4.02/dtr.nc"
> > weight   = 25
> > 
> > #~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
> > 
> > [h2: Precipitation]
> > variable       = "pr"
> > cmap           = "Blues"
> > weight         = 2
> > mass_weighting = True
> > 
> > [FLUXNET2015]
> > source     = "DATA/pr/FLUXNET2015/pr.nc"
> > land       = True
> > weight     = 9
> > table_unit = "mm d-1"
> > plot_unit  = "mm d-1"
> > 
> > [GPCCv2018]
> > source     = "DATA/pr/GPCCv2018/pr.nc"
> > land       = True
> > weight     = 20
> > table_unit = "mm d-1"
> > plot_unit  = "mm d-1"
> > space_mean = True
> > 
> > [GPCPv2.3]
> > source     = "DATA/pr/GPCPv2.3/pr.nc"
> > land       = True
> > weight     = 20
> > table_unit = "mm d-1"
> > plot_unit  = "mm d-1"
> > space_mean = True
> > 
> > #~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
> > 
> > [h2: Surface Relative Humidity]
> > variable       = "rhums"
> > alternate_vars = "hurs"
> > cmap           = "Blues"
> > weight         = 3
> > mass_weighting = True
> > 
> > [CRU4.02]
> > source     = "DATA/rhums/CRU4.02/rhums.nc"
> > weight     = 10
> > 
> > #~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
> > 
> > [h2: Surface Downward SW Radiation]
> > variable = "rsds"
> > weight   = 2
> > 
> > [FLUXNET2015]
> > source   = "DATA/rsds/FLUXNET2015/rsds.nc"
> > weight   = 12
> > 
> > [GEWEX.SRB]
> > source   = "DATA/rsds/GEWEX.SRB/rsds_0.5x0.5.nc"
> > weight   = 15
> > 
> > [WRMC.BSRN]
> > source   = "DATA/rsds/WRMC.BSRN/rsds.nc"
> > weight   = 12
> > 
> > #~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
> > 
> > [h2: Surface Downward LW Radiation]
> > variable = "rlds"
> > weight   = 1
> > 
> > [FLUXNET2015]
> > source   = "DATA/rlds/FLUXNET2015/rlds.nc"
> > weight   = 12
> > 
> > [GEWEX.SRB]
> > source   = "DATA/rlds/GEWEX.SRB/rlds_0.5x0.5.nc"
> > weight   = 15
> > 
> > [WRMC.BSRN]
> > source   = "DATA/rlds/WRMC.BSRN/rlds.nc"
> > weight   = 12
> > 
> >~~~
>{: .solution}
{: .callout}

> ## HOW do I organise data before run ILAMB
>
> ### ILAMB_ROOT
> 
> ILAMB requires files to be organised in a specific directory structure of 
> `DATA` and `MODELS`.
> The root of this directory structure is the `ILAMB_ROOT` path (you should
> export it as `$ILAMB_ROOT`):
> 
> ~~~
> export ILAMB_ROOT=PATH/OF/ILAMB_ROOT/DIRECTORY
> ~~~
> {: .language-bash}
>   
> The following tree represents a typical ILAMB_ROOT setup for CMIP
> comparison on NCI/Gadi:
>
> > ### ILAMB-ROOT Directory Structure
> > ~~~
> > $ILAMB_ROOT/
> > |-- DATA -> /g/data/ct11/access-nri/replicas/ILAMB
> > |-- MODELS
> >   |-- ACCESS-ESM1-5
> >   |   `-- piControl
> >   |       `-- r3i1p1f1
> >                  ├── evspsbl.nc
> >                  ├── hfds.nc
> >                  ├── hfls.nc
> >                  ├── hfss.nc
> >                  ├── hurs.nc
> >                  ├── pr.nc
> >                  ├── rlds.nc
> >                  ├── rlus.nc
> >                  ├── rsds.nc
> >                  ├── rsus.nc
> >                  ├── tasmax.nc
> >                  ├── tasmin.nc
> >                  ├── tas.nc
> >                  └── tsl.nc
> > ~~~
>{: .language-bash}
{: .callout}

> There are two main branches in this directory:
>
> 1. the `DATA` directory: this is where we keep the observational datasets each in a subdirectory bearing the name of the variable. This directory can be setup as a symlink to the [ACCESS-NRI Replicated Datasets for Climate Model Evaluation
](https://geonetwork.nci.org.au/geonetwork/srv/eng/catalog.search#/metadata/f7199_2480_5432_9703).
>
>2. the `MODEL` directory: this directory can be populated with symbolic links to the model outputs.
>
> To facilitate the setup of an ILAMB-ROOT tree. ACCESS-NRI provides a tool to automatically generate the required folder structure.
> The `ilamb-tree-generator` is available in the `access-med` environment of the `xp65` project.
>
> The tool will automatically create the folder structure above. Models output can be added by listing them in a yaml file as follow:
> 
> ```yaml
> datasets:
>  - {mip: CMIP, institute: CSIRO, dataset: ACCESS-ESM1-5, project: CMIP6, exp: historical, ensemble: r1i1p1f1}
>  - {mip: CMIP,institute: BCC, dataset: BCC-ESM1, project: CMIP6, exp: historical, ensemble: r1i1p1f1}
>  - {mip: CMIP,institute: CCCma, dataset: CanESM5, project: CMIP6, exp: historical, ensemble: r1i1p1f1}
>  - {mip: LUMIP,institute: CSIRO, dataset: ACCESS-ESM1-5, project: CMIP6, exp: hist-noLu, ensemble: r1i1p1f1}
> ```
>
> The tool can then be run from the command line:
>
> ```bash
> ilamb-tree-generator --datasets models.yml --ilamb_root $ILAMB_ROOT
> ```
>
> ### ILAMB model selection: `model_setup.txt`
> 
> In the `model_setup.txt`, you can select all the model output that you want to compare.
>
> Assuming you want to compare the three models that we used in [ILAMB_ROOT/MODELS](#ilamb_rootmodels) (ACCESS-ESM1.5, BCC-ESM1, and CanESM5), you would need to create a `model_setup.txt` file wehere you define both the model labels and their paths:
> > #### model_setup.txt
> >```
> > # Model Name (used as label), ABSOLUTE/PATH/TO/MODELS or relative to $ILAMB_ROOT/ , Optional comments
> >   - {mip: CMIP, institute: CSIRO-ARCCSS, dataset: ACCESS-CM2, project: CMIP6, exp: piControl, ensemble: r3i1p10f1}
> >   - {mip: CMIP, institute: CSIRO-ARCCSS, dataset: ACCESS-CM2, project: CMIP6, exp: piControl, ensemble: r3i1p11f1}
> >   - {mip: CMIP, institute: CSIRO-ARCCSS, dataset: ACCESS-CM2, project: CMIP6, exp: piControl, ensemble: r3i1p12f1}
> >   - {mip: CMIP, institute: CSIRO-ARCCSS, dataset: ACCESS-CM2, project: CMIP6, exp: piControl, ensemble: r3i1p13f1}
> >   - {mip: CMIP, institute: CSIRO-ARCCSS, dataset: ACCESS-CM2, project: CMIP6, exp: piControl, ensemble: r3i1p14f1}
> >   - {mip: CMIP, institute: CSIRO-ARCCSS, dataset: ACCESS-CM2, project: CMIP6, exp: piControl, ensemble: r3i1p15f1}
> >   - {mip: CMIP, institute: CSIRO-ARCCSS, dataset: ACCESS-CM2, project: CMIP6, exp: piControl, ensemble: r3i1p16f1}
> >   - {mip: CMIP, institute: CSIRO-ARCCSS, dataset: ACCESS-CM2, project: CMIP6, exp: piControl, ensemble: r3i1p17f1}
> >   - {mip: CMIP, institute: CSIRO-ARCCSS, dataset: ACCESS-CM2, project: CMIP6, exp: piControl, ensemble: r3i1p18f1}
> >   - {mip: CMIP, institute: CSIRO-ARCCSS, dataset: ACCESS-CM2, project: CMIP6, exp: piControl, ensemble: r3i1p19f1}
> >   - {mip: CMIP, institute: CSIRO-ARCCSS, dataset: ACCESS-CM2, project: CMIP6, exp: piControl, ensemble: r3i1p1f1}
> >   - {mip: CMIP, institute: CSIRO-ARCCSS, dataset: ACCESS-CM2, project: CMIP6, exp: piControl, ensemble: r3i1p2f1}
> >   - {mip: CMIP, institute: CSIRO-ARCCSS, dataset: ACCESS-CM2, project: CMIP6, exp: piControl, ensemble: r3i1p3f1}
> >   - {mip: CMIP, institute: CSIRO-ARCCSS, dataset: ACCESS-CM2, project: CMIP6, exp: piControl, ensemble: r3i1p4f1}
> >   - {mip: CMIP, institute: CSIRO-ARCCSS, dataset: ACCESS-CM2, project: CMIP6, exp: piControl, ensemble: r3i1p5f1}
> >   - {mip: CMIP, institute: CSIRO-ARCCSS, dataset: ACCESS-CM2, project: CMIP6, exp: piControl, ensemble: r3i1p6f1}
> >   - {mip: CMIP, institute: CSIRO-ARCCSS, dataset: ACCESS-CM2, project: CMIP6, exp: piControl, ensemble: r3i1p7f1}
> >   - {mip: CMIP, institute: CSIRO-ARCCSS, dataset: ACCESS-CM2, project: CMIP6, exp: piControl, ensemble: r3i1p8f1}
> >   - {mip: CMIP, institute: CSIRO-ARCCSS, dataset: ACCESS-CM2, project: CMIP6, exp: piControl, ensemble: r3i1p9f1}
> >```
>{: .language-bash}
{: .callout}

> ## Run ILAMB
> 
>Now that we have the configuration file set up, you can run the study using the `ilamb-run` script via the aforementioned
> ~~~
> ilamb-run --config config.cfg --model_setup model_setup.txt --regions global
> ~~~
> {: .language-bash}
>
> Because of the computational costs, you need to run ILAMB through a Portable Batch System (PBS) job on Gadi.
>
> ## PBS job example
> The following default PBS file, let's call it `ilamb_test.job`, can help you to setup your own, while making sure to use the correct project (#PBS -P) to charge your computing cost to:
> 
> > ~~~
> > #!/bin/bash
> > 
> > #PBS -N ilamb_test
> > #PBS -l wd
> > #PBS -P your_compute_project_here
> > #PBS -q normalbw
> > #PBS -l walltime=0:20:00  
> > #PBS -l ncpus=14
> > #PBS -l mem=63GB           
> > #PBS -l jobfs=10GB        
> > #PBS -l storage=gdata/ct11+gdata/hh5+gdata/xp65+gdata/fs38+gdata/oi10+gdata/zv30
> > 
> > # ILAMB is provided through projects xp65. We will use the latter here
> > module use /g/data/xp65/public/modules
> > module load conda/access-med
> > 
> > # Define the ILAMB Path, expecting it to be where you start this job from
> > export ILAMB_ROOT=./
> > export CARTOPY_DATA_DIR=/g/data/xp65/public/apps/cartopy-data
> > 
> > # Run ILAMB in parallel with the config.cfg configure file for the models defined in model_setup.txt
> > mpiexec -n 10 ilamb-run --config config.cfg --model_setup model_setup.txt --regions global
> > ~~~
> {: .language-bash}
{: .callout}
> 
> You should adjust this file to your own specifications (including the storage access to your models). Save the file in the `$ILAMB_ROOT` and submit its job to the queue from there via 
> >~~~
> >qsub ilamb_test.job
> >~~~
> {: .language-bash}
{: .callout}
>
> Running this job will create a `_build` directory with the comparison results within `$ILAMB_ROOT`. You can adjust the place of this directory via a agrument `--build_dir` argument for `ilamb-run`.




