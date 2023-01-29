# Understanding jobs
The HPC system involves a lot of coordination between requests and resources. When you're doing computational work on your own computer, at any moment you can spontaneously decide to run some code to analyze some data, just like how you can simply decide to put on some shoes and walk to the corner store for a snack. The HPC cluster is more like an airport: it's a shared infrastructure hub that tries to safely accommodate the comings and goings of as many flights as possible, but during busy periods, that can mean delays. In this airport analogy, the "planes" are what's known in HPC jargon as "jobs". Jobs are a request for HPC resources, and any computation you want to do requires HPC resources. When you're working on an HPC cluster, it's not "just running some code really quick" -- to do anything, you have to submit a job, which will be prioritized as part of a queue, just like an airplane that wants to take off.

There are two kinds of jobs: *interactive* and *batch*. 

## Interactive jobs
Interactive jobs let you reserve some compute resources on an HPC cluster and then use them as if you were interacting with the command line on your own computer. You don't need to learn any new syntax for job creation, you can just run Python (or R, or any other) code from the command line, and you can see any outputs or messages resulting from that code (e.g. any `print` commands) as they are created.

Interactive jobs are much simpler to set up than batch jobs, but there are limitations: while the details vary between HPC systems, you only get a small amount of compute and memory, and by default, a single hour. (You can extend the time, but you can't expand the compute resources available in an interactive job.)

To launch an interactive job, just type `sdev` and hit enter. If you want more time, you can add `-t` and the amount of time you want. For instance, to keep your interactive job open for two hours, you can type `sdev -t 2:00:00`. Interactive job allocation is usually very fast -- you shouldn't have to plan on waiting more than a couple minutes.

```{admonition} 🌲 Interactive jobs on Sherlock
On Sherlock, interactive jobs are limited to a single core, 4 GB of memory, and one hour.

```

When does it make sense to use an interactive job? There are a few scenarios:


### Testing your code
Even if you write code that works on your own computer, you may run into some issues when running it on an HPC cluster. It's faster to iron out those issues using an interactive job than request resources (especially if you're requesting a *lot* of resources to analyze a lot of data), wait for them to be allocated, then watch the job fail due to problems with Python versions, installing libraries, etc. For more about this, see [Troubleshooting](troubleshooting).

