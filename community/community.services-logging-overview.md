---
layout: default
title: "HP Helion OpenStack&#174; Centralized Logging Overview"
permalink: /helion/community/services/logging/overview/
product: community

---
<!--PUBLISHED-->

<script>

function PageRefresh {
onLoad="window.refresh"
}

PageRefresh();

</script>


# HP Helion OpenStack&#174; Centralized Logging Overview

Because a typical HP Helion OpenStack cloud consists of multiple servers, locating a specific log from a specific server can be difficult.

The HP Helion OpenStack Centralized Logging feature collects logs on a central system, rather than leaving the logs scattered across the network. The administrator can use a single graphic interface to view log information in charts, graphs, tables, histograms, and other forms.

Centralized logging helps the administrator triage and troubleshoot the distributed HP Helion OpenStack cloud deployment from a single location. The admin is not required to access the several remote servers (SSH) to view the individual log files.

In addition to each of the [HP Helion services](/helion/community/services/overview/), Centralized Logging also processed logs for the following HP Helion OpenStack features:

- HAProxy
- syslog
- keepalived 

This document describes the Centralized Logging feature and contains the following sections:

* [Installation](install)
* [Centralized Logging components](components)
* [Centralized Logging log types](types)
* [Kibana configuration](kibana)
	* [Logging into Kibana](interface)
* [For more information](info)

## Installation ## {#install}

The Centralized Logging feature is automatically installed in the undercloud as part of the HP Helion OpenStack installation. 

No specific configuration is required to use Centralized Logging. However, you can tune or configure the individual components as needed for your environment. Tuning and configuring these components is outside the scope of this document. 

## Centralized Logging components ## {#components}

Centralized logging consists of several components, including on Logstash, Elasticsearch, RabbitMQ, and Kibana. 

* **Beaver** is a python daemon that takes information in log files and sends the content to RabbitMQ.

* **RabbitMQ** is a message broker for collection of logging data across nodes. 

* **logstash** is a log processing system for receiving, processing and outputting logs. logstash retrieves logs from RabbitMQ, processes and enriches the data, then stores the data in Elasticsearch.  

* **Elasticsearch** is data store offering fast indexing and querying.  

* **Kibana** is a client-side JavaScript application to visualize the data in Elasticsearch through a web browser. Kibana enables you to create charts and graphs using the log data. 

These components are configured to work out-of-the-box and the admin should be able to view log data using the default configurations.

At a high level, the Helion services forward logs to Beaver. Then, Beaver forwards JSON messages to RabbitMQ on the management controller node. Logstash connects to RabbitMQ to read queued messages and process the messages according to the Logstash configuration file. Logstash then forwards the processed log files in Elasticsearch. Users can use the Kibana interface to view and analyze the information, as shown in the following figure:

<img src="media/centrallogging75.png">


**Note:** Logging requires 4GB of disk space to make sure that all logging messages are retained. 


## Centralized Logging log types ## {#types}

The following table lists the types of logs collected by Centralized Logging and provides information on how the logs are maintained.

<table style="text-align: left; vertical-align: top; width:1000px;">
<tr style="background-color: #C8C8C8;">
<th>Data name</th><th>Confidentiality</th><th>Integrity</th><th>
Availability</th><th>Backup?</th><th>Description</th></tr>
<tr>
<td>Log records</td><td>Restricted</td><td>High</td><td>Medium</td><td>No</td><td>Log records have a limited life, and are not archived. The log file on the local filesystem provides a fallback source of logging data (up to 20GB or 45 days) if the logging system fails.</td></tr>
<tr>
<td>Log metadata</td><td>Restricted</td><td>High</td><td>Medium</td><td>No</td><td>Elasticsearch indexes logged data to allow flexible searching.</td></tr>
<tr>
<td>Credentials</td><td>Confidential</td><td>High</td><td>Medium</td><td>No</td><td>Credentials for access to Elasticsearch and RabbitMQ are stored in configuration files owned by root with mode 0600.</td></tr>
</table>

Log rotation will happen daily or when the current logfile reaches 2GB, whichever happens sooner. The number of rotations held will be balanced to attempt to cap logs from all services at 200GB. 



## Kibana configuration ## {#kibana}

You can use the Kibana dashboards to view log data. Kibana is a tool developed to create charts, graphs, tables, and histograms based on logs send to Elasticsearch by logstash. 

While creating Kibana dashboards is beyond the scope of this document, it is important to know that you can use the default Kibana dashboards or create custom dashboards. The dashboards are JSON files that you can modify or create new dashboards based on existing dashboards.

**Note:** Kibana is client-side software. To operate properly, the browser must be able to access port 81 on undercloud. 

### Logging into Kibana ## {#interface}

******WHAT IS THE URL******

Because centralized logging is separate service from the Horizon dashboards, there is separate username/password authentication. By default, the HP Horizon dashboard username and password will not work with Kibana.

After installation, the Kibana username and password are stored on the undercloud in the `/opt/kibana/htpasswd.cfg` file. 

## For more information ## {#info}

For information the centralized logging components, use the following links: 

* [Logstash](http://logstash.net/) 
* [Elasticsearch](http://www.elasticsearch.org/)
* [RabbitMQ](http://www.rabbitmq.com/)
* [Kibana Dashboard](http://www.elasticsearch.org/guide/en/kibana/current/_dashboard_schema.html)

 <a href="#top" style="padding:14px 0px 14px 0px; text-decoration: none;"> Return to Top &#8593; </a>

----

