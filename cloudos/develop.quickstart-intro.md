---
layout: default
title: "API Quick Start - Introduction"
permalink: /cloudos/develop/quickstart-intro/
product: cloudos

---
<!--PUBLISHED-->

<script>

function PageRefresh {
onLoad="window.refresh"
}

PageRefresh();

</script>


<p style="font-size: small;"> <a href="/cloudos/develop/quickstart-intro">&#9664; PREV</a> | <a href="/cloudos/">&#9650; UP</a> | <a href="/cloudos/develop/quickstart-usecase1">NEXT &#9654;</a> </p>

# API Quick Start - Introduction

For developers, HP Cloud OS includes a REST API that allows you to interact programmatically with your cloud.  

This API Quick Start topic is designed to help you learn the API by introducing the HP Cloud OS services and walking through several use cases.  

On your Cloud Controller node, we have provided the HP Cloud OS API Documentation app. It's a framework for describing, producing, consuming, and 
visualizing the API. The app includes comprehensive reference API documentation and an interactive environment where you can try each 
request and view the response from your server. 

* [Prerequisites](#prerequisites)

* [What are the HP Cloud OS Services?](#what-are-the-hp-cloud-os-services)

* [What is the HP Cloud OS API Documentation app?](#what-is-the-hp-cloud-os-api-documentation-app)

* [API Documentation Ports per Service](#api-documentation-ports-per-service)

* [API Security Tokens](#api-security-tokens)

* [Next Step](#next-step) 

Also refer to the [OpenStack API documentation](http://docs.openstack.org). 

### Prerequisites

This topic assumes that you have already:

* Installed and set up HP Cloud OS, following the instructions in [Install &amp; Configure Your Cloud](/cloudos/install/).

* Used the HP Cloud OS Operational Dashboard to define the OS image and related components for your cloud.

* Launched an OS image on your cloud instance.

* Created a project.

If you have not yet set up a cloud, refer to [Install &amp; Configure Your Cloud](/cloudos/install/) for instructions.

**Note:** After a cloud environment has been created in the Operational Dashboard, you manage the cloud through the 
Administration Dashboard. The Administration Dashboard provides a simplified means of creating and managing cloud 
resources, as well as the ability to provision a composite topology described by topology designs.
            
For details about the user interface dialogs, click Help from the top banner of the dashboards, or see on this site:

* <a href="/cloudos/manage/operational-dashboard/">HP Cloud OS Operational Dashboard Help</a> &#8212; This document is a copy of the Help that's available in the HP Cloud OS Operational Dashboard.  

* <a href="/cloudos/manage/administration-dashboard/">HP Cloud OS Administration Dashboard Help</a> &#8212; This document is a copy of the Help that's available in the HP Cloud OS Administration Dashboard. 
         
To submit REST API calls, you will need your OpenStack Keystone v2 authentication credentials so you can generate security tokens. 
The steps are described in the [API Security Tokens](#api-security-tokens) section of this topic.
   
As you walk through the examples in this topic and submit REST calls, you'll need to provide the actual values that pertain to your cloud. We'll 
identify the properties you must modify in the REST calls, including some that are generated by prior calls and shown in the POST responses.
        
In call payloads, we recommend that you set the scope to the default <code>user-project</code> level, 
to prevent unintended impacts on other projects.

## What are the HP Cloud OS Services?

HP Cloud OS provides the following services:

* Eve - topology provisioning service

* Focus - topology document repository service

* Graffiti - resource pool registry and capability tagging service

Here's a summary about the HP Cloud OS services:

<table style="text-align: left; vertical-align: top;">

<tr style="background-color: #C8C8C8;">
<th> Service </th>
<th> Its Role in HP Cloud OS </th>
<th> Good to know... </th>
</tr>

<tr style="background-color: white; color: black;">
<td> Eve </td>
<td> <nobr> Infrastructure Topology Provisioning Service</nobr> and UI. Allows you to:

<ul>
<li>Provision TOSCA-based infrastructure topology templates (composite, hybrid, and distributed). </li>
<li>Separate template creation and resource pool binding, for simpler, reusable templates. </li>
<li>Integrate via a plug-in model with Cloud API's (e.g., OpenStack Nova) for orchestrating provisioning. </li>
<li>Create and configure network services such as virtual load-balancers. </li>
</ul>
</td>
<td> <nobr>TOSCA: A standard means to describe the topology of applications</nobr> along with dependent environments, services 
and artifacts inside a single service template. TOSCA standard enables each customer to deploy and manage topologies 
against the capabilities offered by any cloud provider, regardless of the customer's infrastructure or service model.  
<br /> <br />
About "jobs" : A job is created when the user submits a desired topology to Eve and requests provisioning based on it. Eve creates the job and 
publishes status updates for it. Once the provisioning is finished, the job is marked as completed. </td>
</tr>

<tr style="background-color: white; color: black;">
<td> Focus </td>
<td> As the Topology Template Registry and Repository Service, Focus provides for persistent store and management of TOSCA-based templates. Allows you to:
<ul>
<li> Search for documents based on criteria </li>
<li> Store new documents </li>
<li> Retrieve a version of an existing document </li>
<li> Update the contents and/or metadata of a document </li>
<li> Delete a document or a specific document version </li>
</ul>
</td>
<td> Document Types: 
<ul>
<li> resource_binding </li>
<li> ui_binding </li>
<li> application_type </li>
<li> infrastructure_type </li>
<li> platform_type </li>
<li> application_topology </li>
<li> infrastructure_topology </li>
<li> platform_topology </li>
</ul>
</td>
</tr>

<tr style="background-color: white; color: black;">
<td> Graffiti </td>
<td> Resource Pool Registry and Capability Tagging Service, providing:

<ul>
<li> A dictionary of the "capabilities" of all the resources in a cloud environment </li>
<li> A searchable directory to find cloud resources based on their capabilities </li>
<li> The mechanism for dynamic binding, allowing you to describe requirements rather than concrete bindings </li>
<li> The base concepts of requirements and capabilities within TOSCA </li>
</ul>

<br /> <br /> Graffiti allows you to:

<ul>
<li> Publish a Common Capability Ontology </li>
<li> Publish resources that are described by the Common Capability Ontology </li>
<li> Select resource pools </li>
<li> Search the Directory to find resource(s) based on required common capabilities </li>
<li> Provision a topology binding </li>
<li> Verify that the provisioning was successful in the cloud </li>
</ul>
</td>
<td> About resource pools: Consist of resources in a single administrative domain. As the Resource Pool Registry Service, 
Graffiti encapsulates a collection of descriptions about available resource pools. </td>
</tr>

</table>

## What is the HP Cloud OS API Documentation app?

The HP Cloud OS API Documentation app is a tool for describing, producing, and visualizing RESTful web services. 
The goal is to enable client and documentation systems to update at the same pace as the server. 
The documentation of methods, parameters, and models are tightly integrated into the server code, allowing APIs to stay in sync.

The app is defined in JSON files and is presented in a web interface. Here's an example of the app for the HP Cloud OS Focus service.

<img src="media/cloudos-rest-doc-focus-example1.png" />

The app provides developers who are learning a new REST API quick and easy access to:

* Each interface's URI syntax

* Whether there are any required or optional query parameters

* The data type of each parameter

* The model and its schema

* Interactive "Try it out!" dialogs that let you submit calls to your server, see the results, and figure out how you need to construct your REST URIs and any payloads.

For example, in the HP Cloud OS API Documentation app for Focus, having entered a known ID of a document, we can retrieve it from the 
repository with this <code> GET /1/document_list/1672048591 </code> call, using the "Try it out!" button:

<img src="media/cloudos-try-it-out1.png" />

After clicking "Try it out!", the app displays the response. In this case, the specified document was found:

<img src="media/cloudos-try-it-out2.png" />

The HP Cloud OS API Documentation includes other handy features, as you'll see when we walk through the use cases.

## API Documentation Ports per Service

Your HP Cloud OS instance comes installed with the HP Cloud OS API Documentation. Each service reserves a unique port number in the URL. 
Using a Google Chrome or Mozilla Firefox browser, you can open the HP Cloud OS API Documentation as follows. 
Just replace "my_server" with the DNS hostname or IP address of your Cloud Controller node:

* Eve: http://my_server:21051/?token=&lt;keystone-token-value>

* Focus: http://my_server:21061/?token=&lt;keystone-token-value>

* Graffiti: http://my_server:21071/?token=&lt;keystone-token-value>

## API Security Tokens

Before you can submit REST calls to your HP Cloud OS instance, or use the interactive features of the HP Cloud OS API Documentation app, 
you must generate a v2 security token from Keystone, which is an OpenStack service. If you skip this step, or if you have an expired token, 
you will receive a 401 error message in the response. 

<img src="media/cloudos-unauthorized-example.png" /> 

### Getting the Security Token from the HP Cloud OS Administration Dashboard

The easiest, "non-programmatic" way to get the security token is via the HP Cloud OS Administration Dashboard. Login and click Project > Access and Security > Access and Security > Authentication Token. The UI displays the current security token returned from Keystone. 

Copy the token's value and proceed to the next section of this topic.

### Getting the Security Token via a REST Call

To generate a v2 security token, use:

<pre>
POST http://&lt;keystone_Server>:5000/v2.0/tokens
</pre> 

In the Header, specify:

<pre>
content-type = application/json
accept = application/json
</pre>

In the POST payload, use the following JSON to get a security token. **The values shown here are just examples. 
You must provide your user name, password, project domain name, and project role name.**  
Based on the information provided, Keystone will verify the roles and, if valid, return the token.

<pre>
{
    "auth":{
        "passwordCredentials":{
            "username":"Admin",
            "password":"nottherealpassword"
        },
        "tenantName":"AdminProject"
    }
}

</pre>

> **Note:** In the current release, the Admin account uses a pre-defined password. However the password is not published in the web-hosted documentation. To get the credentials, refer to the readme file included in the same ZIP that contained the HP Cloud OS ISO. If you have not already done so, see the ZIP on the <a href="https://cloudos.hpwsportal.com" target="codn">HP Helion Distribution Network</a>. 

You can include the returned token's value on the <code> ?/token=&lt;value> </code> query parameter with each REST call. 

## Next Step

We are updating our API use case documentation &#8212; coming soon.

<!-- Proceed to the next API Quick Start topic, [Use Case 1: Define &amp; Provision a Topology](/cloudos/develop/quickstart-usecase1). --> 

<a href="#top" style="padding:14px 0px 14px 0px; text-decoration: none;"> Return to Top &#8593; </a>
 
