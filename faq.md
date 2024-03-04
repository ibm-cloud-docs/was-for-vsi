---

copyright:
  years: 2020, 2024
lastupdated: "2024-02-23"

keywords: bug, problem, faqs, Frequently Asked Questions, question

subcollection: was-for-vsi

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:faq: data-hd-content-type='faq'}

# FAQs
{: #faq}

Review frequently asked questions for {{site.data.keyword.was4vsi}} in {{site.data.keyword.cloud}}. To find all FAQs for {{site.data.keyword.cloud_notm}}, see the [FAQ library](/docs/faqs){: external}.
{: shortdesc}

## What products can I install with {{site.data.keyword.was4vsi_short}}?
{: #faq-products}
{: faq}

You can install a {{site.data.keyword.appserver_short}} traditional environment on a virtual server instance (VSI) on {{site.data.keyword.cloud_notm}}. For a description of the topologies that you can install with {{site.data.keyword.was4vsi_short}}, see [Topologies](/docs/was-for-vsi?topic=was-for-vsi-topologies).

## What permissions do I need to use this tile?
{: #faq-permits}
{: faq}

You need Manager role on the Schematics service in at least one resource group. You also need Administrator role for VPC Infrastructure Services in the resource group for the Schematics workspace, VPC, and VSIs.

## Where can I see the installation logs?
{: #faq-logs}
{: faq}

To see the Installation logs, look under the [Schematics workspace](https://cloud.ibm.com/schematics/workspaces).

## Where can I see my installation history?
{: #faq-history}
{: faq}

To see the installation history, look under the [Schematics workspace](https://cloud.ibm.com/schematics/workspaces).

## How do I uninstall?
{: #faq-uninst}
{: faq}

Follow the instructions in [Uninstalling your workspace or resources](/docs/was-for-vsi?topic=was-for-vsi-uninstalling).

## Do I get charged for using the tile?
{: #faq-tile}
{: faq}

No, but you get charged for the infrastructure. Refer to [Topologies](/docs/was-for-vsi?topic=was-for-vsi-topologies) for infrastructure resources provisioned and to [Pricing](https://cloud.ibm.com/vpc-ext/provision/vs) for the associated cost.

## Can I use the tile to upgrade {{site.data.keyword.appserver_short}} after initial installation?
{: #faq-upgrade}
{: faq}

No, you can use the tile only for one installation. After the initial installation, it is your responsibility to manage and upgrade the installation. 

<!--
=== For later === 
Why do I need to specify IBMId and password? What is the alternative?
Can I use IBM Secret Manager?
Where can I find the common errors and their resolution?
-->

<!-- For detailed guidance on what to include on this page, see [FAQs guidance](/docs/developing/writing/faq.html#faqs). You can also check out some examples here: [IBM Cloud IAM FAQs](/docs/developing/Access-Management/iamfaq.html#faqs) and [Account FAQs](/docs/account/account_faq.html#accountfaqs). -->
