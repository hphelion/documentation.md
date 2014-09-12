---
layout: default
title: "HP Helion OpenStack&#174; Orchestration High Availability Service Overview"
permalink: /helion/openstack/ga/services/dvr/overview/
product: commercial.ga

---
<!--UNDER REVISION-->

<script>

function PageRefresh {
onLoad="window.refresh"
}

PageRefresh();

</script>


<p style="font-size: small;"> <a href="/helion/openstack/services/object/overview/">&#9664; PREV</a> | <a href="/helion/openstack/services/overview/">&#9650; UP</a> | <a href="/helion/openstack/services/reporting/overview/"> NEXT &#9654</a> </p>

# HP Helion OpenStack&#174; Distributed Virtual Routing (DVR) Overview and Configuration#

Distributed Virtual Routing (DVR) allows you to define connectivity among different VNS as well as connectivity between VNS hosts and the external network. 

Distributed virtual routing is achieved through a set of distributed virtual routers. Each tenant has its own distributed virtual router to define the connectivity among the VNSs under the same tenant. 


A distributed virtual router is conceptually a single entity, but it is implemented across all the OpenFlow switches in the network. There is no single routing instance running on a single machine/hypervisor that all the VNS traffic must route through. In addition to this there is a system-wide distributed virtual router which connects different tenant routers and defines the connectivity among different tenants and to the outside world. 

HP Helion OpenStack provides Distributed Virtual Routing to the cloud users. 


You need to modify the following files to configure and enable the DVR:



<**WHERE WILL THESE FILES BE LOCATED?? WHAT ARE THE SET OF COMMANDS THAT WILL TAKE THE USER TO THE LOCATION AND START EDIT PROCESS???? APART FROM THIS WHAT ELSE DO WE NEED TO ADD?**>



***<ml2_conf.ini>***

  This flag is enabled for the L2 Agent to address DVR rules.
	

	enable_distributed_routing = True


***<l3_agent.ini>***

Define the working mode for the agent. Allowed values are: **legacy**, **dvr**, **dvr_snat**.
The same l3-agent source runs on Compute nodes, Network nodes and Service nodes with different configurations.

	agent_mode = dvr
	

***<neutron.conf>***

To enable distributed routing this flag is enabled. It can be either **True** or **False**. If **False** is chosen, it works in the *Legacy mode*. If **True** is chosen, it works in the *DVR mode*.

	router_distributed = True

This is disabled by default.

**Note**: Normal user tenants will not be aware or able to control the type of routers being created (legacy/centralized or distributed). Only Cloud Admins can set the default or deploy specific types.

----
####OpenStack trademark attribution
*The OpenStack Word Mark and OpenStack Logo are either registered trademarks/service marks or trademarks/service marks of the OpenStack Foundation, in the United States and other countries and are used with the OpenStack Foundation's permission. We are not affiliated with, endorsed or sponsored by the OpenStack Foundation, or the OpenStack community.*






