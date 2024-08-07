---
title: "The ACCESS-NRI ESMValTool-Workflow"
teaching: 15
exercises: 20
compatibility: ESMValTool v2.11.0

questions:
- "What is the purpose of the quickstart guide?"
- "How do I run ILAMB on GADI?"
- "HOW do I organise data before run ILAMB?"
- "HOW do I configure ILAMB?"
objectives:
- "Understand the purpose of the quickstart guide"
- "Load module for ILAMB"
- "Configure ILAMB"
- "organise data directory structure for ILAMB"
- "run ILAMB"
keypoints:
- "ILAMB work environment has been packaged as a module for GADI users."
- "use `.cfg` file to configure a ilamb run."
- "use `ilamb-tree-generator` to simplely build a data directory sreucture for ILAMB to read."
- "Two ways to specify input data while trigger ILAMB run"
- "use MPI if posiable to speed up ILAMB"
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
>     ~~~
>     module use /g/data/xp65/public/modules
>     module load conda/access-med-0.8
>     ~~~
>     {: .language-bash}
>
> ## How do I configure ILAMB?
>
> ILAMB has a `config.cfg` file as configuration file to initiate a benchmark study. With this file, you configure comparison sections and define which variables from which dataset will be compared.
>
> An example configure file for ILAMB on <i>Gadi</i> could be called `config. cfg` and look like this for comparing your models with two variables of the radiation and energy cycle as measured by <a href="https://ceres.larc.nasa.gov" target="_blank">Clouds and the Earth’s Radiant Energy System (CERES) project</a>:
> ~~~
> # This configure file specifies comparison sections, variables and.
>  observational data for running ILAMB on Gadi.
>
> # See https://www.ilamb.org/doc/first_steps.html#configure-files for the 
> ILAMB Tutorial that addesses Configure Files
>
> # Structure:
> # [h1:] Sections
> # [h2:] Variables
> # [...] Observational Datasets and their paths
> 
> ############################################################################
> 
> [h1: Radiation and Energy Cycle]
> 
> [h2: Surface Upward SW Radiation]
> variable = "rsus"
>
> [CERES]
> source   = "DATA/rsus/CERESed4.1/rsus.nc"
> 
> [h2: Albedo]
> variable = "albedo"
> derived  = "rsus/rsds"
>
> [CERES]
> source   = "DATA/albedo/CERESed4.1/albedo.nc"
> ~~~
> {: .language-bash}
>
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
> ~~~
> $ILAMB_ROOT/
> |-- DATA -> /g/data/ct11/access-nri/replicas/ILAMB
> `-- MODELS
>    |-- ACCESS-ESM1-5
>    |   `-- historical
>    |       `-- r1i1p1f1
>    |           |-- cSoil.nc -> /g/data/fs38/publications/CMIP6/CMIP/CSIRO/ACCESS-ESM1-5/historical/r1i1p1f1/Emon/cSoil/gn/latest/cSoil_Emon_ACCESS-ESM1-5_historical_r1i1p1f1_gn_185001-201412.nc
> ~~~
>{: .language-bash}
>
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
> ```
> # Model Name (used as label), ABSOLUTE/PATH/TO/MODELS or relative to $ILAMB_ROOT/ , Optional comments
> ACCESS_ESM1-5_r1i1p1f1      , MODELS/ACCESS-ESM1-5/historical/r1i1p1f1              , CMIP6
> BCC-ESM1_r1i1p1f1           , MODELS/BCC-ESM1/historical/r1i1p1f1                   , CMIP6
> CanESM5_r1i1p1f1            , MODELS/CanESM5/historical/r1i1p1f1                    , CMIP6
> ```
>
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
> ~~~
> #!/bin/bash
>
> #PBS -N ilamb_test
> #PBS -l wd
> #PBS -P your_compute_project_here
> #PBS -q normalbw
> #PBS -l walltime=0:20:00  
> #PBS -l ncpus=14
> #PBS -l mem=63GB           
> #PBS -l jobfs=10GB        
> #PBS -l storage=gdata/ct11+gdata/hh5+gdata/xp65+gdata/fs38+gdata/oi10
>
> # ILAMB is provided through projects xp65. We will use the latter here
> module use /g/data/xp65/public/modules
> module load conda/access-med
> 
> # Define the ILAMB Path, expecting it to be where you start this job from
> export ILAMB_ROOT=./
> export CARTOPY_DATA_DIR=/g/data/xp65/public/apps/cartopy-data
>
> # Run ILAMB in parallel with the config.cfg configure file for the models defined in model_setup.txt
> mpiexec -n 10 ilamb-run --config config.cfg --model_setup model_setup.txt --regions global
> ~~~
> {: .language-bash}
> 
> You should adjust this file to your own specifications (including the storage access to your models). Save the file in the `$ILAMB_ROOT` and submit its job to the queue from there via 
> ~~~
> qsub ilamb_test.job
> ~~~
> {: .language-bash}
>
> Running this job will create a `_build` directory with the comparison results within `$ILAMB_ROOT`. You can adjust the place of this directory via a agrument `--build_dir` argument for `ilamb-run`.




