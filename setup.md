---
title: pre-requisites
---

## Essential Preparation for the Hackathon

To get the most out of the Hackathon, please complete the following preparation tasks:

### Join Required NCI Data Projects

To run the ESMValTool workflow at NCI, you will need:

- An active NCI account. (All hackathon participants have active NCI user IDs at this stage. Thanks!)
- Join project [xp65](https://my.nci.org.au/mancini/project/xp65)
- Join project [nf33 (ACCESS-NRI training)](https://my.nci.org.au/mancini/project/nf33)
- Join the ACCESS-NRI replicated data collection for Model Evaluation project [ct11](https://my.nci.org.au/mancini/project/ct11)
- Join CMIP6 projects: [fs38](https://my.nci.org.au/mancini/project/fs38), [oi10](https://my.nci.org.au/mancini/project/oi10)
- Join CMIP5 projects: [rr3](https://my.nci.org.au/mancini/project/rr3), [al33](https://my.nci.org.au/mancini/project/al33)
- Join ERA5 projects: [rt52](https://my.nci.org.au/mancini/project/rt52), [zz93](https://my.nci.org.au/mancini/project/zz93)
- Join the CMIP7 collaborative development and evaluation project: [zv30](https://my.nci.org.au/mancini/project/zv30)

This process will give you access to the ACCESS-NRI training project as well as a number of managed data collections. You should receive automated emails from NCI as your memberships are approved. You can also check on the status of your project memberships by logging into MyNCI.

**Note**: Joining NCI projects requires NCI staff to manually approve each request. Approvals may take 1-3 working days to process. We strongly recommend that you submit the listed membership requests **as soon as possible** if you have not already done so.


## Set up our environment to run ESMValTool 

1. Login to Gadi

2. Enter the following two commands (one after the other). The first command loads some necessary dependencies needed to run ESMValTool, and the second command loads the required [ACCESS-NRI ESMValTool-workflow](https://github.com/ACCESS-NRI/ESMValTool-workflow) module.

```bash
module use /g/data/xp65/public/modules
module load esmvaltool
```
3. Run the hackathon setup script from any directory. This verifies that your NCI account has access to the required projects on Gadi and that their respective storage locations are mounted, clones the [CMIP7-Hackathon Github repository](https://github.com/ACCESS-NRI/CMIP7-Hackathon), and automatically runs each of the hackathon ESMValTool recipes as PBS jobs on Gadi.

```bash
check_hackathon
```

This procedure will confirm that you are correctly set up to run ESMValTool on Gadi. Please report any issues to [romain.beucher@anu.edu.au](mailto:romain.beucher@anu.edu.au).

## Visual Studio Code (VS Code)

To get the most out of the hackathon exercises, we recommend using Microsoft Visual Studio Code which can be downloaded free for Windows, MacOS and Linux [here](https://code.visualstudio.com/). For new VS Code users or those simply wanting a quick refresher, we have created a video and written guide to help you set up VS Code on Gadi. Note that the information is roughly the same, so you can choose either video or written guide to follow along.

<div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; height: auto;">
    <iframe style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;" 
            src="https://www.youtube.com/embed/fSxirzDR3iw" 
            frameborder="0" 
            allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" 
            allowfullscreen>
    </iframe>
</div>

- [Written guide: Getting Started Guide for Visual Studio Code with Gadi](https://github.com/ACCESS-NRI/CMIP7-Hackathon/blob/main/docs/1_VSCode_setup_guide_RECOMMENDED.md)


_**NOTE:** There are two pieces of information to clarify from the video tutorial that are explained in the written guide:_
1. _When logging into Gadi for the first time in VS Code, you do not need to write your own config file. You can simply choose the default option that is suggested and VS Code will create one for you. You only need to do this once - the first time you log in._
2. _In addition to the 3 extensions mentioned in the video, you will need to install a 4th extension to view html files (this is how most of the ESMValTool recipes show output plots). You can see our recommended extension "Live Server" in the [written guide above](https://github.com/ACCESS-NRI/CMIP7-Hackathon/blob/main/docs/VSCode_setup_guide.md).

## Australian Research Environment (ARE)
If it is not possible to use VS Code, hackathon exercises can alternatively be run via either an [Australian Research Environment](https://are-auth.nci.org.au/) JupyterLab or an [Australian Research Environment](https://are-auth.nci.org.au/) Virtual Desktop (VDI) session. We have created a written guide to setup each of these for the CMIP7-Hackathon below.
- [Written guide: ACCESS-NRI CMIP7-Hackathon ARE JupyterLab setup guide](https://github.com/ACCESS-NRI/CMIP7-Hackathon/blob/main/docs/2_ARE_JupyterLab_setup_guide.md).
- [Written guide: ACCESS-NRI CMIP7-Hackathon ARE Virtual Desktop (VDI) setup guide](https://github.com/ACCESS-NRI/CMIP7-Hackathon/blob/main/docs/3_ARE_VDI_setup_guide.md).