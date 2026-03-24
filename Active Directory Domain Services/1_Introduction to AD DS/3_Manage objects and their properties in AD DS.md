# Manage Objects and Their Properties in AD DS

Managing the AD DS environment is one of the most common tasks an IT pro performs. There are several tools that you can use to manage AD DS.

## Active Directory Administrative Center

The Active Directory Administrative Center provides a GUI that is based on Windows PowerShell. This enhanced interface allows you to perform AD DS object management by using task-oriented navigation, and it replaces the functionality of Active Directory Users and Computers.

<img width="1365" height="594" alt="image" src="https://github.com/user-attachments/assets/aea26942-7be3-46b4-aa57-11663d547eef" />
A screenshot of Active Directory Administrative Center. The administrator has selected the IT OU in the Contoso.com domain.

Tasks that you can perform by using the Active Directory Administrative Center include:

- Creating and managing user, computer, and group accounts.
- Creating and managing OUs.
- Connecting to and managing multiple domains within a single instance of the Active Directory Administrative Center.
- Searching and filtering AD DS data by building queries.
- Creating and managing fine-grained password policies.
- Recovering objects from the Active Directory Recycle Bin.
- Managing objects that the Dynamic Access Control feature requires.

## Windows Admin Center

Windows Admin Center is a web-based console that you can use to manage server computers and computers that are running Windows 10. Typically, you use Windows Admin Center to manage servers instead of using Remote Server Administration Tools (RSAT).

Windows Admin Center works with any browser that is compliant with modern standards, and you can install it on computers that run Windows 10 and Windows Server with Desktop Experience.

> Note
> 
> You shouldn't install Windows Admin Center on a server computer that is configured as an AD DS domain controller.

With a decreasing number of exceptions, Windows Admin Center supports most current Windows Server and Windows 10 administrative functionality. However, Microsoft intends that Windows Admin Center will eventually support all the administrative functionality that is presently available through RSAT.

<img width="1820" height="800" alt="image" src="https://github.com/user-attachments/assets/3324cc14-ddcf-4073-bc38-5178532b39ff" />
A screenshot of Windows Admin Center. The administrator has selected Server Manager. The Overview pane for a server called SEA-DC1 is displayed.

To use Windows Admin Center, you must first download and install it. You can download Windows Admin Center from the Microsoft download website. After downloading and installing Windows Admin Center, you must enable the appropriate TCP port on the local firewall. On a Windows 10 computer (in standalone mode), this defaults to 6516. On Windows Server (in gateway mode), this defaults to TCP 443. In both cases, you can change it during setup.

> Note
>  
> Unless you're using a certificate from a trusted CA, the first time you run Windows Admin Center, it prompts you to select a client certificate. Ensure you select the certificate labeled Windows Admin Center Client.

## Remote Server Administration Tools

RSAT is a collection of tools which enables you to manage Windows Server roles and features remotely.

<img width="1820" height="1042" alt="image" src="https://github.com/user-attachments/assets/c910d085-69b0-4bc8-ba1b-23a2c540c3d0" />
A screenshot of the Add an optional feature dialog box. Displayed are a list of RSAT tools.

> Note
> 
> You enable RSAT tools from the Settings app. In Settings, search for Manage optional features, select Add a feature, and then select the appropriate RSAT tools from the returned list. Select Install to add the feature.

You can install the consoles available within RSAT on computers running Windows 10 or on server computers that are running the Server with Desktop Experience option of a Windows Server installation. Until the introduction of Windows Admin Center, RSAT consoles were the primary graphical tools for administering the Windows Server operating system.

## Other AD DS Management Tools

Other management tools that you use to perform AD DS administration are described in the following table.

| Management Tool | Description |
|-----------------|-------------|
| **Active Directory module for Windows PowerShell** | The Active Directory module for Windows PowerShell supports AD DS administration, and it's one of the most important management components. Server Manager and the Active Directory Administration Center are based on Windows PowerShell and use cmdlets to perform their tasks. |
| **Active Directory Users and Computers** | Active Directory Users and Computers is a Microsoft Management Console (MMC) snap-in that manages most common resources, including users, groups, and computers. Although many administrators are familiar with this snap-in, the Active Directory Administrative Center replaces it and provides more capabilities. |
| **Active Directory Sites and Services** | The Active Directory Sites and Services MMC snap-in manages replication, network topology, and related services. |
| **Active Directory Domains and Trusts** | The Active Directory Domains and Trusts MMC snap-in configures and maintains trust relationships at the domain and forest functional levels. |
| **Active Directory Schema snap-in** | The Active Directory Schema MMC snap-in examines and modifies the definitions of AD DS attributes and object classes. You don't need to review or change it often. Therefore, by default, the Active Directory Schema snap-in isn't registered. |

---

## Demonstration

Demonstration on how to manage objects in AD DS by using Active Directory Administrative Center. The main steps in the process are:

1. From Server Manager, open Active Directory Administrative Center.
2. Select Dynamic Access Control in the Contoso domain.
3. Perform a global search and review the results.
4. Reset the password for a user in the Contoso domain.
5. Create a new computer object called SEA-CL4.
6. Open the new computer object and review its properties, including its Extensions.
7. Review the Windows PowerShell history and examine the New-ADComputer command.

# Summary

Contoso's IT staff are migrating Contoso on-premises servers to Windows Server 2025. As part of the migration, Contoso evaluated its current AD DS environment. As a Windows Server administrator, you've been responsible for managing AD DS objects, such as users, groups, and OUs. You now understand the fundamental AD DS structure and how users, groups, and group managed service accounts relate to OUs.

## Learn more

- [Active Directory Domain Services Overview](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview)
- [Create an outbound forest trust to an on-premises domain in Microsoft Entra Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/create-forest-trust)
- [Group Managed Service Accounts Overview](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)
- [Delegated Managed Service Accounts Overview](https://docs.microsoft.com/windows-server/security/delegated-managed-service-accounts/delegated-managed-service-accounts-overview)
- [Delegating Administration of Account OUs and Resource OUs](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/delegating-administration-of-account-ous-and-resource-ous)
