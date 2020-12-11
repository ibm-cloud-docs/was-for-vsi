---

copyright:
  years: 2020
lastupdated: "2020-11-09"

keywords: websphere, vsi, virtual server instance, liberty, terraform, deploying

subcollection: was-for-vsi

content-type: tutorial
completion-time: 20m

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
{:step: data-tutorial-type='step'}


# Deploying {{site.data.keyword.was4vsi_notm}}
{: #getting-started}
{: toc-content-type="tutorial"}
{: toc-completion-time="20m"}

The Deploying {{site.data.keyword.was4vsi_notm}} tutorial describes how you can use [{{site.data.keyword.was4vsi_notm}}](https://{DomainName}/catalog/content/.-b9f20fe3-baac-459b-b047-cb4ae9eb46f2-global) to install a {{site.data.keyword.appserver_short}} traditional or Liberty environment on one or more virtual server instances (VSIs) on {{site.data.keyword.cloud}}.
{: shortdesc}

## Before you begin
{: #before}

Before you start the installation, read the terminology and topology descriptions in [Topologies](/docs/was-for-vsi?topic=was-for-vsi-topologies).


## Review the prerequisites
{: #prerequisites}
{: step}

### IBMid and credential for BYOL
{: #1-ibmid-and-credential-for-byol}

To download the {{site.data.keyword.appserver_short}} installation images, your [IBMid](https://www.ibm.com/account/) must have entitlement to download the required images from IBM Passport Advantage. Check your entitlements at [Passport Advantage Online for customers]( https://www.ibm.com/software/passportadvantage/pao_customer.html). Also, see [Specify IBMid credentials for Passport Advantage](#1-specify-ibmid-credentials-for-passport-advantage).

### VSI requirements
{: #2-vsi-requirements}

- The operating system must be {{site.data.keyword.redhat_full}} Enterprise Linux&reg; 7.x or later.
- The minimum CPU is 1 vCPUs.
- The minimum RAM is 1 GB.
- The minimum disk is 10 GB.

  The minimum disk template uses the disk on the VSI to download and install {{site.data.keyword.appserver_short}}. The downloaded content is deleted after installation is complete.
  {: note}

### VSI connectivity
{: #3-vsi-requirements}

- The primary VSI must have an assigned public IP address so that they can be easily accessed over the internet.
- Secondary VSIs can have a private IP but they must be reachable from the primary VSI.
- Primary VSI's `<vsi_username home dir>/.ssh/known_hosts` file should contain entry for secondary VSIs.
- All VSIs must have connectivity to internet to enable download of WebSphere binary files from Passport Advantage. Ensure that you can connect to [https://www.ibm.com](https://www.ibm.com).

### VSI credentials
{: #4-vsi-requirements}

- All VSIs must be set up with a non-root user; for example, `wsadmin`.
- This user must have passwordless sudo access on all VSIs.
- For the primary VSI, this user must have SSH login enabled with password.
- The primary VSIs must have passwordless sshkey login enabled for this user on secondary VSIs.


## Set up VPC and VSIs in {{site.data.keyword.cloud_notm}}
{: #setup-vpc-and-vsis-in-ibm-cloud}
{: step}

### Set up VPC
{: #1-setup-vpc}

You can skip this step if you already have a VPC setup that you want to reuse.

If these instructions do not match the UI, see [VPC docs](/docs/vpc) for the latest steps.
{: note}

1. Follow the [Before you begin](/docs/vpc#prereqs) section of the VPC doc.
2. Log in to [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com) and start the "Virtual Private Cloud" tile from the catalog.
3. Specify a name for your VPC and select a resource group.
4. For **Default security group**, select **Allow SSH** or **Allow ping**.
5. For **Default address prefixes**, select **Create a default prefix for each zone**.
6. In the **New subnet for VPC** section, set a name for the subnet and make sure to use the same resource group.
7. Select a location.
8. Attach the **Public gateway**.
9. Review your data and click **Create virtual private cloud**.

### Provision one or more VSIs
{: #2-provision-vsis}

Provision a primary VSI for the WebSphere product that you want to use. For a WAS.Cell or Liberty.Collective installation, also provision one ore more secondary VSIs. See [Topologies](/docs/was-for-vsi?topic=was-for-vsi-topologies) for information about topologies and VSIs.

If these instructions do not match the UI, see [Virtual Server for VPC](/docs/vpc?topic=vpc-creating-virtual-servers) for the latest steps.
{: note}

1. Click **Virtual server instances** in your VPC instance. You can also search for **Virtual Servers for VPC** in the {{site.data.keyword.cloud_notm}} catalog.
2. Specify the parameters.
3. For **Image**, select **{{site.data.keyword.redhat_notm}} Enterprise Linux** and a profile. For more information, see the [prerequisites](#prerequisites).
4. Select your SSH key in the **SSH Keys** section. Use the **New key** option to add your SSH key.
5. Review your data and click **Create virtual server instance**. It takes few minutes to create the VSI.
6. After the VSI is ready, go to the VSI details, edit the **Network interface** section and select **Reserve a new floating IP**. **Floating IP** is the public IP of the VSI. If the VSI is your primary VSI, this IP is used for the **`vsi_ip`** parameter.
7. Follow the previous steps to provision secondary VSIs. Provision VSIs for installations such as a WAS.Cell or Liberty.Collective.

### Set up non-root user and SSH login on all VSIs
{: #3-setup-non-root-user-and-ssh-login-on-all-vsis}

1. Log in to VSI as root user.
  ```bash
  ssh -i <path to your private key file> root@<public ip address>
  ```
  {: codeblock}

2. Create a non-root user and enable SSH login. This user and its password (on the primary VSI) are used for **`vsi_username`** and **`vsi_password`** parameters. Use the same username on all VSIs. You can have a different password on each VSI.

  You can use a script or manual wsadmin steps to create a non-root user and enable SSH login.

 - Use a script.

    1. Copy the script snippet from [create_os_user.sh](/docs/was-for-vsi?topic=was-for-vsi-create_os_usersh) to a file named `create_os_user.sh`.
    2. Change its permission to make it executable.
      ```bash
      chmod 755 create_os_user.sh
      ```
      {: codeblock}
    3. Run the **`create_os_user.sh`** script.
      ```bash
      ./create_os_user.sh wsadmin <password>
      ```
      {: codeblock}

 - Use manual steps.

    1. Add a user. Run **`adduser wsadmin`**. The following steps use wsadmin as non-root user.
    2. Set up a password for this user. Run **`passwd wsadmin`** and specify a password.
    3. Enable passwordless sudo root access for this user.

        a. Run the **`usermod`** command.
        ```bash
        usermod -aG wheel wsadmin
        ```
        {: codeblock}

        b. Edit the `/etc/sudoers` file and add the following access statement to the end of the file.
        ```bash
        wsadmin         ALL=(ALL)       NOPASSWD: ALL
        ```
        {: codeblock}
    4. Enable password login for the wsadmin user.

       Edit the `/etc/ssh/sshd_config` file and add the following authentication statement to the end of the file.
        ```bash
        Match User wsadmin
                PasswordAuthentication yes
        ```
        {: codeblock}
    5. Restart the SSHD service. Run **`service sshd restart`**.

3. Exit the SSH session and log in with the new user to verify password and root access.
    ```bash
	  ssh wsadmin@<vsi_ip>
	  $sudo su
    ```
    {: codeblock}
4. Follow the previous steps on each of the VSI.

5. Enable passwordless ssh login from the primary VSI to the secondary VSIs. Complete the following steps on the primary VSI:

    1. Log in as `wsadmin` user and set up passwordless key. This step needs to be done only once.
       ```bash
       ssh-keygen -t rsa
       ```
       {: codeblock}

    2. Copy the key to each secondary VSI.
       ```bash
       ssh-copy-id wsadmin@<secondary_VSI_IP>
       ```
       {: codeblock}

       Enter the user password of the secondary VSI when prompted.
    3. Verify that you can log in to the secondary VSI without being prompted for a password.
       ```bash
       ssh wsadmin@<secondary_VSI_IP>
       ```
       {: codeblock}

    4. Release the public IP of the secondary VSI.

## Install WebSphere products
{: #websphere-installation-procedure}
{: step}

Review the list of [parameters or deployment values](/docs/was-for-vsi?topic=was-for-vsi-dep-values).

### Specify IBMid credentials for Passport Advantage
{: #1-specify-ibmid-credentials-for-passport-advantage}

This template downloads WebSphere installation binary files from IBM Passport Advantage with your IBMid credentials. You can either specify **`ibm_id`** and **`ibm_id_password`** parameters in the form or set up a keystore file on the primary VSI and use a **`credential_keystore_file`** parameter.

#### Steps to generate a keystore file on your primary VSI
1. In the form, set an identifiable name for your installation. Specify values for **`vsi_ip`**, **`vsi_username`**, and **`vsi_password`** in the form for your primary VSI. Set the **`install_im_only`** parameter to `true` and click **Install**. This option installs IBM Installation Manager only on your primary VSI. You must complete the form again to install WebSphere.
    Alternatively, you can download and install IBM Installation Manager from [Installation Manager and Packaging Utility download documents](https://www.ibm.com/support/pages/node/609575){: external}.
    {: note}
2. Check the installation log, which provides the Installation Manager location; for example, `Installation Manager successfully installed at /home/wsadmin/IBM/InstallationManager`.
3. SSH to the primary VSI and run the following command to generate the keystore file. You do not need to do this step on the secondary VSIs.
  ```bash
  <IM_INSTALL_DIR>/eclipse/tools/imutilsc saveCredential -passportAdvantage -userName <ibm_id> -userPassword <ibm_id_password> -secureStorageFile <KEYSTORE_FILE>/
  ```
  {: codeblock}
    - **`IM_INSTALL_DIR`** is `<home directory of vsi_username>/IBM/InstallationManager`; for example, `/home/wsadmin/IBM/InstallationManager`.
    - **`KEYSTORE_FILE`** is the full path of the keystore file; for example, `home/wsadmin/credential.store`.

### Install a WebSphere product
{: #2-install-was}

1. Launch [{{site.data.keyword.was4vsi_notm}}](https://{DomainName}/catalog/content/.-b9f20fe3-baac-459b-b047-cb4ae9eb46f2-global)
2. In the form, set an identifiable name for your installation.
3. Specify the **`vsi_ip`** and **`vsi_password`** values of the primary VSI.
4. Specify the **`credential_keystore_file`** value or the **`ibm_id`** and **`ibm_id_password`** values.
5. Review and complete the deployment values.
6. Accept the license (terms and conditions).
7. Click **Install**. It might take 15 minutes or more to complete the installation.
8. Review the log. If you see the following messages towards the end of the log, then the installation was successful.
  ```
  Install of <was_topology> version <was_version> was successful.
  Install directory: /opt/IBM/WebSphere/
  <Admin console URL and login details>
  ```
  {: screen}

  ```
  Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
  ```
  {: screen}

   Ignore warnings such as "External references from destroy provisioners are deprecated." The "Apply complete!" message indicates that the installation was successful. For more information, see [Known issues](/docs/was-for-vsi?topic=was-for-vsi-known-issues).
   {: tip}

  For information about the log and correcting errors, see [FAQs](/docs/was-for-vsi?topic=was-for-vsi-faq), [Known issues](/docs/was-for-vsi?topic=was-for-vsi-known-issues), and [Troubleshooting](/docs/was-for-vsi?topic=was-for-vsi-troubleshoot).

9. Access the administrative console with the **`vsi_username`** and **`vsi_password`** values to verify the installation.


## Next steps
{: #next}

You can [migrate your {{site.data.keyword.appserver_short}} traditional and Liberty product configurations and applications](/docs/was-for-vsi?topic=was-for-vsi-migrating) to your {{site.data.keyword.was4vsi_short}} environment.

To get IBM software support updates, [subscribe to My Notifications and other communications](http://www.ibm.com/software/support/einfo.html){: external}.
