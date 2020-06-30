---
layout: lesson
root: .  # Is the only page that doesn't follow the pattern /:path/index.html
permalink: index.html  # Is the only page that doesn't follow the pattern /:path/index.html
date: '`r format(Sys.time(), "%d %B, %Y")`'
---

This tutorial is built to teach you how to use ESMValTool.  

The Earth System Model Evaluation Tool (ESMValTool) is a community developped software toolkit that aims to faciliate
the diagnosis and evaluation of the causes and effects of model biases and inter-model spread within the CMIP model
ensemble.


This tutorial is structured such that the main body of the tutorial, up to the episode 7, can be done in one sitting.
From there, there are several mini-tutorials which each covers one aspect of working with ESMValTool.
These mini-tutorials can be appended to the main tutorial or worked through independently.

> ## What will you learn in this course
>
>   - What is ESMValTool
>
>   - How to install ESMValTool
>   - How to configure ESMValTool for your local system
>   - How to run ESMValTool
>   - How to work with ESMValTool's suite of preprocessors
>   - How to debug your recipes
>   - How to access and deploy recipes from the ESMValTools gallery (Advanced)
>   - How to develop your own diagnostics and recipes (Advanced)
>   - How to contribute your recipes and diagnostics back into ESMValTool (Advanced)
>   - How to include new observational datasets (Advanced)
>
{: .checklist}

> ## Prerequisites
>
> - Basic understanding of git (optional)
>
> - Basic understanding of your preferred command line interface (ie a bash terminal)
>
> - Access to CMIP data
>
> - Access to a suitable computing system (eg Jasmin, DLR machine)
>
> - Github account (optional)
{: .prereq}

> ## Main things you need to know before starting this course
>
> 1. This tutorial can be taken online independently or taught by one of our instructors.
> 2. Please check the common issues page if you get stuck [Link to common issues page]. Otherwise, help is always available from ESMValTool developers via the github issues page.  
> 3. This tutorial includes several advanced lessons after the conclusion. These advanced lessons should be treated like “mini-tutorials”, and include aspects like “developing your own diagnostic” or “how to include observations”.
> 4. Don’t be alarmed if you can’t work through the entire tutorial in one sitting. It may take some time to get used to working with ESMValTool.
{: .checklist}

{% include links.md %}


> ## Additional resources: 
> - [Read the docs page](https://esmvaltool.readthedocs.io/)
> - [ESMValTool home page](https://www.esmvaltool.org/)
> - [Technical description paper](https://doi.org/10.5194/gmd-13-1179-2020)
> - [Source code (ESMValTool)](https://github.com/ESMValGroup/ESMValTool)
> - [Source code (ESMValCore )](https://github.com/ESMValGroup/ESMValCore)
{: .callout}

## Citation

Please cite this tutorial as:

- ESMValTool 2.0 tutorial, ESMValTool developers, version [ADD VERSION], 
  [https://github.com/ESMValGroup/tutorial](https://github.com/ESMValGroup/tutorial), 
  $date. 

Please cite ESMValTool as:

- Lauer, A., Eyring, V., Bellprat, O., Bock, L., Gier, B. K., Hunter, A., 
  Lorenz, R., Pérez-Zanón, N., Righi, M., Schlund, M., Senftleben, D., 
  Weigel, K., and Zechlau, S.: 
  Earth System Model Evaluation Tool (ESMValTool) v2.0 – diagnostics for 
  emergent constraints and future projections from Earth system models in CMIP, 
  Geosci. Model Dev. Discuss., 
  [https://doi.org/10.5194/gmd-2020-60](https://doi.org/10.5194/gmd-2020-60), 
  in review, 2020.

