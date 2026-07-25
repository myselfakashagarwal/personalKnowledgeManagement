## Docker daemon 
Docker daemon / engine is the main service which does all the heavy lifting like calling underlying utils to create container, image managing drivers etc. To communicate with the host(daemon) there are clients which include cli, and node packages etc using tcp socket for communication. 
##### Docker Daemon sample configuration

```bash
### /usr/lib/systemd/system/docker.service                                                                                                                                                                                                                                                                                                                                                                             

# [Unit]                                                                                                                                                                                                                                                                                                                                                                                                               
# Description=Docker Application Container Engine                                                                                                                                                                                                                                                                                                                                                                      
# Documentation=https://docs.docker.com                                                                                                                                                                                                                                                                                                                                                                                
# After=network-online.target nss-lookup.target docker.socket firewalld.service containerd.service time-set.target                                                                                                                                                                                                                                                                                                     
# Wants=network-online.target containerd.service                                                                                                                                                                                                                                                                                                                                                                       
# Requires=docker.socket                                                                                                                                                                                                                                                                                                                                                                                               
# StartLimitBurst=3                                                                                                                                                                                                                                                                                                                                                                                                    
# StartLimitIntervalSec=60                                                                                                                                                                                                                                                                                                                                                                                             
#                                                                                                                                                                                                                                                                                                                                                                                                                      
# [Service]                                                                                                                                                                                                                                                                                                                                                                                                            
# Type=notify                                                                                                                                                                                                                                                                                                                                                                                                          
# # the default is not to use systemd for cgroups because the delegate issues still                                                                                                                                                                                                                                                                                                                                    
# # exists and systemd currently does not support the cgroup feature set required                                                                                                                                                                                                                                                                                                                                      
# # for containers run by docker                                                                                                                                                                                                                                                                                                                                                                                       
# ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock                                                                                                                                                                                                                                                                                                                                     
# ExecReload=/bin/kill -s HUP $MAINPID                                                                                                                                                                                                                                                                                                                                                                                 
# TimeoutStartSec=0                                                                                                                                                                                                                                                                                                                                                                                                    
# RestartSec=2                                                                                                                                                                                                                                                                                                                                                                                                         
# Restart=always                                                                                                                                                                                                                                                                                                                                                                                                       
#                                                                                                                                                                                                                                                                                                                                                                                                                      
# # Having non-zero Limit*s causes performance problems due to accounting overhead                                                                                                                                                                                                                                                                                                                                     
# # in the kernel. We recommend using cgroups to do container-local accounting.                                                                                                                                                                                                                                                                                                                                        
# LimitNPROC=infinity                                                                                                                                                                                                                                                                                                                                                                                                  
# LimitCORE=infinity                                                                                                                                                                                                                                                                                                                                                                                                   
#                                                                                                                                                                                                                                                                                                                                                                                                                      
# # Comment TasksMax if your systemd version does not support it.                                                                                                                                                                                                                                                                                                                                                      
# # Only systemd 226 and above support this option.                                                                                                                                                                                                                                                                                                                                                                    
# TasksMax=infinity                                                                                                                                                                                                                                                                                                                                                                                                    
#                                                                                                                                                                                                                                                                                                                                                                                                                      
# # set delegate yes so that systemd does not reset the cgroups of docker containers                                                                                                                                                                                                                                                                                                                                   
# Delegate=yes                                                                                                                                                                                                                                                                                                                                                                                                         
#                                                                                                                                                                                                                                                                                                                                                                                                                      
# # kill only the docker process, not all processes in the cgroup                                                                                                                                                                                                                                                                                                                                                      
# KillMode=process                                                                                                                                                                                                                                                                                                                                                                                                     
# OOMScoreAdjust=-500                                                                                                                                                                                                                                                                                                                                                                                                  
#                                                                                                                                                                                                                                                                                                                                                                                                                      
# [Install]                                                                                                                                                                                                                                                                                                                                                                                                            
# WantedBy=multi-user.target                                                                                                                                                                                                                                                                                                                                                                                           
### /run/systemd/system/service.d/zzz-lxc-service.conf                                                                                                                                                                                                                                                                                                                                                                 
# [Service]                                                                                                                                                                                                                                                                                                                                                                                                            
# ProcSubset=all                                                                                                                                                                                                                                                                                                                                                                                                       
# ProtectProc=default                                                                                                                                                                                                                                                                                                                                                                                                  
# ProtectControlGroups=no                                                                                                                                                                                                                                                                                                                                                                                              
# ProtectKernelTunables=no                                                                                                                                                                                                                                                                                                                                                                                             
# NoNewPrivileges=no                                                                                                                                                                                                                                                                                                                                                                                                   
# LoadCredential=                                                                                                                                                                                                                                                                                                                                                                                                      
# ProtectHome=no                                                                                                                                                                                                                                                                                                                                                                                                       
# ProtectSystem=no                                                                                                                                                                                                                                                                                                                                                                                                     
# PrivateDevices=no                                                                                                                                                                                                                                                                                                                                                                                                    
# PrivateTmp=no                                                                                                                                                                                                                                                                                                                                                                                                        
# ProtectKernelLogs=no                                                                                                                                                                                                                                                                                                                                                                                                 
# ProtectKernelModules=no                                                                                                                                                                                                                                                                                                                                                                                              
# ReadWritePaths=
```

