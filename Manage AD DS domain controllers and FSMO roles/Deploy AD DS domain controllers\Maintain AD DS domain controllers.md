# Deploy AD DS Domain Controllers

## Deploy AD DS Domain Controllers in an On-Premises Environment

The domain controller deployment process has two steps. First, you install the binaries necessary to implement the domain controller role. For this purpose, you can use Windows Admin Center or Server Manager. At the end of the initial installation process, you have installed the AD DS files, but not yet configured AD DS on the server. The second step is to configure AD DS role. The simplest way to perform this configuration is by using the Active Directory Domain Services Configuration Wizard. You start the wizard by selecting the AD DS link in Server Manager.

As part of AD DS role configuration, you need to provide answers to the questions in the following table:

| Question | Comments |
|----------|----------|
| Are you installing a new forest, a new tree, or an additional domain controller for an existing domain? | Answering this question determines what additional information you might need, such as the parent domain name. |
| What is the Domain Name System (DNS) name for the AD DS domain? | When you create the first domain controller for a domain, you must specify the fully qualified domain name (FQDN). When you add a domain controller to an existing domain or forest, you use the existing domain name. |
| Which level will you choose for the forest functional level? | The forest functional level determines the available forest features and the supported domain controller operating system (OS). This also sets the minimum domain functional level for the domains in the forest. |
| Which level will you choose for the domain functional level? | The domain functional level determines the domain features that will be available and the supported domain controller operating systems. |
| Will the domain controller be a DNS server? | You can install the DNS role as part of the domain controller deployment. |
| Will the domain controller host the global catalog? | This option is selected by default. |
| Will the domain controller be a read-only domain controller (RODC)? | This option isn't available for the first domain controller in a forest. |
| What will be the Directory Services Restore Mode (DSRM) password? | This is necessary for restoring AD DS database objects from a backup. |
| What is the NetBIOS name for the AD DS domain? | When you create the first domain controller for a domain, you must specify the NetBIOS name for the domain. |
| Where will the database, log files and SYSVOL folders be created? | By default, the database and log files folder is located at `C:\Windows\NTDS`. By default, the SYSVOL folder is located at `C:\Windows\SYSVOL`. |

<img width="665" height="464" alt="image" src="https://github.com/user-attachments/assets/f5620ba6-371d-4cb3-90f3-e04bfda88f7b" />
A screenshot of the Active Directory Domain Services Configuration Wizard Deployment Configuration page is set to add a domain controller to an existing domain.

## Install a Domain Controller on a Server Core Installation of Windows Server

A Windows Server computer that is running a Server Core installation doesn't have the Server Manager graphical user interface (GUI). Therefore, you must use alternative methods to install the files for the domain controller role, and to install the domain controller role itself. You can use Windows Admin Center, Server Manager, Windows PowerShell, or Remote Server Administration Tools (RSAT) installed on any supported version of Windows Server that has the Desktop Experience feature, or any supported Windows client such as Windows 10.

## Install a Domain Controller from Media

If you have a network connection between sites that is slow, unreliable, or costly, you might find it beneficial to add another domain controller at a remote location or branch office. In this scenario, to significantly reduce the amount of traffic moving over the wide area network (WAN) link, you can create an AD DS backup (perhaps to a USB drive) and take this backup to the remote location. When you're at the remote location and run Server Manager to install AD DS, you can select the Install from media option. Most of the copying occurs locally. In this scenario, the WAN link transfers only security-related traffic and AD DS changes following the backup. The WAN link also helps ensure that the new domain controller receives any changes made to the central AD DS after you created the Install from media backup.

## Branch Office Considerations

When you deploy a domain controller in a branch office that can't guarantee physical security, you can use additional measures to reduce the impact of a security breach. One option is to deploy an RODC. The RODC contains a read-only copy of the AD DS database, and by default, it doesn't cache any user passwords. However, you can configure the RODC to cache the passwords for users in the branch office. If an RODC is compromised, the potential loss of information risk is lower than with a full read/write domain controller.

## Upgrade Domain Controllers from the Previous Version

The process for upgrading a domain controller is the same for any version of Windows Server starting with Windows Server 2012 R2 through Windows Server 2025. You can upgrade to a Windows Server 2025 domain using either of the following methods:

- Upgrade the OS on existing domain controllers that are running Windows Server 2012 R2 or later.
- Add servers running Windows Server 2025 as domain controllers in a domain that already has domain controllers running earlier Windows Server versions.

We recommend the latter method, because when you finish you have a clean installation of both the Windows Server 2025 OS and the AD DS database. Whenever you add a new domain controller, Windows Server automatically updates the domain DNS records so clients are able to locate and use this domain controller.

## Deploy AD DS Domain Controllers in Azure Virtual Machines (VMs)

