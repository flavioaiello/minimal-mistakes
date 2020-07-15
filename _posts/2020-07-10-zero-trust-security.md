The Hyperscalers have written several whitepapers explaining zero trust security. As perimeter based security model work less and less for the end users, it's also being questioned for cloud workloads as well. Physical locations do not reflect data centers, enduser, edge and mobile computing based services any longer. There is no more exclusive physical inside a building as per default safe place.

Nowadays, infrastructure is abstracted so that workload is orchestratable. Service to service communication is a lightweight overlay with traffic control, policies, and centralized monitoring for service calls. Container workloads consist of kernel based security measures with no default trust between any service.

When it comes to zero trust security, compute, communication and storage must be protected independently. This encompasses mutual authenticated and encrypted communication between services and at rest, denial of service protection and limitation of blast radius. This protection includes also source code provenance and maintenance. As such, a deterministic work pattern is required to roll out services repeatedly consistently.

This blogpost reflects several pieces of work collected and reflected over time, started 2014 to protect a container based multi-tenant advisory service for banks - now known as cloud-native design - against security threats.

## Motivation

The perimeter based security model assumed at the time a fundamental physical degree of isolation in terms of compute, communication and storage. The sharing of infrastructure between trusted parties is a traditional approach to achieve economical benefits. Traditional sharing of software between trusted parties is also a well known pattern for non internet facing software, such as ERP systems.

The sharing of hardware eg. virtualization introduced software defined computing that made blur traditional boundaries. With the later introduction of software defined networking and storage, the perimeter became virtual as well. As such, a physical representation fully disappeared.

The sharing of operating systems eg. containerization introduced a new level of software defined computing that made not only blur traditional boundaries, but also imposted a deterministic work pattern. Having now the software defined networking and storage terminated in the containers, the workload became portable and any form of perimeter dissolved.

Containerization raised the utilization ratio of infrastructure even more efficient than virtualization. Furthermore it made cloud computing through identity federation and distributed stateless software design became reality. At the same time, containers mitigated lateral movement, if an attacker would breach the perimeter.

![Zero Trust compared to Perimeter](https://flavio.aiello.ch/assets/images/zerotrustperimeter.png)

## Reality
With no fundamental physical degree of isolation, users working from anywhere and services being produced everywhere, there is no more crocodile pit nor a wall perimeter. Actual security concepts are now a zero trust model where security is addressed the same way for services as it is for users, regardless of the network. As such, traditional firewalls may still protect infrastructure, but portable services need to be securely run everywhere.

## Consumerization
From its earliest days, low cost consumer technology or commodity hardware prevailed. The cloud native architecture is designed for commodity based computing. Thanks to portability, distributing multiple low cost stateless software instances across machines is load and fault tolerant without affecting the user experience. When it comes to stateful software, the same orchestration mechanism reschedules containers across multiple machines, so that workload quorums are provided alive.

## Decoupling
Making software resilient and portable is based on loose coupling dependencies to the infrastructure with a high degree of cohesion within the services. This is achieved by read only containers that need to be rebuilt and distributed as required by vulnerabilities and their according risk. The imposed deterministic work pattern is required to roll out services repeatedly consistently through continuous runtime reconciliation. The deterministic work pattern makes the software lifecycle programmable and more secure, maintainable, reproducible and scalable, even more consistent and uniform between teams. As such, developers are enabled to produce secure software more efficiently.

## Evolution
The table below shows how the aspects of traditional infrastructure perimeter security model compared to zero trust security:

|Perimeter|Zero Trust|
|--- |--- |
|The perimeter security model protects physical assets rater then services. Workloads are perimeter delimited with internal networks and according infrastructure in range considered trusted.|Although the edge must still be protected, the perimeter model lacks to protect nowadays, fine grained services. Traditional firewalls on the edge may still protect the physical networks and infrastructure. The network and infrastructure is considered untrusted. Trust is explicit and mutually on communication level between services. With zero trust the obsolete was removed and security measures made portable to follow the services.|
|Traditional workload is installed on specific machines with specific addressing. Firewalls rely on this static model to identify security identifiers and implement according rules.|Fine grained portable services remove the network layer by implementing no default trust. Service based identification to legitimate access between services.|
|Fixed infrastructure and network addressing.|Portable services rely on service endpoints. Automatic addressing and placement, thus raising substantially the utilization ratio.|
|Services run in explicit network zones.|Services run literally anywhere.|
|Individual application security requirements.|Consistent secure software development lifecycle applied to all services.|
|Individual implementation of security requirements.|Standardized overall consistent security requirements.|
|Individual security measures with limited oversight.|Standardized overall observablesecurity measures.|
|Infrequent but therefore extensive changes with higher risk incidence.|Simple, low risk, small frequent standardized and automated changes.|
|Workloads are isolated physically.|Workloads are isolated logically.|


## Security Principles
The outlined zero trust topology requires the following security principles:

|Topic|Principle|
|--- |--- |
|Edge	|Service exposure is protected by volume based attack mitigation and authentication at the edge. Traditional firewalls on the edge still protect the physical infrastructure against the internet|
|Crosstalk|The infrastructure is fully restricted so that no default machine to machine network traffic is allowed|
|Artifacts|Only artifacts of known origin are allowed to be executed|
|Runtime|Only machines are allowed to execute artifacts that comply with the set of execution requirements|
|Policy|Service to Service communication is automatically validated and complying to policies. Services are only allowed to communicate each other by mutual authentication|
|Automation|Source configuration management (SCM) based infrastructure with continuous runtime reconciliation (GitOps) and security observability|
|Isolation|Execution breach mitigation by adequate measures such as shell less machines for root- and deamonless oci execution with sandboxing|
|Organisation|Security must rely on automation and not on individuals|

The implementation of above principles enable scaling while enforcing security and positively affecting the development experience.

## Embrace the change

Zero trust encompasses the protection of the physical infrastructure and the build of basic bricks like identity federation, declarative edge networking and service mesh to spark the development of secure portable reliable services. Physical representation of the user or service location has vanished and there is no more default safe place. Zero trust describes a standard approach to secure portable and decoupled services across traditional data centers, hybrid and public clouds. Zero trust is a traditional least privilege principle applied to services and data instead of network or infrastructure, thus providing isolation between workloads followed by automated vulnerability management, no implicit trusted communication and least privilege access control to data.

As such, security is no more the last deliverable of a traditional project but already incorporated as main pillar during any stage of any creational process. Embracing zero trust makes economical sense due to the high degree of automation. The security principles provide benefits to the developers and security staff to continuously improve and deliver high standing data protection and reliable services.

## Further Readings

*   [Zero Trust by Microsoft](https://www.microsoft.com/en-us/security/business/zero-trust)
*   [BeyondCorp & BeyondProd by Google](https://cloud.google.com/beyondcorp)
