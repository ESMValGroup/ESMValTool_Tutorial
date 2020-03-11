---
title: "Toy example"
teaching: 0
exercises: 0
questions:
- "Key question (FIXME)"
objectives:
- "Explain how ESMValTool is structured."
- "Execute ESMValTool using the example recipe and diagnostic script provided."
- "Inspect the output that was created by running ESMValTool"
keypoints:
- ""
- "The recipe tells ESMValTool what parts of its core functionality it should run and on what
data."
- "The diagnostic script is tailor made by the diagnostic developer where functionalities not
supported by ESMValTool can be implemented in the processing pipeline."
---
Now we have installed and configured ESMValTool it is time to run a simple example and inspect
what it produces.

For this we need our general ESMValTool settings from episode 3 (configuration), the example data
that you downloaded in the
[setup](https://escience-academy.github.io/lesson-esmvaltool/setup.html) and an example recipe to
tell ESMValTool what it should do. Let's make sure you have all set to continue. Check whether
you can find these three items in your ESMValTool folder.

> ## Exercise
>
> Find all the
>
{: .challenge}


As already mentioned in the
[introduction](https://escience-academy.github.io/lesson-esmvaltool/01-introduction/index.html),
ESMValTool consists of two parts: the core and the diagnostics. The core part (also called
ESMValCore after its repository name) contains the code to run the actual software as well as
several preprocessing functions that are widely used. The diagnostics part (confusingly situated
in the repository called ESMValTool) mainly contains so called recipes and diagnostic scripts.

The recipes describe the analysis pipeline and tell ESMValTool which actions to perform on what
data.

~~~
esmvaltool -c config-user.yml recipes/recipe_python.yml
~~~

Once this recipe has run, you will notice a new folder has been created in
the destination directiory. The destination directory is defined in the
`output_dir` item of your `config-user.yml` file.

The new directory will have the format:
~~~
output_dir/recipe_date_time
~~~
For instance, the default output for the command above will be something like:
~~~
esmvaltool_output/recipe_python_20200311_131241
~~~

If ESMValTool has run succesfully, this directory will contain four subdirectories:
~~~
run preproc plots work
~~~
These directory contain:

- run: All the log and debug files, a copy of the recipe, the settings.yml, the metadata.yml file.
- preproc: The preprocessed netCDF data files and metadata.
- plots: This is the output location for all the plots produced in the diagnostic, split up into individual diagnosics and fields.
- work: This is a place for post-processed files such as NetCDF file containing data derived in the diagnostic. The content of this file is specific to each diagnostic.

{: .source}

{% include links.md %}
