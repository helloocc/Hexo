
---
title: Openstack project 简介
date: 2017-11-02 23:28:11
tags:
---

#### Compute
- Nova
提供计算实例（也称为虚拟服务器）的OpenStack项目。Nova支持创建虚拟机，裸机服务器（通过使用ironic）..Nova作为一组守护进程运行在现有的Linux服务器之上，以提供该服务。

- glance
openstack用image创建以及重构虚拟机，openstack允许用户upload一定数量的image供创建虚拟机使用。image由openstack glance服务管理，glance服务主要包括两个进程，glance-api和glance-registry, 前者提供对glance服务相关的api，后者负责image注册等。

- Ironic
它提供裸机。在一些场景下，虚拟化的环境是不合适的，这些场景的用户需要的是真实的、物理的、裸金属的服务器。为了能够满足这些场景的自助服务需求，OpenStack 需要支持裸机的部署。在 OpenStack 中部署裸机就意味着用户可以直接操作硬件设施、部署应用负载（镜像）到真正的物理机器，而不是运行在 hypervisor 之上的虚拟机中。为了实现这个功能，Nova 包含的虚拟化驱动中其中一个就是调用 Ironic 来启动裸机节点。


<!--more-->

- Magnum
由OpenStack Containers Team开发的OpenStack API服务，可用作OpenStack中的头等资源，作为Docker Swarm，Kubernetes和Apache Mesos等容器编排引擎。Magnum使用Heat来编排包含Docker和Kubernetes的OS映像，并在群集配置中的虚拟机或裸机中运行该映像。

- Storlets
Openstack Swift的扩展，它允许通过使用Docker容器以安全和隔离的方式在对象存储库内运行用户定义的代码。

- Zun
是OpenStack的集装箱管理服务。它旨在提供一个用于启动和管理由不同容器技术支持的容器的OpenStack API。


#### Storage, Backup & Recovery

- Swift
是一个高可用性，分布式，最终一致的对象/ blob存储。Swift的目的是使用普通硬件来构建冗余的、可扩展的分布式对象存储集群，存储容量可达PB级。
Swift并不是文件系统或者实时的数据存储系统，它是对象存储，用于永久类型的静态数据的长期存储，这些数据可以检索、调整，必要时进行更新。最适合存储的数据类型的例子是虚拟机镜像、图片存储、邮件存储和存档备份。Swift通过在软件层面引入一致性哈希技术和数据冗余性，牺牲一定程度的数据一致性来达到高可用性（High Availability，简称HA）和可伸缩性，支持多租户模式、容器和对象读写操作，适合解决互联网的应用场景下非结构化数据存储问题。

- Cinder
操作系统获得存储空间的方式一般有两种：
    1. 通过某种协议（SAS,SCSI,SAN,iSCSI 等）挂接裸硬盘，然后分区、格式化、创建文件系统；或者直接使用裸硬盘存储数据（数据库）
    2. 通过 NFS、CIFS 等 协议，mount 远程的文件系统
第一种裸硬盘的方式叫做 Block Storage（块存储），每个裸硬盘通常也称作 Volume（卷） 
第二种叫做文件系统存储。NAS 和 NFS 服务器，以及各种分布式文件系统提供的都是这种存储。 
Block Storage Servicet 提供对 volume 从创建到删除整个生命周期的管理。 从 instance 的角度看，挂载的每一个 Volume 都是一块硬盘。


- Manila
File Share Service.其实就是实现一个共享服务，也就是说我可以创建共享目录，任意虚拟机可以对该目录进行读写操作.

- Karbor
处理保护包括OpenStack部署的应用程序的数据和元数据，防止丢失/损坏（例如备份，复制） - 不要与应用程序安全或DLP混淆。它通过提供API和服务的标准框架来实现，使得供应商能够将各种数据保护服务引入用户的一致和统一的流程。

- Freezer
是分布式备份还原和灾难恢复作为服务平台。它被设计为多操作系统（Linux，Windows，OSX，* BSD），专注于为基于块的备份提供效率和灵活性，基于文件的增量备份，时间点动作，作业同步（即多个节点上的备份同步）和许多其他功能。


#### Networking & Content Delivery 

- Neutron(Networking)
 OpenStack 项目中负责提供网络服务的组件，它基于软件定义网络的思想，实现了网络虚拟化下的资源管理。

