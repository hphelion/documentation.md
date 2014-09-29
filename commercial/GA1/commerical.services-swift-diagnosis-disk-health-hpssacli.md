---
layout: default
title: "HP Helion OpenStack&#174; Object Operations Service Overview"
permalink: /helion/openstack/ga/services/swift/diagnosis-disk-health/hpssacli/
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

#Diagnosis of disk health using hpssacli utility for HP servers

The health of the disk  of the HP servers can be diagnosed using hpsacli utility.


##HP Smart Storage Administrator CLI 2.0.22.0(HPSSACLI)

The HP Smart Storage Administrator CLI (HPSSACLI) is a commandline-based disk configuration program for Smart Array Controllers.

##Deployment of utility on servers

The hpssacli utility is deployed onto the servers wherever the disks are to be monitored. For example:  Swift, VSA, and Compute nodes. This page explains about the collection of diagnostic report of disks in the servers where the utility is loaded.

###Download the hpssacli utility into the KVM host

TBD

Where should the user login??




###Copy the utility to seed and to the servers where the disks has to be monitored

Use `scp` to copy the utility package on to the servers and install it.

1. Copy the package from KVM host to SEED.

		# scp hpssacli.tar.gz root@<IP address of Seed>

2. Copy the package from SEED to machine where the disks to be monitored.

		# scp hpssacli.tar.gz heat-admin@<IP address of machine>

3. Log in to the server where the utility is copied and install the package.

		# ssh heat-admin@<IP address of machine>

4. Change the directory

		# cd /home/heat-admin/
		
5. List the files

		# ls
	All the files present in that directory will be displayed.

6. Search `tar -xvf hpssacli.tar.gz`

7. Extract the tar file 

		 tar -xvf hpssacli.tar.gz

	The extracted binary file will be available in `/home/heat-admin/hp/hpssacli/bld`

8. Execute the binary file.

	 	 ./hpssacli help
 
	**Note**: The diagnostics can be done in the server or from SEED through ssh.

9. You can collect the diagnostic report of the controller either in server or through Seed using ssh.


**To collect the diagnostic report of the controllers in the server**
<!---
1. Log in to the server

		# ssh heat-admin@<IP address of machine>
2. Change the directory

		/home/heat-admin/hp/hpssacli/bld
--->

1. Enter the following command to know the controller slot
		
		./hpssacli ctrl all show status
 
The following sample displays slot details:

		Smart Array P420i in Slot 0 (Embedded)
		
		   Controller Status: OK
		
		   Cache Status: OK
		
		   Battery/Capacitor Status: OK

2.Generate the diagnostic report of the particular slot

		./hpssacli ctrl slot=(slot number) diag file=<filename.zip>
Or

Generate the report of all the slots 

		./hpssacli ctrl all diag file=<filename.zip>


The file will be in location you mentioned.

3.Copy the generated file to the desired location.

4.Extract the file

Or


**To generate diagnostic report using ssh from Seed**

<!---
1. Log in to the server

		ssh heat-admin@<Machine IP address>

2. Change the directory

		/home/heat-admin/hp/hpssacli/bld
--->

3. Generate the diagnostic report of the particular slot

		 ssh heat-admin@<Machine IP address> "sudo /home/heat-admin/hp/hpssacli/bld/hpssacli ctrl slot=<slot number> diag file=all_details

4. Copy the report from the server to Seed Vm

 		# scp heat-admin@<Machine IP address>:/home/heat-admin/<filename.zip> 

5. Copy the report <filename.zip> to the KVM_host.
	
		# scp all_details.zip ubuntu@<KVM_Host IP address>:

6. Enter login credentails ???

5. Extract the file.


10.Open the `ADUReport.htm` file in the browser. The html page displays complete details of the controller and the health status of the physical disks available in the machine.

<image = utility_ADUR-report>

11.Generate SmartSSD Wear Gauge Report either from local server or Seed

* From local server

		/home/heat-admin/hp/hpssacli/bld# ./hpssacli ctrl slot=<slot number> diag file=<filename.zip> ssdrpt=on
* From Seed

		# ssh heat-admin@<machine IP address> "sudo /home/heat-admin/hp/hpssacli/bld/hpssacli ctrl slot=<slot number> diag file=<filename.zip>

**Now retrieve the ssd_report.zip to kvm host using scp from server to analyse.??? is this applicable for seed only??** 

The following SmartSSDWearGaugeReport.txt sample file displays the details of the  SSD drives. 

	
	ADU Version                             2.0.22.0
	
	Diagnostic Module Version               8.0.22.0
	
	Time Generated                          Tuesday September 16, 2014 12:26:57PM
	
	Device Summary:
	
	   Smart Array P420i in Embedded Slot
	
	Report for Smart Array P420i in Embedded Slot
	
	---------------------------------------------
	
	Smart Array P420i in Embedded Slot : Internal Drive Cage at Port 2I : Box 2 : Physical Drive (400 GB SAS SSD) 2I:2:6 : SmartSSD Wear Gauge
	
	   Status                               OK
	
	   Supported                            TRUE
	
	   Log Full                             FALSE
	
	   Utilization                          0.000000
	
	   Power On Hours                       1798
	
	   Has Smart Trip SSD Wearout           FALSE
	
	Smart Array P420i in Embedded Slot : Internal Drive Cage at Port 2I : Box 2 : Physical Drive (400 GB SAS SSD) 2I:2:7 : SmartSSD Wear Gauge
	
	   Status                               OK
	
	   Supported                            TRUE
	
	   Log Full                             FALSE
	
	   Utilization                          0.000000
	
	   Power On Hours                       1797
	
	   Has Smart Trip SSD Wearout           FALSE
	
	Smart Array P420i in Embedded Slot : Internal Drive Cage at Port 2I : Box 2 : Physical Drive (400 GB SAS SSD) 2I:2:8 : SmartSSD Wear Gauge
	
	   Status                               OK
	
	   Supported                            TRUE
	
	   Log Full                             FALSE
	
	   Utilization                          0.000000
	
	   Power On Hours                       1798
	
	   Has Smart Trip SSD Wearout           FALSE



##Other useful commands

1. To get the details of the physical disks and also the logical group status on the server

        ssh heat-admin@<machine IP address> "sudo /home/heat-admin/hp/hpssacli/bld/hpssacli ctrl slot=<slot number> show config detail"

 

2. To execute other operations on physical drives

        ssh heat-admin@<machine IP address> "sudo /home/heat-admin/hp/hpssacli/bld/hpssacli help physicaldrive"

3. Use help to explore other options with the utility

        ssh heat-admin@<machine IP address> "sudo /home/heat-admin/hp/hpssacli/bld/hpssacli help"
 
<a href="#top" style="padding:14px 0px 14px 0px; text-decoration: none;"> Return to Top &#8593; </a>


*The OpenStack Word Mark and OpenStack Logo are either registered trademarks/service marks or trademarks/service marks of the OpenStack Foundation, in the United States and other countries and are used with the OpenStack Foundation's permission. We are not affiliated with, endorsed or sponsored by the OpenStack Foundation, or the OpenStack community.*