## Docker Image 
An image is bundle of packages and binaries with dependency managed. An image is composed of layer/s which is nothing but a virtual filesystem with program files with dependencies managed. A layer is unique to others in terms of data, multiple layer are combine together | single, init top is the representative of merged filesystem (done with help of storage driver). This VFS is readable combined with writeable and configurations a container is spawned.

The layers of an image can be found at `/var/lib/docker/image/<STORAGE_DRIVER>/`
## Imperative to declarative with dockerfile
Once container is spawned multiple instructions can be performed on it (ofc on the writeable layer) like copy files, invoking process setting up env etc to get desired work-state or with the runtime arguments, however once the container is exited → removed the changes made are gone, making it imperative. However if that could be the part of image it will be reproducible. Forit dockerfile is used as a declarative way to build images init each instruction adds a new layer (reusable). The writeable layer needs minimum to no further configuration. The instructions configuration fields are just as arguments.
### Docker file instructions
#### Syntax and parser directives 
The docker file have a syntax like INSTRUCTION ARGUMENTS. The lines/stuff after the first instruction starting with # is counted as a comment. Lines before the first instruction starting with # is called a parser directive which sets the formatting of the docker file. Types of parser directive include : (conventionally lowercase ) (# directive=value1)
- syntax - refer to docker file formatting (must be untouched)
- escape - sets the escape sequence default = \
- check - build check (TODDO)
#### Metadata 
```dockerfile
# Add metadata in the image 
LABEL key1="value1" key2="value2" 
```
#### Storage 
```dockerfile
# To add to container file system with copy from host 
COPY [ARGS] SOURCE DESTINATION 
  --chown=USER|UID|NONE:GROUP|GID|NONE(REMOVING ':') # TO set ownership default is 0
  --chmod=PERMISSIONS-IN-OCTAL # To set permission 
  --from=$ STAGE | IMAGE | CONTEXT # copy files from other local sources 

# Last argument will be considered as the destination else source 
# if source is directory content will be copied to target 
# supports pattern matching 
# Non existant destination are created on the go 
```

```dockerfile
# Add file to container file system
ADD [ARGS] SOURCE DESTINATION 
  --keep-git-dir=true  # for keeping git directory too 
  --checksum=$CHECKSUM # for verifying checksum of remote source (only https)
  @ supports all the arguments from copy 
	
# SOURCE 
# if source is git repo the content will be moved to target dirpath 
# if source is tarball traball's content will be moved to target dirpath 
# wild cards are accepted 
# if source is url the permission will be 0600
# if the source is remote git url the permission set is 644 | 755 (executeable)
# if url is used and path ends with / the end url subpath is used to name it 
# if url is used and the path ends with a name the destination pathname is used 
# auth is not available
# use --ssh flag when using private git repos 

# DESTINATION 
# Non existant destination are created on the go 
```
#### Storage (existing during build)
```dockerfile
# To send files aprt from build context during the build files 
RUN COMMAND --mount=type=$TYPE,from=$STAGE,source=$PATH,target=$PATH COMMAND
# target,dst,destination - container mount path
# from - to previous named stage build from build context
# source - sourcing path 
# default permissions are writeable use rw
```

```dockerfile
RUN --mount=type=cache,id=pip-cache,target=/root/.cache/pip COMMAND
# target,dst,destination - container mount path
# id - cache identifier (use same id across builds to reuse cache)
# default permissions are writable use rw
RUN --mount=type=cache,id=build-tmp,target=/tmp/build,sharing=private COMMAND
# target - container path (/tmp/build)
# id - unique cache name
# sharing - private (each writer gets its own cache), options: shared, private, locked
RUN --mount=type=cache,target=/app/cache,mode=0700,uid=1000,gid=1000 COMMAND
# target - container path
# mode - file permission bits
# uid, gid - owner user and group
RUN --mount=type=cache,from=builder,source=/root/cache,target=/build/cache COMMAND
# target,dst,destination - container mount path (/build/cache)
# from - build stage or context to mount cache from (e.g., builder stage)
# source - subpath in the from (e.g., /root/cache)
# default permissions are writable use rw

# THE SOURCE IS PASSED DURING BUILD EXAMPLE 
docker buildx build --cache-to=type=local,dest=./cache \\
                    --cache-from=type=local,src=./cache \\
```

