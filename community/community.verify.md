---
layout: default
title: "HP Helion OpenStack&#174; Verifying your installation"
permalink: /helion/community/verify/
product: community

---
<!--PUBLISHED-->

<script>

function PageRefresh {
onLoad="window.refresh"
}

PageRefresh();

</script>

<!--
<p style="font-size: small;"> <a href="/helion/community/">&#9664; PREV</a> | <a href="/helion/community/">&#9650; UP</a> | <a href="/helion/community/install-overview/">NEXT &#9654;</a> </p>
-->

# HP Helion OpenStack&#174; Verifying your installation

Once your installation is complete, you should make sure you can connect to your HP Helion OpenStack Community cloud. You can accomplish this in any of the following ways:

* [Connecting to the undercloud Horizon console](#connectunder)
* [Connecting to the overcloud Horizon console](#connectover)
* [Connecting to demo VM](#connectvm)
* [Connecting to Monitoring UI](#connectmonitor)

### Connecting to the undercloud Horizon console ### {#connectunder}

From the seed cloud host, connect to the undercloud Horizon console.

1. Obtain the passwords for the `demo` and `admin` users 

	`cat /root/tripleo/tripleo-overcloud-passwords`

2. Point your web browser on the seed cloud host to the undercloud Horizon console using the `UNDERCLOUD_IP_ADDRESS` obtained after the install.

	If you did not retrieve the overcloud IP from the end of the install, enter the following command:

		. /root/tripleo/tripleo-undercloud-passwords
		TE_DATAFILE=/root/tripleo/ce_env.json . /root/tripleo/tripleo-incubator/undercloudrc
		OVERCLOUD_IP=$(heat output-show overcloud KeystoneURL | cut -d: -f2 | sed s,/,,g )
		echo $UNDERCLOUD_IP

4. Log in as `demo` or `admin` using the corresponding passwords obtained in step 1.

5. In the Horizon console, you can obtain the IP address of the demo VM:

		a. Click Project > Compute > Instance.
		b. Note the public IP address of the **demo** instance, starting with `192`.

### Connecting to the overcloud Horizon console ### {#connectover}



From the seed cloud host, connect to the overcloud Horizon console.

1. Obtain the passwords for the `demo` and `admin` users 

	`cat /root/work/tripleo/tripleo-overcloud-passwords`

2. Point your web browser on the seed cloud host to the overcloud Horizon console using the `OVERCLOUD_IP_ADDRESS` obtained after the instal.

	If you did not retrieve the overcloud IP from the end of the install, enter the following command:

		. /root/work/tripleo/tripleo-undercloud-passwords
		TE_DATAFILE=/root/work/tripleo/ce_env.json . /root/work/tripleo/tripleo-incubator/undercloudrc
		OVERCLOUD_IP=$(heat output-show overcloud KeystoneURL | cut -d: -f2 | sed s,/,,g )
		echo $OVERCLOUD_IP

4. Log in as `demo` or `admin` using the corresponding passwords obtained in step 1.

5. In the Horizon console, you can obtain the IP address of the demo VM:

		a. Click Project > Compute > Instance.
		b. Note the public IP address of the **demo** instance, starting with `192`.

### Connecting to the demo VM ### {#connectvm}

From the seed cloud host, you can connect to the demo VM using the following steps:

<!-- Maybe not needed per Chris Cannon
1. Export the overcloud passwords:

	`. /root/work/tripleo/tripleo-overcloud-passwords`

2. Export the overcloud users:

	`TE_DATAFILE=/root/work/tripleo/ce_env.json . /root/work/tripleo/tripleo-incubator/overcloudrc-user`

3. Verify you can view the nova instances:

	`nova list`

4. Assign the demo VM IP address to a variable:

	`DEMO_IP=$(nova list | grep " demo " | awk ' { print $13 } ')`
-->

5. Connect to the demo VM using the IP address you obtained from the Horizon console:

	`ssh debian@${DEMO_IP}`

	**Note:** It might take a few minutes for the demo vm to become available using ssh after finishing the installation.

	If the prompt changes to `debian@demo`, you have successfully connected to the demo VM.

6. Before proceeding, enter `exit` to disconnect from the demo VM.

### Connecting to the monitoring and logging interfaces ### {#connectmonitor}

HP Helion OpenStack Community includes monitoring logging. The monitoring service uses [Icinga](/helion/community/services/icinga/) interface and the logging service uses the Kibana interface. 


You can access these services with the following steps:

1. To access the undercloud monitoring console, launch a web browser on the seed cloud host to the following IP address, using the undercloud IP address from the end of the install:

		http://<undercloud IP>/icinga/

	**Example:**

		http://192.0.2.2/icinga

	If you did not retrieve the undercloud IP from the end of the install, enter the following command:

		. /root/stackrc
		UNDERCLOUD_IP=$(nova list | grep "undercloud" | awk ' { print $12 } ' | sed s/ctlplane=// )
		echo $UNDERCLOUD_IP

2. Log in with the user name `icingaadmin` and password `icingaadmin`.

3. To access the undercloud logging console, first obtain the Kibana password.

	a. From the seed cloud host log in to the undercloud as super user:

		ssh heat-admin@<undercloud IP> 
		sudo su - 

	b. Enter the following command to display the password:

		cat  /opt/kibana/htpasswd.cfg 

	Make note of the password.

4. Launch a web browser on the seed cloud host to the following IP address, using the undercloud IP address from the end of the install:

		http://<undercloud IP>:81 

	**Example:**

		http://192.0.2.2:81

5. Log in with the user name `kibana` and the password you obtained above.

## Next Step

* Review [OpenStack documentation](/helion/community/related-links/) 
* See how to [use the Horizon dashboard](/helion/community/dashboard/how-works/).


 <a href="#top" style="padding:14px 0px 14px 0px; text-decoration: none;"> Return to Top &#8593; </a>

----
