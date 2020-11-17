---

copyright:
  years: 2020
lastupdated: "2020-11-05"

keywords: known issue, bug, problem

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

# Known issues
{: #known-issues}

The current release of {{site.data.keyword.was4vsi}} in {{site.data.keyword.cloud}} has the following known issues.
{: shortdesc}

<!-- where the first xxx is the long name of your service and the following xxx are pulled from your popular troubleshooting topics -->

## Installation ends with Warning: External references from destroy provisioners are deprecated
{: issue-warning}

After a successful installation, the following message is shown:

```
Warning: External references from destroy provisioners are deprecated
  on main.tf line 36, in resource "null_resource" "was_install":
  36:     user = var.vsi_username

Destroy-time provisioners and their connection configurations may only
reference attributes of the related resource, via 'self', 'count.index', or
'each.key'.

References to other resources during the destroy phase can cause dependency
cycles and interact poorly with create_before_destroy.
...

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

The `Apply complete! Resources: 1 added, 0 changed, 0 destroyed.` message indicates that the installation was successful. You can ignore the warnings in the rest of the message.
