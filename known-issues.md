---

copyright:
  years: 2020, 2021
lastupdated: "2021-10-14"

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

The current release of {{site.data.keyword.was4vsi}} in {{site.data.keyword.cloud}} has the following known issue.
{: shortdesc}


## javacore generated after VSI reboot
{: #issue-warning}

After the VSI is rebooted, all services start successfully but a javacore file is generated in the `/opt/IBM/WebSphere/AppServer/profiles/profile_name/` directory.

This is working as expected. Refer to following documents if you want to disable the creation of javacore file:
- [PH39733: PROVIDE A SWITCH TO DISABLE JAVACORES FOR UNEXPECTED SHUTDOWNS (ibm.com)](https://www.ibm.com/support/pages/apar/PH39733)
- [Setting generic JVM arguments in WebSphere Application Server (ibm.com)](https://www.ibm.com/support/pages/setting-generic-jvm-arguments-websphere-application-server)
