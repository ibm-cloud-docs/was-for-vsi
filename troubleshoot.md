---

copyright:
  years: 2020
lastupdated: "2020-11-12"

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
<!-- where the first xxx is the long name of your service and the following xxx are pulled from your popular troubleshooting topics -->

## Required installation files are not found
{: #troubleshoot-notfound}
{: troubleshoot}

The installation failed with `CRIMA1161E ERROR: Failed to find required installation files.`
{: tsSymptoms}

The error might occur due to network or IBM Passport Advantage issues.
{: tsCauses}

Run the `terraform destroy` command and retry the installation. Also, make sure you are using correct values for `was_topology` and `was_version`.
{: tsResolve}


## Keystore file does not exist at the specified path
{: #troubleshoot-keystorepath}
{: troubleshoot}

The installation failed with the following error message: `keystore_file is set to <path> but no file exists at this path`.
{: tsSymptoms}

The path to the keystore file is incorrect or `credential_keysfote_file` and `vsi_username` do not have permission to access the file.
{: tsCauses}

Verify that you have provided the full path to the keystore file as `credential_keysfote_file` and `vsi_username` is allowed to access this file. Refer to [Deployment values](/docs/was-for-vsi?topic=was-for-vsi-dep-values) for more details.
{: tsResolve}


## The yum install command failed
{: #troubleshoot-yum}
{: troubleshoot}

```
The system is not registered with an entitlement server. You can use subscription-manager to register.
null_resource.script (remote-exec): There are no enabled repos.
null_resource.script (remote-exec):  Run "yum repolist all" to see the repos you have.
null_resource.script (remote-exec):  To enable {{site.data.keyword.redhat_notm}} Subscription Management repositories:
null_resource.script (remote-exec):      subscription-manager repos --enable <repo>
null_resource.script (remote-exec):  To enable custom repositories:
null_resource.script (remote-exec):      yum-config-manager --enable <repo>
```
{: tsSymptoms}

Your VSI is not registered with an entitlement server.
{: tsCauses}

Register your VSI with the {{site.data.keyword.redhat_full}} Satellite server. If you are getting a VSI from {{site.data.keyword.cloud}}, your VSI should already be registered with the {{site.data.keyword.redhat_notm}} Satellite Server.
{: tsResolve}

If the VSI is not registered, as root user, run the following command on the VSI that is not registered to the {{site.data.keyword.redhat_notm}} Satellite server.

```bash
# sh /var/lib/cloud/instance/scripts/vendor/part-003
```
{: codeblock}

If the previous command fails, then run the following commands as root user.
```bash
Step 1. subscription-manager remove --all
Step 2. subscription-manager unregister
Step 3. subscription-manager clean
Step 4. sh /var/lib/cloud/instance/scripts/vendor/part-003
```

If the VSI still does not register with the {{site.data.keyword.redhat_notm}} Satellite server, then open an [{{site.data.keyword.cloud}} Support case](/docs/was-for-vsi?topic=was-for-vsi-support).  


## Cannot install a directory on the VSI
{: #troubleshoot-exist}
{: troubleshoot}

A warning such as the following is shown during installation:
```
WARN: Cannot install in an existing directory: /opt/IBM/WebSphere/AppServer. Please cleanup and try again.
```
{: tsSymptoms}

A topology exists on the VSI.
{: tsCauses}

Only one topology can be installed on a VSI. [Uninstall your workspace or resources](/docs/was-for-vsi?topic=was-for-vsi-uninstalling) or clean up the VSI and try the installation again.


## Cannot access the administrative console after installation
{: #troubleshoot-console}
{: troubleshoot}

After a successful installation of {{site.data.keyword.appserver_short}} traditional, the administrative console is not available in a browser.
{: tsSymptoms}

The URL for the administrative console might be incorrect or the port might not allow incoming traffic.
{: tsCauses}

Use the correct URL to the console.
{: tsResolve}

Make sure that incoming traffic is allowed at the specified port. Refer to [{{site.data.keyword.cloud}} VPC network](/docs/vpc?topic=vpc-about-networking-for-vpc) documents for more detail. You might need to add an inbound rule to the security group to allow incoming traffic.
