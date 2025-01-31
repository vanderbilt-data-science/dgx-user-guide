# Vanderbilt Data Science Institute - DGX A100 User Guide

A comprehensive guide to using the DGX A100 systems for authorized users.

The Data Science Institute (DSI) has four DGX A100 systems, now integrated into ACCRE's cluster. Access is provided to participants of DSI projects or those awarded a [DSI Compute Grant for DGX](https://docs.google.com/forms/d/e/1FAIpQLScWr3SPiwxeQFQxuesn8R2fDF7k0jOTzFXPNCly-AsEHPh5fw/viewform?usp=sf_link).

## Infrastructure

The DSI maintains 4 DGX A100 systems, available in two configurations:

1. **8 x 40GB A100 GPUs** (2 Machines, up to 320GB total GPU RAM each).
2. **8 x 80GB A100 GPUs** (2 Machines, up to 640GB total GPU RAM each).

These machines are interconnected via InfiniBand for multi-GPU and multi-node High-Performance Computing (HPC). Access to the GPUs is allocated on a first-come, first-served basis.

### Resource Allocation

- The DGX systems are shared among DSI Graduate Students, Faculty, Staff, and affiliated lab groups.
- Job and resource management is handled via SLURM, a dynamic job scheduler.
- High-demand periods or large resource requests may increase wait times.

### Data Management

- All work is saved to your ACCRE home directory.
- Upon logging in, you will start in your ACCRE home directory.

### Containers

- Custom **Singularity** containers are supported.
- **Docker** is not available due to security concerns.

