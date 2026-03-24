# Define AD DS

AD DS and its related services form the foundation for enterprise networks that run Windows operating systems. The AD DS database is the central store of all the domain objects, such as user accounts, computer accounts, and groups. AD DS provides a searchable, hierarchical directory and a method for applying configuration and security settings for objects in an enterprise.

AD DS includes both logical and physical components. You should understand how AD DS components work together so that you can manage your infrastructure efficiently. In addition, you can use AD DS options to perform actions such as:

- Installing, configuring, and updating apps.
- Managing the security infrastructure.
- Enabling Remote Access Service and DirectAccess.
- Issuing and managing digital certificates.

---

## What Are the Logical Components?

AD DS logical components are structures that you use to implement an AD DS design that is appropriate for an organization. The following table describes the types of logical components that an AD DS database contains.

| Logical Component | Description |
|-------------------|-------------|
| **Partition** | A partition, or naming context, is a portion of the AD DS database. Although the database consists of one file named Ntds.dit, different partitions contain different data. For example, the schema partition contains a copy of the Active Directory schema. The configuration partition contains the configuration objects for the forest, and the domain partition contains the users, computers, groups, and other objects specific to the domain. Active Directory stores copies of partitions on multiple domain controllers and updates them through directory replication. |
| **Schema** | A schema is the set of definitions of the object types and attributes that you use to define the objects created in AD DS. |
| **Domain** | A domain is a logical administrative container for objects such as users and computers. A domain maps to a specific partition and you can organize the domain with parent-child relationships to other domains. |
| **Domain tree** | A domain tree is a hierarchical collection of domains that share a common root domain and a contiguous Domain Name System (DNS) namespace. |
| **Forest** | A forest is a collection of one or more domains that have a common AD DS root, a common schema, and a common global catalog. |
| **OU** | An OU is a container object for users, groups, and computers that provides a framework for delegating administrative rights and administration by linking Group Policy Objects (GPOs). |
| **Container** | A container is an object that provides an organizational framework for use in AD DS. You can use the default containers, or you can create custom containers. You can't link GPOs to containers. |

---

## What Are the Physical Components?

Physical components in AD DS are those objects that are tangible, or that described tangible components in the real world.

<img width="842" height="562" alt="image" src="https://github.com/user-attachments/assets/8bf9bcd1-0c07-4d76-adfc-dc195944bd20" />

The following table describes some of the physical components of AD DS.

| Physical Component | Description |
|--------------------|-------------|
| **Domain controller** | A domain controller contains a copy of the AD DS database. For most operations, each domain controller can process changes and replicate the changes to all the other domain controllers in the domain. |
| **Data store** | A copy of the data store exists on each domain controller. The AD DS database uses Microsoft Jet database technology and stores the directory information in the Ntds.dit file and associated log files. The C:\Windows\NTDS folder stores these files by default. |
| **Global catalog server** | A global catalog server is a domain controller that hosts the global catalog, which is a partial, read-only copy of all the objects in a multiple-domain forest. A global catalog speeds up searches for objects that might be stored on domain controllers in a different domain in the forest. |
| **Read-only domain controller (RODC)** | An RODC is a special read only installation of AD DS. RODCs are common in branch offices where physical security isn't optimal, IT support is less advanced than in the main corporate centers, or line-of-business applications need to run on a domain controller. |
| **Site** | A site is a container for AD DS objects, such as computers and services that are specific to a physical location. This is in comparison to a domain, which represents the logical structure of objects, such as users and groups, in addition to computers. |
| **Subnet** | A subnet is a portion of the network IP addresses of an organization assigned to computers in a site. A site can have more than one subnet. |

---

## Define Users, Groups, and Computers

In addition to the high-level components and objects, AD DS contains other objects such as users, groups, and computers.

---

## Create User Objects

In AD DS, you must provide all users that require access to network resources with a user account. With this user account, users can authenticate to the AD DS domain and access network resources.

In Windows Server, a user account is an object that contains all the information that defines a user. A user account includes:

- The username.
- A user password.
- Group memberships.

