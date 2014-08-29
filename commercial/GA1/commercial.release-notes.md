---
layout: default
title: "HP Helion OpenStack&#174; Release Notes"
permalink: /helion/openstack/ga/release-notes/
product: commercial

---
<!--UNDER REVISION-->


<script>

function PageRefresh {
onLoad="window.refresh"
}

PageRefresh();

</script>

<p style="font-size: small;"> <a href="/helion/openstack/">&#9664; PREV | <a href="/helion/openstack/">&#9650; UP</a> | <a href="/helion/openstack/faq/">NEXT &#9654; </a></p>

# HP Helion OpenStack&reg; Release Notes

Thanks for your interest in HP Helion OpenStack! This release note contains the following information on our software package:

* [Features in HP Helion OpenStack](#features) 
* [Recommendations for HP Helion OpenStack](#recommendations)
* [For further information](#for-further-information) 

##Features in HP Helion OpenStack## {#features}

**OpenStack Icehouse service** - The full set of standard OpenStack services is available for you to use, including Nova, Swift, Cinder, Neutron, Glance, Keystone, Horizon, and Heat.

**HP StoreVirtual VSA** - The VSA is used as the default fault-tolerant, software-defined storage backend for Cinder, and provides easy deployment on KVM nodes meeting on-demand scale-out requirements of cloud storage without sacrificing performance.

**Support for VMWare ESXi hypervisor** - Makes ESX host onboarding and management easier and lets you setup the ESX proxy node during installation of the overcloud. 

**HP Virtual Cloud Networking (VCN) application** - Allows you to create networks, subnets and ports, as well as security groups and security group rules for your ESX cluster.

##Recommendations for HP Helion OpenStack## {#recommendations}

This section describes the HP recommendations for optimizing performance, utility, and functionality with this software version. We have also provided information about some known issues for reference purposes. This section contains:

* [Best practices recommendations](#best-practices) for optimizing functionality, performance and reliability 
* [Known issues](#known-issues) in this release 

###Recommendations ### {#best-practices} 

HP recommends the following best practices for this software version.

**Installation**

* To ensure a trouble-free installation, we recommend against restarting the system running the installer and seed VM.  If you perform a restart, you may disrupt the baremetal bridge networking configuration and disable the undercloud and overcloud, requiring that you repeat the installation process.
* For the most reliable installation results, use a dedicated system, referred in this documentation as the *installation system* (also called the *KVM host*).  The installation system running the seed cloud is sometimes reconfigured during the installation process, which may include installing additional software packages and making changes to the network or visualization configuration.

	**Note:** The installation system is the system where Kernel-based Virtual Machine is installed.

* For best performance and resilience, if you are using servers with RAID controllers you must pre-configure them to present their storage as a single logical disk.
* As an aid for future hardware maintenance, we recommend that for your physical servers you track the physical location (slot number and rack) and associated identifiers (such as MAC addresses) for each  server.  The TripleO automation tracks only virtual MAC address and not physical server locations; recording the location information will speed future maintenance efforts on your physical devices.

**Operations**

* We recommend cleaning up any VMs using excess space, by executing the following commands:
    - Delete the KVM VMs and their storage volumes with the `virsh` command.
    - Delete the file `/tmp/seed_options`.
    - Uninstall any packages that you no longer require.

* For optimum VM operation and to avoid generating inaccurate error codes, we recommend you not reboot the overcloud controller but rather restart the applicable services by executing the following commands:
    - sudo service nova-compute restart
    - sudo service nova-scheduler restart
    - sudo service nova-conductor restart
    - sudo service neutron-openvswitch-agent restart

* After deleting a cloud or an entire HP Helion OpenStack Commercial installation, make sure you perform a full disk erasure on the associated storage devices to prevent exposure of sensitive data that might have been stored there. 	


###Known issues in this release### {#known-issues}

The following are the known issues for HP Helion OpenStack:

**Operations**

* If you determine that your VM seed has not started correctly when you execute the `hp_ced_start_seed` script, run the script a second time to ensure you start the seed.
* If for some reason the overcloud controller is rebooted, the VMs might be in an ERROR state. Execute the following commands to restart the services and remove the error(s):
  
		$ sudo service nova-compute restart
		$ sudo service nova-scheduler restart
		$ sudo service nova-conductor restart
		$ sudo service neutron-openvswitch-agent restart

* In some instances, the centralized logging feature does not function after the  product installation. If this occurs, repeat the following steps on the overcloud controller and all compute nodes

        service rsyslog restart
        

##For further information## {#for-further-information}

For additional information on HP Helion OpenStack, OpenStack Cloud Software and TripleO:

* [HP Helion OpenStack Documentation web site](/helion/openstack/): Provides the topics listed below.
* [FAQ](/helion/openstack/faq/): Contains some frequently asked questions about the HP Helion OpenStack
* [Installation and configuration](/helion/openstack/install-overview/): A set of tasks that walk you through the installation and configuration process.
* [Related Topics](/helion/openstack/related-links/): Links to pertinent HP Helion OpenStack and OpenStack documentation.
* [OpenStack documentation home page](http://docs.openstack.org/)
* [TripleO documentation](https://wiki.openstack.org/wiki/TripleO)
* [HP Support Center](http://us-support.external.hp.com/portal/site/hpsc/public): Report issues with HP Helion OpenStack.


###Legal Notices and Disclaimer### {#legal-notices-disclaimer}

<!--See the [HP Helion OpenStack Open Source and Third-Party Software License Agreements](/helion/openstack/commercial-3rd-party-license-agreements/).-->

Linux&reg; is the registered trademark of Linus Torvalds in the U.S. and other countries.

The OpenStack Word Mark is either a registered trademark/service mark or trademark/service mark of the OpenStack Foundation, in the United States and other countries and is used with the OpenStack Foundation's permission. We are not affiliated with, endorsed or sponsored by the OpenStack Foundation, or the OpenStack community.

**HP Helion OpenStack Warranty**: Linux hLinux 3.14.6-1-amd64-hlinux #1 SMP Mon Jun 9 11:35:36 MDT 2014 x86_64
The programs included with the hLinux system are free software; the exact license terms for each program are described in the individual files in /usr/share/doc/*/copyright. hLinux comes with ABSOLUTELY NO WARRANTY, to the extent permitted by applicable law.

<a href="#top" style="padding:14px 0px 14px 0px; text-decoration: none;"> Return to Top &#8593; </a>

----
####OpenStack trademark attribution
*The OpenStack Word Mark and OpenStack Logo are either registered trademarks/service marks or trademarks/service marks of the OpenStack Foundation, in the United States and other countries and are used with the OpenStack Foundation's permission. We are not affiliated with, endorsed or sponsored by the OpenStack Foundation, or the OpenStack community.*