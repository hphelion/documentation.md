---
layout: public-cloud
title: "Release Notes for the HP Helion Public Cloud CLI Software for Windows PowerShell"
permalink: /publiccloud/cli/windows/release-notes/
product: win-2-cli

---
<!--PUBLISHED-->
# Release Notes for the HP Helion Public Cloud CLI Software for Windows PowerShell

These are the release notes for the HP Helion Public Cloud CLI software for Windows PowerShell.  The current release number for the [HP Helion Public Cloud CLI software for Windows PowerShell](/cli/windows) is version 1.3.5.7, released on 11/15/2013.  These release notes contain the following information:

* [Release 1.3.3.9 Features](#v1339)
* [Release 1.3.1.9 Features](#v1319)
* [Release 1.3.0.6 Features](#v1306)
* [Release 1.3.0.1 Features](#v1301)
* [Release 1.2.0.6 Features](#v1206)
* [Release 1.2.0.1 Features](#v1201)

[Please download the latest version of the HP Helion Public Cloud CLI software for Windows PowerShell (Version 1.3.5.7)](/file/WinCLI-1.3.5.7.zip)!

<!--##Release 1.3.5.7 Features## {#v1357}

This release was made available on 11/15/2013 and contains the following new features:

* List

##Known Issues##

* List
-->


##Release 1.3.3.9 Features## {#v1339}

This release was made available on 4/22/2013 and contains the following new features:

* Support for automatic update checking when opening the shell.
* Added cmdlets `[New-Container](/publiccloud/cli/windows/reference#New-Container)` and `[Remove-Container](/publiccloud/cli/windows/reference#Remove-Container)` to add and remove containers owned by other users to your list of available containers.
* Support for file uploads for files greater than 1 gigbyte in size has been added.
* The `Connect-Server` cmdlet now works with non-Windows instances in conjunction with *putty.exe*.
* New argument for `Get-Zones` cmdlet. Adding `-v` will return more details about each zone including containers in that zone and available services. Example: `get-zones -v`.
* The `Set-Zone` cmdlet shows more details about the newly selected zone including containers in that zone and the available services.

##Known Issues##

* **Resolved**: `copy` command now displays the correct file copy details.
* **Resolved**: After setting the zone color, the key pair content now displays with the correct color when creating a new key pair with the `New-KeyPair` command.
* **Resolved**: You are now informed when drive permissions are revoked or changed during exception handling. 
* **Resolved**: You can now add a new security group rule to an existing security group.
* **Resolved**: The server keyname is now displayed when you list the available servers.-->


##Release 1.3.1.9 Features## {#v1319}

This release was made available on 10/29/2012 and contains the following new features:

* Support for temporary URLs via new attributes for the cmdlet `[get-uri](/publiccloud/cli/windows/reference#ObjectStorage)`.
* Support for wild cards for Object-Storage with the commands, `[ls, del, copy](/publiccloud/cli/windows/reference#ObjectStorage)`.
* Support for [Windows images](/cli/windows/compute#CreateanImageofaServer)
* Added cmdlet `[connect-server](/publiccloud/cli/windows/reference#Compute)` to quick-connect to Windows Instances
* Added cmdlet `[get-limits](/publiccloud/cli/windows/reference#Compute)` to show Compute quota information
* Added cmdlet `[ping-server](/publiccloud/cli/windows/reference#Compute)` to ping a server.
* Added cmdlet `[get-password](/cli/windows/reference#Compute)` for recovering of Administrator password for Windows Instances
* Support for syncing Windows Administrator password for cmdlet `[reset-password](/publiccloud/cli/windows/reference#Compute)`

###Known Issues###

* **Issue**: You cannot add a new security group rule to an existing security group.  
    *WorkAround*: Use the 
[HP Helion Public Cloud Console](https://horizon.hpcloud.com/) to make this change.

##Release 1.3.0.6 Features## {#v1306}

This release was made available on 9/6/2012 and contains the following new features:

* Support for changing the color assigned to an availability zone.

###Known Issues###

None this release.

##Release 1.3.0.1 Features## {#v1301}

This release was made available on 8/28/2012 and contains the following new features:

* Support for listing, adding, attaching, detaching and removing block volumes
* Support for creating and removing block storage snapshots

###Known Issues###

* **Resolved**: You can now access the keypair information from the command-line interface (CLI).

##Release 1.2.0.6 Features## {#v1206}

###New Features###

* *Migrate-Drive*: This object storage command gives you the capability to migrate existing stores of data from external sources like S3, Dropbox and Skydrive to a target Container within HP Object Storage, saving you the trouble of having to do it by hand or (worse) one file at a time.  Take a look at [Travis' blog post](http://h30529.www3.hp.com/t5/HP-Scaling-the-Cloud-Blog/Migrating-your-files-with-the-Windows-CLI/ba-p/523) on this feature for more info.

###Known Issues###

None this release.

##Release 1.2.0.1 Information## {#v1201}

###New Features###

* *Nova compute*: Full access to the OpenStack Nova compute infrastructure via the Windows command line, allowing you to manage and manipulate servers, flavors, images, keypairs, security groups, and so on.
* *CDN*: Support through the CLI for the HP Helion Public Cloud Services content delivery network (CDN), providing you with distributed caching of object storage objects on geographically dispersed caching servers. Copies of the object are stored at a location more convenient to the end user system, allowing for better performance.

###Known Issues###

* **Issue**: You cannot access the keypair information from the command-line interface (CLI).  
    *WorkAround*: Access the keypair information through the management console.
