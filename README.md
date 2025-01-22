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

More information about the ways you can access these machines follow in the [Accessing the DGXs](#Accessing-the-DGXs) section.

## Setup

### Request an account be created for you on the DSI DGX's. 

Before continuing with your setup, you'll need to request to have a DGX account set up for you by completing a [DGX User Account Registration Form (Compute Grant)](https://docs.google.com/forms/d/e/1FAIpQLScWr3SPiwxeQFQxuesn8R2fDF7k0jOTzFXPNCly-AsEHPh5fw/viewform?usp=sf_link)

### Accessing the DGXs

Once access is provided, you will be able to access the DGXs in 4 main ways:
   - Jupyter Notebooks
   - ACCRE GPU Desktop
   - salloc
   - SLURM
  
#### Jupyter Notebooks 

**Note: ACCRE is still working to get this set up. As of 1/22/2025, this solution is not yet available.** 

Jupyter notebooks is the most straightforward way to access the GPUs. However, this approach does limit what you can do with the GPUs to only what you can acheive from within a Jupyter Notebook. If you are running custom containers and applications, please see the [salloc](#salloc) method

1. Proceed to the *new* ACCRE Visualization Portal at http://viz.accre.vu
2. Select Interactive Apps
3. Select Jupyter Notebook (GPU)
4. Provide your GPU Enabled Slurm Account - p_dsi_dgx (*Access to this group is provided via the [Compute Grant](https://docs.google.com/forms/d/e/1FAIpQLScWr3SPiwxeQFQxuesn8R2fDF7k0jOTzFXPNCly-AsEHPh5fw/viewform?usp=sf_link) or to those part of DSI projects*)
5. Provide the number of hours and GPU resources (number of GPUs) you require
6. Select a GPU Architecture (DGX-A100-40GB or DGX-A100-80GB)
7. If using a custom virtual environment or container, provide that information under Python or Conda Virtual Environment and Custom Module Collection
8. Launch the notebook - your job will be initially queued. Depending on the amount of resources you requested, your session will be launched based on availability. Generally, the more resources you ask for, the longer the wait time.

#### ACCRE GPU Desktop

1. Proceed to the *new* ACCRE Visualization Portal at http://viz.accre.vu
2. Select Interactive Apps
3. Select ACCRE GPU Desktop
4. Provide the number of hours for which you need access (Note: Longer times will likely result in longer wait times)
5. Provide your GPU Enabled Slurm Account - p_dsi_dgx (*Access to this group is provided via the [Compute Grant](https://docs.google.com/forms/d/e/1FAIpQLScWr3SPiwxeQFQxuesn8R2fDF7k0jOTzFXPNCly-AsEHPh5fw/viewform?usp=sf_link) or to those part of DSI projects*)
6. Provde the number of GPU resources you require
7. Select a GPU Architecture (DGX-A100-40GB or DGX-A100-80GB)
8. You may provide a screen resolution if required
9. Launch the GPU Desktop session - your job will be initially queued. Depending on the amount of resources you requested, your session will be launched based on availability. Generally, the more resources you ask for, the longer the wait time.

