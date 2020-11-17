---

copyright:
  years: 2020
lastupdated: "2020-11-05"

keywords: topology, topologies, products, websphere, liberty, vsi

subcollection: was-for-vsi

---

{:shortdesc: .shortdesc}
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:term: .term}

# Topologies
{: #topologies}

Use {{site.data.keyword.was4vsi}} to install a {{site.data.keyword.appserver_short}} traditional or Liberty environment on a virtual server instance (VSI) on {{site.data.keyword.cloud}}.
{: shortdesc}

## Terminology
{: #terminology}

This documentation refers to {{site.data.keyword.appserver_short}} and IBM Liberty as ***WebSphere*** or ***WAS***.

### Single server
{: #singles}

{{site.data.keyword.appserver_short}} traditional (WAS.Base) and {{site.data.keyword.appserver_short}} Liberty (Liberty.Base) provide single servers. For single server installation, the VSI is referred to as ***single server VSI*** or ***primary VSI***.

### Multiple servers
{: #multiples}

{{site.data.keyword.appserver_short}} Network Deployment traditional (WAS.Cell) and {{site.data.keyword.appserver_short}} Network Deployment Liberty (Liberty.Collective) provide multiple servers.
- For WAS.Cell installation, the deployment manager VSI is referred to as ***primary VSI*** and custom node VSIs are referred to as ***secondary VSIs***.
- For Liberty.Collective, the Liberty controller VSI is referred to as ***primary VSI*** and Liberty host VSIs are referred to as ***secondary VSIs***.

IBM HTTP Server also is provided on the primary VSI for the multiple server installations.

## Supported topologies
{: #supported}

{{site.data.keyword.was4vsi}} offers the following topologies for single and multiple server installations.


| Topology                  | Primary VSI | Secondary VSI | Installation time|
|---------------------------|------------------|-----------------------|------------------------|
| WAS.Base | WAS base | N/A | 17 mins|
| WAS.Cell | DMgr + IBM HTTP Server |  Custom node(s) | 27 mins|
| Liberty.Base | Liberty base | N/A | 17 mins|
| Liberty.Collective | Liberty controller + IBM HTTP Server | Collective host(s) | 27 mins|
{: caption="Table 1. TopologiesLiberty to WebSphere version mapping" caption-side="top"}

* Installation time varies depending on the VSI capacity, network connection, and number of secondary VSIs.  
* You must not have any previous installation of WebSphere on any VSI.  
* Installing multiple topologies on a VSI is not supported.  
* After initial installation, you cannot use the steps in [Deploying {{site.data.keyword.was4vsi_notm}}](/docs/was-for-vsi?topic=was-for-vsi-getting-started) to add additional secondary VSIs. You can still add them directly using WebSphere installation procedures.
{: note}
