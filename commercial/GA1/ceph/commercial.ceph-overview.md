---
layout: default
title: "HP Helion OpenStack&#174; Edition: HP Helion Ceph"
permalink: /helion/openstack/services/ceph/
product: commercial

---
<!--UNDER REVISION-->


<script>

function PageRefresh {
onLoad="window.refresh"
}

PageRefresh();

</script>
<!--
<p style="font-size: small;"> <a href="/helion/openstack/install-beta/kvm/">&#9664; PREV</a> | <a href="/helion/openstack/install-beta-overview/">&#9650; UP</a> | <a href="/helion/openstack/install-beta/esx/">NEXT &#9654;</a> </p>
-->


# HP Helion OpenStack&#174; Enterprise Edition 1.1 Ceph Firefly 80.7 Storage Solution 


The HP Helion OpenStack Enterprise Edition 1.1 Ceph Storage Solution provides an unified scaleable and stable storage solution for the management of Helion OpenStack Image service (Glance), Compute (Nova Boot Volumes) service, and Volume Storage (Cinder persistent Volumes) service. The solution also supports user backup and archive workloads to the Object Storage (Swift) API service writing to the same unified Ceph storage platform. 

This guide is focuses on post installation, configuration and integration between HP Helion OpenStack &#174;: Enterprise Edition 1.1 and Ceph Firefly 80.7 running on the hlinux kernel 3.14-6.2.


This guide assumes that you are familiar with the concepts of OpenStack and Ceph. The main purpose of this guide is describe the integration of Ceph Block Storage with HP Helion OpenStack&#174;: 1.1, detail steps to install dependencies, configure HP Helion OpenStack and Ceph Firefly, and provide  troubleshooting guidance.

<!---Although installation steps are outlined, these are mostly as validity checks for dependencies. Most Enterprise Customers should have HP size and assist with the installation of HP Helion OpenStack Enterprise Edition 1.1, and Inktank size and assist with the installation of Ceph Firefly 80.7. --->


##Ceph Overview

Ceph is an Open Source, scalable, software-defined storage system running on HP servers comprised of a  [block storage](#block-storage), [object storage](#object-storage) and [file system](#file-system) with a unified management. <!---HP is committed to contribute to OpenStack integration with  management and extensions to Ceph Open Source Storage as a Solution.--->

Ceph is designed to deliver different types of storage interfaces to the end user in the same storage platform. The integration  of HP Helion OpenStack&#174; Enterprise Edition and Ceph is the usage of [block storage](#block-storage) of Ceph's RADOS Block Device (RDB). Also, the integration between User Application Archive and Backup Workloads running externally or in Virtual Machines in HP Helion OpenStack is the usage of [object storage](#object-storage) of the Ceph RADOS Gateway (RADOSGW).

The Ceph RADOSGW provides a REST interface with extensions which offers compatibility with the Swift API. Therefore the existing applications with the integration to the HP Helion OpenStack Swift API can port seamlessly from a OpenStack Swift backend storage platform to the Ceph Solution.


###Understading Block Storage {#block-storage}

The Ceph Block Device is a network-attached device, which is introduced to the HP Helion OpenStack Controller and Compute Nodes for Glance, Nova and Cinder storage utilization, or, in order to meet unique SLAs, to the Virtual Machines in the KVM Helion OpenStack managed hypervisor. Glance can be configured with either Ceph object or block storage. Hence, HP has followed Ceph's best practices and is limiting support for Glance storage of images to block storage. This allows for the support of Ceph's Clone and Copy on Write Feature (COW).


###Understanding Object Storage {#object-storage}

The Ceph RADOSGW is a REST API which supports the Swift API. HP Helion OpenStack supports object storage primarily for user workloads, ranging from COTS applications running in Virtual Machines (such as MySQL which frequently needs to archive tar files to a reliable and resilient archive) to custom LOB Solutions (which require frequent and aggressive snapshots orchestrated in a consistency group across many VMs and external baremetal systems).


###File System {#file-system} 
**[supported?]**

Ceph provides a POSIX-compliant network file system that aims for high performance, large data storage and so on. **[However file system interface is still not production ready as of this writing (December 2014)**. In this release we are not supporting the File Interface. There is an alternative way suggested, that is, either leveraging Ceph's RBD Block storage, Ceph's RADOSGW Object Storage through Swift API, or, for user archive and backup type workloads, Open Source Swift API-compatible File System interfaces such as Duplicity.

###Understanding the Ceph Cluster

The Ceph Object Storage Daemon (OSD) stores data, handles data replication, recovery, rebalancing, and provides information to Ceph monitors by verifying other Ceph OSD Daemons for a heartbeat. It is best to maintain three OSD Servers to take into account the default three replicas. The number of OSD Daemons per Server is a part of the sizing exercise unique to each customer. For an example, a stable configuration is nine OSD servers with 12 2TB disks per server. Other examples are nine OSD servers with 25 4TB disks per server such as the HP SL4540.

The Ceph Monitor maintains a master copy of the Ceph Cluster Map including the OSD map, monitor map, placement group (PG) map, and the CRUSH map. Ceph maintains a history called an "epoch" of each state change in the Ceph monitors, Ceph OSD daemons, and placement groups. It is best to maintain in Production three Ceph Monitors (if one Monitor fails, the System will still be operational). If you need additional monitors, upgrade the number of Monitors in increments of odd numbers, such as five Ceph monitors. Server types range from HP SL230s to HP DL360s.


The following diagram shows communication from Helion OpenStack to the Ceph Cluster.


<img src="media/communication_diagram_from_helion_ceph.png"/)>


###Understanding the Ceph Object Gateway

The Ceph object gateway is built on `librgw` to provide applications with a RESTful gateway to Ceph storage clusters. The Ceph object storage supports the following two interfaces:

1. **Swift-compatible**: Provides object storage functionality with a large subset of the OpenStack Swift API. HP supports this interface and 3rd-party integrations with the Swift API.

2. **S3-compatible**: Provides object storage functionality with a large subset of the Amazon S3 RESTful API. This functionality is not support in this release <!---This is not supported by HP as part of the Solution, but it has passed minimal API testing.--->

The Ceph RADOSGW daemon (radosgw) is a fastCGI module for interacting with a Ceph storage cluster. The Ceph object gateway can store data in the same Ceph storage cluster used 
to store data from Ceph block device clients or from Ceph file system clients. Ceph maintains its own user authentication and allows for extension to HP Helion OpenStack Keystone Authentication. For User Archive and Backup workloads originating from HP Helion OpenStack Tenant Project Virtual Machines, HP recommends integrating through Keystone as a consistent authentication model.


##Understanding the HP Helion OpenStack to Ceph Storage Reference Architecture

Ceph cluster includes one admin node, one monitor node (MON) and three object storage daemons (OSDs). The monitor node can also coexist on the same Host as one of the OSDs. Administrative and control operations are typically issued from the admin node. There is also a Metadata Server node MDS) which stores metadata on behalf of the Ceph Filesystem. Ceph Block Devices and Ceph Object Storage do not use MDS. Since the Ceph Filesystem interface is not supported in this release of Helion OpenStack, there is no requirement for a MDS node.