```bash
[Unit]
Description=Docker Application Container Engine
Documentation=https://docs.docker.com
After=network-online.target nss-lookup.target docker.socket firewalld.service containerd.service time-set.target
Wants=network-online.target containerd.service
Requires=docker.socket

[Service]
Type=notify
# the default is not to use systemd for cgroups because the delegate issues still
# exists and systemd currently does not support the cgroup feature set required
# for containers run by docker

# Reset original ExecStart, then define the new one
ExecStart=
ExecStart=/usr/bin/dockerd -H fd:// -H tcp://0.0.0.0:2375 --containerd=/run/containerd/containerd.sock

ExecReload=/bin/kill -s HUP $MAINPID
TimeoutStartSec=0
RestartSec=2
Restart=always

# Note that StartLimit* options were moved from "Service" to "Unit" in systemd 229.
# Both the old, and new location are accepted by systemd 229 and up, so using the old location
# to make them work for either version of systemd.
StartLimitBurst=3

```

```bash
# For tls based service config use these addons 
dockerd \
  --host=tcp://HOST:2376 \
  --tlsverify \
	  --tlscacert=/etc/docker/ca.pem \
	  --tlscert=/etc/docker/server-cert.pem \
	  --tlskey=/etc/docker/server-key.pem
```
## Docker client (cli)
Docker cli is the client for controlling docker daemon, this client uses tcp socket to communicate with docker. This client can communicate with remote docker host, by exposing docker daemon to publicly available network interface. 
### Contexts
A context can be defined as a definition of the remote host thy which cli calls to. The default context defines docker host as the file descriptor. Docker context contains NAME, HOST (docker endpoint), TLS material if any, and CONTEXT data storage. Context can be treated as a namespace as the container running in one context are separated from other

tls material location: `~/.docker/contexts/tls/<context_id>/docker/`
##### Sample Context file without tls 
```JSON
// Sample context file no tls 

[
    {
        "Name": "HACKCTL",
        "Metadata": {},
        "Endpoints": {
            "docker": {
                "Host": "tcp://198.19.249.186:2375",
                "SkipTLSVerify": false
            }
        },
        "TLSMaterial": {},
        "Storage": {
            "MetadataPath": "/Users/myselfakashagarwal/.docker/contexts/meta/edfcc40ca8748abfbbf89ec21fcdda8d390fa43ac6a40af68e373b31c8a07655",
            "TLSPath": "/Users/myselfakashagarwal/.docker/contexts/tls/edfcc40ca8748abfbbf89ec21fcdda8d390fa43ac6a40af68e373b31c8a07655"
        }
    }
]

// context with tls 

[
	{
	  "Name": "secure-remote",
	  "Metadata": {},
	  "Endpoints": {
	    "docker": {
	      "Host": "tcp://198.19.249.186:2376",
	      "SkipTLSVerify": false
	    }
	  },
	  "TLSMaterial": {
	    "docker": [
	      "ca.pem",
	      "cert.pem",
	      "key.pem"
	    ]
	  },
	  "Storage": {
	    "MetadataPath": "~/.docker/contexts/meta/<context_id>",
	    "TLSPath": "~/.docker/contexts/tls/<context_id>"
	  }
	}
]

```
##### Related Commands 
```bash
docker context 

create CONTEXT-NAME  # option to create a context out of current
  --docker "host=DOCKER-ENDPOINT",from=OTHER-CONTEXT # option to create context of remote docker host 
  --from OTHER-CONTEXT # option to create context out of existing context
  --description "DESCRIPTION" # option to add description to context
export CONTEXT-NAME FILPATH # option to store conetext configration into a file 
import FILPATH # option to create context out of declarative config file 
inspect CONTEXT # option to get context info 
  --format $FORMAT
ls # flag to list contexts 
rm CONTEXT # option to delete context
use CONTEXT # option to switch from one context to another 
update CONTEXT # option to switch from one context to another 
  --docker DOCKER-ENDPOINT,from=OTHER-CONTEXT # option for remote docker host 
  --description "DESCRIPTION" # option add/edit description to context
```
