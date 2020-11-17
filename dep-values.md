---

copyright:
  years: 2020
lastupdated: "2020-11-17"

keywords: deployment values, install, websphere liberty, vsi, passport advantage, ppa

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

<dl>
<dt><strong>`credential_keystore_file`</strong></dt>
<dd>Full path to secure keystore file name on the virtual server instance that is used to download Software from Passport Advantage. Either this variable or **`ibm_id`** and **`ibm_id_password`** must be set. For example, `/tmp/was_tf/credential.store`. For more information, see [Specify IBMid credentials for Passport Advantage](/docs/was-for-vsi?topic=was-for-vsi-getting-started#1-specify-ibmid-credentials-for-passport-advantage).</dd>
<dt><strong>`ibm_id`</strong></dt>
<dd>IBMid to download the {{site.data.keyword.appserver_short}} installation images. This IBMid must have entitlement to download the required software from Passport Advantage. You can check your entitlements at [Passport Advantage Online for customers](https://www.ibm.com/software/passportadvantage/pao_customer.html).</dd>
<dt><strong>`ibm_id_password`</strong></dt>
<dd>IBMid password.</dd>
</dl>


## Parameters for {{site.data.keyword.appserver_short}}
{: #was-parms}

<dl>
<dt><strong>`was_topology`</strong></dt>
<dd>The topology to install. Specify one of the supported topologies: `WAS.Base`, `WAS.Cell`, `Liberty.Base`, or `Liberty.Collective`. The default is `WAS.Base`.</dd>
<dt><strong>`was_version`</strong></dt>
<dd>The {{site.data.keyword.appserver_short}} or Liberty version that you want to install. You must specify the version in the V.R.M.F format (for example, `9.0.5.5` or `20.0.0.9`). WebSphere Application Server version 8.5.5.13 and later is supported. Liberty version 19.0.0.5 and later is supported. For available versions, see [Recommended updates for WebSphere Application Server](https://www.ibm.com/support/pages/recommended-updates-websphere-application-server). The default is the last `WAS.Base` version.</dd>
<dt><strong>`was_tf_dir`</strong></dt>
<dd>Temporary directory to copy installation scripts. The default is `/tmp/was_tf`.</dd>
</dl>


## Parameters for primary and secondary VSIs
{: #vsi-parms}

<dl>
<dt><strong>`vsi_ip` (required)</strong></dt>
<dd>The public IP of the primary virtual server instance (VSI). For WAS.Cell, specify the IP address of the deployment manager. For Liberty.Collective, specify the IP address of the controller VSI.</dd>
<dt><strong>`vsi_username`</strong></dt>
<dd>The user name used to log in to one or more **`vsi_ip`** instances. For more information, see the [VSI connectivity requirements](/docs/was-for-vsi?topic=was-for-vsi-getting-started#prerequisites). The default is `wsadmin`.</dd>
<dt><strong>`vsi_password` (required)</strong></dt>
<dd>The password for **`vsi_username`** to log in to the **`vsi_ip`**.</dd>
<dt><strong>`vsi_node_ips`</strong></dt>
<dd>This parameter is required for WAS.Cell or Liberty.Collective. Specify a comma-separated list of custom nodes or Liberty hosts private IPs. Do not specify any value for single-server installation. For more information, see the [VSI connectivity requirements](/docs/was-for-vsi?topic=was-for-vsi-getting-started#prerequisites).</dd>
</dl>


## Other parameters
{: #other-parms}

<dl>
<dt><strong>`install_im_only`</strong></dt>
<dd>When set to `true`, specifies to install IBM Installation Manager only. The default is `false`. You can use this option to generate a keystore file. For more information, see [Specify IBMid credentials for Passport Advantage](/docs/was-for-vsi?topic=was-for-vsi-getting-started#1-specify-ibmid-credentials-for-passport-advantage).</dd>
<dt><strong>`verbose`</strong></dt>
<dd>When set to `true`, specifies to log debug output to the console. The default is `true`.</dd>
</dl>