<img src="media/helion-openstack-ceph-storage-reference-architecture.png"/)>

<img src="media/logical-referrence-architecture-for-ceph-cluster.png"/)>


##Integrating Ceph Block Storage with HP Helion OpenStack

The following sections provides details on the integration of Ceph Block Storage with HP Helion OpenStack:


1. [HP Helion OpenStack Enterprise Edition 1.1 Ceph Firefly 80.7 Storage Solution: Prerequisites]( /helion/openstack/ceph/prerequisite/)
2. [HP  Helion OpenStack Installation](/helion/openstack/install/overview/)
3. [Ceph Manual Installation](/helion/openstack/ceph-manual-install/)
4. [CEPHX Authentication]( /helion/openstack/ceph-authentications/)
5. [HP Helion OpenStack Ceph Configuration]( /helion/openstack/ceph-hp-helion-openstack-ceph-configuration/)
7. [Helion OpenStack Glance-Ceph Interoperability]( /helion/openstack/ceph-hp-helion-openstack-glance-ceph-interoperability/)
8. [Helion OpenStack Cinder Ceph Storage]( /helion/openstack/ceph-hp-helion-openstack-cinder-ceph-storage)
9. [Helion OpenStack Nova Ceph Storage]( /helion/openstack/ceph-helion-openstack-nova-ceph-storage/)
10. [Ceph Rados Gateway]( /helion/openstack/ceph-rados-gateway/)
11. [Ceph RADOS Gateway- Keystone Authentication](/helion/openstack/ceph-rados-gateway-keystone-authentication/)
12. [Ceph RADOS Gateway - DMZ HAProxy](/helion/openstack/ceph-rados-gateway-dmz-ha-proxy/)
12. [HP Helion OpenStack Ceph Storage Administration services](/helion/openstack/ceph-helion-openstack-ceph-storage-administration-services/)
13. [Ceph Configuration Recovery and Backup Procedure HP Helion Nodes](/helion/openstack/ceph-configuration-recovery-and-backup-procedure-HP-Helion-nodes/)
14. [Related Topics](/helion/openstack/ceph-related-topics/)









 
## Next Steps

[HP  Helion OpenStack Installation](/helion/openstack/install/overview/).
 

<a href="#top" style="padding:14px 0px 14px 0px; text-decoration: none;"> Return to Top &#8593; </a>