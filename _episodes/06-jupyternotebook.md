---
title: "Use a Jupyter Notebook to Run a Recipe"
teaching: 20
exercises: 30
compatibility: ESMValTool v2.11.0

questions:
- "How to load esmvaltool module in ARE?
- "How to view and run a recipe in a Jupyter Notebook?"
- "How to run a single diagnostic or preprocessor task?"
objectives:
- "Learn about the esmvalcore experimental API"
- "View the Recipe output in a Jupyter Notebook"

keypoints:
- "ESMValTool can be used in a Jupyter Notebook"
- "ESMValTool uses ESMValCore as a tool"
---

This episode..

## Start a session in ARE
Log in to [ARE][are]{:target="_blank"} with your NCI account to start a JupyterLab session.
Refer to this [extras page]({{ page.root }}{% link _extras/02-aresetup.md %})for more details.
Open the folder to your hackathon folder in `nf33` where you can create a new notebook and find 
solution notebooks.
This tutorial is using material from the [documentation][experimental-api]{:target="_blank"} 
which is a good point for reference and a [short tutorial from EGU22]
(https://github.com/ESMValGroup/EGU22-short-course/blob/main/notebooks/Introduction_to_ESMValTool.ipynb, target="_blank").
![Warming stripes](../assets/img/ESMValTool-logo.png)

<div style="text-align: center;">
    <img width=200 src=\"https://docs.esmvaltool.org/en/v2.5.0/_static/ESMValTool-logo-2.png\"> <img width=200 src=\"https://jupyter.org/assets/homepage/hublogo.svg\"> <img width=200 src=\"https://zenodo.org/api/files/00000000-0000-0000-0000-000000000000/is-enes3/logo.png\"> <img width=200 src=\"https://www.smhi.se/polopoly_fs/1.135796.1527766089!/image/LoggaEUCP.png_gen/derivatives/Original_366px/image/LoggaEUCP.png\"> <img width=200 src=\"https://www.dkrz.de/@@site-logo/dkrz.svg\"> <img width=200 src=\"https://upload.wikimedia.org/wikipedia/commons/8/85/SMHI_Logo.svg\"> <img width=200 src=\"https://www.dtls.nl/wp-content/uploads/2015/03/NleSc.png\">
</div>

## Finding a recipe
list
print - metadata
recipe object

## Configuration in the notebook
CFG object as dictionary
```
esmvaltool.CFG

esmvaltool.CFG['offline'] = False
```

## Running the recipe


## Recipe output
output mapping
image files
datafiles


{% include links.md %}