A user account also contains settings that you can configure based on your organizational requirements.

<img width="1043" height="641" alt="image" src="https://github.com/user-attachments/assets/782a7c7b-f837-4e15-b424-ca6ca349b050" />

The username and password of a user account serve as the user's sign-in credentials. A user object also includes several other attributes that describe and manage the user. You can use the following to create and manage user objects in AD DS:

- Active Directory Administrative Center.
- Active Directory Users and Computers.
- Windows Admin Center.
- Windows PowerShell.
- The dsadd command-line tool.

---

## What Are Managed Service Accounts?

Many apps contain services that you install on the server that hosts the program. These services typically run at server startup or are triggered by other events. Services often run in the background and don't require any user interaction. For a service to start up and authenticate, you use a service account. A service account might be an account that is local to the computer, such as the built-in Local Service, Network Service, or Local System accounts. You also can configure a service account to use a domain-based account located in AD DS.

To help centralize administration and to meet program requirements, many organizations choose to use a domain-based account to run program services. While this does provide some benefit over using a local account, there are a number of associated challenges, such as the following:

- Extra administration effort might be necessary to manage the service account password securely.
- It can be difficult to determine where a domain-based account is being used as a service account.
- Extra administration effort might be necessary to manage the service principal name (SPN).

Windows Server supports an AD DS object, named a managed service account, which you use to facilitate service-account management. A managed service account is an AD DS object class that enables:

- Simplified password management.
- Simplified SPN management.

---

## What Are Group Managed Service Accounts?

Group Managed Service Accounts (gMSA) enable you to extend the capabilities of standard managed service accounts to more than one server in your domain. In server farm scenarios with Network Load Balancing (NLB) clusters or IIS servers, there often is a need to run system or program services under the same service account. Standard managed service accounts can't provide managed service account functionality to services that are running on more than one server. By gMSA, you can configure multiple servers to use the same managed service account and still retain the benefits that managed service accounts provide, like automatic password maintenance and simplified SPN management.

To support group managed service account functionality, your environment must meet the following requirement:

- You must create a KDS root key on a domain controller in the domain.

To create the KDS root key, run the following command from the Active Directory Module for Windows PowerShell on a Windows Server domain controller:

```powershell
Add-KdsRootKey –EffectiveImmediately
```
You create group managed service accounts by using New-ADServiceAccount Windows PowerShell cmdlet with the –PrinicipalsAllowedToRetrieveManagedPassword parameter.

For example:

```powershell
New-ADServiceAccount -Name LondonSQLFarm -PrincipalsAllowedToRetrieveManagedPassword SEA-SQL1, SEA-SQL2, SEA-SQL3
```

# What Are Delegated Managed Service Accounts?

Windows Server 2025 introduces a new type of service account called the Delegated Managed Service Account (dMSA). The dMSA account type enables users to transition from traditional service accounts to machine accounts that have managed and fully randomized keys, while also disabling the original service account passwords. Authentication for dMSA is linked to the device identity, which means that only specified machine identities mapped in AD can access the account. By using dMSA, users can prevent the common issue of credential harvesting using a compromised account that is associated with traditional service accounts.

dMSAs and gMSAs are two types of managed service accounts that are used to run services and applications in Windows Server. A dMSA is managed by an administrator and is used to run a service or application on a specific server. A gMSA is managed by AD and is used to run a service or application on multiple servers. Other differences include:

- Utilizing gMSA concepts to limit scope of usage using Credential Guard to bind machine authentication.
- Credential Guard can be used to enhance security in dMSA by automatically rotating passwords and binding all service account tickets. Legacy accounts are then disabled to further improve security.
- Although gMSAs are secured with machine generate and autorotated passwords, the passwords are still not machine bound and can be stolen.

---

## What Are Group Objects?

Although it might be practical to assign permissions and rights to individual user accounts in small networks, this becomes impractical and inefficient in large enterprise networks.

For example, if several users need the same level of access to a folder, it's more efficient to create a group that contains the required user accounts, and then assign the required permissions to the group.

