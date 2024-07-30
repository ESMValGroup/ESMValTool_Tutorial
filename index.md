---
layout: lesson
root: .  # Is the only page that doesn't follow the pattern /:path/index.html
permalink: index.html  # Is the only page that doesn't follow the pattern /:path/index.html
---

<div style="text-align: center;">
  <img src="./assets/img/cmip7_hackathon_logo.png" alt="Description of Image" style="width: 100%; height: auto;" />
</div>

This tutorial helps you to use ESMValTool.

The Earth System Model Evaluation Tool (ESMValTool) is a community developed
software toolkit that aims to facilitate the diagnosis and evaluation of the
causes and effects of model biases and inter-model spread within the CMIP model
ensemble.

> ## Disclaimer    
>    <p>The training material presented in this hackathon reproduces and adapts parts of the ESMValTool Tutorial. The original tutorial can be cited as follows:</p>
>>
>>        ESMValTool Development Team (2020): ESMValTool version 2.0. Zenodo. https://doi.org/10.5281/zenodo.3974591
>>
> This material has been specifically adapted for the ACCESS-NRI ESMValTool Workflow deployed on the NCI Gadi computing system.
{: .callout}

> ## What will you learn in this course
>
> - What is ESMValTool
> - How to run ESMValTool on NCI-Gadi
> - How to work with ESMValTool's suite of preprocessors
> - How to debug your recipes
> - How to develop your own diagnostics and recipes
>
{: .checklist}

> ## Prerequisites
>
> The prerequisites for the tutorial are listed on the
> [tutorial setup page]({{ page.root }}{% link setup.md %}).
>
{: .prereq}


## Visual Studio Code (VS Code)

To get the most out of the hackathon exercises, we recommend using Microsoft Visual Studio Code which can be downloaded free for Windows, MacOS and Linux [here](https://code.visualstudio.com/). For new VS Code users or those simply wanting a quick refresher, we have created a video and written guide to help you set up VS Code on Gadi. Note that the information is roughly the same, so you can choose either video or written guide to follow along.

<div style="position: relative; padding-bottom: 56.25%; height: 0;">
  <iframe 
    src="https://www.youtube.com/embed/fSxirzDR3iw" 
    style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;" 
    frameborder="0" 
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" 
    allowfullscreen>
  </iframe>
</div>


- [Written guide: Getting Started Guide for Visual Studio Code with Gadi]()


_**NOTE:** There are two pieces of information to clarify from the video tutorial that are explained in the written guide:_
1. _When logging into Gadi for the first time in VS Code, you do not need to write your own config file. You can simply choose the default option that is suggested and VS Code will create one for you. You only need to do this once - the first time you log in._
2. _In addition to the 3 extensions mentioned in the video, you will need to install a 4th extension to view html files (this is how most of the ESMValTool recipes show output plots). You can see our recommended extension "Live Server" in the [written guide above](https://github.com/ACCESS-NRI/CMIP7-Hackathon/blob/main/docs/VSCode_setup_guide.md).

## Australian Research Environment (ARE)
If it is not possible to use VS Code, hackathon exercises can alternatively be run via either an [Australian Research Environment](https://are-auth.nci.org.au/) JupyterLab or an [Australian Research Environment](https://are-auth.nci.org.au/) Virtual Desktop (VDI) session. We have created a written guide to setup each of these for the CMIP7-Hackathon below.
- [Written guide: ACCESS-NRI CMIP7-Hackathon ARE JupyterLab setup guide](https://github.com/ACCESS-NRI/CMIP7-Hackathon/blob/main/docs/2_ARE_JupyterLab_setup_guide.md).
- [Written guide: ACCESS-NRI CMIP7-Hackathon ARE Virtual Desktop (VDI) setup guide](https://github.com/ACCESS-NRI/CMIP7-Hackathon/blob/main/docs/3_ARE_VDI_setup_guide.md).

## Logging into Gadi

The first thing to do is to login to Gadi. You can choose to run the following commands either from your computer's terminal or from the terminal within VS Code. If you follow the above steps to set up VS Code on Gadi, you can skip to the next section ["Set up our environment to run ESMValTool"](#set-up-our-environment-to-run-ESMValTool) below (since the [video](https://youtu.be/fSxirzDR3iw) and [written guide](https://github.com/ACCESS-NRI/CMIP7-Hackathon/blob/main/docs/VSCode_setup_guide.md) already walk you through how to connect to Gadi in VS Code). 

Log into Gadi using ssh via the command line using your registed NCI *username* and enter your password when prompted. Once you see the welcome note you have successfully logged in. 
```
$ ssh [username]@gadi.nci.org.au
```

> ## Additional Resources
>
> - [Documentation](https://docs.esmvaltool.org)
> - [ESMValTool home page](https://www.esmvaltool.org/)
> - [Discussions page](https://github.com/ESMValGroup/ESMValTool/discussions)
> - [Papers](https://esmvaltool.org/references/)
> - [ESMValTool Source code](https://github.com/ESMValGroup/ESMValTool)
> - [ESMValCore Source code](https://github.com/ESMValGroup/ESMValCore)
> - [ESMValTool Citation info](https://tutorial.esmvaltool.org/about/index.html)
>
{: .callout}

{% include links.md %}
