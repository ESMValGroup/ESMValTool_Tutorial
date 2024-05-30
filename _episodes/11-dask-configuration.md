---
title: "Configuring Dask"
teaching: 20 (+ optional 10)
exercises: 40 (+ optional 20)
compatibility: ESMValCore v2.10.0

questions:
  - "What is the Dask configuration file and how should I use it?"
  - "What are Dask workers"
  - "What is the Dask scheduler"

objectives:
  - "Understand the contents of the dask.yml file"
  - "Prepare a personalized dask.yml file"

keypoints:
  - "The ``~/.esmvaltool/dask.yml`` file tells ESMValCore how to configure Dask."
  - "``cluster`` can be used to start a new Dask cluster for each run."
  - "``client`` can be used to connect to an already running Dask cluster."
  - "The Dask default scheduler can be configured by editing the files in ``~/.config/dask``."
  - "The Dask Dashboard can be used to see if the Dask workers have sufficient memory available."

---

## Introduction

When processing larger amounts of data, and especially when the tool crashes
when running a recipe because there is not enough memory available, it is
usually beneficial to change the default
[Dask configuration](https://docs.esmvaltool.org/
projects/ESMValCore/en/latest/quickstart/configure.html#dask-configuration).

The preprocessor functions in ESMValCore use the
[Iris](https://scitools-iris.readthedocs.io) library, which in turn uses Dask
Arrays to be able to process datasets that are larger than the available memory.
It is not necesary to understand how these work exactly to use the ESMValTool,
but if you are interested there is a
[Dask Array Tutorial](https://tutorial.dask.org/02_array.html) as a well as a
[guide to "Lazy Data"](https://scitools-iris.readthedocs.io/
en/stable/userguide/real_and_lazy_data.html)
available. Lazy data is the term the Iris library uses for Dask Arrays.


### Dask Workers
The most important concept to understand when using Dask Arrays is the concept
of a Dask *worker*. With Dask, computations are run in parallel by little
Python programs that are called *workers*. These could be on running on the
same machine that you are running ESMValTool on, or they could be on one or
more other computers. Dask workers typically require 2 to 4 gigabytes (GiB) of
memory (RAM) each. In order to avoid running out of memory, it is important
to use only as many workers as your computer(s) have memory for. ESMValCore
(or Dask) provide configuration files where you can configure the number of
workers.

Note that only array computations are run using Dask, so total runtime may not
decrease as much as you might expect when you increase the number of Dask
workers.

### Dask Scheduler

In order to distribute the computations over the workers, Dask makes use of a
*scheduler*. There are two different schedulers available. The default
scheduler can be good choice for smaller computations that can run
on a single computer, while the scheduler provided by the Dask Distributed
package is more suitable for larger computations.

> ## On using ``max_parallel_tasks``
>
> In the config-user.yml file, there is a setting called ``max_parallel_tasks``.
> Any variable or diagnostic script in the recipe is considered a 'task' in this
> context and when settings this to a value larger than 1, these will be
> processed in parallel on the computer running the ``esmvaltool`` command.
>
> With the Dask Distributed scheduler, all the tasks running in parallel
> can use the same workers, but with the default scheduler each task will
> start its own workers. If a recipe does not run with ``max_parallel_tasks``
> set to a value larger than 1, try reducing the value or setting it to 1.
> This is especially the case for recipes with high resolution data or many
> datasets per variable.
>
{: .callout}

## Starting a Dask distributed cluster

The workers and the scheduler together are called a Dask "cluster".
Let's start the the tutorial by configuring ESMValCore so it runs its
computations on a cluster with just one worker.

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

This tells ESMValCore to start a new cluster of one worker, that can use 2
gigabytes (GiB) of memory and run computations using 2 threads. For a more
extensive description of the available arguments and their values, see
[``distributed.LocalCluster``](https://distributed.dask.org/
en/stable/api.html#distributed.LocalCluster).

To see this configuration in action, run we will run a version of
[recipe_easy_ipcc.yml](https://docs.esmvaltool.org/
en/latest/recipes/recipe_examples.html) with just two datasets.
This recipe takes a few minutes to run, once you have the data available.
Download the recipe [here](../files/recipe_easy_ipcc_short.yml) and run it
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
bottom shows the progress of all work that the scheduler currently has been
asked to do.

> ## Explore what happens if workers do not have enough memory
>
> Reduce the amount of memory that the workers are allowed to use to 2GiB and
> run the recipe again. Watch what happens.
>
>> ## Solution
>>
>> We use `memory_limit` entry in the `~/.esmvaltool/dask.yml` file to set the
>> amount of memory allowed to 2GiB:
>>```yaml
>> cluster:
>>    type: distributed.LocalCluster
>>    n_workers: 1
>>    threads_per_worker: 2
>>    memory_limit: 2GiB
>>```
>> Note that the bars representing the memory use turn
>> orange as the worker reaches the maximum amount of memory it is
>> allowed to use and it starts 'spilling' (writing data temporarily) to disk.
>> The red blocks in the top right panel represent time spent reading/writing
>> to disk. While 2 GiB per worker may be enough in other cases, it is
>> apparently not enough for this recipe.
>>
> {: .solution}
{: .challenge}


> ## Tune the configuration to your own computer
>
> Look at how much memory you have available on your machine (e.g. by running
> the command ``grep MemTotal /proc/meminfo`` on Linux), set the
> ``memory_limit`` back to 4 GiB per worker and increase the number of Dask
> workers so they use total amount available minus a few gigabytes for your
> other work. Run the recipe again and notice that it completed faster.
>
>> ## Solution
>>
>> For example, if your computer has 16 GiB of memory and you do not have too
>> many other programs running, it can use 12 GiB of memory for Dask workers,
>> so you can start 3 workers with 4 GiB of memory each.
>>
>> Use the `num_workers` entry in the `~/.esmvaltool/dask.yml` file to set the
>> number of workers to 3:
>>```yaml
>> cluster:
>>    type: distributed.LocalCluster
>>    n_workers: 3
>>    threads_per_worker: 2
>>    memory_limit: 4GiB
>>```
>> and run the recipe again with the command
>> ``esmvaltool run recipe_easy_ipcc_short.yml``.
>> The time it took to run the recipe is printed to the screen.
>>
> {: .solution}
{: .challenge}

## Using an existing Dask Distributed cluster

In some cases, it can be useful to start the Dask Distributed cluster before
running the ``esmvaltool`` command. For example, if you would like to keep the
Dashboard available for further investigation after the recipe completes
running, or if you are working from a Jupyter notebook environment, see
[dask-labextension](https://github.com/dask/dask-labextension) and
[dask_jobqueue interactive use](https://jobqueue.dask.org/
en/latest/interactive.html)
for more information.

To use a cluster that was started in some other way, the following configuration
can be used in ``~/.esmvaltool/dask.yml``:

```yaml
client:
  address: "tcp://127.0.0.1:33041"
```
where the address depends on the Dask cluster. Code to start a
[``distributed.LocalCluster``](https://distributed.dask.org/
en/stable/api.html#distributed.LocalCluster)
that automatically scales between 0 and 2 workers depending on demand, could
look like this:

```python
from time import sleep

from distributed import LocalCluster

if __name__ == '__main__':  # Remove this line when running from a Jupyter notebook
    cluster = LocalCluster(
        threads_per_worker=2,
        memory_limit='4GiB',
    )
    cluster.adapt(minimum=0, maximum=2)
    # Print connection information
    print(f"Connect to the Dask Dashboard by opening {cluster.dashboard_link} in a browser.")
    print("Add the following text to ~/.esmvaltool/dask.yml to connect to the cluster:" )
    print("client:")
    print(f'  address: "{cluster.scheduler_address}"')
    # When running this as a Python script, the next two lines keep the cluster
    # running for an hour.
    hour = 3600 # seconds
    sleep(1 * hour)
    # Stop the cluster when you are done with it.
    cluster.close()
```

> ## Start a cluster and use it
>
> Copy the Python code above into a file called ``start_dask_cluster.py`` (or
into a Jupyter notebook if you prefer) and start the cluster using the command
``python start_dask_cluster.py``. Edit the ``~/esmvaltool/dask.yml`` file so
ESMValCore can connect to the cluster. Run the recipe again and notice that the
Dashboard remains available after the recipe completes.
>
>> ## Solution
>>
>> If the script printed
>> ```
>> Connect to the Dask Dashboard by opening http://127.0.0.1:8787/status in a browser.
>> Add the following text to ~/.esmvaltool/dask.yml to connect to the cluster:
>> client:
>>   address: "tcp://127.0.0.1:34827"
>> ```
>> to the screen, edit the file ``~/.esmvaltool/dask.yml`` so it contains the
lines
>> ```yaml
>> client:
>>   address: "tcp://127.0.0.1:34827"
>> ```
>> open the link "http://127.0.0.1:8787/status" in your browser and
>> run the recipe again with the command ``esmvaltool run recipe_easy_ipcc_short.yml``.
> {: .solution}
{: .challenge}

When running from a Jupyter notebook, don't forget to `close()` the cluster
when you are running on an HPC facility (see below), to avoid wasting
compute hours you are not using.

## Using the Dask default scheduler

It is recommended to use the Distributed scheduler explained above for
processing larger amounts of data. However, in many cases the default scheduler
is good enough. Note that it does not provide a Dashboard, so it is less
instructive and that is why we did not use it earlier in this tutorial.

To use the default scheduler, comment out all the contents of
``~/.esmvaltool/dask.yml`` and create a file in ``~/.config/dask``, e.g. called
``~/.config/dask/default.yml`` but the filename does not matter, with the
contents:
```yaml
scheduler: threads
num_workers: 4
```
to set the number of workers to 4. The ``scheduler`` can also be set to
``synchronous``. In that case it will use a single thread, which may be useful
for debugging.

> ## Use the default scheduler
>
> Follow the instructions above to use the default scheduler and run the recipe
> again. To keep track of the amount of memory used by the process, you can
> start the ``top`` command in another terminal. The amount of memory is shown
> in the ``RES`` column.
>
>> ## Solution
>>
>> The recipe runs a bit faster with this configuration and you may have seen
>> a memory use of around 5 GB.
>>
> {: .solution}
{: .challenge}

## Optional: Using dask_jobqueue to run a Dask Cluster on an HPC system

The [``dask_jobqueue``](https://jobqueue.dask.org) package provides functionality
to start Dask Distributed clusters on High Performance Computing (HPC) or
High Throughput Computing (HTC) systems. This section is optional and only
useful if you have access to a such a system.

An example configuration for the
[Levante HPC system](https://docs.dkrz.de/doc/levante/index.html)
could look like this:

```yaml
cluster:
  type: dask_jobqueue.SLURMCluster  # Levante uses SLURM as a job scheduler
  queue: compute                    # SLURM partition name
  account: bk1088                   # SLURM account name
  cores: 128                        # number of CPU cores per SLURM job
  memory: 240GiB                    # amount of memory per SLURM job
  processes: 64                     # number of Dask workers per SLURM job
  interface: ib0                    # use the infiniband network interface for communication
  local_directory: "/scratch/username/dask-tmp"  # directory for spilling to disk
  n_workers: 64                     # total number of workers to start
```

In this example we use the popular SLURM scheduduler, but other schedulers are
also supported, see [this list](https://jobqueue.dask.org/en/latest/api.html).

In the above example, ESMValCore will start 64 Dask workers
(with 128 / 64 = 2 threads each) and for that it will need to launch a single
SLURM batch job on the ``compute`` partition. If you would set ``n_workers`` to
e.g. 256, it would launch 4 SLURM batch jobs which would each start 64 workers
for a total of 4 x 64 = 256 workers. In the above configuration, each worker is
allowed to use 240 GiB per job / 64 workers per job = ~4 GiB per worker.

It is important to read the documentation about your HPC system and answer
questions such as:
- Which batch scheduler does my HPC system use?
- How many CPU cores are available per node (a computer in an HPC system)?
- How much memory is available for use per node?
- What is the fastest network interface (run `ip a` to find the available
  interfaces, infiniband `ib*` is much faster than ethernet `eth*`)?
- What path should I use for storing temporary files on the nodes (try to
  avoid slower network storage if possible)?
- Which computing queue has the best availability?
- Can I use part of a node or do I need to use the full node?
  - If you are always charged for using the full node, asking for only part of
    a node is wasteful of computational resources.
  - If you can ask for part of a node, make sure the amount of memory you
    request matches the number of CPU cores if possible, or you will be charged
    for a larger fraction of the node.

in order to find the optimal configuration for your situation.

> ## Tune the configuration to your own computer
>
> Answer the questions above and create an ``~/.esmvaltool/dask.yml`` file that
> matches your situation. To benefit from using an HPC system, you will probably
> need to run a larger recipe than the example we have used so far. You could
> try the full version of that recipe (
> ``esmvaltool run examples/recipe_easy_ipcc.yml``) or use your own recipe.
> To understand how the different settings affect performance, you may want to
> experiment with different configurations.
>
>> ## Solution
>>
>> The best configuration depends on the HPC system that you are using.
>> Discuss your answer with the instructor and the class if possible.
>> If you are taking this course by yourself, you can have a look at the
>> [Dask configuration examples in the ESMValCore documentation](
>> https://docs.esmvaltool.org/projects/ESMValCore/en/latest/quickstart/
>> configure.html#dask-distributed-configuration).
>>
> {: .solution}
{: .challenge}

{% include links.md %}
