## Volumes
Volumes are stored locally on the host and are persistent. During runtime, the container's data is redirected to the volume, which is stored locally on the host and persists beyond the container's lifecycle. The data that is generated and redirected is not actually part of the container's filesystem, making it immune to the container's lifecycle. With volumes, even if the container is destroyed or dies, the data remains intact on the host system.
### Types
#### Named Volumes
Named volumes are user-defined storage units in Docker that persist data independently of container lifecycles. They are explicitly created and referenced by name, stored under Docker’s volume subsystem (`/var/lib/docker/volumes` on Linux). These volumes can be shared across multiple containers, managed using Docker CLI/API, and can leverage volume drivers for integration with external storage backends.
#### Anonymous Volumes
Anonymous volumes, on the other hand, are automatically created by Docker at runtime when a mount point is specified without a named source. These volumes have system-generated identifiers and are typically used for temporary storage needs. While they persist on disk, they are usually removed when the container is deleted, unless explicitly preserved.
### Mounting
When an empty volume is mounted on a non-existing file path, everything works flawlessly. However, when the file path on which the volume is mounted contains data, there are two possible scenarios:

First scenario: If an empty volume is mounted on a path that contains data, the existing data is copied to the empty volume, which is then used. This process is called data population.

Second scenario: If a non-empty volume is mounted on a file path that contains data, the container's data is overwritten by the volume's data.

During the mounting process, if the mount point is not defined, Docker throws an error. However, if the mount point is defined but no data source is specified, an anonymous volume is automatically created, and all data from the provided mount path is redirected to that volume.
##### Related commands 

```bash
docker volume [COMMAND/ARGUMENTS] 

create VOLUMENAME # To create a named volume 
	rm VOLUMENAME # To remove a volume 
	prune # To remove unused volumes 
	ls # To list all the volumes 
	inspect VOLUMENAME # To get info about volume 
```