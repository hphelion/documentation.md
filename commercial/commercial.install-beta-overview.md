---
layout: default
title: "HP Helion OpenStack Installation and Configuration"
permalink: /helion/openstack/install-beta-overview/
product: commercial

---


<script>

function PageRefresh {
onLoad="window.refresh"
}

PageRefresh();

</script>

<p style="font-size: small;"> <a href="/helion/openstack/support-matrix-beta/">&#9664; PREV</a> | <a href="/helion/openstack/">&#9650; UP</a> | <a href="/helion/openstack/install-beta/prereqs/">NEXT &#9654;</a> </p>

# HP Helion OpenStack Beta Installation and Configuration
This page provides an overview of the installation process and requirements for  HP Helion OpenStack beta &mdash; a baremetal multi-node deployment consisting of a minimum of 5 baremetal servers, to which you can add **up to 30 Compute nodes**:


* 1 undercloud
* 1 overcloud controller
* 2 overcloud Swift nodes 
* At least 1 overcloud Compute nodes 

HP Helion OpenStack beta uses three linked installation phases to deploy a complete OpenStack cloud. <a href ="https://wiki.openstack.org/wiki/TripleO">TripleO</a> simulates the deployment of OpenStack by creating and configuring baremetal servers to successfully run a cloud deployment. 

* Seed &mdash; The seed VM is started as a VM from a specific seed VM image. It contains a number of self-contained OpenStack components that are used to deploy the undercloud. The seed deploys the undercloud by using Ironic baremetal driver to deploy a specific undercloud machine image.

* Undercloud &mdash; In a typical HP Helion OpenStack beta deployment, the undercloud is a baremetal server. The undercloud is a complete OpenStack installation, which is then used to deploy the overcloud.

* Overcloud &mdash; The overcloud is the end-user OpenStack cloud. In a typical HP Helion OpenStack beta deployment, the overcloud comprises several baremetal servers.


## [Installing &amp; Configuring Your Cloud](/helion/openstack/install-beta-overview/)
Before you start your installation, we suggest you review these pages to understand requirements, pre-installation tasks, supported deployment scenarios, and required installations:

* [Support matrix](/helion/openstack/support-matrix-beta/) 
* [Before you begin](/helion/openstack/install-beta/prereqs/) 
* [Installing and configuring with a KVM hypervisor](/helion/openstack/install-beta/kvm)
* [Installing HP StoreVirtual VSA support](/helion/openstack/install-beta/vsa/)
* [Installing and configuring with an ESX hypervisor](/helion/openstack/install-beta/esx/)
* [Deploying and configuring OVSvApp for HP Virtual Cloud Networking (VCN) on ESX hosts](/helion/openstack/install-beta/ovsvapp/)
* [Installing and configuring DNSaaS support](/helion/openstack/install-beta/dnsaas/)

This rest of this page provides you with the following installation information:

