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
