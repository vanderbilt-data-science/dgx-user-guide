# Vanderbilt Data Science Institute - DGX A100 User Guide

A guide to all things DGX for authorized users.

The Data Science Institute has four DGX A100's. These systems have now been incorporated into ACCRE's cluster. Access is still granted to those who are part of DSI projects, or those who have been awarded a [DSI Compute Grant for DGX](https://docs.google.com/forms/d/e/1FAIpQLScWr3SPiwxeQFQxuesn8R2fDF7k0jOTzFXPNCly-AsEHPh5fw/viewform?usp=sf_link). 

## Infrastructure

The Data Science Institute has 4 DGX A100s housed under ACCRE. These are available in 2 configurations: 

1. 8 x 40GB A100 GPUs (2 Machines, up to 320GB total GPU RAM)
2. 8 x 80GB A100 GPUs (2 Machines, up to 640GB total GPU RAM)

These machines are set up to work together via InfiniBand for multi-GPU and multi-node High-Performance Computing (HPC). 

Access to the GPUs is provisioned on a first-come first-served basis once you have been granted access. The DGXs are a shared resource amongst DSI Graduate Students, Faculty, Staff and affiliated Faculty members and lab groups. All jobs and access is managed through SLURM, a dynamic job scheduler. During times of high usage, wait times may be longer. Additionally, asking for a large amount of resources for a longer timeframe may also result in longer wait times as SLURM works to free up compute for your jobs. For high compute jobs like training of models, we recommend using a [SLURM](#SLURM) batch job. Once enough compute is available, you job will commence and save your results to a specified location. 

All GPUs are set up such that any work you do is saved to your ACCRE home directory. Upon logging in via any method, you will land in your ACCRE home directory. 

You may only use custom **Singularity** containers for your work if using custom containers. Docker is not installed on the DGXs due to security issues. 

More information about the ways you can access these machines follow in the [Accessing the DGXs](#Accessing-the-DGXs) section.

## Setup

### Request an account be created for you on the DSI DGX's. 

You'll need to first request to have a DGX account set up for you by completing a [DGX User Account Registration Form (Compute Grant)](https://docs.google.com/forms/d/e/1FAIpQLScWr3SPiwxeQFQxuesn8R2fDF7k0jOTzFXPNCly-AsEHPh5fw/viewform?usp=sf_link)

## Accessing the DGXs

Once access is provided, you will be able to access the DGXs in 4 main ways:
   - Jupyter Notebooks
   - ACCRE GPU Desktop
   - salloc
   - SLURM
  
### Jupyter Notebooks 

**Note: ACCRE is still working to get this set up. As of 1/22/2025, this solution is not yet available.** 

Jupyter notebooks is the most straightforward way to access the GPUs. However, this approach does limit what you can do with the GPUs to only what you can acheive from within a Jupyter Notebook. If you are running custom containers and applications, please see the [salloc](#salloc) method

1. Proceed to the *new* ACCRE Visualization Portal at http://viz.accre.vu
2. Select Interactive Apps
3. Select Jupyter Notebook (GPU)
4. Provide your GPU Enabled Slurm Account - p_dsi_dgx (*Access to this group is provided via the [Compute Grant](https://docs.google.com/forms/d/e/1FAIpQLScWr3SPiwxeQFQxuesn8R2fDF7k0jOTzFXPNCly-AsEHPh5fw/viewform?usp=sf_link) or to those part of DSI projects*)
5. Provide the number of hours and GPU resources (number of GPUs) you require
6. Select a GPU Architecture (DGX-A100-40GB or DGX-A100-80GB)
7. If using a custom virtual environment or container, provide that information under Python or Conda Virtual Environment and Custom Module Collection
8. Launch the notebook - your job will be initially queued. Depending on the amount of resources you requested, your session will be launched based on availability. Generally, the more resources you ask for, the longer the wait time. Once your session has started, you'll be able to connect to Jupyter. 

<img width="924" alt="image" src="https://github.com/user-attachments/assets/73aa9fb5-0d37-472f-96b0-e32f04eedcb9" />



### ACCRE GPU Desktop

ACCRE GPU Desktop provides you with a virtual desktop and an interactive way to work with the GPUs. 

1. Proceed to the *new* ACCRE Visualization Portal at http://viz.accre.vu
2. Select Interactive Apps
3. Select ACCRE GPU Desktop
4. Provide the number of hours for which you need access (Note: Longer times will likely result in longer wait times)
5. Provide your GPU Enabled Slurm Account - p_dsi_dgx (*Access to this group is provided via the [Compute Grant](https://docs.google.com/forms/d/e/1FAIpQLScWr3SPiwxeQFQxuesn8R2fDF7k0jOTzFXPNCly-AsEHPh5fw/viewform?usp=sf_link) or to those part of DSI projects*)
6. Provde the number of GPU resources you require
7. Select a GPU Architecture (DGX-A100-40GB or DGX-A100-80GB)
8. You may provide a screen resolution if required
9. Launch the GPU Desktop session - your job will be initially queued. Depending on the amount of resources you requested, your session will be launched based on availability. Generally, the more resources you ask for, the longer the wait time.

<img width="1728" alt="image" src="https://github.com/user-attachments/assets/3aac3a59-99c0-45d6-9970-b96fe89ee7c1" />


### salloc

salloc is essentially the same login approach as the ACCRE GPU Desktop, but through a shell terminal instead of an interactive desktop. This is the best approach when it comes to running custom applications or dashboards on the DGXs. We recommend that you be very comfortable working in a linux shell environment before using this approach. 

1. Open a new terminal/shell
2. Type in the following ```ssh <VUnetID>@login.accre.vu
3. Provide your VUnetID password
4. You now have shell access to the ACCRE file system. ```ls``` will show you that you're currently in your ACCRE home directory. This is currently a CPU-only gateway.
5. Use the ```salloc``` command to request a direct shell into the DGX: ```salloc --time=1:00:00 --partition=interactive --account=p_dsi_dgx --gres=gpu:nvidia_a100-sxm4-40gb:1```
  - Here, we ask for a shell into the DGX with 1 40GB A100 GPU for 1 hour
  - Note the partition and account. These must be provided every time
  - To request the 80GB A100 GPUs, use ```--gres=gpu:nvidia_a100-sxm4-80gb:1```
  - You can run ```nvidia-smi``` to confirm you have the resources you requested.
6. You can now launch your custom singularity containers for your workflows. 

<img width="1288" alt="image" src="https://github.com/user-attachments/assets/f490b305-81c9-47c4-acd7-9d752161ddf7" />



### SLURM

SLURM is the ideal approach when working on high compute jobs like training of LLMs. You can submit batch jobs. Once enough compute is available, you job will commence and save your results to a specified location. 

1. Open a new terminal/shell
2. Type in the following ```ssh <VUnetID>@login.accre.vu
3. Provide your VUnetID password
4. You now have shell access to the ACCRE file system. ```ls``` will show you that you're currently in your ACCRE home directory. This is currently a CPU-only gateway.
5. Have your python script ready in your ACCRE home directory. You may upload this using the interactive portal at http://viz.accre.vu
6. Create your SLURM script. This instructs the scheduler how to run the Python script and what to do with the results. Below is a sample SLURM script (filename.slurm):

```
#!/bin/bash
#SBATCH --job-name=python_job        # Job name
#SBATCH --output=python_job_slurm.txt # Output file
#SBATCH --error=python_job_slurm.txt  # Error file (merged with output)
#SBATCH --partition=p_dsi_dgx         # Partition to submit to
#SBATCH --gres=gpu:nvidia_a100-sxm4-80gb:1 # Request 1 NVIDIA A100 GPU with 80GB memory
#SBATCH --time=01:00:00              # Time limit (1 hour)

# Load necessary modules (if any)
module load python

# Print some information about the GPU used
nvidia-smi

# Run your Python script
python your_script.py
```
7. Submit your batch job using ```sbatch filename.slurm```. You will now see the following: ```Submitted batch job (job id)```. You may also use ```squeue --job (job id)``` to check the status of your job. 

SLURM allows you to specify many parameters related to your job. For more specifics, please see ACCRE's [Wiki](https://help.accre.vanderbilt.edu/index.php?title=Overview). We also highly recommend you complete the [ACCRE SLURM Training](https://www.vanderbilt.edu/accre/required-training/). Their training walks new users through the ACCRE system, UNIX as well as SLURM. 

Please reach out to [Umang Chaudhry](mailto:umang.chaudhry@vanderbilt.edu) with any questions either over email or the Vanderbilt Data Science Slack. 
