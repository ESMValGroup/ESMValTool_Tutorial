---
title: "Use a Jupyter Notebook to run a recipe"
teaching: 20
exercises: 30
compatibility: ESMValTool v2.11.0

questions:
- "How to load esmvaltool module in ARE?"
- "How to view and run a recipe in a Jupyter Notebook?"
- "How to run a single diagnostic or preprocessor task?"
objectives:
- "Learn about the esmvalcore experimental API"
- "View the Recipe output in a Jupyter Notebook"
keypoints:
- "ESMValTool can be used in a Jupyter Notebook"
- "ESMValTool uses ESMValCore as a tool"
---

This episode shows us how we can use ESMValTool in a Jupyter notebook. We are using material from a [short tutorial from EGU22][EGU22-tutorial]{:target="_blank"} 
and the [documentation][experimental-api]{:target="_blank"} which is a good place for further reference.

![logo](../assets/img/logobanner.png)

## Start a session in ARE
Log in to [ARE][are]{:target="_blank"} with your NCI account to start a JupyterLab session.
Refer to this [ARE setup guide]({{ page.root }}{% link _extras/02-aresetup.md %}) for more details.
Open the folder to your hackathon folder in `nf33` where you can create a new notebook or use the 
`Introduction_to_ESMValTool.ipynb` notebook.

## Finding a recipe
Let's start by importing the tool and some other tools we can use later. Note that we are importing from `esmvalcore` and calling
it `esmvaltool`.
```python
# Import the tool
import esmvalcore.experimental as esmvaltool

# Import tools for plotting
import matplotlib.pyplot as plt
import iris.quickplot
```
There is a *utils* submodule we can use to find and get recipes. Call the `get_all_recipes()` function to get a
list of all available recipes from which you can use the `find()` method to return any matches. If you already know the
recipe you want you can use the `get_recipe()` function.

> Let's use the `examples/recipe_python.yml` for this exercise, the documentation for it can be found 
> [here](https://docs.esmvaltool.org/en/latest/recipes/recipe_examples.html). Then see what's in the recipe metadata.
>
> > ## Solution
> > ```python
> > example_recipe = esmvaltool.get_recipe("examples/recipe_python.yml")
> > example_recipe
> > ```
> > For reading the recipe:
> > ```python
> > print(example_recipe.path.read_text())
> > ```
> > The `example_recipe` here is a Recipe class with attributes `data` and `name`, see the 
> > [reference][experimental-recipe]{:target="_blank"}.
> {: .solution}
{: .challenge}

> ## Pro tip: remember the command line?
> This is another way of doing a similar thing from the command line:
> ```bash 
> esmvaltool recipes get <recipe file>
> ```
{: .callout}

## Configuration in the notebook
We can look at the default user configuration file, `~/.esmvaltool/config-user.yml` 
which a `CFG` object can be called as a dictionary which gives us the ability to edit the settings.
Since version 2.4, the tool can automatically download the climate data files required to run a recipe for you, 
enable the setting and ensure the download directory is created.
```python
esmvaltool.CFG

esmvaltool.CFG['offline'] = False
esmvaltool.CFG['download_dir'].mkdir(exist_ok=True)
```
You can also check your output directory where your recipe runs will be saved.
As the command in the `xp65` module we use is a wrapper, check this location 
is your `\scratch\nf33\$USERNAME\esmvaltool_outputs\`
```python
CFG['output_dir']
```
This `CFG` object is from the `config` module in the ESMValCore API, for more details see [here][api-config].

## Running the recipe
You can now run the recipe and get the output.
```python
output = example_recipe.run()
output
```
## Recipe output
The output can return the files as well as the image files and data files, we will show some in this tutorial,
also see the [reference page][experimental-output].
> Let's look through this recipe output.
> - Get the file paths.
> - Look at one of the plots.
> - Access the data used for the plots.
>
> > ## Solution
> > Print the file paths.
> > ```python
> > for result in output['map/script1']:
> >     print(result.path)
> > ```
> > Look at a plot from the list of plots.
> > ```python
> > plots = [f for f in output['map/script1'] if isinstance(f, esmvaltool.recipe_output.ImageFile)]
> > plots[0]
> > ```
> > Load one of the preprocessed data files.
> > ```python
> > data_files = [f for f in output['map/script1'] if isinstance(f, esmvaltool.recipe_output.DataFile)]
> > 
> > cube = data_files[0].load_iris()[0]
> > cube
> > ```
> {: .solution}
{: .challenge}

> ## Use the loaded data to make your own plot in your notebook.
>
> > ## Solution
> > ```python
> > # Create plot
> > iris.quickplot.contourf(cube)
> > 
> > # Set the size of the figure
> > plt.gcf().set_size_inches(12, 10)
> > 
> > # Draw coastlines
> > plt.gca().coastlines()
> > 
> > # Show the resulting figure
> > plt.show()
> > ```
> > ![image]()
> {: .solution}
{: .challenge}

{% include links.md %}
