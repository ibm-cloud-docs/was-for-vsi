---

copyright:
  years: 2020, 2024
lastupdated: "2024-08-20"

keywords: bug, problem, troubleshoot, troubleshooting, question

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
{:troubleshoot: data-hd-content-type='troubleshoot'}

# Troubleshooting
{: #troubleshoot}

General problems with using {{site.data.keyword.was4vsi}} in {{site.data.keyword.cloud}} might include the inability to connect to your application or to see your environment. In many cases, you can recover from these problems by following a few easy steps.
{: shortdesc}

## Error: the provided instance profile ID does not exist
{: #troubleshoot-profilenotfound}
{: troubleshoot}

The error pertains to a vsi profile value, which follows `"ibm_is_instance"` in the error message. 
{: tsSymptoms}

```text
on main.tf line 184, in resource "ibm_is_instance" "primary":
 184: resource "ibm_is_instance" "primary" {
```

The error occurs when a VSI profile value is not valid.
{: tsCauses}

- If the error message refers to "ibm_is_instance" "primary" then the **`base_vsi_profile`** or **`cell_dmgr_vsi_profile`** parameter value is invalid.
- If it refers to "ibm_is_instance" "ihs" then the **`cell_ihs_vsi_profile`** parameter value is invalid.
- If it refers to "ibm_is_instance" "node" then the **`cell_node_vsi_profile`** parameter value is invalid.

These VSI instance profile parameters are described in [Deployment values](/docs/was-for-vsi?topic=was-for-vsi-dep-values). For information about available profiles, see [VPC Profiles](https://cloud.ibm.com/docs/vpc?topic=vpc-profiles). 
{: tsResolve}

## Error: No SSH Key found
{: #troubleshoot-sshkeynotfound}
{: troubleshoot}

Error: No SSH Key found with name <INVALID_KEY_NAME>
{: tsSymptoms}

The error means that the specified SSH key name (**`vpc_sshkey_name`** parameter) is wrong or the key does not exist in the region based on the `Region` parameter.
{: tsCauses}

Make sure that the SSH key name is specified correctly and that the key exists in the region where the VPC will be created. Refer to [SSH keys for VPC](https://cloud.ibm.com/vpc-ext/compute/sshKeys) for details. For information about the **`vpc_sshkey_name`** parameter, see [Deployment values](/docs/was-for-vsi?topic=was-for-vsi-dep-values).
{: tsResolve}

## Error: SSH authentication failed while creating VSI
{: #troubleshoot-sshauthnfailed}
{: troubleshoot}

Error in Schematics log: timeout - last error: SSH authentication failed (USER@IP:22): ssh: handshake failed: ssh: unable to authenticate, attempted methods [none password], no supported methods remain
{: tsSymptoms}

The error means that the specified value for the **`vsi_os_admin_name`** or **`vsi_websphere_admin_name`** parameter is wrong.
{: tsCauses}

If you changed the default value for the **`vsi_os_admin_name`** or **`vsi_websphere_admin_name`** parameter, then make sure that the new value does not contain the `virtuser` or `wsadmin` default value. For information about the parameters, see [Deployment values](/docs/was-for-vsi?topic=was-for-vsi-dep-values).
{: tsResolve}
