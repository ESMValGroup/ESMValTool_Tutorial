---
title: "Quickstart Guide"
teaching: 5
exercises: 10
questions:

- "How do I activate the ESMValTool environment?"
- "How do I configure the ESMValTool?"
- "How do I run a recipe?"

objectives:

- "Activate the ESMValTool environment"
- "Configure the ESMValTool"
- "Successfully run a recipe"

keypoints:
- "See the installation guide for activating the ESMValtool environment."
- "The config file has to be set up, uncommenting the environment you use."
- "The command for running a recipe is `esmvaltool run`."
---

> ## Installing and getting started with the ESMValTool
>
> To install the ESMValTool please see 
> [here](https://docs.esmvaltool.org/en/latest/quickstart/installation.html#)
{: .callout}

> ## Configuring ESMValtool
>
> With ESMValTool installed, we can now set up our configuration file (which 
> tells ESMValTool how to run, where to get data from, and where to output it).
>
> You can find out more about this in the 
> [documention](https://docs.esmvaltool.org/projects/ESMValCore/en/latest/quickstart/configure.html).
>
>
> - Ensure you are in the directory you want to get started in:
>
>   ~~~bash
>    esmvaltool config get_config_user
>    ~~~
> - Open the config-user.yml file in a text editor:
>
>   ~~~bash
>   ~/.esmvaltool/config-user.yml 
>   ~~~
> - To set up the config-user.yml file, uncomment the lines relating to the
> host you are using to run the ESMValTool. For example if you were using 
> Jasmin you should uncomment:
>
>   ```yaml
>   # Site-specific entries: Jasmin
>   # Uncomment the lines below to locate data on JASMIN
>   auxiliary_data_dir: /gws/nopw/j04/esmeval/aux_data/AUX
>   rootpath:
>     CMIP6: /badc/cmip6/data/CMIP6
>     CMIP5: /badc/cmip5/data/cmip5/output1
>     CMIP3: /badc/cmip3_drs/data/cmip3/output
>     OBS: /gws/nopw/j04/esmeval/obsdata-v2
>     OBS6: /gws/nopw/j04/esmeval/obsdata-v2
>     obs4MIPs: /gws/nopw/j04/esmeval/obsdata-v2
>     ana4mips: /gws/nopw/j04/esmeval/obsdata-v2
>     CORDEX: /badc/cordex/data/CORDEX/output
>   drs:
>     CMIP6: BADC
>     CMIP5: BADC
>     CMIP3: BADC
>     CORDEX: BADC
>     OBS: default
>     OBS6: default
>     obs4MIPs: default
>     ana4mips: default
>   ```
>
> - Make sure to find offline and change it to true as well as setting max_parallel_tasks to 1.
{: .challenge}


> ## Running a Recipe
> 
> - To run a recipe:
> 
>   ~~~bash
>   esmvaltool run examples/recipe_python.yml 
>   ~~~
> - If everything has been set up correctly, the last line of code should look
>   something like this: 
>
>    ```yaml
>    YYYY-MM-DD HH:mm:SS, NNN UTC [93691] INFO    Run was successful
>    ```
>
> - To view the file used in this example:
>
>   ~~~bash
>    esmvaltool recipes get examples/recipe_python.yml 
>   ~~~
> 
> - Then open `recipe_python.yml` in a text editor.
>
> - You can find out more about this recipe in the 
>   [documentation](https://docs.esmvaltool.org/en/latest/recipes/recipe_examples
.html).
{: .challenge}

> ## Outputs of a Recipie
>
> The outputs of a recipe can be found in the output_dir specified in the 
> config file. 
{: .callout}
