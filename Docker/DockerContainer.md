## Docker Container 
A container is just an isolated process, provided resources hid with the help of name spaces. This process reside in the virtual file system with packages & managed dependencies, from where the process' program files are present. Image is a templated for this process which is just a set of multiple different file systems merged into one in a layered architecture.  

The container is a writeable layer upon the image layers (readable).  Since the process resides in VFS, the processes init are isolated inside out. 
## Container File system 
Container process is spawned from the virtual file system, composed of merged file system via storage driver like overlay, These virtual file systems can be found at `/var/lib/docker/STORAGE-DRIVER/LAYER-ID` One thing to be noted that bind, mounts does not belong to either of both.
### Types of VFS include: 
- `/lower` → represents the stacked image layer on which container’s writeable layer is build upon
- `/merged` → contains the merged readable and writeable layer reflecting the current state of the container
- `/diff /upper` → all the changes made in the writeable layer i.e within the container
- `/work` → act as a mediator for performing file operations such as rename
## Container lifecycle 
Since container is a process its has lifecycle lie it which includes 
- `created` - the virtual file system for the container is created but the first process of the container is to start.
- `starting` - the execution period of the container first process execution
- `restarting` - start loop if the first process fails (depends on restart policy
- `running` - the execution of first process and other to follow
- `paused` - the first process execution is on hold yet still in the memory
- `stopped` - the container is not in the memory (the first process has been with held manually)
- `exited` - the container stopped automatically aka completed its life cycle
- `removed` - the container file system no longer resides on the host
##### Related commands 
```bash
docker container [COMMAND/ARGUMENTS] 

create $IMAGE # option to create a container but do not create the process out of it 
start $CONTAINER # option to spawn process out of created container / image
restart $CONTAINER # option to restart the container process but reserve the changes made to writeable layer
stop $CONTAINER # option to stops container, release resources and move container to exited state
kill $CONTAINER # option to kills the root process of docker container forceful shutdown 
remove $CONTAINER # option to remove container from existance    
pause $CONTAINER # option to halt further execution of process within container
unpause $CONTAINER # option to start the manually frozen process within container
```

```bash
docker container [COMMAND/ARGUMENTS] 

diff CONTAINER # option to get all the info of changes made to writeable layer 
port CONTAINER # option to get all the info about container port mapping 
stats CONTAINER # option to  get info about the resources in use 
top CONTAINER # option to get the stats of the process running in the container 
inspect CONTAINER # option to  get the all configuration and details about container 
logs CONTAINER # option to get the logs of the container 
ls # flag to get list of all the containers -a flag to get stopped and exited too  
```

```bash
docker container [COMMAND/ARGUMENTS] 

export --output="FILE.tar" CONTAINER # option to export container filesystem into a tarball
commit CONTAINER-NAME IMAGE-NAME # option to cast an image out of current state of container
	--pause # flag to pause container processes during commit to minimize faults 
	--author AUTHOR-NAME # option to add author name 
	--message MESSAGE # option to set commit messege 
	--change "DOCKER FILE INSTRUCTION" # option to apply docker file instruction on the commit 
```

```bash
docker container [COMMAND/ARGUMENTS] 

rename CONTAINER NEWNAME # option to rename a container
attach CONTAINER # option to addtach attach container stdio with the current tty session
exec [options] CONTAINER COMMAND # option to execute a process within a container 
```