```dockerfile
RUN --mount=type=secret,id=mysecret,target=/run/secrets/mysecret COMMAND
# target,dst,destination - container mount path (default: /run/secrets/<id>)
# id - secret identifier (defaults to basename of target if not set)
# env - mount the secret as an environment variable instead of a file (e.g., env=MY_SECRET)
# required - fail the instruction if the secret is unavailable (default: false)
# mode - permission bits for the secret file (default: 0400)
# uid, gid - file owner user and group ID (default: 0 for both)
# NOTE: The secret (file or env var) is ephemeral and not persisted in image layers

#THE SOURCE IS PASSED DURING BUILD EXAMPLE 
docker build \\
  --secret id=mysecret,src=./secrets/token.txt \\
  -t myimage .
```

```dockerfile
RUN --mount=type=ssh,id=$ID,mode=0400,uid=1000,gid=1000,target=$PATH COMMAND 

# HERE SOURCE IS PASSED DURING BUILD EXAMPLE 
docker build --ssh $ID=$PATHTOSSHKEY .
```
#### Networking
```dockerfile
# To run command in seperate network mode 
RUN --network=type=$NETWORK-TYPE COMMAND 
# default - behaves like bridge can refer existing containers by their name
# none - referes offline in absence of network.
# host - referes the docker host's network 
```

```dockerfile
# To set port on which traffic container listens to 
EXPOSE PORT/PROTOCOL 
# (metadata,optional)
```
#### Env
```dockerfile
# To set environment variables
ENV VARIABLE=VALUE  
# overrideable , current -> all the way bottom infulence 
# use command subsitution for variable use just like in basic-shell 
```

```dockerfile
# To set working directory
WORKDIR DIRPATH
# overrideable , current -> all the way bottom infulence 
# If a relative path is provided, it will be relative to the path of the previous WORKDIR
```

```dockerfile
# to set the user for the process
user username:groupname 
# overrideable , current -> all the way bottom infulence 
# (if group is absent the other instruction are invoked via root)
```
#### Processes
The process invoking works in two types shell and exec, in shell the shell is invokes the command which also add ability for shell options, other is exec which loads binary directly into the memory and is handled via system calls.
- Shell format : INSTRUCTION sleep 5 && echo "
- Exec format : INSTRUCTION ["sleep", "5", "&&", "echo", "Done"] # Fail due to shell logic of && variable expansion will not work in EXEC until the shell is invoked init

```dockerfile
# To set the interpreter for command defined in instruction 
SHELL ["$SHELL", "ARGUMNETS"] 

# command below it will be executed by the shell defined above 
# can be overwriteable 
```

```dockerfile
# to set default command after the conatiner is started aka first process
ENTRYPOINT [ "C1" , "C2" ] 

# Argumnets passed will be considered as args for entrypoint command
```

```dockerfile
# To set imperative command for container first process 
CMD [ "C1" , "C2" ] 

# if args are passed they are considered as args for entrypoint if present
# if entrypoint is absent and args are passed they are considered as whole command
```

```dockerfile
# To set default signal for kiling container porcess 
STOPSIGNAL $SIGNAL 

# Default is SIGTERM 
```
#### Build configurations & Stages
```dockerfile
 # Add base layer/s for build
 FROM REGISTRY/NAMESPACE/IMAGE:LABEL
```

```dockerfile
# To add/set variables during the build times 
ARG ARGNAME=VALUE 

# Use
FROM $ARGNAME

# ARG won't work in the CMD and ENTRYPOINT unless and until they are copied to ENV 
# ARG is only available during the build time any instruction that is used at the run time will not have access to ARG value 
```
## Building Image 
### Build Context
A build context can be defined as the environment in which the build happens, during the time of build the files are bundled, archive & passed to docker daemon from the defined PATH thy which context starts. For example if the context is /abc/bcd the the starting path of the content is bcd files before it cannot be reached.
### Build Stages
Every Build is composed of stages, where each stage is limited to next FROM, these stages can be named using AS NAME. During build time all stages present in the docker file are build. By default the last is counted to be the image. During the build times one stage can have files from another previous named stage.
### Building 
Docker have a legacy builder which simply builds the image with simple options however multi arch build were built with little to no support to solve this, buildx was developed and build command was promoted to a hybrid of legacy builder and buildx. 
##### Buildx file configuration entry in daemon.json
```bash
# enabling buildx from daemon configuration 
# /etc/docker/daemon.json

{
  "features": {
    "buildkit": true
  }
}

# enable using env variable 
# DOCKER_BUILDKIT=1 
# Default buildx instance is tied to current instance 
# Global instance is independent of any context 
```
##### Related Commands
```bash
docker build | buildx CONTEXT-WITH-DOCKERFILE

  --add-host HOSTNAME:IP # Option to add entries to /etc/hosts
  --build-arg ARG=VALUE # Option to build-time variables
  --file DOCKER-FILE-PATH # Option to custom Dockerfile path
  --iidfile FILENAME # Option to write image ID to file
  --label "L1=V1" # Option to add labels
  --no-cache # Flag to avoid cache during build
  --platform ARCH # Option to platform (legacy: limited, Buildx: full support)
  --pull # Flag for always attempting to pull the latest base image
  --quiet # Flag to suppress output, print image ID on success
  --tag IMAGE-IDENTIFIER # Option to set image name with tag
  --target STAGE # Option to set custom last stage for build
```