> Tip: As an added benefit, you can change users' file permissions by adding or removing them from groups rather than editing the file permissions directly.

Before you implement groups in your organization, you must understand the scope of various AD DS group types. In addition, you must understand how to use group types to manage access to resources or to assign management rights and responsibilities.

---

## Group Types

In a Windows Server enterprise network, there are two types of groups, described in the following table.

| Group Type | Description |
|------------|-------------|
| **Security** | Security groups are security-enabled, and you use them to assign permissions to various resources. You can use security groups in permission entries in access control lists (ACLs) to help control security for resource access. If you want to use a group to manage security, it must be a security group. |
| **Distribution** | Email applications typically use distribution groups, which aren't security-enabled. You also can use security groups as a means of distribution for email applications. |

> Note: When you create a group, you choose the group type and scope. The group type determines the capabilities of the group.

---

## Group Scopes

Windows Server supports group scoping. The scope of a group determines both the range of a group's abilities or permissions and the group membership. There are four group scopes.

**Local.** You use this type of group for standalone servers or workstations, on domain-member servers that aren't domain controllers, or on domain-member workstations. Local groups are available only on the computer where they exist. The important characteristics of a local group are:

- You can assign abilities and permissions on local resources only, meaning on the local computer.
- Members can be from anywhere in the AD DS forest.

**Domain-local.** You use this type of group primarily to manage access to resources or to assign management rights and responsibilities. Domain-local groups exist on domain controllers in an AD DS domain, and so, the group's scope is local to the domain in which it resides. The important characteristics of domain-local groups are:

- You can assign abilities and permissions on domain-local resources only, which means on all computers in the local domain.
- Members can be from anywhere in the AD DS forest.

**Global.** You use this type of group primarily to consolidate users who have similar characteristics. For example, you might use global groups to join users who are part of a department or a geographic location. The important characteristics of global groups are:

- You can assign abilities and permissions anywhere in the forest.
- Members can be from the local domain only and can include users, computers, and global groups from the local domain.

**Universal.** You use this type of group most often in multidomain networks because it combines the characteristics of both domain-local groups and global groups. Specifically, the important characteristics of universal groups are:

- You can assign abilities and permissions anywhere in the forest similar to how you assign them for global groups.
- Members can be from anywhere in the AD DS forest.

---

## What Are Computer Objects?

Computers, like users, are security principals, in that:

- They have an account with a sign-in name and password that Windows changes automatically on a periodic basis.
- They authenticate with the domain.
- They can belong to groups and have access to resources, and you can configure them by using Group Policy.

A computer account begins its lifecycle when you create the computer object and join it to your domain. After you join the computer account to your domain, day-to-day administrative tasks include:

- Configuring computer properties.
- Moving the computer between OUs.
- Managing the computer itself.
- Renaming, resetting, disabling, enabling, and eventually deleting the computer object.

---

## Computers Container

Before you create a computer object in AD DS, you must have a place to put it. The Computers container is a built-in container in an AD DS domain. This container is the default location for the computer accounts when a computer joins the domain.

This container isn't an OU. Instead, it's an object of the Container class. Its common name is CN=Computers. There are subtle but important differences between a container and an OU. You can't create an OU within a container, so you can't subdivide the Computers container. You also can't link a Group Policy Object to a container. Therefore, we recommend that you create custom OUs to host computer objects, instead of using the Computers container.

---

# Use Cases and Solutions

## Logical Component Design Use Cases

- **Multi-domain enterprise structure** - Organizations need to design hierarchical Active Directory structures across multiple geographic locations
  - *Solution:* Implement domain trees with parent-child relationships sharing contiguous DNS namespaces, organize into forests with common schema and global catalog, and use OUs for administrative delegation and GPO linking
  - *Reference:* [Active Directory Domain Services Overview](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview)

- **Schema customization requirements** - Applications require custom object types or attributes in Active Directory
  - *Solution:* Extend the AD DS schema to define new object types and attributes, understanding that schema changes affect the entire forest

---

## Physical Component Deployment Use Cases