### Estimating resource needs
When you're submitting a batch job, you need to be specific about what resources you're requesting: how many cores (or nodes, depending on how the HPC cluster is set up), and how long you need them for. But how do you know what to ask for? An interactive job can be a way to ballpark how long your job will take. If your job consists of a bunch of small tasks, unrelated to one another (e.g. NLP parsing every text in your corpus) you can run your code on a small sample of your data, see how long that takes, and then multiply it by the number of texts you have. (Don't forget to add some extra time as a buffer.) If your job involves calculations that are inter-connected (e.g. clustering a group of texts), unfortunately, you can't accurately simulate the time required by running it on a subset of texts in an interactive job, though there are other tricks you can use to try to estimate the resources needed.

### Quick calculations
If you're using HPC because of security restrictions on your data, rather than your data being inherently so large that you can't easily run it on your own computer, you might want an interactive job to do quick calculations. Imagine you've already run a set of readability metrics for most of the texts in your corpus, but you just acquired three new text files. It might not be worth the hassle of setting up and submitting a batch job to run the metrics on just those three files when you could start an interactive job and run your Python script for calculating metrics on just those three individual files.

## Batch jobs
The vast majority of computation done on HPC clusters happens using batch jobs. To run a batch job, you have to create a plain-text file with instructions for the cluster: both what you're asking from it in terms of resources (memory, CPUs/nodes, amount of time) and what you want it to do (e.g. load some libraries and run a script). These plain-text files use the extension `.sbatch`.

### Anatomy of an sbatch file
Every sbatch file has what's called a "Slurm script header" at the top. We'll cover more about Slurm below, but for the moment, you can think of it as the air traffic controllers in the metaphorical HPC airport -- it's the scheduling system that receives your request, and figures out where your spot is in the queue, among other related tasks. The "Slurm script header" is the specifics of your request: the "what" and the "how much" and the "how long". Here's an example of a simple sbatch file, with components explained below. The Slurm script header is at the top, and each line is preceded by a # symbol:

```
#!/bin/bash
#SBATCH --job-name=myjob
#SBATCH --time=1:00:00 
#SBATCH -c=1
#SBATCH --mem=4G
#SBATCH --output=/home/users/qad/myjob-output.txt
#SBATCH --error=/home/users/qad/myjob-errors.txt
#SBATCH --mail-type=END,FAIL
#SBATCH --mail-user=email@university.edu

ml reset
ml python/3.6.1

pip3 install --user textstat==0.7.2

srun python3 /home/users/qad/readability.py
```

#### job-name
Every job has a name. You'll see the name when you check and see what jobs you have running, but it doesn't really matter what you call it. Your name shouldn't have any spaces or special characters in it.

#### time
This is the parameter for how long you want the job to run. It's formatted, very precisely, as hours:minutes:seconds, so in this example, we're requesting one hour of compute time.

#### c
In an Slurm script header, -c is short for cores. 

#### mem
How much memory each you're requesting.

#### output
This is the file where you'll see all the text that would otherwise be written to the screen if you were running the code in interactive mode, e.g. things like the output of any Python `print()` statement. If your script includes a step for creating output files with the results (e.g. readability scores for your texts), those files will *also* be created using the filename, location, etc. that you specify in your script. This output is *only* what would have been written to the screen.

#### error
This is the file where you'll see any errors that your script produces, which can be helpful if you're trying to debug.

#### mail-type
You don't have to ask the HPC system to email you when your job completes or fails, but it can be convenient so you don't have to monitor its status. If you don't want email, you can leave out this line and `mail-user`

#### mail-user
This is the email address that will be sent mail if you include the `mail-type` parameter.

### What comes next?
The sbatch header just contains the commands specific to the Slurm scheduler. Next, you need to load the software that you're going to use (even programming languages need to be loaded), and make sure any libraries get installed.

### Loading software & libraries
After the sbatch header, the next line is usually `ml reset`, which unloads any software that was already loaded in your environment, possibly from earlier tests or experiments you were doing. It helps to start with a clean slate to reduce the number of things that can conflict with each other and cause problems.

Once you've reset the environment, you can load in the software that you do want. For more details on how to find out what software you want, see [Software](software). In this example, we'll say that we want to load Python version 3.6.1 (the most recent version of Python available on my cluster as of writing this, but it's worth periodically checking for updates as described in the [Software](software) section.) That command is `ml python/3.6.1`

Next, if there are any specific libraries that you need to run your code, you should add a line to install them. For instance, to install the Python Textstat library, you can use `pip3 install --user textstat==0.7.2`

### Running your script
Once you've loaded your software and installed any libraries, you can launch your actual script using the command `srun`. Make sure your script is somewhere on the HPC system before you run this code; in this case, I've put it in my home directory (see more about [data storage](data-storage)).

`srun python3 /home/users/qad/readability.py`

### Launching a batch job
Once you've created your plain-text .sbatch file, you'll need to put it on the HPC system (see [data transfer](data-transfer)). Your home directory is a convenient place for it, since that's where you will be when you log in.

Assuming that you're in the same directory as your .sbatch file (and you should be, if you've just [logged in](logging-in) to the HPC system and your .sbatch file is in your home directory), you can start the job by typing this (if your .sbatch file is named `myjob.sbatch`):

`sbatch myjob.sbatch`

It won't obviously do anything -- there isn't a "job submitted successfully, everything is fine!" message that pops up. The way to find out what's going on with your job is to check on its status.

### Checking on batch jobs
The command to check on batch jobs is: `squeue -u $USER` (or you can just put your own username instead of $USER). If you have any jobs running (or that very recently finished or failed), you'll see a table with the system's numeric job ID, the partition (part of the HPC system it's running on, defaults to `normal`), the name you gave the job in your sbatch script, your username, the status, how long it's been running (if it's running yet; sometimes jobs have to wait), how many nodes it's using, and which nodes it's using.

### Canceling batch jobs
Maybe you've realized you made an error in your code, or didn't upload the right data file to the system. It's better to cancel the job than to let it run to completion or error, if only because having run fewer jobs will get you higher priority when you want to resubmit it. To cancel a job, first run `squeue -u $USER` and take note of the job ID of the job you want to cancel; let's imagine the job number is 1234. Then type `scancel 1234` (or whatever the job number is).


## Wait times




*Special thanks to the [Schumer Lab](https://openwetware.org/wiki/Schumer_lab:_Submitting_slurm_jobs) for their documentation on job submission*