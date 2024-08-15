---

copyright:
  years: 2020, 2024
lastupdated: "2024-08-15"

keywords: deployment values, install, websphere, vsi, passport advantage, ppa, license, profile, ihs, cell

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

# Deployment values
{: #dep-values}

When you install the WebSphere products, select the parameter values for deployment.
{: shortdesc}

## Parameters for Passport Advantage entitlement
{: #pa-parms}

**`ibm_id`**
:   This offer is Bring Your Own License (BYOL). To deploy this offer, you must enter your registered IBMid and your IBMid must have active WebSphere entitlements that are associated with it. You can check your entitlements at [Passport Advantage Online for customers](https://www.ibm.com/software/passportadvantage/pao_customer.html).

**`ibm_id_password`**
:   IBMid password.


## Parameters for Virtual Private Cloud (VPC) setup
{: #vpc-parms}

**`Region`**
:  The region and zone where the VPC is created, such as `us-south-1` or `us-east-2`. For the latest list of {{site.data.keyword.cloud_notm}} regions, see the [Virtual Private Cloud (VPC) documentation](/docs/vpc?topic=vpc-creating-a-vpc-in-a-different-region).

**`vpc_name`**
:  The unique name for this VPC. The name must start with a lowercase letter, use only lowercase alphanumeric characters and hyphens (without spaces), and cannot end with a hyphen. This offer creates a ([VPC](/vpc-ext/network/vpcs)) and one or more [virtual server instances (VSIs)](/vpc-ext/compute/vs) where WebSphere is installed.

**`vpc_sshkey_name`**
:  The name of the SSH key in your {{site.data.keyword.cloud_notm}} account. Use this key to [SSH log in to VSIs](/vpc-ext/compute/sshKeys). Make sure that the SSH key exists in the same region where the VPC is created.


## Required parameters for {{site.data.keyword.appserver_short}}
{: #was-parms}

**`deploy_was_topology`**
:   The topology to install. Specify a supported topology, `WAS.Base` or `WAS.Cell`.

**`vsi_os_admin_password`**
:   The password for **`vsi_os_admin_name`** to log in to a VSI. The password must have 12 or more characters and must contain only letters, numbers, and the -_=?.@#% special characters.

**`vsi_websphere_admin_password`**
:   The password for **`vsi_websphere_admin_name`** to log in to the administrative console and VSI. The password must have 12 or more characters and must contain only letters, numbers, and the -_=?.@#% special characters.

## Optional parameters for {{site.data.keyword.appserver_short}}
{: #was-opt-parms}

**`base_vsi_profile`**
:   The instance profile for the WAS base VSI in a `WAS.Base` topology. The default is `cx2-2x4`. See [available profiles](/docs/vpc?topic=vpc-profiles) and [pricing](https://cloud.ibm.com/vpc-ext/provision/vs).

**`cell_dmgr_vsi_profile`**
:   The instance profile for the DMgr VSI in a `WAS.Cell` topology. The default is `cx2-2x4`. See [available profiles](/docs/vpc?topic=vpc-profiles) and [pricing](https://cloud.ibm.com/vpc-ext/provision/vs).

**`cell_ihs_setup_vsi`**
:   When set to `true`, sets up the IBM HTTP Server VSI in a `WAS.Cell` topology. The default is `true`. Only applicable when **`deploy_was_topology`** is `WAS.Cell`.

**`cell_ihs_vsi_profile`**
:   The instance profile for the IBM HTTP Server VSI in a `WAS.Cell` topology. The default is `cx2-2x4`. See [available profiles](/docs/vpc?topic=vpc-profiles) and [pricing](https://cloud.ibm.com/vpc-ext/provision/vs). Only applicable when **`deploy_was_topology`** is `WAS.Cell`.

**`cell_node_count`**
:   The number of custom nodes you want to provision for `WAS.Cell`, which must be between 1 and 20 (inclusive). This parameter is ignored for `WAS.Base`.

**`cell_node_vsi_profile`**
:   The instance profile for custom node VSIs in a `WAS.Cell` topology. The default is `cx2-2x4`. See [available profiles](/docs/vpc?topic=vpc-profiles) and [pricing](https://cloud.ibm.com/vpc-ext/provision/vs). Only applicable when **`deploy_was_topology`** is `WAS.Cell`.

**`vsi_os_admin_name`**
:   The VSI Administrator username that is used to log in to VSI. The value must be 1-30 characters long and must contain letters and numbers only. The value must not contain `virtuser` or `wsadmin`. The default is `virtuser`.

**`vsi_websphere_admin_name`**
:   The WebSphere Administrator username that is used to log in to the administrative console and VSI. The value must be 1-30 characters long and must contain letters and numbers only. The value must not contain `virtuser` or `wsadmin`. The default is `wsadmin`.


## Other parameters
{: #other-parms}

**`verbose`**
:   When set to `true`, specifies to log debug information. The default is `true`.
