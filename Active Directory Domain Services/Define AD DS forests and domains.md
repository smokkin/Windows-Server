# Define AD DS Forests and Domains

An AD DS forest is a collection of one or more AD DS trees that contain one or more AD DS domains. Domains in a forest share:

- A common root.
- A common schema.
- A global catalog.

An AD DS domain is a logical administrative container for objects such as:

- Users
- Groups
- Computers

---

## What Is an AD DS Forest?

A forest is a top-level container in AD DS. Each forest is a collection of one or more domain trees that share a common directory schema and a global catalog. A domain tree is a collection of one or more domains that share a contiguous namespace. The forest root domain is the first domain that you create in the forest.

The forest root domain contains objects that don't exist in other domains in the forest. Because you always create these objects on the first domain controller, a forest can consist of as few as one domain with a single domain controller, or it can consist of several domains across multiple domain trees.

<img width="1845" height="1048" alt="image" src="https://github.com/user-attachments/assets/85c8c1a8-28ed-4f2b-bb28-07b501fb2383" />

The following objects exist in the forest root domain:

- The schema master role.
- The domain naming master role.
- The Enterprise Admins group.
- The Schema Admins group.

> Note: Although the schema and domain naming master roles are assigned initially in the root domain on the first domain controller you create, you can move them to other domain controllers anywhere in the forest.

An AD DS forest is often described as:

- **A security boundary.** By default, no users from outside the forest can access any resources inside the forest. In addition, all the domains in a forest automatically trust the other domains in the forest. This makes it easy to enable access to resources for all the users in a forest, regardless of the domain to which they belong.
- **A replication boundary.** An AD DS forest is the replication boundary for the configuration and schema partitions in the AD DS database. Therefore, organizations that want to deploy applications with incompatible schemas must deploy additional forests. The forest is also the replication boundary for the global catalog. The global catalog makes it possible to find objects from any domain in the forest.

> Tip: Typically, an organization creates only one forest.

The following objects exist in each domain (including the forest root):

- The RID master role.
- The Infrastructure master role.
- The PDC emulator master role.
- The Domain Admins group.

---

## What Is an AD DS Domain?

An AD DS domain is a logical container for managing user, computer, group, and other objects. The AD DS database stores all domain objects, and each domain controller stores a copy of the database.

<img width="2025" height="1027" alt="image" src="https://github.com/user-attachments/assets/557516d6-aad5-4357-babe-55d2abed9a50" />

The most commonly used objects are described in the following table:

| Object | Description |
|--------|-------------|
| **User accounts** | User accounts contain information about users, including the information required to authenticate a user during the sign-in process and build the user's access token. |
| **Computer accounts** | Each domain-joined computer has an account in AD DS. You can use computer accounts for domain-joined computers in the same way that you use user accounts for users. |
| **Groups** | Groups organize users or computers to simplify the management of permissions and Group Policy Objects in the domain. |

> Note: AD DS allows a single domain to contain nearly two billion objects. This means that most organizations need only deploy a single domain.

An AD DS domain is often described as:

- **A replication boundary.** When you make changes to any object in the domain, the domain controller where the change occurred replicates that change to all other domain controllers in the domain. If multiple domains exist in the forest, only subsets of the changes replicate to other domains. AD DS uses a multi-master replication model that allows every domain controller to make changes to objects in the domain.
- **An administrative unit.** The AD DS domain contains an Administrator account and a Domain Admins group. By default, the Administrator account is a member of the Domain Admins group, and the Domain Admins group is a member of every local Administrators group of domain-joined computers. Also, by default, the Domain Admins group members have full control over every object in the domain.

> Note: The Administrator account in the forest root domain has additional rights.

An AD DS domain provides:

- **Authentication.** Whenever a domain-joined computer starts or a user signs in to a domain-joined computer, AD DS authenticates it. Authentication verifies that computer or user has the proper identity in AD DS by verifying its credentials.
- **Authorization.** Windows uses authorization and access control technologies to determine whether to allow authenticated users to access resources.

> Tip: Organizations with decentralized administrative structures or multiple locations might consider implementing multiple domains in the same forest to accommodate their administrative needs.

---

## What Are Trust Relationships?

AD DS trusts enable access to resources in a complex AD DS environment. When you deploy a single domain, you can easily grant access to resources within the domain to users and groups from the domain. When you implement multiple domains or forests, you should ensure that the appropriate trusts are in place to enable the same access to resources.

In a multiple-domain AD DS forest, two-way transitive trust relationships generate automatically between AD DS domains so that a path of trust exists between all the AD DS domains.

> Note: The trusts that create automatically in the forest are all transitive trusts, which means that if domain A trusts domain B, and domain B trusts domain C, then domain A trusts domain C.

