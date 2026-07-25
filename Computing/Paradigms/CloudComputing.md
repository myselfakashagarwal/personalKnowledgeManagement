Cloud computing aims to maximize flexibility, reliability and scaling with a servicing model where you pay for the infrastructure,  provided applications and platform level applications as long as they used with an almost never ending quick supply with access. Can be release as soon as the resource is no longer needed hence you don’t own but rent at different levels resource oriented computational layers. 
## Essential Characteristics (NIST)
The technical definition of cloud computing is governed by five essential characteristics that distinguish it from traditional virtualization or hosted data centers:
On-demand self-service: Users can provision compute, storage, and network resources automatically through APIs without requiring human interaction with the service provider.
### Broad network access
Services are available over the network and accessed through standard mechanisms (HTTP/REST) that promote use by heterogeneous thin or thick client platforms.
### Resource pooling
The provider’s physical resources are pooled to serve multiple consumers using a multi-tenant model. Resources are dynamically assigned and reassigned according to demand, with a degree of location independence.
### Rapid elasticity
Resources can be elastically provisioned and released, in some cases automatically, to scale rapidly outward and inward commensurate with demand. To the consumer, the available resources often appear to be infinite.
### Measured service
Cloud systems automatically control and optimize resource use by leveraging a metering capability at some level of abstraction appropriate to the type of service (e.g., storage, processing, bandwidth, and active user accounts).
## Architecture 
The cloud architecture at high level can be divided into three parts based of resources `hardware` `system` `application/api` 
### Hardware Layer
The hardware layer is the physical execution foundation and defines all primary failure domains. A cloud provider operates multiple regions, each region being a geographically isolated deployment boundary composed of multiple availability zones. An availability zone is an independent fault domain containing one or more physically separate data centers within the same metropolitan area, each with isolated power, cooling, and network aggregation. A data center contains rows organized for thermal and electrical efficiency, rows contain racks connected through top of rack switches and shared power distribution, and racks contain servers. 

A server is the atomic compute unit and includes CPU sockets with multiple cores and hardware threads, NUMA topology, DRAM channels, local NVMe or SSD storage, PCIe fabric, NICs with SR IOV capability, and firmware controllers. This layer provides raw compute cycles, memory bandwidth, storage throughput, and network capacity, but no multi tenant abstraction.
### Systems Layer 
The system layer virtualizes and aggregates these discrete hardware resources into schedulable, isolated pools. Each server runs a hypervisor such as KVM or a provider specific minimal hypervisor architecture integrated with offload hardware.

CPU virtualization maps virtual CPUs to hardware threads using hardware assisted virtualization extensions, with time sliced scheduling and quota enforcement. Memory is virtualized through second level address translation with strict page level isolation.
I/O devices are exposed via para virtual drivers or SR IOV virtual functions with IOMMU enforced isolation. 

Hosts register with a regional control plane that maintains inventory, health state, hardware class metadata, and topology awareness including rack and availability zone mapping. Placement engines allocate instances based on capacity, NUMA alignment, accelerator requirements, rack diversity, and zone level isolation constraints. Distributed block storage systems replicate or erasure code data across multiple servers and racks within an availability zone. 

Software defined networking constructs overlay networks spanning racks and data centers, attaching virtual interfaces to instances and enforcing routing and policy at hypervisor or offload hardware level. Tenancy models are enforced here: shared tenancy multiplexes multiple tenants on a single host with hardware enforced isolation, dedicated tenancy reserves a full host for one tenant account, and bare metal exposes physical hardware directly while remaining integrated with control planes.
### Api
The API layer provides the external programmable interface that translates user intent into system layer operations. Services such as AWS EC2 expose authenticated APIs for instance lifecycle management, storage provisioning, network configuration, identity enforcement, scaling, and telemetry. 