For details on access methods, see [Accessing the DGXs](#accessing-the-dgxs).

## Setup

### Requesting an Account

To use the DGX systems, you must request an account by completing the [DGX User Account Registration Form (Compute Grant)](https://docs.google.com/forms/d/e/1FAIpQLScWr3SPiwxeQFQxuesn8R2fDF7k0jOTzFXPNCly-AsEHPh5fw/viewform?usp=sf_link). If you have recieved an email stating you've been provisioned access, you do not need to complete this form.

## Accessing the DGXs

There are four primary methods to access the DGX systems:

- Jupyter Notebooks
- ACCRE GPU Desktop
- `salloc`
- SLURM batch jobs

### Jupyter Notebooks

Jupyter Notebooks offer a straightforward way to access GPUs, though this method is limited to notebook-based workflows. For custom applications or containers, consider using the `salloc` method.

1. Visit the ACCRE Visualization Portal: [http://viz.accre.vu](http://viz.accre.vu). Log in with your VUnetID and password.
2. Select **Interactive Apps**.
3. Choose **Jupyter Notebook (GPU)**.
4. Provide your GPU-enabled SLURM account (`p_dsi_dgx`). Access is granted through the [Compute Grant](https://docs.google.com/forms/d/e/1FAIpQLScWr3SPiwxeQFQxuesn8R2fDF7k0jOTzFXPNCly-AsEHPh5fw/viewform?usp=sf_link) or via DSI projects.
5. Specify required resources (hours, GPUs, and architecture: DGX-A100-40GB or DGX-A100-80GB).
6. If using a custom virtual environment or container, provide the necessary information.
7. Launch the notebook. Your session will queue and begin based on resource availability.

<img width="924" alt="image" src="https://github.com/user-attachments/assets/73aa9fb5-0d37-472f-96b0-e32f04eedcb9" />

### ACCRE GPU Desktop

ACCRE GPU Desktop offers a virtual desktop environment for interactive GPU workflows.

1. Visit the ACCRE Visualization Portal: [http://viz.accre.vu](http://viz.accre.vu). Log in with your VUnetID and password.
2. Select **Interactive Apps**.
3. Choose **ACCRE GPU Desktop**.
4. Provide your GPU-enabled SLURM account (`p_dsi_dgx`). Access is granted through the [Compute Grant](https://docs.google.com/forms/d/e/1FAIpQLScWr3SPiwxeQFQxuesn8R2fDF7k0jOTzFXPNCly-AsEHPh5fw/viewform?usp=sf_link) or via DSI projects.
5. Specify the session duration, GPU resources, and architecture (DGX-A100-40GB or DGX-A100-80GB).
6. Optionally, provide a custom screen resolution.
7. Launch the session. Your session will queue and start based on availability.

<img width="1728" alt="image" src="https://github.com/user-attachments/assets/3aac3a59-99c0-45d6-9970-b96fe89ee7c1" />

### `salloc`

The `salloc` method provides direct shell access to the DGX systems and is ideal for running custom applications or workflows.

1. Open a terminal and run:
   ```bash
   ssh <VUnetID>@login.accre.vu
   ```
2. Enter your VUnetID password.
3. Navigate your ACCRE home directory using `ls`.
4. Request a direct shell into the DGX system with the following command:
   ```bash
   salloc --time=1:00:00 --partition=interactive --account=p_dsi_dgx --gres=gpu:nvidia_a100-sxm4-40gb:1
   ```
   - For 80GB GPUs, use:
     ```bash
     --gres=gpu:nvidia_a100-sxm4-80gb:1
     ```
   - Adjust the time and GPU count as needed.
5. Use `nvidia-smi` to verify your resources.
6. Launch your workflows using Singularity containers.

### Running Jupyter notebooks via ```salloc```

Running Jupyter notebooks from within a container requires a few extra steps due to the need for port forwarding: 

1. Open a terminal and run:
   ```bash
   ssh <VUnetID>@login.accre.vu
   ```
2. Enter your VUnetID password.
3. Navigate your ACCRE home directory using `ls`.
4. Request a direct shell into the DGX system with the following command:
   ```bash
   salloc --time=1:00:00 --partition=interactive --account=p_dsi_dgx --gres=gpu:nvidia_a100-sxm4-40gb:1
   ```
5. **Make note of the machine you landed on (dgx01, dgx02, dgx03, or dgx04)**
6. Navigate to the location of your singularity container
7. Run the following command: ```singularity exec --bind /home/VUnetID:/home/VUnetID pytorch_25.01-py3.sif jupyter-lab --notebook-dir=/home/VUnetID --ip=0.0.0.0 --no-browser```. This starts a Jupyter Lab session with the workspace bound to your home directory. You can modify this to work off of any directory of your choice. Ensure you have Read, Write and Execute access to this directory.
8. Open a NEW terminal window. Keep your previous terminal open and running.
9. Run the following: ```ssh VUnetID@login.accre.vu -L 8888:<dgx03>:8888``` (Replace dgx03 with your machine from step 5)
10. Now, copy the link provided to you by the Jupyter session running on the first terminal window.
11. Open a browser and paste the link. CHANGE the "hostname" in the link to "localhost". See example below:
   Original Link: ```http://hostname:8888/lab?token=8f89a890e5b48ad3a4e08058f7843f0d76e777cbd158071e```
   New Link: ```http://localhost:8888/lab?token=8f89a890e5b48ad3a4e08058f7843f0d76e777cbd158071e```

<img width="1288" alt="image" src="https://github.com/user-attachments/assets/f490b305-81c9-47c4-acd7-9d752161ddf7" />

### SLURM

SLURM is recommended for high-compute jobs such as model training. Use batch jobs to manage workloads efficiently.

1. Open a terminal and run:
   ```bash
   ssh <VUnetID>@login.accre.vu
   ```
2. Enter your VUnetID password.
3. Prepare a Python script and upload it to your ACCRE home directory.
4. Create a SLURM script (e.g., `filename.slurm`) with the following example:

   ```bash
   #!/bin/bash
   #SBATCH --job-name=python_job        # Job name
   #SBATCH --output=python_job_slurm.txt # Output file
   #SBATCH --error=python_job_slurm.txt  # Error file (merged with output)
   #SBATCH --partition=p_dsi_dgx         # Partition
   #SBATCH --gres=gpu:nvidia_a100-sxm4-80gb:1 # Request 1 GPU
   #SBATCH --time=01:00:00              # Time limit (1 hour)

   # Load necessary modules
   module load python

   # Print GPU info
   nvidia-smi

   # Run your Python script
   python your_script.py
   ```

5. Submit your batch job:
   ```bash
   sbatch filename.slurm
   ```
6. Monitor your job status:
   ```bash
   squeue --job <job id>
   ```

For additional details, refer to the [ACCRE Wiki](https://help.accre.vanderbilt.edu/index.php?title=Overview) and [ACCRE SLURM Training](https://www.vanderbilt.edu/accre/required-training/).

## Support

For questions, contact [Umang Chaudhry](mailto:umang.chaudhry@vanderbilt.edu) via email or the Vanderbilt Data Science Slack.