```bash
docker build CONTEXT-WITH-DOCKERFILE

  --compress # Flag to compress the build context using gzip (legacy only)
  --cpu-period DURATION # Option to set CPU CFS (Completely Fair Scheduler) period for the container
  --cpu-quota MICROSECONDS # Option to limit the CPU CFS quota
  --cpuset-cpus LIST # Option to cPUs in which to allow execution (e.g., 0-3, 0,1)
  --cpuset-mems LIST # Option to limit memory nodes in which to allow execution (e.g., 0-3, 0,1)
  --memory BYTES # Option to limit memory for the container (e.g., 512m, 2g)
  --memory-swap BYTES # Option to set total memory (memory + swap), set to -1 to disable swap
  --shm-size SIZE # Option to limit size of /dev/shm (e.g., 64m)
  --ulimit name=SOFT:HARD # Set ulimit options for build container
  --force-rm # Flag to remove intermediate containers, even on build failure
  --rm # Flag to remove intermediate containers after a successful build
```

```bash
docker buildx [COMMAND/ARGUMNETS]

  create CONTEXT | ENDPOINT  # Option to create a buidx instance which can be used cocurrently 
    --name NAME # Option to name the instance 
	--bootstrap # Flag to Boot instance after creation 
	--driver docker-container | kubernetes | remote # Option to add driver to buidx instance 
	--use # Flag to Explicit use decalration is required to use the buildx 
	--platform ARCH # Option to declare fixed platforms 
  prune BUILDX-INSTANCE # Option to removed un used buildx instances  
  rm BUILDX-INSTANCE # Option to delete buildx instance 
  stop BUILDX-INSTANCE # Option to stop the buildx instance
  ls # Flag to list all the buildx instances 
  du # Flag to find the disk usage by buildx 
  use BUILDX-INSTANCE # Option to Use specific buildx instance   
	--global # Flag to setup global buildx instance available across all context 
	--default # Flag to setup buildx context as default which used under the hood by build
```

```bash
docker buildx CONTEXT-WITH-DOCKERFILE

  --allow=network.host | security.insecure # Option to allow specific entitlements for the build (requires BuildKit)
  --annotation "KEY=VALUE" # Option to add image annotations (OCI-compatible)
  --build-context NAME=SOURCE # Option to define additional build contexts with optional source
  --metadata-file FILE # Option to Write build result metadata to the specified file
  --output TYPE[,KEY=VALUE,...] # Option to control the build output format (e.g., type=image, type=local)
  --progress=auto|plain|tty # Option to set type of progress output
  --provenance=mode # Option to control provenance generation (e.g., false, mode=max, min)
  --push # Flag to push the image to registry after build
  --sbom=mode # Option to generate Software Bill of Materials (SBOM)
  --secret id=mysecret,src=secret.txt # Option to mount secrets into the build (BuildKit-specific)
  --ssh default=/path/to/socket # Option to add SSH agent forwarding support in build
  --builder BUILDER # Option to specify which builder instance to use (e.g., docker-container)
  --load # Flag to load the image at loaclhost for testing purposes 
```
###  Tagging and pushing the build 
The build are stored to local system with the name provided at the time of build 
```bash
# Tagging the image 

docker tag IMAGE-ID REPOSITORY/NAMESPAECE/IMAGE-NAME:TAG
```

