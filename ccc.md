# CCC final

## week 1

### Cloud computing

- Definition
  - Cloud computing is a synonym for distributed computing over a network and means the ability to run a program on many connected computers at the same time.
- Essential characteristics
  - **On-demand self-service**. A consumer can provision computing capabilities as needed without requiring human interaction with each service provider.
  - **Networked access**. Capabilities are available over the network and accessed through standard mechanisms that promote use by heterogeneous client platforms.
  - **Resource pooling**. The provider's computing resources are pooled to serve multiple consumers using a multi-tenant model potentially with different physical and virtual resources that can be dynamically assigned and reassigned according to consumer demand.
  - **Rapid elasticity**. Capabilities can be elastically provisioned and released, in some cases automatically, to scale rapidly upon demand.
  - **Measured service**. Cloud systems automatically control and optimize resource use by leveraging a metering capability at some level of abstraction appropriate to the type of service.
- Flavours
  - Computer clouds 计算机云: Amazon、Azure
  - Data clouds 数据云：Amazon、Google docs、iCloud、Dropbox
  - Application clouds 应用程序云：App store, Virtual image factories, Community-specific
  - Private, Public, Hybrid, Mobile, health, … clouds
- Pros
  - avoid upfront infrastructure costs
  - focus on projects that differentiate their businesses instead of on infrastructure
  - get their applications up and running faster, with improved manageability and less maintenance
  - enables IT to more rapidly adjust resources to meet fluctuating and unpredictable business demand.
  - Cloud providers typically use a "pay as you go" model.
- Cons
  - unexpectedly high charges if administrators do not adapt to the cloud pricing model

### Distributed System

#### Key distributed systems focus mid-90s

- Transparency and heterogeneity of computer-computer interactions
- finding/discovering resources (trader!),
- binding to resources in real time, 
- run time type checking, 
- invoking resources
- dealing with heterogeneity of systems
  - applications and operating systems
- focused on computer-computer interaction

#### Challenges of earlier distributed system implementations

- Complexity of implementations: Middleware bloat and lock-in, Vision and challenges of reality
- Vendor specific solutions: Less mature standards
- Scale of the problem area: Telecoms, banking, The growth (boom!) of the web

### Grid computing

From computer-computer focus to organisation-organisation focus

#### Grid technology

- Globus Toolkit Project: GT2 - Complex software system for large, scale distributed software systems development
- Move to service-based approach: GT3 - core technologies refactored as “Grid Services”
- Complete reassessment of OGSI approach to be “purer” web services: GT4 - Web service resource framework (WSRF)

#### How hard can a compute Grid be

- Information Systems:What resources are available. Servers, CPUs, memory, storage, queues,OS, applications, databases
- Monitoring and Discovery Systems: What is the status of those resources
- Job scheduling/resource brokering: Fastest, most secure/reliable, cheapest
- Virtual organisation support: Security, Public Key Infrastructures

## week 2

## week 3

### Compute Scaling

- Vertical Computational Scaling
  - Have faster processors
    - n GHz CPU → 2n GHz ⇒ 2x faster
    - Easy to do, but costs more
  - Limits of fundamental physics/matter

- Horizontal Computational Scaling
  - Have more processors
    - Easy to add more; cost increase not so great
    - Harder to design, develop, test, debug, deploy, manage, understand
    - Transistor count still rising
    - Clock speed flattening sharply
  - High Throughput Computer (HTC) far more important than HPC

### Add "More" Options

- Single machine multiple cores
  - Typical laptop/PC/server these days
- Loosely coupled collection/cluster of machines
  - Pooling/sharing of resources
    - Dedicated vs available only when not in use by others
    - Web services, ... Condor, Seti@home, Boinc
- Tightly coupled cluster of machines
  - Typical HPC/HTC set-up (SPARTAN, NCI, …)
    - Many servers in same rack/server room (often with fast message passing interconnects)
- Widely distributed clusters of machines
  - UK NGS, EGEE, … distributed systems more generally
- Hybrid combinations of the above
  - Leads to many challenges with distributed systems
    - Shared state (or lack thereof)
    - Message passing paradigms – dangers of delayed/lost messages

### Limitations

T(1) time for serial computation  
T(N) time for N parallel computations  
S(N) speed up  
Proportion of speed up depends on parts of program that cannot be parallelized  

#### Amdahl’s Law

$\alpha$ the proportion of the program that cannot be parallelized and must be executed sequentially(非并行运算中不可并行的时间占比)

$T(1) = F(不可并行部分) + P(可并行部分)$, $T(N) = F + P/N$  

$S = \frac{T(1)}{T(N)}=\frac{F + P}{F + P/N}=\frac{1+P/F}{1+P/NF}$  

$P/F = \frac{1-\alpha}{\alpha}$  

$S = \frac{1+\frac{1-\alpha}{\alpha}}{1+\frac{1-\alpha}{N\alpha}}=\frac{1}{\alpha+\frac{1-\alpha}{N}}=\frac{1}{\alpha}$  

**Limitation**: 1) each processor has to deal with loop overheads such as calculation of bounds, 2) test for loop completion 3) loop overhead acts as a further (serial) overhead in running the code. 4) Also getting data to/from many processor overheads.  
Amdahl’s law assumes a fixed problem size. It cannot predict length of time required for some jobs.  

#### Gustafson’s Law （scaled speed-up)

Gustafson’s law proposes that programmers tend to set the size of problems to fully exploit the computing power that becomes available as the resources improve. Therefore, if faster equipment is available, larger problems can be solved within the same time. The law redefines efficiency, due to the possibility that limitations imposed by the sequential part of a program may be countered by increasing the total amount of computation.

$\alpha$ 并行运算中不可并行的时间占比

$T(1) = F(不可并行部分) + Np(可并行部分)$, $T(N) = F + p$  

$S = \frac{T(1)}{T(N)}=\frac{F + Np}{F + p}=\frac{F}{F+p}+\frac{Np}{F+p}$  

$p/F = \frac{1-\alpha}{\alpha}$ (单个进程中的可并行部分时间和不可并行的时间之比)  

$S = \alpha+N(1-\alpha)=N-\alpha(N-1)$  

(Amdahl's law: 给定的任务给无穷多的资源也不能让运行时间少于某一值，Gustafson's Law: 数据规模加大n倍, 处理器加n倍，并行的时间一样)

## week 4

## week 5

## week 6

## week 7

## week 8

## week 9

## week 10

## week 11