- Designate(DNS Service)
OpenStack的多租户DNSaaS服务。它提供了一个集成了Keystone认证的REST API。可以将其配置为基于Nova和Neutron操作自动生成记录。


- Gragonflow(Neutron Plugin)
Dragonflow是支持分布式交换，路由，DHCP等功能的OpenStack®Neutron™的分布式SDN控制器。Dragonflow旨在支持大规模部署，重点是延迟和性能，以及提供在每个计算节点上本地运行的高级创新服务，并考虑容器技术。

- Kuryr(Container plugin)
Kuryr背后的想法是能够利用Neutron及其插件和服务中的抽象和所有艰苦的工作，并使用它来为容器用例提供生产级网络。而不是每个独立的Neutron插件或解决方案试图找到和弥补差距，我们可以集中精力集中在一个地方 - Kuryr。Kuryr旨在成为两个社区Docker和Neutron之间的“一体化桥梁”

- Octavia（Load Balancer）
Octavia是一款开源的，运营商级的负载平衡解决方案，旨在与OpenStack配合使用。octaviay由 neutron LBaaS项目承担。Octavia通过管理一组虚拟机，集装箱或裸机服务器（统称为amphorae）来完成其负载平衡服务的交付。这种按需的水平缩放功能将Octavia与其他负载平衡解决方案区分开来，从而使Octavia真正适合“为云”。

- Tacker(NFV Orchestration)
构建通用VNF管理器（VNFM）和NFV管理器（NFVO），以在像OpenStack这样的NFV基础设施平台上部署和运行网络服务和虚拟网络功能（VNF）。它基于ETSI MANO架构框架，并使用VNF向端到端的协调网络服务提供功能堆栈。

- Tricircle(Networking Automation for Multi-Region Deployments)
The Tricircle is to provide networking automation across Neutron in multi-region OpenStack deployments.

#### Data & Analytics

- Trove
Openstack Trove是openstack为用户提供的数据库即服务(DBaaS)。所谓DBaaS，即trove既具有数据库管理的功能，又具有云计算的优势。使用trove，用户可以："按需"获得数据库服务器/配置所获得的数据库服务器或者数据库服务器集群/对数据库服务器或者数据库服务器集群进行自动化管理/根据数据库的负载让数据库服务器集群动态伸缩

- Sahara(Big Data Processing Framework Provisioning)
Sahara可以为用户提供部署Hadoop集群的能力，可以通过简单配置如Hadoop版本、集群结构、节点硬件信息等等完成集群部署。主要应用在提供在OpenStack上快速配置和部署Hadoop集群的能力，提供分析即服务的数据分析服务。
- searchlight(Indexing and Search)
Searchlight大大提高了用户关注的搜索功能和代表各种OpenStack云服务的性能。它通过从现有API服务器卸载用户搜索查询并将其数据索引到ElasticSearch来实现此目的

#### Security, Identity & Compliance
- keystone
Keystone是一个OpenStack服务，通过实施OpenStack的Identity API，提供API客户端认证，服务发现和分布式多租户授权 。

- barbican(Key Management)
Barbican是一种专为安全存储，配置和管理密码（如密码，加密密钥和X.509证书）而设计的REST API。

- congress(Governance)
旨在为所有云服务集合提供政策服务，以便为动态基础架构提供治理和合规性。

- mistral(Workflow service)
Mistral是一个工作流服务。大多数业务流程由多个不同的互连步骤组成，需要在分布式环境中以特定顺序执行。可以将这种过程描述为一组任务和任务关系，并将这种描述上传到Mistral，以便它处理状态管理，正确的执行顺序，并行性，同步和高可用性。Mistral还提供灵活的任务调度，以便我们可以根据指定的时间表（即每个星期日下午4:00）运行一个进程，而不是立即运行。我们将这样的任务和关系称为工作流。

#### Management Tools

- Horizon
Horizo​​n是OpenStack的仪表板的规范实施，它为OpenStack服务提供了基于Web的用户界面，包括Nova，Swift，Keystone等。

- Openstack client
是OpenStack的命令行客户端，它将计算，身份，图像，对象存储和块存储API的命令集合在具有统一命令结构的单个shell中。