- **Branch office security concerns** - Remote locations lack optimal physical security and advanced IT support
  - *Solution:* Deploy Read-only Domain Controllers (RODCs) in branch offices to provide local authentication while minimizing security risks from physical compromise
  - *Reference:* [RODC Planning and Deployment](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/read-only-domain-controller-rodc-planning-and-deployment)

- **Multi-domain search optimization** - Users need to find objects across different domains efficiently
  - *Solution:* Configure Global Catalog servers to host partial, read-only copies of all forest objects, speeding up cross-domain searches

- **Site-based replication optimization** - Domain controllers need efficient replication across WAN links
  - *Solution:* Define sites and subnets to align AD DS replication with physical network topology, ensuring efficient directory replication between domain controllers

---

## User Management Use Cases

- **Centralized user administration** - IT teams need efficient tools for managing user accounts
  - *Solution:* Create user objects using Active Directory Administrative Center, Active Directory Users and Computers, Windows Admin Center, Windows PowerShell, or dsadd command-line tool based on administrative preference and automation needs

- **Service account security** - Traditional service accounts create password management and SPN administration challenges
  - *Solution:* Implement Managed Service Accounts (MSAs) for simplified password and SPN management on single servers

- **Server farm service accounts** - Load-balanced applications need the same service account across multiple servers
  - *Solution:* Deploy Group Managed Service Accounts (gMSAs) by creating KDS root keys and using New-ADServiceAccount with -PrincipalsAllowedToRetrieveManagedPassword for automatic password maintenance across multiple servers
  - *Reference:* [Group Managed Service Accounts](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/group-managed-service-accounts)

- **Credential theft prevention** - Compromised service accounts enable credential harvesting attacks
  - *Solution:* Transition to Delegated Managed Service Accounts (dMSAs) in Windows Server 2025, binding authentication to device identity with Credential Guard, automatically rotating passwords, and disabling legacy accounts

### Group Management Use Cases

- **Efficient permission management** - Individual user permission assignment becomes unwieldy at scale
  - *Solution:* Create Security groups to consolidate users with similar access needs, assigning permissions to groups rather than individuals, and manage access by adding/removing users from groups
  - *Reference:* [Active Directory Groups](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/active-directory-groups)

- **Email distribution lists** - Organizations need non-security-enabled groups for email applications
  - *Solution:* Create Distribution groups for email distribution, noting that Security groups can also serve this dual purpose

- **Resource access control** - Administrators need to manage access to resources within a single domain
  - *Solution:* Use Domain-local groups to assign permissions on domain-local resources, with members from anywhere in the forest

- **User consolidation across domains** - Multi-domain environments need to consolidate similar users
  - *Solution:* Implement Global groups to join users from the same domain (department, location), then assign permissions anywhere in the forest

- **Enterprise-wide permission management** - Large organizations need flexible groups spanning multiple domains
  - *Solution:* Deploy Universal groups combining domain-local and global characteristics, with forest-wide permission assignment and forest-wide membership capabilities

### Computer Management Use Cases

- **Computer lifecycle management** - IT teams need to manage computer objects from creation to decommissioning
  - *Solution:* Create computer objects and join to domain, then perform day-to-day tasks: configure properties, move between OUs, rename, reset, disable, enable, and eventually delete computer objects
  - *Reference:* [Computer Accounts](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/computer-accounts)

- **Group Policy application** - Organizations need to apply configuration and security settings to computers
  - *Solution:* Avoid the default Computers container (which cannot host OUs or link GPOs), and instead create custom OUs to host computer objects for proper Group Policy management

---

## References

- [Active Directory Domain Services Overview](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview)
- [Active Directory Schema](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/active-directory-schema)
- [Understanding Domains](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/understanding-domains)
- [Understanding Forests](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/understanding-forests)
- [Organizational Units](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/organizational-units)
- [RODC Planning and Deployment](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/read-only-domain-controller-rodc-planning-and-deployment)
- [Group Managed Service Accounts](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/group-managed-service-accounts)
- [Active Directory Groups](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/active-directory-groups)
- [Computer Accounts](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/computer-accounts)
