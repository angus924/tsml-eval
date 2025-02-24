# ADA Python

Installation guide for Python packages on ADA and useful slurm commands.

The HPC webpage provides a lot of useful information and getting started guides for using ADA.

https://my.uea.ac.uk/divisions/it-and-computing-services/service-catalogue/research-it-services/hpc/ada-cluster

Server address: ada.uea.ac.uk

## Windows interaction with ADA

You need to be on a UEA network machine or have the VPN running to connect to ADA. Connect to ada.uea.ac.uk.

The recommended way of connecting to the Kraken is using Putty as a command-line interface and WinSCP for file management.

Copies of data files used in experiments must be stored on the cluster, the best place to put these is on your user area scratch storage. Alternatively, you can read from someone elses directory (i.e. ajb).

## Installing on cluster

Complete these steps sequentially for a fresh install.

### 1. Enter interactive mode

By default commands will be run on the login node. Beyond simple commands or scripts, an interactive session should be started.

>interactive

### 2. Clone the code from GitHub

The default location for files should be your user area. Either copy over the code files you want to run manually or clone them from a GitHub page.

>git clone GITHUB_LINK

e.g. https://github.com/time-series-machine-learning/tsml-eval

### 3. Activate an ADA Python installation

Python is activated by default, but it is good practice to manually select the version used. The ADA module should be added before creating and editing an environment.

>module add python/anaconda/2019.10/3.7

You may also need to activate the shell script like this to use some conda commands:

> source /gpfs/software/ada/python/anaconda/2019.10/3.7/etc/profile.d/conda.sh

You can check the current version using:

>conda --version

>python --version

### 4. Create a conda environment

Create a new environment with a name of your choice. Replace PYTHON_VERSION with 3.10 for CPU jobs and 3.8 for GPU jobs.

>conda create -n ENV_NAME python=PYTHON_VERSION

Activate the new environment.

>conda activate ENV_NAME

Your environment should be listed now when you use the following command:

>conda info --envs

__Tip__: instead of running the module, source, and conda activate commands every time, if the following line is added to the .bashrc file everything is done in one step (command ALIAS_NAME):

> alias ALIAS_NAME="module add python/anaconda/2019.10/3.7; source /gpfs/software/ada/python/anaconda/2019.10/3.7/etc/profile.d/conda.sh; conda activate ENV_NAME;"

Note that this ALIAS_NAME has to be run after the interactive.

### 5 Install package and dependencies

Install the package and required dependencies. The following are examples for a few packages and scenarios.

After installation, the installed packages can be viewed with:

>pip list

>conda list

#### 5.1 tsml-eval CPU

Move to the package directory and run:

>pip install --editable .

For release specific dependency versions you can also run:

>pip install -r requirements.txt

Extras may be required, install as needed i.e.:

>pip install esig tsfresh

If any a dependency install is "Killed", it is likely the interactive session has run out of memory. Either give it more memory, or use a non-cached package i.e.

>pip install PACKAGE_NAME --no-cache-dir

#### 5.1 tsml-eval GPU

For GPU jobs we require two additional ADA modules, CUDA and cuDNN:

>module add cuda/10.2.89
>module add cudnn/7.6.5

A specific Tensorflow version is required to match the available CUDA install.

>pip install tensorflow==2.3.0 tensorflow_probability==0.11.1

Next, move to the package directory and run:

>pip install --editable .

## Running CPU experiments

TODO

**NOTE: Scripts will not run properly if done whilst the conda environment is active.**

base it on scripts here, see comments for options e.g. queues
https://github.com/time-series-machine-learning/tsml-eval/tree/main/ada_uea_experiments
For GPU, you need a specific queue

## Running GPU experiments

TODO

Book more slots on GPU
https://outlook.office365.com/owa/calendar/ITCSResearchandSpecialistComputingHPCGPUFarm@ueanorwich.onmicrosoft.com/bookings/

## Monitoring jobs on ADA

check queues

>lshosts

list processes for user (mind that the quotes may not be the correct ones)

>squeue -u USERNAME --format="%12i %15P %20j %10u %10t %10M %10D %20R" -r

__Tip__: to simplify and just use 'queue' in the terminal to run the above command, add this to the .bashrc file located in your home:

> alias queue='squeue -u USERNAME --format="%12i %15P %20j %10u %10t %10M %10D %20R" -r'

GPU queue

>squeue -p gpu-rtx6000-2

To kill all user jobs

>scancel -u USERNAME

To delete all jobs on a queue it’s:

>scancel -p QUEUE

To delete one job it’s:

>scancel 11133013_1

## Helpful links

ADA webpage:
https://my.uea.ac.uk/divisions/it-and-computing-services/service-catalogue/research-it-services/hpc/ada-cluster

ADA submitting jobs page:
https://my.uea.ac.uk/divisions/it-and-computing-services/service-catalogue/research-it-services/hpc/ada-cluster/using-ada/jobs

conda cheat sheet:
https://docs.conda.io/projects/conda/en/4.6.0/_downloads/52a95608c49671267e40c689e0bc00ca/conda-cheatsheet.pdf
