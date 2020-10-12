---
title: Glossary
---

## ESMValTool Glossary

- **Amon**: Monthly atmospheric data.

- **CMIP**: The Coupled Model Intercomparison Project (CMIP) aims at better understanding past, present and future climate changes arising from natural, unforced variability or in response to changes in radiative forcing in a multi-model context. For more information, check the [WCR-Climate webpage](https://www.wcrp-climate.org/).

- **CMOR**: CMOR stands for [*Climate Model Output Rewriter* library](https://pcmdi.github.io/cmor-site/index.html). It comprises a set of C-based functions, with bindings to both Python and FORTRAN 90, that can be used to produce CF-compliant NetCDF files that fulfill the requirements of many of the climate community's standard model experiments.

- **dataset**: In a recipe, dataset refers to the name of the model or observation data, e.g. HadGEM2-ES.

- **diagnostic**: In a recipe, the diagnostic section(s) define the tasks that will be executed when running the recipe. It can also include a diagnostic script that converts the preprocessed input data to the desired output like plots or NetCDF files.

- **ensemble**: An ensemble is a collection of experiments based on standardised inputs. This ensures that ensemble calculations use the same initial states, initialisation methods, and physics details. Ensemble members are named in the rip-nomenclature, *r* for realization, *i* for initialisation and *p* for physics, followed by an integer, e.g. r1i1p1.

- **experiment**: An experiment is defined as an activity aimed at addressing a specific scientific problem. In CMIP, experiments are numerical experiments with climate models.

- **grid**: In the dataset section of a recipe, a grid is the array of coordinates on which a variable has been sampled.

- **mask**: Masks are used to isolate regions. For example, if you apply an ocean mask, then coordinates in the ocean will not be taken into account for the preprocessors/diagnostics. Masks can also be used to work with regions that have missing or invalid entries.

- **mip**: Typically used to refer to a table of variables in ESMValTool. These are standardized per project, e.g. the [MIP tables for CMIP6](http://clipc-services.ceda.ac.uk/dreq/index/miptable.html).

- **Omon**: Monthly ocean data.

- **pr**: CMIP short-name for 'precipitation'.

- **preprocessor**: A preprocessor is an operation that is applied to a dataset.

- **project**: In the dataset section of a recipe, "project" Typically refers to a standard experimental protocol for global models, e.g. CMIP.

- **RCP**: *Representative Concentration Pathways* are scenarios that represent the full bandwidth of possible future emission trajectories. Depending on population growth and the development of energy production, food production and land use, various emission trajectories are possible.

- **recipe**: A recipe contains the instructions to be carried out by the ESMValTool. This includes four main sections: datasets, preprocessors, diagnostics and description.

- **ta**: CMIP short-name for 'air temperature'.

- **tas**: CMIP short-name for 'near-surface air temperature'.

- **thetao**: Sea water potential temperature variable (assume reference height is sea level).

- **thetaoga**: Global average sea water potential temperature

- **tos**:  CMIP short-name for 'sea surface temperature'.

- **ts**: CMIP short-name for 'surface temperature'.

- **yaml**: YAML is a human-readable data-serialization language. It is commonly used for configuration files and in applications where data is being stored or exchanged.


> ## Additional info
>
> Additional info can be found at the [ESMValTool homepage](https://esmvaltool.org)
> and in the [ESMValTool documentation](https://esmvaltool.readthedocs.io/).
{: .callout}


{% include links.md %}
