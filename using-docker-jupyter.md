# Running Docker and Jupyter notebooks on the DGX A100s

If your user account has been given docker permissions, you will be able to use docker as you can on any machine. Below are some specific instructions for using Jupyter notebooks in a collaborative setting on the DGXs.

## Jupyter Notebooks on the DGX A100

**Pre-requisite:** This guide assumes you have permission to use docker with your DGX account. If you are unable to access docker, please reach out to the DSI Data Science Team for support. If you do not have access to docker, you will see an error like the following: ```permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post "http://%2Fvar%2Frun%2Fdocker.sock/v1.24/volumes/create": dial unix /var/run/docker.sock: connect: permission denied```

There are 2 steps involved in creating a fully functional jupyter session. First, you must create a docker volume to save your work to. You then need to use the appropriate port forwarding in order to connect to the jupyter session successfully.

**USER 1** 
 
* Log in: `ssh -L xxxx:localhost:xxxx username@XX.XX.XX.XX` "xxxx" is your port number. XX.XX.XX.XX is the IP Address of the machine you have been assigned to. This information can be found on the [DGX Assignments](https://docs.google.com/spreadsheets/d/1BQjkEN3PJDlljziHQeCBBTwa9unaWivCDnwpmLqmxUQ/edit?usp=sharing) spreadsheet
* Change directory to your directory in /raid: `cd /raid/username`
* Create a folder where you would like to store your work within your raid home directory: `mkdir <testfolder>`. ```testfolder``` is a placeholder. This "testfolder" must be globally unique if being used to create a docker volume. Use `docker volume ls` to see what volume names have already been created. 

**NOTE**: Docker volume names are system-wide unique. Use `docker volume ls` to check if the name you want to use is already taken

* Create the docker volume: `docker volume create --driver local --opt type=none --opt device=/raid/username/<testfolder> --opt o=bind <testfolder>`

* Now use `docker volume ls` to check if the volume was successfully created. **Deleting a docker volume is not easy - ensure you are ok with the name of the volume you created**

* If you plan on using a repository, clone it within the new folder you created now. 

* Launch Docker: `docker run --gpus all --net=host -it -v <testfolder>:/workspace/<testfolder> nvcr.io/nvidia/pytorch:22.12-py3` (or any other container image that supports Jupyter notebooks)
 
* Launch Jupyter Lab/Notebook: `jupyter-lab --port xxxx`
 
* Now navigate to the URL provided. Change "hostname" to "localhost" and port "8888" to port "xxxx" in the URL. 

* Create any new files in the folder called 'testfolder' within your jupyter session. Any files created in subdirectories above this will not save anywhere. 
 
**For other users wanting to use the same jupyter session (USER 2):**
 
* Log in: `ssh -L yyyy:localhost:8888 username@XX.XX.XX.XX` (note the use of the different localhost – this will allow you to work on your own localhost and not run into the issue of someone else using the same one)
 
* Connect to the running docker container: use `docker ps` to see what containers are running. Identify the one you need (you will need to communicate with your team to determine the correct container. 
 
* Use the command `docker exec -it <container name> /bin/bash` to get a bash shell in the container.
 
* Launch Jupyter Lab/Notebook: `jupyter-lab --port yyyy`
 
**Provide the SAME TOKEN AS USER 1**
 
You should now be able to use the same jupyter session and docker container. 

## Modifying and saving a PyTorch docker container to include new libraries
1. Launch the PyTorch docker container using the steps above.

2. Once in the docker container do a `pip install` of the missing libraries.

3. Make a second SSH connection into the DGX.

4. From the second connection, check running docker images by running `docker ps`. Note the CONTAINER_ID of the PyTorch container running in the other SSH connection.

5. Run the following docker command to create a new image:<br>
Generic Example:<br>
docker commit CONTAINER_ID NEW_IMAGE_NAME<br><br>
Real-world Example:<br>
docker commit d4576eda8284 na_docker

6. To run the newly created container run:<br>
Generic Example:<br>
docker run --gpus all --net=host -it -v testfolder:/workspace/testfolder na_docker<br><br>
Real-world Example:<br>
docker run --gpus all --net=host -it -v n_arc:/workspace/n_arc na_docker

Please reach out to Umang Chaudhry at umang.chaudhry@vanderbilt.edu with any questions.