You can deploy other types of trusts. The following table describes the main trust types.

| Trust Type | Direction | Description |
|------------|-----------|-------------|
| **Parent and child** | Transitive | Two-way | When you add a new AD DS domain to an existing AD DS tree, you create new parent and child trusts. |
| **Tree-root** | Transitive | Two-way | When you create a new AD DS tree in an existing AD DS forest, you automatically create a new tree-root trust. |
| **External** | Nontransitive | One-way or two-way | External trusts enable resource access with a Windows NT 4.0 domain or an AD DS domain in another forest. You also can set these up to provide a framework for a migration. |
| **Realm** | Transitive or nontransitive | One-way or two-way | Realm trusts establish an authentication path between a Windows Server AD DS domain and a Kerberos version 5 (v5) protocol realm that implements by using a directory service other than AD DS. |
| **Forest (complete or selective)** | Transitive | One-way or two-way | Trusts between AD DS forests allow two forests to share resources. |
| **Shortcut** | Nontransitive | One-way or two-way | Configure shortcut trusts to reduce the time taken to authenticate between AD DS domains that are in different parts of an AD DS forest. No shortcut trusts exist by default, and an administrator must create them if they're required. |

When you set up trusts between domains within the same forest, across forests, or with an external realm, Windows Server creates a trusted domain object to store the trusts' information, such as transitivity and type, in AD DS. Windows Server stores this trusted domain object in the System container in AD DS.

---

## Define OUs

An OU is a container object within a domain that you can use to consolidate users, computers, groups, and other objects. You can link Group Policy Objects (GPOs) directly to an OU to manage the users and computers contained in the OU. You can also assign an OU manager and associate a COM+ partition with an OU.

You can create new OUs in AD DS by using:

- Active Directory Administrative Center.
- Active Directory Users and Computers.
- Windows Admin Center.
- Windows PowerShell with the Active Directory PowerShell module.

---

## Why Create OUs?

There are two reasons to create an OU:

1. **To consolidate objects to make it easier to manage them by applying GPOs to the collective.** When you assign GPOs to an OU, the settings apply to all the objects within the OU. GPOs are policies that administrators create to manage and configure settings for computers or users. You deploy the GPOs by linking them to OUs, domains, or sites.
2. **To delegate administrative control of objects within the OU.** You can assign management permissions on an OU, thereby delegating control of that OU to a user or a group within AD DS, in addition to the Domain Admins group.

You can use OUs to represent the hierarchical, logical structures within your organization. For example, you can create OUs that represent the departments within your organization, the geographic regions within your organization, or a combination of both departmental and geographic regions. You can use OUs to manage the configuration and use of user, group, and computer accounts based on your organizational model.

---

## What Are the Generic Containers?

AD DS has several built-in containers, or generic containers, such as Users and Computers. These containers store system objects or function as the default parent objects to new objects that you create. Don't confuse these generic container objects with OUs. The primary difference between OUs and containers is the management capabilities. Containers have limited management capabilities. For example, you can't apply a GPO directly to a container.

<img width="842" height="562" alt="image" src="https://github.com/user-attachments/assets/4a7b898d-0e63-4542-a01e-72a435f63923" />

Installing AD DS creates the Domain Controllers OU and several generic container objects by default. AD DS primarily uses some of these default objects, which are also hidden by default. The following objects are displayed by default:

| Object | Description |
|--------|-------------|
| **Domain** | The top level of the domain organizational hierarchy. |
| **Builtin container** | A container that stores several default groups. |
| **Computers container** | The default location for new computer accounts that you create in the domain. |
| **Foreign Security Principals container** | The default location for trusted objects from domains outside the local AD DS domain that you add to a group in the local AD DS domain. |
| **Managed Service Accounts container** | The default location for managed service accounts. AD DS provides automatic password management in managed service accounts. |
| **Users container** | The default location for new user accounts and groups that you create in the domain. The Users container also holds the administrator, the guest accounts for the domain, and some default groups. |
| **Domain Controllers OU** | The default location for domain controllers' computer accounts. This is the only OU that is present in a new installation of AD DS. |

There are several containers that you can review when you select Advanced Features. The following table describes the objects that are hidden by default.

| Object | Description |
|--------|-------------|
| **LostAndFound** | This container holds orphaned objects. |
| **Program Data** | This container holds Active Directory data for Microsoft applications, such as Active Directory Federation Services (AD FS). |
| **System** | This container holds the built-in system settings. |
| **NTDS Quotas** | This container holds directory service quota data. |
| **TPM Devices** | This container stores the recovery information for Trusted Platform Module (TPM) devices. |