```bash
# Pushing the image 

docker push IMAGE
  --all-tags # Flag to push all tags 
  --quiet # Flag for supressing output 
```
## Storing Image with Registry
The images build remain onto the local, these images can be hosted via registry for sharing. 
### Structure of registry 
```text
repositories/nginx/
├── _layers/
│   └── sha256/
│       ├── layer1_digest/
│       │   └── link → ../../../../blobs/sha256/xx/layer1_digest/data
│       ├── layer2_digest/
│       │   └── link → ../../../../blobs/sha256/xx/layer2_digest/data
│       └── config_digest/
│           └── link → ../../../../blobs/sha256/xx/config_digest/data
│
├── _manifests/
│   ├── revisions/sha256/
│   │   ├── manifest1_digest/
│   │   │   └── link → ../../../../blobs/sha256/xx/manifest1_digest/data
│   │   └── manifest2_digest/
│   │       └── link → ../../../../blobs/sha256/xx/manifest2_digest/data
│   │
│   └── tags/
│       ├── latest/
│       │   ├── current/link → ../../revisions/sha256/manifest2_digest/link
│       │   └── index/sha256/
│       │       ├── manifest1_digest/link
│       │       └── manifest2_digest/link
│       │
│       └── v1.21/
│           ├── current/link → ../../revisions/sha256/manifest1_digest/link
│           └── index/sha256/
│               └── manifest1_digest/link
│
└── _uploads/
    └── upload_session_uuid/
        ├── data
        ├── hashstates/sha256/0
        └── startedat
```
#### _layers/ Directory
The `_layers/` directory maintains repository-specific references to all content blobs (filesystem layers and image configs) through subdirectories named by SHA256 digests, each containing a `link` file with the full digest pointing to global blob storage. This enables efficient garbage collection by quickly identifying which blobs are actively referenced by the repository, provides scoped enumeration of repository content without parsing manifests, and supports repository-level operations by creating a fast lookup mechanism for all associated blobs.
#### _manifests/revisions/ Directory
The `_manifests/revisions/` directory provides immutable storage for every unique manifest version using content-addressed naming, where each subdirectory named by the manifest's SHA256 digest contains a `link` file pointing to the actual manifest JSON in blob storage. This ensures identical manifests are stored only once across repositories, enables permanent historical access through cryptographic hashes, supports rollback operations, and provides the foundation for content trust verification since manifests never change once created.
#### _manifests/tags/ Directory
The `_manifests/tags/` directory implements mutable tag-to-manifest mapping through two subdirectories per tag: `current/` contains a `link` file pointing to the active manifest digest (updated on each push), while `index/` maintains complete historical references to every manifest the tag has pointed to in digest-named subdirectories. This structure enables immediate tag resolution for pulls, historical tracking for rollbacks and audits, and provides necessary information for tag-aware garbage collection while bridging human-readable names with content-addressed storage.
#### _uploads/ Directory
The `_uploads/` directory manages resumable blob uploads through UUID-named session directories containing: `data` file accumulating chunked upload content, `hashstates/sha256/0` maintaining SHA256 calculation state for resumption, and `startedat` timestamp for cleanup. This enables large layer uploads in manageable chunks with integrity verification, supports resumption from failure points, and provides temporary staging before content moves to permanent blob storage with cleanup of stale sessions.
#### blobs/ Directory
The `blobs/` directory serves as the global content-addressed storage foundation using SHA256 digests as unique identifiers, organized in a two-level hierarchy where the first two digest characters form parent directories and complete digests form subdirectories containing actual content in `data` files. This provides automatic deduplication of identical content across all repositories, cryptographic integrity verification, immutable storage guarantees, efficient hash-to-path mapping for retrieval, and acts as the single source of truth with all other directories containing only references pointing back to this global storage pool.
### Login with registry to push images
Public repository does not need authentication to pull images, however pushing needs auth similar case with private registry. # the credentials can be found at ~/.docker/config.json
##### Related commands
```bash 
echo "$DOCKER_PASSWORD" | docker login myregistry.example.com -u "$DOCKER_USERNAME" --password-stdin
```

```bash
# For aws ecr login with aws cli 

aws ecr get-login-password --region region | docker login --username AWS --password-stdin aws_account_id.dkr.ecr.region.amazonaws.com
```
## Image management 
##### Related Commands 
```bash
docker image 

history $IMAGE # Option to get info about the image layers 
  --human # Flag to print details in human readbale format
inspect $IMAGE # Option to get image info 
save $IMAGE > NAME.tar # Option to Output the content of the container to output stream which can bbe piped
  --output FILE # Option to write output to a file which can be loaded usign docker load 
import $TARBALL $IMAGENAME # To import a tar ball as image , no metadata 
  --change $INSTRUCTION # Option to import with applied docker file changes 
  --message "MESSAGE" # Option to import with a commit message 
load $TARBALL $IMAGENAME # Option to load image from archived CFS 
  --input $FILE # Option to load image from the file 
pull $IMAGE:TAG # Option to get image from registry to locale
  --quiet # Flag to supress output 
  --all-tags # Flag to pull all tags 
docker push IMAGE
  --all-tags # Flag to push all tags 
  --quiet # Flag for supressing output 
rm $IMAGE # Option to  delete an image 
  --force # Flag to delete by force 
  --no-prune # Flag to avoid deleting untagged parents 
ls 
  --digests # Flag to show digests 
  --quiet # Flag to show only image id 
  --no-trunc # Flag to disbale shortening of the image id 
prune -a # To remove all the images which have no container 
```