Azure provides infrastructure as a service (IaaS), which is a cloud-based virtualization platform. When deploying AD DS on Azure IaaS, you're installing the domain controller on a VM, so all the rules that apply to virtualizing a domain controller apply to deploying AD DS in Azure.

When you implement AD DS in Azure, consider the following:

- **Network topology.** To meet AD DS requirements, you must create an Azure Virtual Network and attach your VMs to it. If you intend to join an existing on-premises AD DS infrastructure, you can extend network connectivity to your on-premises environment. You can achieve this through hybrid connectivity methods such as a virtual private network (VPN) connection or an Azure ExpressRoute circuit, depending on the speed, reliability, and security that your organization requires.
- **Site topology.** As with a physical site, you should define and configure an AD DS site that corresponds to the IP address space of your Azure Virtual Network.
- **IP addressing.** All Azure VMs receive Dynamic Host Configuration Protocol (DHCP) addresses by default, but you can configure static addresses that will persist across restarts and shutdowns.
- **DNS.** Azure's built-in DNS does not meet the requirements of AD DS, such as Dynamic DNS and service (SRV) resource records. To provide DNS functionality for an AD DS environment in Azure, you can use the Windows Server DNS server role or other DNS solutions available in Azure, such as Azure private DNS zones.
- **Disks.** You have control of caching Azure VM disk configurations. When you install AD DS to an Azure VM, you should place the NTDS.DIT and SYSVOL files on one of its data disks, and set the Host Cache Preference setting of that disk to NONE.

---

# Maintain AD DS Domain Controllers

There are operational aspects applicable to every AD DS environment that focus on maintaining business continuity of the authentication services. This includes backup and recovery of domain controllers, and the AD DS objects they host.

## Maintain AD DS Domain Controller Availability

Domain controllers use a multi-master replication process to copy data from one domain controller to another. As a best practice, an AD DS domain should have at least two domain controllers per AD DS site. This makes the AD DS database more available and spreads the authentication load during peak sign-in times.

&gt; **Important**
&gt; 
&gt; For most enterprises, consider two domain controllers per geographical region as the absolute minimum to help ensure high availability and performance.

## Plan for AD DS Backup and Restore

Maintaining the reliability of the Active Directory data is important. Performing regular backups can play a part in this process but knowing how to restore or recover data after a failure is vital.

### Restoring Deleted AD DS Objects by Using Recycle Bin

Because restoring objects deleted from AD DS by using traditional backup methods involves temporary OS downtime, Windows Server offers the Active Directory Recycle Bin feature, which provides a straightforward method to restore deleted objects with no AD DS downtime. After you enable Active Directory Recycle Bin, the Deleted Objects container displays in Active Directory Administrative Center. Deleted objects persist in this container until their deleted object lifetime expires. For new AD DS deployments, that lifetime is set to 180 days, but you have the option to change it. You can choose to restore the objects either to their original location or to an alternate location within AD DS.

&gt; **Important**
&gt; 
&gt; You can't use Active Directory Recycle Bin to revert changes to existing objects. In such cases, you must use the traditional methods of backing up and restoring AD DS.

### AD DS Backup and Restore

To restore AD DS, a backup must explicitly include system state data. System state is a collection of critical OS and server role files that include the AD DS database and the registry.

&gt; **Important**
&gt; 
&gt; A full server backup that is used for full server recovery doesn't support this scenario.

To perform an AD DS restore, you must have full access to the files on the domain controller. This requires restarting the domain controller in DSRM. If you're restarting a domain controller locally, open the advanced startup options and choose the DSRM from the menu.

When you start a domain controller in DSRM, you sign in as Administrator with the DSRM password. You then can use Windows Server Backup to restore the directory database. After completing the restore process, you must restart the server you're recovering. The domain controller ensures that its database is consistent with the rest of the domain by pulling from its replication partners the changes to the directory that have occurred since the date of the backup.

### Nonauthoritative Restore

By default, you restore an AD DS backup as of a known good date. Essentially, you roll the domain controller back in time. When AD DS restarts on the domain controller, the domain controller contacts its replication partners and requests all subsequent updates. In other words, the domain controller catches up with the rest of the domain by using standard replication mechanisms.

This type of restore is useful when the directory on a domain controller has been damaged or corrupted, but the problem has not spread to other domain controllers. However, in some scenarios, this approach is not suitable. For example, this won't enable you to recover an object you deleted after the backup took place, if that deletion has replicated to other domain controllers. If you restore a known good version of AD DS and restart the domain controller, the deletion that happened after the backup took place will simply replicate back to the domain controller.

### Authoritative Restore

An authoritative restore allows you to restore a known good copy of AD DS objects, which replaces the current version of these objects in the AD DS database. In an authoritative restore, you start with the same sequence of steps as the nonauthoritative restore. However, before you restart the domain controller, you mark the restored objects that you want to persist as authoritative, so they'll replicate from the restored domain controller outbound to its replication partners.