> Note: Containers in an AD DS domain can't have GPOs linked to them. To link GPOs to apply configurations and restrictions, create a hierarchy of OUs and then link the GPOs to them.

---

## Use a Hierarchical Design

The administrative needs of the organization dictate the design of an OU hierarchy. Geographic, functional, resource, or user classifications could all influence the design. Whatever the order, the hierarchy should make it possible to administer AD DS resources as effectively and flexibly as possible. For example, if you need to configure all IT administrators' computers in a certain way, you can group all the computers in an OU and then assign a GPO to manage those computers.

You also can create OUs within other OUs. For example, your organization might have multiple offices, each with its own IT administrator who is responsible for managing user and computer accounts. In addition, each office might have different departments with different computer-configuration requirements. In this situation, you can create an OU for each office, and then within each of those OUs, create an OU for the IT administrators and an OU for each of the other departments.

Although there's no limit to the number of levels in your OU structure, limit your OU structure to a depth of no more than 10 levels to ensure manageability. Most organizations use five levels or fewer to simplify administration.

> Note: Applications that work with AD DS can impose restrictions on the OU depth within the hierarchy for the parts of the hierarchy that they use.

---

## Use Cases and Solutions

### Forest Design Use Cases

- **Application schema conflicts** - Organizations need to deploy applications with incompatible schemas
  - *Solution:* Deploy additional forests, as the forest is the replication boundary for configuration and schema partitions; each forest maintains independent schemas
  - *Reference:* [Understanding Forests](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/understanding-forests)

- **Cross-forest resource sharing** - Two organizations need to share resources between separate AD DS forests
  - *Solution:* Establish Forest trusts (complete or selective) between AD DS forests, which are transitive and can be one-way or two-way

- **Migration framework** - Organizations need to migrate from Windows NT 4.0 or external domains
  - *Solution:* Create External trusts (nontransitive, one-way or two-way) to enable resource access with Windows NT 4.0 domains or AD DS domains in other forests

- **Kerberos realm integration** - Organizations need authentication with non-AD DS directory services
  - *Solution:* Deploy Realm trusts (transitive or nontransitive, one-way or two-way) to establish authentication paths between Windows Server AD DS domains and Kerberos v5 protocol realms using other directory services

- **Authentication optimization across domains** - Users experience slow authentication between domains in different parts of the forest
  - *Solution:* Configure Shortcut trusts (nontransitive, one-way or two-way) to reduce authentication time between distant domains in the same forest

### Domain Design Use Cases

- **Decentralized administration** - Organizations with multiple locations need separate administrative boundaries
  - *Solution:* Implement multiple domains in the same forest, creating parent-child trusts automatically, with each domain serving as an administrative unit with its own Domain Admins group and FSMO roles

- **Large-scale object management** - Organizations need to manage billions of objects
  - *Solution:* Deploy multiple domains (each supporting nearly two billion objects) within a forest to distribute object storage and replication load

### OU Design Use Cases

- **Departmental Group Policy management** - IT needs to apply specific configurations to all computers in a department
  - *Solution:* Create OUs representing organizational departments, consolidate computers into departmental OUs, and link GPOs directly to OUs for centralized configuration management
  - *Reference:* [Organizational Units](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/organizational-units)

- **Delegated administration** - Branch offices need local IT administrators with limited management scope
  - *Solution:* Create geographic OUs for each office, then create nested OUs for IT administrators and departments; assign OU management permissions to delegate control to local administrators without Domain Admin rights

- **Complex organizational structures** - Enterprises need to represent both geographic and functional hierarchies
  - *Solution:* Design OU hierarchies combining geographic and departmental classifications, nesting OUs as needed (e.g., Office OU &gt; Department OU &gt; IT Admin OU), keeping depth to 10 levels or fewer (recommended five or fewer)

- **GPO application requirements** - Organizations need to apply policies to users and computers
  - *Solution:* Avoid generic containers (Builtin, Computers, Users) which cannot link GPOs; instead create custom OUs and link GPOs to them for applying configurations and restrictions

### Trust Management Use Cases

- **Cross-domain resource access** - Users in one domain need access to resources in another domain
  - *Solution:* Leverage automatic two-way transitive trusts between domains in the same forest, or manually create appropriate trust types (External, Forest, Realm, Shortcut) for cross-forest or external scenarios

---

## References

- [Understanding Forests](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/understanding-forests)
- [Understanding Domains](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/understanding-domains)
- [Organizational Units](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/organizational-units)
- [Active Directory Trust Types](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/active-directory-forest-trusts)
- [Active Directory FSMO Roles](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/active-directory-forest-operations-masters-roles)
