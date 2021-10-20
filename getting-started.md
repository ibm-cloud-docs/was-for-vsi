---

copyright:
  years: 2020, 2021
lastupdated: "2021-10-14"


keywords: websphere, vsi, virtual server instance, terraform, deploying

subcollection: was-for-vsi

content-type: tutorial

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
{:step: data-tutorial-type='step'}


# Deploying {{site.data.keyword.was4vsi_notm}}
{: #getting-started}
{: toc-content-type="tutorial"}

The Deploying {{site.data.keyword.was4vsi_notm}} tutorial describes how you can use [{{site.data.keyword.was4vsi_notm}}](https://{DomainName}/catalog/content/.::1-b9f20fe3-baac-459b-b047-cb4ae9eb46f2-global) to install a {{site.data.keyword.appserver_short}} traditional environment on one or more {{site.data.keyword.redhat_full}} Enterprise Linux&reg; 8.4 virtual server instances (VSIs) on {{site.data.keyword.cloud}}.
{: shortdesc}


## Prepare for installation
{: #prerequisites}
{: step}

1. Read the terminology and topology descriptions in [Topologies](/docs/was-for-vsi?topic=was-for-vsi-topologies).
2. Check your IBMid and credential for BYOL. To download the {{site.data.keyword.appserver_short}} installation images, your [IBMid](https://www.ibm.com/account/) must have entitlement to download the required images from IBM Passport Advantage. Check your entitlements at [Passport Advantage Online for customers](https://www.ibm.com/software/passportadvantage/pao_customer.html).
3. Set up your access and SSH key in your account. See the "Before you begin" section of [Getting started with Virtual Private Cloud (VPC)](https://cloud.ibm.com/docs/vpc?topic=vpc-getting-started#prereqs).


## Install a WebSphere product
{: #2-install-was}
{: step}

1. Launch [{{site.data.keyword.was4vsi_notm}}](https://{DomainName}/catalog/content/.::1-b9f20fe3-baac-459b-b047-cb4ae9eb46f2-global){: external}.
2. In the form, set an identifiable name for your installation.
3. Specify all the required parameters. Set a strong password for `vsi_os_admin_password` and `vsi_websphere_admin_password`.
4. Review and specify optional parameters as needed. Some optional parameters depend on the value you chose for `deploy_was_topology`.
5. Accept the license (terms and conditions).
6. Click **Install**. It might take 5 or more minutes to complete the installation. Installation time varies depending on the available interim fixes, network speed, and VSI specifications.
7. Review the log. If you see the following messages toward the end of the log, then the installation was successful.
   ```text
   Install of IBM WebSphere Application Server [Network Deployment] version <Version> was successful
   Install directory: /opt/IBM/WebSphere/
   <Admin console URL and login details>
   [If IHS was setup] HTTP Server URL:
   Other details:
     VPC name:
     Subnet name:
     VSIs and their IP addresses:
   Notes:
   1. For more configuration information, see /etc/virtualimage.properties on your VSI.
   2. Refer to IBM Cloud Billing (https://cloud.ibm.com/billing) for the charges incurred for these resources.
   ```
   {: screen}


   For information about the log and correcting errors, see [FAQs](/docs/was-for-vsi?topic=was-for-vsi-faq), [Known issues](/docs/was-for-vsi?topic=was-for-vsi-known-issues), and [Troubleshooting](/docs/was-for-vsi?topic=was-for-vsi-troubleshoot).
8. Access the administrative console or IBM HTTP Server console with the **`vsi_websphere_admin_name`** and **`vsi_websphere_admin_password`** values to verify the installation. You can also SSH as `root` user with the SSH key that you set up.


## Next steps
{: #next}

You can [migrate your {{site.data.keyword.appserver_short}} product configurations and applications](/docs/was-for-vsi?topic=was-for-vsi-migrating) to your {{site.data.keyword.was4vsi_short}} environment.

To get IBM software support updates, [subscribe to My Notifications and other communications](https://www.ibm.com/support/pages/node/718119){: external}.

Join the [IBM WebSphere, Liberty & DevOps Community](https://community.ibm.com/community/user/wasdevops/communities/websphere-home){: external} or [other communities](https://community.ibm.com/community/user/sitemap){: external} to collaborate and share knowledge in forums and blogs.
