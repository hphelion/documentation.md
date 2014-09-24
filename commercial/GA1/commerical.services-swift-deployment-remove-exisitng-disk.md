---
layout: default
title: "HP Helion OpenStack&#174; Object Operations Service Overview"
permalink: /helion/openstack/ga/services/swift/deployment/remove-existing-disk/
product: commercial.ga

---
<!--UNDER REVISION-->

<script>

function PageRefresh {
onLoad="window.refresh"
}

PageRefresh();

</script>

<!--
<p style="font-size: small;"> <a href=" /helion/openstack/ga/services/object/overview/scale-out-swift/">&#9664; PREV</a> | <a href="/helion/openstack/services/overview/">&#9650; UP</a> | <a href="/helion/openstack/services/overview/"> NEXT &#9654</a> </p>-->


#Remove an existing Disk from Object Nodes


Perform the following steps to remove a disk from object nodes.


##Prerequisite

1. HP Helion OpenStack cloud is successfully deployed and has the following: 
	* Seed
	* Undercloud
	* Overcloud 
	* Two Swift nodes (which is functional)
2. Scale-out object-ring:1 is deployed


**IMPORTANT**:  
 
*  All of the rings generated must be stored at multiple location. These rings should be consistent all across nodes.

* It is recommended to take a backup of rings before any operation.


##Removing disks from ring

Perform the following steps to remove disks from ring:

1. Login to Undercloud 

		ssh heat-admin<Undercloud IP address> 
		#sudo -i

2. Change the directory to ring builder

		#cd /root/ring-building
<**how do we identify container ring file**>
3. List the disks in the current `object-1.builder` file

		ringos view-ring -f /root/ring-building/object-1.builder 

4. Identify the disk to be removed from the list.

**Recommendation**:

* Remove a drive gradually using a weighted approach to avoid degraded performance of Swift cluster. The weight will gradually decrease by 25% until it becomes 0%. Initial weight is 25.


5.Set weight of the driver 

		ringos set-weight -f object-1.builder -s d<value> -w <value>


6.Re-balance both account and container ring

		ringos rebalance-ring -f /root/ring-building/object-1.builder

**Note**: Wait for min&#095;part_hours before another re-balance succeeds.

7.List all the Swift nodes

		ringos list-swift-nodes -t all
		
		
8.Copy `object-1.ring.gz` file to all nodes

	ringos copy-ring -s /root/ring-building/account.ring.gz -n <IP address of Swift nodes>
	

9.Repeat steps from 5 - 8 with the weights 50, 25, and 0 (w= 50, 25, 0).

10.Once weight is set to 0, remove the disk from the ring

	ringos remove-disk-from-ring -f object-1.builder -s d<device ID>



 
<a href="#top" style="padding:14px 0px 14px 0px; text-decoration: none;"> Return to Top &#8593; </a>


*The OpenStack Word Mark and OpenStack Logo are either registered trademarks/service marks or trademarks/service marks of the OpenStack Foundation, in the United States and other countries and are used with the OpenStack Foundation's permission. We are not affiliated with, endorsed or sponsored by the OpenStack Foundation, or the OpenStack community.*