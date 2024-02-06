# Vanderbilt Data Science Institute - DGX A100 User Guide

A guide to all things DGX for authorized users.

The Data Science Institute has two DGX A100's. These systems are not part of the ACCRE share, and user access to them is granted to those who are part of DSI projects, or those who have been awarded a [DSI Compute Grant for DGX](https://docs.google.com/forms/d/e/1FAIpQLScWr3SPiwxeQFQxuesn8R2fDF7k0jOTzFXPNCly-AsEHPh5fw/viewform?usp=sf_link). 


### Steps

1. Request an account be created for you on the DSI DGX's. 

Before continuing with your setup, you'll need to request to have a DGX account set up for you by completing a [DGX User Account Registration Form (Compute Grant)](https://docs.google.com/forms/d/e/1FAIpQLScWr3SPiwxeQFQxuesn8R2fDF7k0jOTzFXPNCly-AsEHPh5fw/viewform?usp=sf_link)

2. Once access is provided, get your assigned Port number and GPU information from the [DGX Assignments] (https://docs.google.com/spreadsheets/d/1BQjkEN3PJDlljziHQeCBBTwa9unaWivCDnwpmLqmxUQ/edit#gid=0) speadsheet

4. Set up the Vanderbilt VPN on your machine (if not connected to VUnet)

5. Set up Duo on your mobile device

6. Log in to the VPN

7. Log in to the DGX (instructions available on the using-docker-jupyter.md file)

8. Set up and run a Docker container on the DGX

9. Start your Jupyter Notebook. 

**Unlike other systems, try not to close your Jupyter notebook--leave it running. ** You can close the browser window and connect later. The docker container, and your Jupyter kernal, continue on the DGX in a quiescent state. 

## Contents



* [Running Jupyter Notebooks on the DGX](using-docker-jupyter.md)

## Additional Resources 

* [Generating an SSH Key](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

* [Vanderbilt IT Help](https://it.vanderbilt.edu/)

* [Setting up DUO on your phone](https://it.vanderbilt.edu/files/Enrollment_and_Login.pdf)
