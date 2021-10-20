---

copyright:
  years: 2021
lastupdated: "2021-10-20"

keywords: release notes, update, fix pack, fixpack, version, whats new, new in release

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

# Release notes
{: #release-notes}

Use the release notes to learn about the latest changes to {{site.data.keyword.was4vsi}}. The notes are grouped by release date or version number.
{: shortdesc}

## 20 October 2021: {{site.data.keyword.was4vsi_short}} 2.1.0 on {{site.data.keyword.cloud_notm}}
{: #v101}

Version 2.1.0 changes the Version 1.0 product:
* The product name was shortened to *{{site.data.keyword.appserver_short}}* from *{{site.data.keyword.appserver_short}} for VSI*.
* The product continues to offer the WAS.Base and WAS.Cell topologies for {{site.data.keyword.appserver_short}} traditional environments. The Liberty environments are no longer offered.
* Installation uses VSI images with the latest version of WebSphere traditional Version 9.0.5 pre-installed on {{site.data.keyword.redhat_full}} Enterprise Linux&reg; 8.4.
* Installation is now quicker and simpler. You no longer must set up virtual private clouds (VPCs) or provision virtual server instances (VSIs).

For information about the 2.1.0 product, see the following documentation:
* [Topologies](/docs/was-for-vsi?topic=was-for-vsi-topologies) describes the product and available topologies.
* [Deploying {{site.data.keyword.was4vsi_notm}}](/docs/was-for-vsi?topic=was-for-vsi-getting-started) describes how to install a {{site.data.keyword.appserver_short}} traditional environment on one or more VSIs.
* [Migrating your workload](/docs/was-for-vsi?topic=was-for-vsi-migrating) points to information that you can follow to migrate your existing {{site.data.keyword.appserver_short}} traditional configurations and applications to VSIs.
* [Troubleshooting](/docs/was-for-vsi?topic=was-for-vsi-troubleshoot) describes problems that you might encounter when you install and use the product and provides instructions for fixing the problems.


## 5 February 2021: {{site.data.keyword.was4vsi_short}} 1.0.1 on {{site.data.keyword.cloud_notm}}
{: #v101}

Version 1.0.1 provides enhancements and a command change:
* Automatic restarting of services after a VSI reboots. Before, you had to restart {{site.data.keyword.appserver_short}} traditional and Liberty manually after a VSI rebooted.
* Miscellaneous bug fixes
* The command to [uninstall a WebSphere environment from the primary VSI](/docs/was-for-vsi?topic=was-for-vsi-uninstalling#uninst-vsi) changes from **`./destroy.sh`** to **`sudo ./destroy.sh`**.


## 20 November 2020: {{site.data.keyword.was4vsi_short}} 1.0.0 on {{site.data.keyword.cloud_notm}}
{: #v100}

{{site.data.keyword.was4vsi_notm}} 1.0.0 is the first release of the product. Currently, {{site.data.keyword.was4vsi_short}} is available on
{{site.data.keyword.redhat_full}} Linux&reg; 7.x or later.

For information about the product, see the following documentation:
* [Topologies](/docs/was-for-vsi?topic=was-for-vsi-topologies) describes the product and available topologies.
* [Deploying {{site.data.keyword.was4vsi_notm}}](/docs/was-for-vsi?topic=was-for-vsi-getting-started) describes how to install a {{site.data.keyword.appserver_short}} traditional or Liberty environment on one or more virtual server instances (VSIs).
* [Migrating your workload](/docs/was-for-vsi?topic=was-for-vsi-migrating) points to information that you can follow to migrate your existing {{site.data.keyword.appserver_short}} traditional or Liberty configurations and applications to VSIs.
* [Troubleshooting](/docs/was-for-vsi?topic=was-for-vsi-troubleshoot) describes problems that you might encounter when you install and use the product and provides instructions for fixing the problems.
