---

copyright:
  years: 2020, 2024
lastupdated: "2024-05-07"

keywords: topology, topologies, products, websphere, vsi, server, deployment manager, cell, ihs, vpc, ports, consoles

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

Use {{site.data.keyword.was4vsi}} to install a {{site.data.keyword.appserver_short}} traditional environment on a {{site.data.keyword.redhat_full}} Enterprise Linux&reg; 8.4 virtual server instance (VSI) on {{site.data.keyword.cloud}}.
{: shortdesc}

This documentation refers to {{site.data.keyword.appserver_short}} as ***WebSphere*** or ***WAS***.

{{site.data.keyword.was4vsi}} offers two topologies: **`WAS.Base`** and **`WAS.Cell`**. Specify one of these topologies as a value for the **`deploy_was_topology`** parameter.

**WAS.Base**
:   The `WAS.Base` topology provisions one VSI with the latest release of {{site.data.keyword.appserver_short}} traditional V9.0.5. `WAS.Base` provides a single server. This VSI has a public (floating) IP address.

**WAS.Cell**
:   The `WAS.Cell` topology provides multiple VSIs with the latest release of {{site.data.keyword.appserver_short}} Network Deployment traditional V9.0.5. `WAS.Cell` provisions one VSI with the deployment manager (DMgr), with one or more VSIs for custom nodes. You can also set up an IBM HTTP Server (IHS) VSI. DMgr and IHS VSIs have public (floating) IP addresses, while custom node VSIs have private IP addresses.

The installation sets up a new virtual private cloud (VPC), one or more VSIs, and relevant networking.
{: note}

## VPC details
{: #vpc-details}

The VPC, VSI, and related networking artifacts are created in the same resource group where the Schematics workspace is created. After the installation is complete, refer to the **Resources** section of the Schematics workspace for these artifacts. For more details about the artifacts, see [information about virtual private clouds](https://cloud.ibm.com/vpc-ext).

## VSI details
{: #vsi-details}

- All the VSIs are set up within the same subnet.

- The VSIs for Base, DMgr, and IHS have public (floating) IP addresses and private IP addresses. For `WAS.Cell`, the custom nodes VSIs have private IP addresses only.

- Each VSI is set up with the following three user IDs. For descriptions of the parameters, see [Deployment values](/docs/was-for-vsi?topic=was-for-vsi-dep-values).

   root
   : Use the sshkey (**`vpc_sshkey_name`** parameter) that you set up to log in (ssh) to the VSIs as root.

   OS administrator (**`vsi_os_admin_name`**)
   : The default value for the OS administrator is `virtuser`. You can change the default value. Use the **`vsi_os_admin_password`** parameter value to log in (ssh) as this user.

   WebSphere administrator (**`vsi_websphere_admin_name`**)
   : The default value for the WebSphere administrator is `wsadmin`. You can change the default value. Use the **`vsi_websphere_admin_password`** parameter value to log in (ssh) as this user.

- OS administrator and WebSphere administrator have password-less sudo access enabled on all VSIs. For `WAS.Cell`, the OS administrator and WebSphere administrator also have password-less sudo access enabled among the VSIs.

- Several ports are opened by default. Depending on your scenario, you might need to open more ports for your application to work.
    - `22` for ssh login
    - `80` for the IBM HTTP Server console, if the **`cell_ihs_setup_vsi`** parameter value is `true`
    - `8080`, `9043-9443` for the WebSphere administrative console and applications


- For more configuration information, see the `/etc/virtualimage.properties` file on the VSI. All properties that are listed in this file might not apply to your specified topology.
