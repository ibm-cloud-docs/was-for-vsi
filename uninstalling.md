---

copyright:
  years: 2020, 2021
lastupdated: "2021-10-14"

keywords: uninstalling, removing, workspace, resource

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


# Uninstalling your workspace or resources
{: #uninstalling}

You can uninstall {{site.data.keyword.was4vsi_short}} from the {{site.data.keyword.cloud_notm}} Schematics workspace. Uninstallation deletes all the VSIs, VPC, and related artifacts that were set up during the installation.
{: shortdesc}

## Uninstall from the Schematics workspace
{: #uninst-wksp}

1. Log in to {{site.data.keyword.cloud_notm}} and [find the Schematics workspace](/schematics/workspaces) that was created during the installation.
2. Click **Actions > Destroy**. This will delete all the VSIs, VPC, and related artifacts that were set up during the installation. If you created or changed any infrastructure configuration, then you might need to clean up the artifacts manually.
3. You can also delete the workspace by clicking **Actions > Delete workspace**.
{: important}
