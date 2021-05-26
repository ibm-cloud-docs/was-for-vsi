---

copyright:
  years: 2020, 2021
lastupdated: "2021-05-26"

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


## Unable to create directory when setting up a non-root user on VSIs
{: #troubleshoot-dirnotcreated}
{: troubleshoot}

The installation failed with `create_was_dirs.sh: ERROR: Unable to create directory /opt/IBM/WebSphere/Profiles.`
{: tsSymptoms}

The default umask for your **`vsi_username`** is not `002`. A umask value of `027` can cause the error.
{: tsCauses}

1. Run the `umask` command to get your current umask value.
2. If the umask value is not `002`, change the value in the file where umask is defined, typically in `/etc/bashrc` but also in `/etc/profile` or `~/.bashrc`. The recommended value is `002` but another value might be suitable for your VSI.
3. For the changes to take effect, reload the configuration scripts by relogging or with the `source` command.

For more information, see [Set up non-root user and SSH login on all VSIs](/docs/was-for-vsi?topic=was-for-vsi-getting-started#3-setup-non-root-user-and-ssh-login-on-all-vsis).
{: tsResolve}


## Required installation files are not found
{: #troubleshoot-notfound}
{: troubleshoot}

The installation failed with `CRIMA1161E ERROR: Failed to find required installation files.`
{: tsSymptoms}

The error might occur due to network or IBM Passport Advantage issues.
{: tsCauses}

Run the `terraform destroy` command and retry the installation. Also, make sure you are using correct values for `was_topology` and `was_version`.
{: tsResolve}


## Installation fails with a status 126 error
{: #troubleshoot-noexec}
{: troubleshoot}

Installation ends with the `Process exited with status 126` error.
{: tsSymptoms}

The `/tmp` directory for a provisioned VSI is mounted with the `noexec` mount option. Installation scripts are run in the `/tmp` directory and the directory must not have the `noexec` mount option.
{: tsCauses}

1. Check the mount option for the `/tmp` directory with the **`findmt`** command.
   ```bash
   findmnt -l | grep /tmp
   ```
   {: codeblock}
2. Look for `noexec` in the `OPTIONS` column.
3. If `noexec` is in the mount options, remove it. Open the `/etc/fstab` file as root and remove the `noexec` option for the `/tmp` directory.
4. Run **`mount -o remount /tmp`** to remount the `/tmp` directory. The **`mount`** command checks the options in the `/etc/fstab` file and remounts the `/tmp` directory without the `noexec` option.
5. [Start the installation](/docs/was-for-vsi?topic=was-for-vsi-getting-started#2-install-was) again.
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

If the VSI is not registered, as root user, register the VSI. See [How do I reregister a RHEL VSI?](/docs/vpc?topic=vpc-troubleshooting-your-virtual-servers-for-vpc#troubleshooting-reregister-RHEL-VSI) in the VPC documentation.

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

Make sure that incoming traffic is allowed at the specified port. Refer to [{{site.data.keyword.cloud_notm}} VPC network](/docs/vpc?topic=vpc-about-networking-for-vpc) documents for more detail. You might need to add an inbound rule to the security group to allow incoming traffic.
