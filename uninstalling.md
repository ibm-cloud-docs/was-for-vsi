---

copyright:
  years: 2020, 2021
lastupdated: "2021-02-04"

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

You can uninstall {{site.data.keyword.was4vsi_short}} from the {{site.data.keyword.cloud_notm}} Schematics workspace or from the primary VSI.
{: shortdesc}

## Uninstall from the Schematics workspace
{: #uninst-wksp}

1. Log in to {{site.data.keyword.cloud_notm}} and [find the Schematics workspace](/schematics/workspaces) that was created during the installation.
2. Click **Actions > Delete**.
3. Select **Delete all associated resources to uninstall**.

If you also select **Delete workspace**, then you will not be able to review the deletion logs. To view the logs, delete the resources first, and then delete the workspace.
{: important}


## Uninstall from the primary VSI
{: #uninst-vsi}

1. Log in to the primary VSI and switch to the `<was_tf_dir>/scripts` directory.
2. Run **` sudo ./destroy.sh`** to uninstall the WebSphere environment.
