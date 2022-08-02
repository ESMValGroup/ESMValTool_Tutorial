---
title: "Quickstart"
teaching: 5
exercises: 10
questions:
- Questions

objectives:
- Set up ESMValTool
- Successfully run a recipe

keypoints:
- here are the key points of the quickstart
---

## Quickstart Guide for the ESMValTool

## Installing and getting started with the ESMValTool

To load the ESMValTool community enviroment:
~~~bash
module load scitools/community/esmvaltool/2.5.0 
~~~

## Configuring ESMValtool

With ESMValTool installed, we can now set up our configuration file (which tells
ESMValTool how to run, where to get data from, and where to output it).

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
~~~config-user
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
~~~
   Here is an example of what it could look like:
~~~config-example
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
~~~

## Running a Recipe

To run a recipe:
~~~bash
esmvaltool run examples/recipe_python.yml
~~~
If everything has been set up correctly, the last line of code should look
something like this: 
~~~output
 2022-08-02 12:10:57,112 UTC [84634] INFO    Run was successful
~~~

To view the file used in this example:
~~~bash
 esmvaltool recipes get examples/recipe_python.yml
 code recipe_python.yml 
~~~

