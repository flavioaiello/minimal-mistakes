Is isolation secure? Is sharing insecure? The latest advances in the sharing technology extend compute virtualization to the network and storage as well. So how secure are those abstraction layers?

The main objective is privacy. A privacy context is bound to those isolation boundaries and their relative strength.

## Multitenancy
Multitenancy is the ability of systems to independently represent legal and/or organizational units, denominated below as «entity». This is accomplished by sharing capacities such as computing, communication and persistency. Setup and management of capacities both technically and organizationally concerned can take place in shared manner as well.

## Software Supply Chain 
The software supply chain encompasses all steps between development to production. The separation of environments requires data fixtures and synthetic data on all preproduction stages and a high degree of data management. Only if this can be ensured, then the specific degree of multitenancy can vary stage by stage.

## Sharing
Models are not bounded to vendors or implementations. Virtualization is the ability to abstract a lower level technology to a higher level. A shared hardware in fact, well known as virtual machine, is a hardware abstraction, so that multiple operating systems can coexist on a physical machine. The same pattern is now applied to network and storage domain.

![Sharing Model](https://flavio.aiello.ch/assets/images/usm.png)

The specific degree of reuse between of «share all» and «share nothing» eg. fully dedicated is motivated by an economic point of view, while legal and regulatory compliance must be followed. Sharing of capacities such as compute, network and storage and far-reaching automation do also represent a technical and organizational challange. The one million dollar question is: Which degree of sharing is adequate to guarantee the required level of privacy context?

## Conclusions
There is no need to reinvent the wheel on every new technology arising. Usually contradictions are indicating a redefinition of something existent, thus representing just an implementation detail. 

The specific degree of segregation is depending on data-protection requirements but must also take into account the management perspective if anyway fully shared in case of SaaS offerings. 

Sharing must be considered by fully taking into account all base ingredients like compute, network, storage and the management. 

Reducing the specific degree of sharing does not necessairly increase the level of protection.

Security must be economical adequate, thus globally do encryption at some level as countermeasure of non quantified risks is probably not rationale.

## Recommendations

1.	Define your segregation methods for compute, network and storage as well
2.	Challenge your methods with your business requirements and non-functionals (legal and regulation point of view)
3.	Continuously adjust and improve

## The sharing model

### Management domain

#### Organization
- Dedicated: Named Individuals on same company as the entity itself
- Shared: Outsourcing mandate 

#### Technical
Technical management from a legacy point of view (imperative maintenance). Today «orchestration» for new declarative technologies eg. cloud and container based services. Maybe there will be a new definition for AI based management, but the topic will remain the same.

- Dedicated: Management consoles and orchestrators per entity
- Shared: Networks like VPN, Management Conosoles, Orchestrators etc.

### Compute domain

#### Hardware
- Dedicated: One physical hardware per OS
- Shared: Virtual hardware accomplished by hypervisor based virtualization like KVM, ESX, LPAR, etc.

#### Operating system
- Dedicated: One OS per process
- Shared: OS sharing accomplished by plain old multitasking, BSD Jails, Sun Zones, LXC, Containers etc. 

#### Process
- Dedicated: One process per application
- Shared: Process sharing accomplished by application runtimes like JVM, .NET CLR, Tomcat, Weblogic, Wildfly, etc.

#### Application
- Dedicated: One application per privacy context
- Shared: Application sharing accomplished by propritary methods

### Network domain

#### Interface
- Dedicated: Physical interface per entity
- Shared: Virtual interface accomplished by shared hardware or shared operating system

#### Domain
- Dedicated: Broadcast domain delimited by dedicated network cables and devices per entity
- Shared: Virtual broadcast domains delimited by tagging or encapsulation technologies like VLAN, VXLAN, MPLS, GRE, etc.

#### Address
- Dedicated: Specific routable addressing per entity
- Shared: Overlay addressing accomplished by extensive NAT and encapsulation methods

#### Socket
- Dedicated: Single listener software service per entity
- Shared: Multiple software services on the same listener 

### Storage domain

#### Device
- Dedicated: Physical device per entity
- Shared: Virtual device accomplished by partitioning

#### Volume
- Dedicated: Logical volume per entity
- Shared: Logical volume accomplished 

#### File system
* Dedicated: Mount point per entity 
* Shared: One mount point accomplished by NFS, NIFS, SMB, etc.

#### File
* Dedicated: Clear delimited folders and files per entity
* Shared: Non delimited data persistency fully depending on software service logic (application)
