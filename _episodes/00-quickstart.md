---
title: "Quickstart Guide"
teaching: 5
exercises: 10
questions:
- How do I set up with ESMValTool quickly?

objectives:
- Set up the ESMValTool
- Successfully run a recipe

keypoints:
- The enviroment has to be loaded before the ESMValTool can be used.
- The config file has to be set up.
- The recipe will say 'run was successful' if ran properly.
---

## Installing and getting started with the ESMValTool

To install the ESMValTool please see 
[here](https://docs.esmvaltool.org/en/latest/quickstart/installation.html#)


## Configuring ESMValtool

With ESMValTool installed, we can now set up our configuration file (which tells
ESMValTool how to run, where to get data from, and where to output it).

You can find out more about this in the 
[documention](https://docs.esmvaltool.org/projects/ESMValCore/en/latest/quickstart/configure.html).

In this example, visual studio code is used as the default text editor, so when 
'code' is shown on the command line instructions, replace it with syntax for 
initialising your favourite text editor.

1. Ensure you are in the directory you want to get started in:

~~~bash
esmvaltool config get_config_user
~~~
2. Open the config-user.yml file:

~~~bash
code ~/.esmvaltool/config-user.yml 
~~~
3. To set up the config-user.yml file, change the following lines to:

```yaml
 output_dir: ~/pathtoyourworkingdir/esmvaltool_output 
 download_dir: /anypathyoulike/<download folder> 
 auxiliary_data: /anypathyoulike/auxiliary_data 
 max_parallel_tasks: 1 
 offline: true 
 rootpath: 
   default: /anypathyoulike/<download folder> 
   CMIP5: /<defaultpathabove>/CMIP5 
   CMIP6: /<defaultpathabove>/CMIP6 
 drs: 
   CMIP5: default 
   CMIP6: default
```

   Here is an example of what it could look like:
```yaml
 output_dir: /home/h02/tgeddes/Esmvaltool/esmvaltool_output
 download_dir: /home/h02/tgeddes/Esmvaltool/download_data
 auxiliary_data_dir: /home/h02/tgeddes/Esmvaltool/auxiliary_data
 max_parallel_tasks: 1
 offline: true
 rootpath:
   default: /project/cma/esmvaltool/obs
   CMIP5: /project/cma/esmvaltool/cmip5/output1
   CMIP6: /project/cma/esmvaltool/CMIP6
   OBS: /project/cma/esmvaltool/obs
 drs:
   CMIP5: BADC
   CMIP6: BADC
```

## Running a Recipe

To run a recipe:

~~~bash
esmvaltool run recipe_radiation_budget.yml
~~~
If everything has been set up correctly, the last line of code should look
something like this: 

~~~output
2022-08-02 14:00:58,128 UTC [93691] INFO    Run was successful
~~~

To view the file used in this example:

~~~bash
 esmvaltool recipes get recipe_radiation_budget.yml
 code recipe_radiation_budget.yml 
~~~

You can find out more about this recipe in the 
[documentation](https://docs.esmvaltool.org/en/latest/recipes/recipe_radiation_budget.html).

