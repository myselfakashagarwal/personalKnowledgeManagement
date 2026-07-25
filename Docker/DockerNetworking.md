## Docker networking 
On linux systems the docker daemon process runs in host network, the daemon creates a default network `bridge` in which containers with no network specified are placed init, matter fact containers can communicate with each other via IPs and host. 
### Networking modes 
#### None 
In the none networking mode the network access to the container is restricted the container runs offline 
#### Host 
In the host network driver the containers share the host network, host client processes can access the container, no need of port mapping. 
#### Bridge 
The bridge network is the default network in which new containers are placed, in this network containers can address each other and host network can access them if the port mapping is present. In case of user defined bridge network containers can address using their container name. Its like a private subnet. 
#### Macvlan
In the macvlan network driver, containers are assigned a MAC address and appear as physical devices on the LAN. Each container gets a unique IP from the local network, and external devices can communicate with containers directly without NAT. Suitable for scenarios needing containers to look like real hosts on the network.
#### Overlay
In the overlay network driver, multiple Docker daemons can connect containers across different hosts. It uses VXLAN tunneling to create a distributed network. This is commonly used in Docker Swarm or multi-host setups where containers need to communicate across nodes securely.
#### IPVLAN
In the ipvlan network driver, containers share the host’s MAC address but are given unique IPs. It is more lightweight than macvlan since it does not require a unique MAC per container. Useful in environments where the network restricts multiple MAC addresses per interface.
##### Related Commands 
```bash
docker network [COMMAND/ARGUMENTS]

create NETWORKNAME # To creates a network with default driver bridge 
  --driver DRIVERNAME # provide driver for the network 
  --subnet SUBNETIP/RANGE # add a subnetwrok 
  --gateway GATEWAYIP # gateway ip for in-out communication 
  --ip-range CIDRBLOCK # allocate ips from provided cidr block 
  --internal # only internal communical is feasible 
  --ipv6 BOOLEAN # ip addressing used 
  --ipv4 BOOLEAN # ip addressing used 
connect NETWORKNAME CONTAINER 
  --ip IPV4 # provide custom ipv4
  --ip6 IPV6 # provide custom ipv6 
disconnect NETWORKNAME CONTAINER 
  --force # Force the container to disconnect from a network
ls # list all networks 
prune # remove unused network 
rm NETWORKNAME # delete network 
  --force #	Force delete a network

```
