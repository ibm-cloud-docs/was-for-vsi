---

copyright:
  years: 2021
lastupdated: "2021-04-26"

keywords: subnet, configuring, nodes, deployment

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


# Configuring subnets for multiple-node deployments
{: #subnets-multinode}

You can use separate subnets for the primary and secondary VSIs in a multiple-node deployment. Separate subnets enable you to secure each subnet differently through configuring public gateways, access control lists (ACLs), and security groups (SGs).
{: shortdesc}

Typically, the primary VSI (DMGR or Controller) resides in a subnet with ACLs and SGs, which allows the primary VSI to be reached from the public internet, while the secondary VSIs (custom nodes or Liberty hosts) reside in a more constrained subnet that prevents them from being reached by anything other than the primary VSI.

## Subnet configurations

You can configure a VPC with VSIs spread across multiple subnets in different ways. In general, it's sufficient to have two subnets:
- A perimeter network (or *DMZ*) that is configured with an ACL to be open to the public internet
- An internal or secure network that is configured with an ACL so it can be accessed from the DMZ only, not from the public internet

In this configuration, the primary VSI resides in the DMZ subnet. A floating IP is attached to it. The floating IP enables the VSI to reach and be reached by the public internet. Thus, a public gateway is not needed on this subnet. The secondary VSIs reside in the secure subnet without floating IPs or a public gateway, so it cannot reach or be reached by the public internet. By default, the secure subnet can be reached only by its private IP from machines in the same VPC.

Finally, ACLs for both subnets are configured with a high-priority rule to allow traffic to and from the other subnet. The secure subnet ACL denies all other traffic. The DMZ subnet ACL allows some traffic, such as HTTP and HTTPS traffic, and denies the rest.


## Set up in {{site.data.keyword.cloud_notm}}
{: #subnet-setup}

1. Create a VPC with a primary subnet. Use this subnet as the DMZ. It does not need a public gateway because its VSI has a floating IP.
2. After the VPC is provisioned, create a second subnet for the internal, secure network. Add a public gateway to this subnet. Leave the access control list at its default value for the VPC for now. You create a separate ACL for this subnet next.
3. Go to your Access Control Lists view. Rename the default ACL to make it easily distinguishable as the ACL for the DMZ, then duplicate it and name the new ACL for the internal subnet.
4. Set your internal subnet to use the new ACL you created for it.
5. [Create your VSIs](/docs/was-for-vsi?topic=was-for-vsi-getting-started#2-provision-vsis) and assign them to the correct subnets. The primary VSI goes in the perimeter (DMZ) subnet, while the secondary VSIs go in the internal subnet.

## Configure access control lists
{: config-acl}

You can configure the ACLs in two ways, the [easy way](#the-easy-way-to-configure-acls) and the [hard way](#the-hard-way-to-configure-acls). Optionally, you also can [open the ports](#enable-connection-to-the-admin-panel) that are needed to access your server administration view in advance.

### The easy way to configure ACLs
{: easy-acl}

The easy way is to leave the ACLs in their default state until after installation, which permits all inbound and outbound traffic.

| Subnet access control list | Rules |
| --- | --- |
| Perimeter inbound | allow ALL from ANY to ANY |
| Perimeter outbound | allow ALL from ANY to ANY |
| Internal inbound | allow ALL from ANY to ANY |
| Internal outbound | allow ALL from ANY to ANY |
{: caption="Table 1. Subnet ACLs and rules in the easy way to configure ACLs" caption-side="top"}

### The hard way to configure ACLs
{: hard-acl}

The hard way is to restrict traffic as much as possible in advance, which causes the installation to run more slowly and still requires minor reconfiguration after installation because both VSIs need to be enabled to access the internet to download IBM Installation Manager and other packages.

Open all required ports so that the installation does not fail. If the installation fails, you must run `terraform destroy`, fix the configuration, and then apply again.
{: tip}

| Subnet access control list | Rules |
| --- | --- |
| Perimeter inbound | allow ALL from **{internal subnet}** to ANY<sup>1</sup></br>allow UDP from **{nameserver}**<sup>2</sup>:53 to ANY</br>allow TCP from ANY to ANY:22 (SSH)</br>allow TCP from ANY:80 to ANY (HTTP)</br>allow TCP from ANY:443 to ANY (HTTPS)</br>Deny ALL from ANY to ANY |
| Perimeter outbound | allow ALL from ANY to ANY |
| Internal inbound |  allow ALL from **{perimeter subnet}** to ANY<sup>1</sup></br>allow UDP from **{nameserver}**<sup>2</sup>:53 to ANY</br>allow TCP from ANY to ANY:22 (SSH)</br>allow TCP from ANY:80 to ANY (HTTP)</br>allow TCP from ANY:443 to ANY (HTTPS)</br>Deny ALL from ANY to ANY |
| Internal outbound | allow ALL from ANY to ANY |
{: caption="Table 2. Subnet ACLs and rules in the hard way to configure ACLs" caption-side="top"}

<sup>1</sup> It is technically possible to restrict which ports are open between the subnets, but this is difficult and not recommended. If you really want to do this, permit all traffic between subnets during installation, and then close any unneeded ports afterward.

<sup>2</sup> You must allow inbound traffic from the nameserver on port 53 so name resolution works. To determine the IP address of the nameserver for your VSIs, you need `nslookup`, which is not installed by default. (To install `nslookup`, allow all traffic in ACLs and SGs and run `sudo yum install bind-utils`.) Run the `nslookup -query=ns google.com` command on the machine; it lists the IP address of the nameserver that is used to resolve.

### Enable connection to the admin panel
{: connect-acl}

You can open the ports that are needed to access your server administration view.

| Topology | Inbound rules |
| --- | --- |
| Cell | allow TCP from ANY<sup>3</sup> to ANY:9043</br>allow TCP from ANY<sup>3</sup> to ANY:9060</br>allow TCP from ANY<sup>3</sup> to ANY:9080</br>allow TCP from ANY<sup>3</sup> to ANY:9443  |
| Collective | allow TCP from ANY<sup>3</sup> to ANY:9043</br>allow TCP from ANY<sup>3</sup> to ANY:9080 |
{: caption="Table 3. Inbound rules for cell and collective topologies" caption-side="top"}

<sup>3</sup>You can limit the source port to 80 or 443, and also permit only HTTP or HTTPS communication.

## Configure security groups
{: config-sg}

You can configure the security groups in two ways, with [one shared security group](#one-shared-security-group) or with [two separate security groups](#two-separate-security-groups). The configuration that you use depends on whether the machines share one security group (SG) or have two separate SGs, one for the perimeter VSI and the other for internal VSIs. If the VSIs share one SG, they are implicitly permitted to freely communicate with each other through any protocol on any port. However, if two SGs are used, you must specifically permit traffic on the ports that are needed by `WAS.Cell` or `Liberty.Collective`.

### One shared security group
{: one-sg}

The ports differ slightly for Cell and Collective, but overall using a single shared SG is simple. An ICMP rule to allow ping is optional. A rule to permit SSH on port 22 is required during installation but can be removed afterwards. The other ports permit connecting to the administration page.

| Topology | Inbound rules |
| --- | --- |
| Cell | CMP (type: 8; code: any) from any source (allows ping; optional)</br>22/tcp from any source (allows SSH)</br>9043/tcp from any source</br>9060/tcp from any source</br>9080/tcp from any source</br>9443/tcp from any source  |
| Collective | ICMP (type: 8; code: any) from any source (allows ping; optional)</br>22/tcp from any source (allows SSH)</br>9043/tcp from any source</br>9080/tcp from any source |
{: caption="Table 4. Inbound rules for cell and collective topologies in one shared security group" caption-side="top"}

### Two separate security groups
{: two-sg}

Two separate security groups have a more complicated configuration. The easiest solution is to permit all traffic (all protocols and all ports) from the CIDR block of the subnet in which the other VSIs reside. For example, the SG for the primary VSI in the perimeter (DMZ) subnet should permit all traffic from the CIDR block of the internal subnet.

| Topology | Security group | Inbound rules |
| --- | --- | --- |
| Cell | Primary/perimeter VSI SG | ICMP (type: 8; code: any) from any source (allows ping; optional)</br>22/tcp from any source (allows SSH)</br>9043/tcp from any source</br>9060/tcp from any source</br>9080/tcp from any source</br>9443/tcp from any source</br>ALL from **{internal subnet}** |
| Cell | Secondary/internal VSI(s) SG | ALL from **{perimeter subnet}** |
| Collective | Primary/perimeter VSI SG | ICMP (type: 8; code: any) from any source (allows ping; optional)</br>22/tcp from any source (allows SSH)</br>9043/tcp from any source</br>9080/tcp from any source</br>ALL from **{internal subnet}** |
| Collective | Secondary/internal VSI(s) SG | ALL from **{perimeter subnet}** |
{: caption="Table 5. Inbound rules for cell and collective topologies in two separate security groups" caption-side="top"}

## Install and reconfigure access control lists and security groups
{: reconfig}

Install the product and then reconfigure access control lists and security groups.

### Install the product
{: instprodsn}

[Install the WebSphere product](/docs/was-for-vsi?topic=was-for-vsi-getting-started#websphere-installation-procedure).

If you set ACLs to use the [hard way](#the-hard-way-to-configure-acls), installation can take much longer than usual.
{: note}

### Reconfigure access control lists
{: rec-acl}

After installation you can remove the rules to permit SSH, provided you don't intend to manage your servers from the command line with SSH. You also can remove the rules to restrict HTTP or HTTPS, provided your VSIs don't need internet connectivity. At minimum, you need the following rules:

| Subnet access control list | Rules |
| --- | --- |
| Perimeter inbound | allow ALL from **{internal subnet}** to ANY</br>allow TCP from ANY to ANY:22 (SSH)</br>allow TCP from ANY to ANY:**{required ports}** |
| Perimeter outbound | allow ALL from ANY to ANY |
| Internal inbound | allow ALL from **{perimeter subnet}** to ANY |
| Internal outbound | allow ALL from ANY to **{perimeter subnet}** |
{: caption="Table 6. Rules for reconfigured subnet ACLs" caption-side="top"}

For **required ports**, see [Enable connection to the admin panel](#enable-connection-to-the-admin-panel). You must permit connections to the administrative console and permit traffic to ports on which your application is served (8080, 8443, and so on).

### Reconfigure security groups
{: rec-sg}

After installation, you can remove the rules to permit SSH, provided you don't intend to manage your servers from the command line with SSH. At minimum, you need the following rules:

| Topology | Security group | Inbound rules |
| --- | --- | --- |
| Cell | Primary/perimeter VSI SG | 9043/tcp from any source</br>9060/tcp from any source</br/>9080/tcp from any source</br>9443/tcp from any source</br>**{required ports}** from any source</br>ALL from **{internal subnet}** |
| Cell | Secondary/internal VSIs SG | ALL from **{perimeter subnet}** |
| Collective | Primary/perimeter VSI SG | 9043/tcp from any source</br>9080/tcp from any source</br>**{required ports}** from any source</br>ALL from **{internal subnet}** |
| Collective | Secondary/internal VSIs SG | ALL from **{perimeter subnet}** |
{: caption="Table 7. Inbound rules for reconfigured SGs in Cell and Collective topologies" caption-side="top"}

For the reconfigured SG inbound rules, **required ports** refers to ports on which your application is served (8080, 8443, and so on).

### Remove the public gateway
{: rem-gw}

If you don't need your internal subnet to have internet connectivity and have reconfigured the ACL and SGs for it, also remove the public gateway. Without a public gateway the VSIs are isolated from the internet.