- rally
Rally是一个基准测试工具，可以自动化和统一多节点OpenStack部署，云验证，基准测试和分析。它可以用作OpenStack CI / CD系统的基本工具，可以不断提高其SLA，性能和稳定性。

- senlin(Clustering service)
Senlin是专为管理其他OpenStack服务中同类对象而设计的集群服务。它的特点在于拥有一个开放的框架，开发者能够为指定类型的对象提供插件以实现托管，以及在特定集群运行时想执行的策略。

- vitrage(RCA (Root Cause Analysis service))
Vitrage是用于组织，分析和扩展OpenStack报警和事件的OpenStack RCA（根本原因分析）服务，提供有关问题根本原因的见解，并在直接检测到之前推断其存在。

- watcher(Optimization Service)
OpenStack Watcher为多租户基于OpenStack的云提供灵活可扩展的资源优化服务。Watcher提供了一个完整的优化循环，包括从度量接收器，复杂事件处理器和分析器，优化处理器和操作计划应用程序的所有内容

#### Deployment Tools
- chef openstack(Chef cookbooks for OpenStack)
允许自动化OpenStack云部署的构建，操作和消费。

- kolla(Container deployment)
提供生产就绪的容器和部署工具来运行OpenStack云

- openstack charms(Juju Charms for OpenStack)
提供了快速，可重复的OpenStack部署，同时OpenStack服务之间的耦合也相应减弱。

- openstackansible
OpenStack-Ansible为OpenStack环境的部署和配置提供了可选的playbook和role。

- puppet openstack(Puppet Modules for OpenStack)
Puppet OpenStack模块为OpenStack云部署带来可扩展和可靠的IT自动化。

- tripleo(Deployment service)
TripleO是一个旨在使用OpenStack自己的云设施安装，升级和运行OpenStack云的项目，作为基于Nova，Ironic，Neutron和Heat的基础，以数据中心规模自动化云管理。

#### Application Services
- heat(Orchestration)
Heat是OpenStack的负责编排计划的主要项目。它可以基于模板来实现云环境中资源的初始化，依赖关系处理，部署等基本操作，也可以解决自动收缩,负载均衡等高级特性。基于模板文件对应用进行管理，在模板文件中可以定义应用需要的资源，资源可以包括多种类型（CFN以及HOT支持的资源类型可能会存在一定的差别）例如IP,网络，镜像，用户，实例等。定义资源的同时也可以指定资源之间的依赖关系，例如使用云硬盘创建创建一个实例时，可以指定在创建实例时必须要创建云硬盘。

- zaqar(Messaging Service)
Zaqar是面向网络和移动开发人员的多租户云消息传递和通知服务。该服务具有REST API，开发人员可以使用REST API通过使用各种通信模式在其SaaS和移动应用程序的各种组件之间发送消息。这个API的基础是一个高效的消息传递引擎，设计具有可扩展性和安全性。Websocket API也可用。
- murano(Application Catalog)
将应用程序目录与多功能工具相结合，以简化和加速包装和部署。它可以与OpenStack中几乎任何应用程序和服务一起使用。

- solum(Software Development Lifecycle Automation)
旨在使云服务更易于消费并集成到应用程序开发过程中

#### Monitoring & Metering
- ceilometer(Metering & Data Collection Service)
一种数据收集服务，提供标准化，并与工作正在进行，以支持未来的OpenStack组件在所有当前OpenStack的核心组件转换数据的能力。Ceilometer是遥测项目的组成部分。其数据可用于在所有OpenStack核心组件中提供客户计费，资源跟踪和报警功能。

- cloudkitty(Billing and chargebacks)
Rating As A Service,旨在将指标转换为价格

- monasca(Monitoring)
Monasca是与OpenStack集成的开源多租户，高度可扩展，性能优良，容错的监控即服务解决方案。它使用REST API进行高速指标处理和查询，并具有流式报警引擎和通知引擎。

- aodh(Alarming Service)
报警服务（aodh）项目提供了一项服务，可以根据定义的规则触发由Ceilometer或Gnocchi收集的公制或事件数据的操作。

- panko(Event, Metadata Indexing Service)
Panko项目是一个事件存储服务，提供存储和查询由Ceilometer生成的事件数据和潜在的其他来源的能力。Panko是遥测项目的组成部分。
