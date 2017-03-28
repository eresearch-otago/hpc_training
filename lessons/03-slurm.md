# SLURM

NeSI Documentation for running a job: https://support.nesi.org.nz/hc/en-gb/articles/115000194910-Submitting-Slurm-Jobs-on-Pan

Adapted from Jordi Blasco's [Introduction to SLURM documentation](https://wiki.auckland.ac.nz/download/attachments/63145549/introduction-slurm.pdf?api=v2)

SLURM is the software used on the NeSI Pan cluster for managing and allocating the cluster resources when you submit a job. To run a job on the cluster, first you have to tell SLURM the requirements so that it can best allocate resources for all users over the entire cluster.

## About SLURM

- SLURM was an acronym for Simple Linux Utility for Resrouce Management
- evolved into a capable job scheduler
- Job manager and scheduler used on the NeSI Pan cluster

## Features of SLURM

- Full control over CPU and memory usage
- Job array support
- Integration with MPI
- Supports interactive sessions
- Debugger friendly
- Environment privacy
- Job profiling

## Resource management

### Nodes and Job States

Nodes

- state (up/down/idle/allocated/mix/drained)

Jobs

- queued/pending and running
- suspended/preempted
- cancelled/completed/failed

## Queues

On the NeSI Pan cluster there is a queuing system that assigns priority to jobs based on:

- Project type (decided upon at time of project application)
  - merit
  - institution
  - proposal development
  - postgraduate

- Wall time (specified at job runtime)
  - high ( under 6 hours)
  - medium ( over 6 hours but less than 24 hours)
  - low (longer than 24 hours)
 
Other considerations made by the queue are the number of jobs previously run by the user, and how long the job as spent in the queue

## Running a job

### SLURM Commands


- *sbatch* - submits a script job
- *scancel* - cancels a running or pending job
- *srun* - runs a command across nodes
- sbcast Transfer file to a compute nodes allocated job
- interactive - opens an interactive job session
- sattach - connect stdin/out/err for an existing job or job step
- squeue - displays the job queue

### Commonly used SLURM variables

- $SLURM_JOBID (job id)
- $SLURM_JOB_NODELIST (nodes allocated for job)
- $SLURM_NNODES (number of nodes)
- $SLURM_SUBMIT_DIR (directory job was submitted from)
- $SLURM_ARRAY_JOB_ID (job id for the array)
- $SLURM_ARRAY_TASK_ID (job array index value)

### Standard job script directives

```
#!/bin/bash
#SBATCH -J JobName
#SBATCH -A nesi99999		# Project Account
#SBATCH --time=08:00:00		# Walltime
#SBATCH --mem-per-cpu=4096	# memory/cpu (in MB)
#SBATCH --ntasks=2		# 2 tasks
#SBATCH --cpus-per-task=4	# number of cores per task
#SBATCH --nodes=1		# number of nodes
#SBATCH -C sb			# sb=Sandybridge,wm=Westmere
```



### Examples of submitting jobs

Let's submit a first job via SLURM. We will do it using the `sbatch` command to interact with SLURM. Remember to replace [Project Account] with the actual project ID that you want to use.

```
sbatch -A [Project Account] -t 10 --wrap "echo hello world"

```

The options `-t` stands for *time* and sets a limit on the total run time of the job allocation. Note that each partition on which the jobs are run has its own time limit. If the set time limit exceeds the limit for the partition, the job will become "PENDING" (for more information on job statuses, see below).
`--wrap` option means that the following string (in "") will be turned by SLURM in a simple shell script. 