* [Installation options](#installation-options)
* [Installation requirements](#installation-requirements)
   * [Hardware configuration](#hardware-configuration)
   * [Network configuration](#network-configuration)
* [For more information](#for-more-information)


### Installation options
With HP Helion OpenStack, you have two baremetal installation options depending on your system configuration.

* **KVM hypervisor with HP StoreVirtual VSA support**

    HP Helion OpenStack supports KVM hypervisor. With our KVM integration, you  can provision and manage an overcloud KVM cluster

    HP StoreVirtual VSA allows you to consolidate multiple storage nodes into pools of storage. The available capacity and performance is aggregated and made available to every volume in the cluster. 

    [Learn how to to install and configure with a KVM hypervisor](/helion/openstack/install-beta/kvm). 

* **ESX hypervisor with HP Virtual Cloud Networking (VCN) application support**

    HP Helion OpenStack supports VMWare ESX hypervisor. With our ESX integration, you can provision and manage an overcloud ESX cluster.

    HP Virtual Cloud Networking (VCN) application enables you to build a robust, multi-tenant networking infrastructure. Once deployed, the Open vSwitch vApp template enables networking between the tenant VMs provisioned on your ESX compute nodes.

    [Learn how to to install and configure with an ESX hypervisor](/helion/openstack/install-beta/esx/).  

After installing HP Helion OpenStack, you have the option to install HP Helion OpenStack DNS as a service (DNSaaS) support. No matter what hypervisor you use, our managed DNS service, based on the Openstack Designate project, is engineered to help you create, publish, and manage your DNS zones and records securely and efficiently to either a public or private DNS server network.

[Learn how to to install and configure DNSaaS support](/helion/openstack/install-beta/dnsaas/).  



### Installation requirements
These requirements pertain to both the KVM and ESX hypervisor baremetal installations. 

* [Hardware configuration](#hardware-configuration)
* [Network configuration](#network-configuration)

For the performance and stability of the HP Helion OpenStack environment, it is very important to meet the requirements and conform to the minimum recommendations. See the [Support matrix](/helion/openstack/support-matrix-beta) for additional information.

#### Hardware configuration

To install a HP Helion OpenStack baremetal multi-node configuration, you must have the following hardware configuration.

* At least 5 and up to 30 baremetal systems with the following configuration:

    * A minimum of 32 GB of physical memory
    * A minimum of 2 TB of disk space
    * A minimum of 1 x 10 GB NIC with PXE support

      * For systems with multiple NICs, the NICs must not be connected to the same Layer 2 network or VLAN.

    * Capable of hosting VMs
    * The boot order configured with Network/PXE boot as the first option
    * The BIOS configured: 
     
      * To the correct date and time
      * With only one network interface enabled for PXE/network boot and any additional interfaces should have PXE/network boot disabled
      * To stay powered off in the event of being shutdown rather than automatically restarting

    * Running the latest firmware recommended by the system vendor for all system components, including the BIOS, BMC firmware, disk controller firmware, drive firmware, network adapter firmware, and so on



* An installer system to run the baremetal install and host the seed VM with the following configuration:

    * A minimum of 8 GB of physical memory
    * A minimum of 100 GB of disk space
    * Virtualization enabled 
    * One of the following operating systems installed:

      * Ubuntu 13.10
      * Ubuntu 14.04

    
* **Important** 
    * **Installer system** &mdash; This system might be reconfigured during the installation process so a dedicated system is recommended. Reconfiguration might include installing additional software packages, and changes to the network or virtualisation configuration.
    
    * **Installer package** &mdash; The installer currently uses only the first available disk; servers with RAID controllers need to be pre-configured to present their storage as a single logical disk. RAID across multiple disks is strongly recommended for both performance and resilience.

    * **Physical servers** &mdash; When installing HP Helion OpenStack, it is your responsibility to track the physical location (slot number and rack) and associated identifiers (such as MAC addresses) for each physical server to aid in future hardware maintenance. This is necessary because when HP Helion OpenStack is installed on physical servers, the TripleO automation only tracks MAC network addresses of servers; the physical locations of servers are not tracked. This means there is no automated way to inform a service technician which slot or rack to go to when service is needed on a particular physical server. 

#### Network configuration

To ensure a successful installation, you must also satisfy these network configuration requirements:

* The seed VM, the baremetal systems and the IPMI controller for all systems must be on the same network

* Ensure network interfaces that are not used for PXE boot are disabled from BIOS to prevent PXE boot attempts from those devices.

* If you have other DHCP servers on the same network as your system, you must ensure that the DHCP server does not hand out IP addresses to your physical nodes as they PXE boot.

* The network interface intended as the bridge interface should be configured and working before running the installer. The installer creates a network bridge on the system running the installer, attaching the bridge interface to the network bridge. The installer uses the IP address of the bridge interface for the network bridge.


## For more information
For more information on HP Helion OpenStack Community, see:

* [Before you begin](/helion/openstack/install-beta/prereqs/) 
* [Support matrix](/helion/openstack/support-matrix-beta/) 
* [FAQ](/helion/openstack/faq/) 
* [Release notes](/helion/openstack/release-notes/) 

<a href="#top" style="padding:14px 0px 14px 0px; text-decoration: none;"> Return to Top &#8593; </a>