A request to launch an instance triggers policy validation, quota checks, availability zone selection, host placement, hypervisor instantiation, virtual network interface allocation, block storage attachment, and control plane state updates. This layer does not directly manage hardware; it orchestrates the system layer, which in turn virtualizes the hardware layer. Hardware defines capacity and failure domains, the system layer transforms that capacity into isolated resource pools with topology aware scheduling, and the API layer exposes deterministic, programmable control over those pools.
## Deployment models 
Cloud and infrastructure deployment models define ownership, control boundary, tenancy model, and operational responsibility. The distinction is primarily about who owns the hardware substrate and who operates the system and control planes.
### Public Cloud  
Infrastructure is owned and operated by a third party provider. Hardware, system virtualization, networking, and control planes are fully managed by the provider. Customers consume compute, storage, and networking through APIs. Tenancy is typically multi tenant with hypervisor enforced isolation, although dedicated host options exist. Elastic scaling and geographic distribution are intrinsic. Examples include Amazon Web Services, Microsoft Azure, and Google Cloud.
### Private Cloud  
Infrastructure is dedicated to a single organization but still follows cloud operational principles such as resource pooling, self service APIs, and virtualization. Hardware may be hosted in enterprise data centers or colocations. Platforms such as OpenStack and VMware vSphere are used to virtualize and pool resources. Control remains entirely within the organization.
### On Premises  
On premises infrastructure is traditional enterprise data center deployment without necessarily implementing cloud abstractions. Hardware, hypervisors, operating systems, networking, and storage are owned and directly managed by the organization. Resource allocation is often static or manually provisioned. API driven orchestration and elastic pooling may not exist. Virtualization may be present, but the operational model is not cloud native. This model maximizes physical control but requires full lifecycle management including hardware procurement, capacity planning, and failure handling.
### Hybrid Cloud  
Hybrid integrates on premises or private cloud infrastructure with public cloud environments. Identity, networking, and workload mobility are interconnected. Control planes may remain separate but are bridged via secure connectivity. Services such as Azure Arc and AWS Outposts extend public cloud control planes into enterprise environments. This model enables workload distribution based on latency, regulatory constraints, or data residency.
### Multi Cloud  
Multi cloud uses multiple public cloud providers simultaneously. Each provider maintains independent control planes and regional isolation. Cross cloud orchestration is typically implemented using abstraction layers such as Kubernetes. This model increases resilience against provider level outages but adds operational and networking complexity.
### Community Cloud  
Infrastructure is shared among organizations with similar regulatory or mission requirements. Governance is shared or delegated to a managing entity. Isolation exists at tenant boundary but operational policies are aligned across participants.
## [Service level Agreements](https://aws.amazon.com/what-is/service-level-agreement/)
## Servicing models 
The resources provided can be at all levels across the stack these include 
### Infrastructure  as a Service (IAAS) 
In IAAS the resources provided are just virtualized hardware resources like Compute - VMs , Storage - NFS , Disks , Databases etc. These have to be configured by the user itself in terms of installing and setting up system level services like build pipelines , web servers, monitoring etc along with development of customer facing applications. 
### Platform as a Service  (PAAS)
In PAAS the cloud providers provide pro priority cloud managed application for the infrastructure like CICD pipelines monitoring hosting, however in this layer too the applications need to be developed at home. 
### Software as a Service (SAAS)
In SAAS all the tech stack is build managed and configured by cloud including infra, platform & application. The clients have to deal with business logic and customer onboarding. 
## Responsibility model 
![](https://docs.microsoft.com/en-us/azure/security/fundamentals/media/shared-responsibility/shared-responsibility.png)
## Pricing models 
### Capital Expenditure (CapEx)
A (usually large) amount of money invested in an asset (building, computers, equipment) spent up front, and it returns profits slowly over time; major cash drain or loan required; cannot be deducted from your taxes in one year, depreciated over several years
### Operating Expenditure (OpEx)
an amount of money spent “every month” as an operating expense; hopefully, you earn more money in revenue from it than you spend; can be deducted from your taxes immediately; many accountants prefer OpEx over CapEx for the tax and cash flow benefits
### Consumption-Based Model
paying for something based on how much you used, as opposed to paying for something no matter if you use it or not.

## Related terms 
| Term         | Technical Explanation                                                                                                              |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------- |
| Elasticity   | Automatic provisioning and deprovisioning of cloud resources in response to real time demand without manual intervention.          |
| Scalability  | Ability to increase or decrease resource capacity horizontally or vertically to handle workload growth within a cloud environment. |
| Availability | Percentage of time a cloud service remains operational and accessible, typically defined in service level agreements.              |
| Reliability  | Probability that a cloud service performs correctly without failure over a defined time interval.                                  |
| Durability   | Measure of data persistence over time, typically achieved through multi zone replication and redundancy.                           |
| Auto Scaling | Policy driven mechanism that adjusts the number of active resources based on workload metrics.                                     |
| Resilience   | Ability of a cloud workload to recover automatically from infrastructure or service failures.                                      |
|              |                                                                                                                                    |
|              |                                                                                                                                    |


More @  [Cloud Services](https://notes.kodekloud.com/)