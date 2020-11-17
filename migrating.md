---

copyright:
  years: 2020
lastupdated: "2020-11-09"

keywords: migrating, migrate, move, workspace, resource, profiles, applications

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


# Migrating your workload
{: #migrating}

You can migrate your {{site.data.keyword.appserver_short}} traditional and Liberty product configurations and applications to your {{site.data.keyword.was4vsi_short}} environment.
{: shortdesc}

## Migrating {{site.data.keyword.appserver_short}} traditional configurations and applications
{: #migrat-twas}

To migrate your {{site.data.keyword.appserver_short}} traditional product configurations to a VSI, [use {{site.data.keyword.appserver_short}} migration tools](https://www.ibm.com/support/knowledgecenter/SSAW57_9.0.5/com.ibm.websphere.migration.nd.doc/ae/tmig_admin.html){: external}. Follow the instructions for migrating to new host or remote machines.

The following two topics provide helpful instructions:
* [Migrating to a Version 9.0 stand-alone application server on a remote machine](https://www.ibm.com/support/knowledgecenter/SSAW57_9.0.5/com.ibm.websphere.migration.nd.doc/ae/tmig_to70sasr.html){: external}
* [Migrating cells to new host machines using the command-line tool](https://www.ibm.com/support/knowledgecenter/SSAW57_9.0.5/com.ibm.websphere.migration.nd.doc/ae/tmig_migrate_remote_commandline.html){: external}

Administrative security configuration is migrated as-is into the new profiles on the VSI. If the administrative security configuration is set up to authenticate against an LDAP server that is not reachable from the VSI, you must disable security on the new deployment manager (`dmgr`) profile before you migrate the nodes.

To migrate from a mixed cell environment, make sure you have inbound communication set up to your source environment from the destination VSIs. Inbound communication might be needed for some migration steps.

## Migrating Liberty applications
{: #migrat-libertys}

To migrate Liberty applications to a VSI, see [Migrating applications to Liberty](https://www.ibm.com/support/knowledgecenter/SSAW57_liberty/com.ibm.websphere.wlp.nd.multiplatform.doc/ae/twlp_mig.html){: external}.
