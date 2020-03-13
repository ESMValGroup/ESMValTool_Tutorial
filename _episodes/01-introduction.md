---
title: "Introduction"
teaching: 0
exercises: 0
questions:
- "Key question (FIXME)"
objectives:
- "First learning objective. (FIXME)"
keypoints:
- "First key point. Brief Answer to questions. (FIXME)"
---

Typically, the introduction talk introduces ESMValTool with a focus on how the front-end interface works from a users perspective. There’s no need for an in-depth discussion of how the backend works here. Instead, this talk should include:

- Introduction to ESMValTool project
- The difference between ESMValTool and ESMValCore
- How preprocessors and preprocessor chains work
- How diagnostics work
- The config-user.yml file
- How to include model and observational data in a recipe
- Examples of recipes including plots and results that are included in the main branch.


What do you need to know to write your own ESMValTool recipes?

You’ll need to know:
 - The main parts of ESMValTool
 - How each part of a recipe works.

Starting from an existing recipe:
 - Where are existing recipes?
 - What CMIP data is available on jasmin?
 - How to change the preprocessors arguments?
 - How to find or treat observational data?

How to make your own diagnostic script.
 - Where to find existing diagnostics?
 - What does ESMValTool pass to the diagnostic?


## The difference between ESMValTool and ESMValCore

ESmValTool is built from two repositories.

  - (ESMValTool)[https://github.com/ESMValGroup/ESMValTool]
  - (ESMValCore)[https://github.com/ESMValGroup/ESMValCore]



Most users will only need to interact with the ESMValTool repository.

## Four main parts

There are four main parts of ESMValTool:
1. Preprocessors
2. Diagnostics
3. Recipes
4. Configuration files

You will need to Understand all four parts before you can run a recipe.


### Recipes
Recipes are the instructions telling ESMValTool:
  - What datasets to load ie:
    - Model
    - Project (CMIP5 or 6, observations…)
    - Scenario
    - Ensemble member
    - Time resolution
    - Time range
    - Activity

The recipes also contains lists of preprocessor, and the path to the diagnositcs.

### Preprocessors
The preproccors are the manipulations that are performed to the dataset.
For instance, once the data is loaded, you may want to extract a specific
time range or region. You may want to produce an annual average or
perhaps that the average over the entire region.
Each of these operations is specified as a preprocessor.
The list of preprocessors is specified in the recipe.
ESMVAlTool includes a very diverse and powerful set of preprocessors, including:

- Utilities:
  - Load, concatenate, save files
  - Fix broken metadata or CF compliance
- Time:
  - Extract seasons, months, annual averages.
  - Average along time dimension
- Area/Volume:
  -Regrid
  - Extract regions
  - Extract depth range or specific depth
  - Extract transect
  - Averages weighted by Area or Volume
  - Depth integration
  - Zonal mean
- Masks:
  - Mask region, threshold
  - Multi model statistics:
  - Mean, median, maximum, minimum


### Diagnostics
Diagnostics are functions that run after the completion of the preprocessor.
They take the processed data, read the metadata files and do something with it:
  - Make plots
  - Perform statistics
  - Further process netcdfs.
  - Combine multiple preprocessed datasets
Diagnostics can be written in any of the supported languages:
  - Python
  - NCL
  - R
  - Julia

ESMValTool includes several ready to go diagnostics which are called by related recipes,
but there are no native, generic or “smart” ESMValTool diagnostics.
Each diagnostic and recipe pair perform a specific evaluation.
You will need to know what each diagnostic does and the required format of the input files.
If the plot or statistic that you’re looking for is not already included in ESMValTool,
you may need to make your own diagnostic.

The diagnostics section of the recipe tells ESMValTool which variables to load,
which preprocessors to apply to each variable, and which diagnostic scripts to run.


### User Configuration

The user configuration is not called in the recipe, but rather at run time.
This is a `yaml` file which defines the global settings and preferences.
They tell ESMValTool things like:

  - Your preferred image output format: png, jpg…
  - Debugging preferences:
    - Whether to save intermediary cubes (useful for debugging)
    - Your preferred debugging output level
  - How to locate the model and obs data
  - Where you want to save your output

Exercise:
  Find and inspect the example `user-config.yml` file.





{% include links.md %}
