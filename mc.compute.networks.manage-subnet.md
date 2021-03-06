---
layout: default
title: "Management console: Managing a subnet"
permalink: /mc/compute/networks/manage-subnet/
product: mc-compute
published: false

---
<!--PUBLISHED-->
# Management console: Managing a subnet

This page covers how to create and delete a subnet using the networks screen of the management console (MC).  This page covers the following topics:

* [Before you begin](#Overview)
* [Creating a subnet](#Creating)
* [Editing a subnet](#Editing)
* [Enabling and disabling DHCP](#DHCP)
* [Deleting a subnet](#Deleting)
* [For further information](#ForFurtherInformation)


##Before you begin## {#Overview}

Before you can create or delete a subnet of an existing network, you must:

* [Sign up for an HP Helion Public Cloud compute account](https://horizon.hpcloud.com/register)
* [Activate compute service on your account](https://horizon.hpcloud.com/landing/)
* [Create a network](/mc/compute/networks/create-network#Creating/)


##Creating a subnet## {#Creating}

By default, when you [create a network](/mc/compute/networks/create-network/), that network is created with no subnets.  

<img src="media/compute-networks05.jpg" width="580" alt="" />

To create a subnet, in the `Manage` column, select the `Options` button for the network for which you want to create a subnet and select `Create Subnet`:

<img src="media/compute-networks06.jpg" width="580" alt="" />

This launches the subnet creation screen.  

<img src="media/create-subnet.png" width="580" alt="" />

In the subnet creation screen, select your desired values for the following fields:

* A name for your subnet in the `Name` text-entry field
* The network address for the subnet in the `Network Address` field
* A Gateway IP address in the `Gateway IP` field (this is optional; if you want to assign a specific Gateway IP rather than use the default)
* `DNS Nameservers` addresses (if desired)
* `Allocation Pools` starting and ending IP addresses (if desired)
* `Host Routes` destination and next hop addresses (if desired)

Click the `Create Subnet` button.  You are returned to the networks screen and your new subnet is displayed in the `Subnets` column:

<img src="media/compute-networks06.jpg" width="580" alt="" />

You can also create a subnet from the [network details](/mc/compute/networks/view-network/) screen.

<img src="media/compute-networks10.jpg" width="580" alt="" />

In the network details screen, just click the `Create Subnet` button.  This launches the subnet creation screen as outlined above; follow the same process from there.


<!--##Advanced subnet options## {#Advanced}

You can use the [subnet creation screen](#Creating) to add other options to your subnet.  This pane is contracted by default; just click the `Advanced Options` button:

<img src="media/advanced-subnet-options.jpg" width="580" alt="" />

You can set the following options via the Advanced Options pane of the create subnet screen:

* [Allocation Pools](#AllocationPools)
* [DNS Nameservers](#DNSNameservers)
* [Host Routes](#HostRoutes)

**Note**:  CIDR and allocation pools are immutable.


###Allocation Pools### {#AllocationPools}

To create an allocation pool in the [subnet creation screen](#Creating), enter the `Start IP` and `End IP` values you want for your subnet in the text entry fields.  If you want to create more than one allocation pool, click the `+Add more` button and additional text entry fields are added.

<img src="media/allocation-pools.jpg" width="580" alt="" />

###DNS Nameservers### {#DNSNameservers}

To create a DNS Nameserver in the [subnet creation screen](#Creating), enter the `IP Address` you want for your subnet in the text entry field.  If you want to create more than one DNS nameserver, click the `+Add more` button and additional text entry fields are added.

<img src="media/dns-nameservers.jpg" width="580" alt="" />

###Host Routes### {#HostRoutes}

To create host routes in the [subnet creation screen](#Creating), enter the `Destination CIDR` and `Next Hop` you want for your subnet in the text entry fields.  If you want to create more than one host route, click the `+Add more` button and additional text entry fields are added.

<img src="media/host-routes.jpg" width="580" alt="" /> -->


##Editing a subnet## {#Editing}

To modify details of your subnet (such as the name, gateway IP address, host routes, and so on), you can use the edit subnet details screen.  There are 

From the main networks screen, click the name of the subnet you want to modify in the `Subnets` column of the `Networks` list:

<img src="media/choose-subnet.jpg" width="580" alt="" />

This launches the subnet details screen.  In the subnet details screen, click the `Edit Subnet` button:

<img src="media/subnet-details.jpg" width="580" alt="" />

In the edit subnet screen, edit the detail you want to edit and then click the `Update Subnet` button to save your changes.

<img src="media/edit-subnet-details.jpg" width="580" alt="" />


##Enabling and disabling DHCP## {#DHCP} 

Dynamic host configuration protocol (DHCP) is enabled on your subnet by default.  To disable DHCP for your subnet, in the main networks screen, click the name of the subnet you want to modify in the `Subnets` column of the `Networks` list:

<img src="media/choose-subnet.jpg" width="580" alt="" />

This launches the subnet details screen.  In the subnet details screen, click the `Disable DHCP` button.  DHCP is now disabled on your subnet

If you have at some point disabled DHCP and want to re-enable, click the name of the subnet you want to modify in the `Subnets` column of the `Networks` list:

<img src="media/choose-subnet.jpg" width="580" alt="" />

This launches the subnet details screen.  In the subnet details screen, click the `Enable DHCP` button.  DHCP is now enabled on your subnet


##Deleting a subnet## {#Deleting}

By default, when you [create a network](/mc/compute/networks/create-network/), that network is created with no subnets. If you used the subnet creation screen [create a subnet](#Creating) to create a subnet that you now want to delete, click the subnet you want to delete:

<img src="media/compute-networks07.jpg" width="580" alt="" />

This launches the subnet information screen.

<img src="media/compute-networks08.jpg" width="580" alt="" />

To delete the subnet, click the `Delete Subnet` button.  A verification window appears asking if you want to delete this network:

<img src="media/delete-verify-subnet.jpg" width="580" alt="" />

Select the button `Yes, delete this subnet`.  Your subnet is deleted and no longer appears in the list in the `Subnets` column on the [networks screen](/mc/compute/networks/).

You can also launch the subnet details screen from the [network details](/mc/compute/networks/view-network/) screen.

<img src="media/compute-networks10.jpg" width="580" alt="" />

Just click the subnet name for the subnet you want to delete, and then follow the process outlined above. 


##For further information## {#ForFurtherInformation}

* For information about the subnet details screen, take a look at the [Viewing subnet details](/mc/compute/networks/view-subnet/) page
* For basic information about our HP Helion Public Cloud compute services, take a look at the [HP Helion Public Cloud compute overview](/compute/) page
* Use the MC [site map](/mc/sitemap) for a full list of all available MC documentation pages
* For information about the Open Stack networking features, surf on over to [their networking wiki](https://wiki.openstack.org/wiki/Quantum)
