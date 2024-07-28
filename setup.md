---
title: pre-requisites
---

## Essential Preparation for the Hackathon

To get the most out of the Hackathon, please complete the following preparation tasks:

### Join Required NCI Data Projects

**Estimated time: 1-2 days (to receive membership approvals)**

To run the ESMValTool workflow at NCI, you will need:

- An active NCI account. (All hackathon participants have active NCI user IDs at this stage. Thanks!)
- Join project [xp65](#)
- Join project [nf33 (ACCESS-NRI training)](#)
- Join the ACCESS-NRI replicated data collection for Model Evaluation project [ct11](#)
- Join CMIP6 projects: [fs38](#), [oi10](#)
- Join CMIP5 projects: [rr3](#), [al33](#)
- Join ERA5 projects: [rt52](#), [zz93](#)

This process will give you access to the ACCESS-NRI training project as well as a number of managed data collections. You should receive automated emails from NCI as your memberships are approved. You can also check on the status of your project memberships by logging into MyNCI.

**Note**: Joining NCI projects requires NCI staff to manually approve each request. Approvals may take 1-3 working days to process. We strongly recommend that you submit the listed membership requests **as soon as possible** if you have not already done so.

### Run Basic ESMValTool Test on Gadi

**Estimated time: 5 minutes**

Once you have membership of the prerequisite NCI projects described above, run the following quick test in a Gadi (gadi.nci.org.au) login session:

```bash
module use /g/data/xp65/public/modules
module load esmvaltool
check_hackathon
```

This procedure will confirm that you are correctly set up to run ESMValTool on Gadi. Please report any issues to [romain.beucher@anu.edu.au](mailto:romain.beucher@anu.edu.au).

### Download and Set Up VS Code for Use with Gadi

**Estimated time: 15 minutes**

We will use the code editor VS Code during the Hackathon. Please download and install the tool before arriving at the Hackathon. We have made a video that will take you through the steps of installing and using VS Code with NCI’s Gadi.

- Watch Video: [VSCODE workflow on Gadi](#) (6 min)

### Learn More About ESMValTool and Run Some Examples

We strongly encourage everyone to work through additional ACCESS-NRI ESMValTool training materials including videos and tutorials:

- Watch: [What is ESMValTool?](#)

<div style="text-align: center;">
<iframe width="560" height="315" src="https://www.youtube.com/embed/YSs6A0H1MwE" frameborder="0" allowfullscreen></iframe>
</div>

- Watch: [ESMValTool – NCI Quickstart Guide](#)


<div style="text-align: center;">
<iframe width="560" height="315" src="https://www.youtube.com/embed/LSOzl6_CNy8" frameborder="0" allowfullscreen></iframe>
</div>

- Work through the [ESMValTool-workflow guide and examples](#)
- Watch additional videos from ACCESS-NRI's [ESMValTool YouTube playlist](#)
- Walk through ESMValTool’s [collection of tutorials](#)

