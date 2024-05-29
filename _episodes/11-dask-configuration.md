---
title: "Dask Configuration"
teaching: 10
exercises: 10
compatibility: ESMValCore v2.10.0

questions:
- What is the [Dask](https://www.dask.org/) configuration file and how should I use it?

objectives:
- Understand the contents of the dask.yml file
- Prepare a personalized dask.yml file
- Configure ESMValCore to use some settings

keypoints:
- The ``dask.yml`` file tells ESMValCore how to configure Dask.
- "``client`` can be used to an already running Dask cluster."
- "``cluster`` can be used to start a new Dask cluster for each run."
- "The Dask default scheduler can be configured by editing the files in ~/.config/dask."

---

## The Dask configuration file

The preprocessor functions in ESMValCore use the
[Iris](https://scitools-iris.readthedocs.io) library, which in turn uses Dask
Arrays to be able to process datasets that are larger than the available memory.
It is not necesary to understand how these work exactly to use the ESMValTool,
but if you are interested there is a
[Dask Array Tutorial](https://tutorial.dask.org/02_array.html) as a well as a
[guide to "Lazy Data"](https://scitools-iris.readthedocs.io/en/stable/userguide/real_and_lazy_data.html)
available. Lazy data is the term the Iris library uses for Dask Arrays.

The most important concept to understand when using Dask Arrays is the concept
of a Dask "worker". With Dask, computations are run in parallel by Python
processes or threads called "workers". These could be on the
same machine that you are running ESMValTool on, or they could be on one or
more other computers. Dask workers typically require 2 to 4 gigabytes of
memory (RAM) each. In order to avoid running out of memory, it is important
to use only as many workers as your computer(s) have memory for. ESMValCore
(or Dask) provide configuration files where you can configure the number of
workers.

In order to distribute the computations over the workers, Dask makes use of a
"scheduler". There are two different schedulers available. The default
scheduler can be good choice for smaller computations that can run
on a single computer, while the scheduler provided by the Dask Distributed
package is more suitable for larger computations. 

> ## On using ``max_parallel_tasks``
>
> In the config-user.yml file, there is a setting called ``max_parallel_tasks``.
> With the Dask Distributed scheduler, all the tasks running in parallel
> can use the same workers, but with the default scheduler each task will
> start its own workers. For recipes that process large datasets, it is usually
> beneficial to set ``max_parallel_tasks: 1``, while for recipes that process
> many small datasets it can be beneficial to increasing this number.
>
{: .callout}

## Starting a Dask distributed cluster

Let's start the the tutorial by configuring ESMValCore so it runs its
computations using 2 workers.

We use a text editor called ``nano`` to edit the configuration file:

~~~bash
  nano ~/.esmvaltool/dask.yml
~~~

Any other editor can be used, e.g. many systems have ``vi`` available.

This file contains the settings for:

- Starting a new cluster of Dask workers
- Or alternatively: connecting to an existing cluster of Dask workers

Add the following content to the file ``~/.esmvaltool/dask.yml``:

```yaml
cluster:
  type: distributed.LocalCluster
  n_workers: 1
  threads_per_worker: 2
  memory_limit: 4GiB
```

This tells ESMValCore to start a cluster of one worker, that can use 2
gigabytes (GiB) of memory and run computations using 2 threads. For a more
extensive description of the available arguments and their values, see
[``distributed.LocalCluster``](https://distributed.dask.org/en/stable/api.html#distributed.LocalCluster).

To see this configuration in action, run we will run a version
of [recipe_easy_ipcc.yml](https://docs.esmvaltool.org/en/latest/recipes/recipe_examples.html) with just two datasets. Download
the recipe [here](../files/recipe_easy_ipcc_short.yml) and run it
with the command:

~~~bash
  esmvaltool run recipe_easy_ipcc_short.yml
~~~

After finding and downloading all the required input files, this will start
the Dask scheduler and workers required for processing the data. A message that
looks like this will appear on the screen:

```
2024-05-29 12:52:38,858 UTC [107445] INFO    Dask dashboard: http://127.0.0.1:8787/status
```

Open the Dashboard link in a browser to see the Dask Dashboard website.
When the recipe has finished running, the Dashboard website will stop working.
The top left panel shows the memory use of each of the workers, the panel on the
right shows one row for each thread that is doing work, and the panel at the
bottom shows the progress.

> ## Explore what happens if workers do not have enough memory
>
> Reduce the amount of memory that the workers are allowed to use to 2GiB and
> run the recipe again. Note that the bars representing the memory use turn
> orange as the worker reaches the maximum amount of memory it is
> allowed to use and starts 'spilling' (writing data temporarily) to disk.
> The red blocks in the top right panel represent time spent reading/writing
> to disk.
>
>> ## Solution
>>
>> We use `memory_limit` entry in the `~/.esmvaltool/dask.yml` file to set the
>> amount of memory allowed to 2 gigabytes:
>>```yaml
>> cluster:
>>    type: distributed.LocalCluster
>>    n_workers: 1
>>    threads_per_worker: 2
>>    memory_limit: 2GiB
>>```
>>
> {: .solution}
{: .challenge}


> ## Tune the configuration to your own computer
>
> Look at how much memory you have available on your machine (run the command
> ``grep MemTotal /proc/meminfo`` on Linux), set the ``memory_limit`` back to
> 4 GiB and increase the number of Dask workers so they use total amount
> available minus a few gigabytes for your other work.
>
>> ## Solution
>>
>> For example, if your computer has 16 GiB of memory, it can comfortably use
>> 12 GiB of memory for Dask workers, so you can start 3 workers with 4 GiB
>> of memory each.
>> Use the `num_workers` entry in the `~/.esmvaltool/dask.yml` file to set the
>> number of workers to 3.
>>```yaml
>> cluster:
>>    type: distributed.LocalCluster
>>    n_workers: 3
>>    threads_per_worker: 2
>>    memory_limit: 4GiB
>>```
>>
> {: .solution}
{: .challenge}

{% include links.md %}
