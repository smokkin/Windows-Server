# Manage AD DS Schema

Many applications and services utilize data that's stored in an AD DS database. Some of them, such as Contoso's newly developed in-house application that you need to implement, require that data to be in a specific format. This, in turn, might require extending AD DS schema.

## What is a Schema?

AD DS stores and retrieves information from a wide variety of applications and services. It does this, in part, by standardizing how the AD DS directory stores data. By standardizing data storage, AD DS can retrieve, update, and replicate data while helping to maintain data integrity.

An AD DS schema is the component that defines all the object classes and attributes that AD DS uses to store data. All domains in a forest contain a copy of the schema that applies to that forest. Any change in the schema replicates to every domain controller in the forest via their replication partners. However, changes originate at the schema master.

### Objects

AD DS uses objects as units of storage. The schema defines all object types. Each time the directory manages data, the directory queries the schema for an appropriate object definition. Based on the object definition in the schema, the directory creates the object and stores the data.

Object definitions specify both the types of data that the objects can store and the data syntax. You can only create objects that the schema defines. Because objects store data in a rigidly defined format, AD DS can store, retrieve, and validate the data that it manages, regardless of which application supplies it.

### Relationships Among Objects, Rules, Attributes, and Classes

AD DS schema objects consist of attributes, which are grouped together into classes. Each class has rules that define which attributes are mandatory and which are optional. For example, the user class consists of more than 400 possible attributes, including cn (the common name attribute), givenName, displayName, objectSID, and manager. Of these attributes, the cn and objectSID attributes are mandatory.

The user class is an example of a structural class. A structural class is the only type of class that can have objects in an AD DS database. To modify the schema, you can create an auxiliary class with its own attributes, and then reference it in the definition of a structural class.

<img width="790" height="477" alt="image" src="https://github.com/user-attachments/assets/1b09b9d3-edbb-491b-852d-9662e8f5bf1b" />

> A screenshot of an Active Directory Schema console displays a list of user class attributes.

## Manage AD DS Schema

When managing the AD DS schema, you can modify the schema only if you're a member of the Schema Admins group in the root domain of the AD DS forest. For this purpose, you can use the Active Directory Schema snap-in.

> **Important**
> 
> AD DS schema doesn't support deletions.

You should change the schema only when necessary because the schema controls the storage of information. Additionally, any changes made to the schema affect every domain controller. Before you change the schema, you should review the changes and implement them only after you've performed testing. This helps ensure that the changes Won't adversely affect the rest of the forest or any applications that use AD DS.

<img width="791" height="419" alt="image" src="https://github.com/user-attachments/assets/3e8b12fa-b7cc-4c6c-9f8c-f501edc20b87" />

> Context menu for the schema container in the navigation pane of the ADSI Edit console. The Update Schema Now option is selected.

# Summary

Using the knowledge acquired in this module, you deployed AD DS domain controllers in the manner that ensured the optimal placement of operation masters and global catalog roles. You also applied custom AD DS schema extensions to accommodate deployment of a new in-house developed application.

## Learn more

- [AD DS Deployment](https://docs.microsoft.com/windows-server/identity/ad-ds/deploy/ad-ds-deployment)
- [Planning Operations Master Role Placement](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/planning-operations-master-role-placement)
- [Planning Global Catalog Server Placement](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/planning-global-catalog-server-placement)
- [How to Extend the Schema](https://docs.microsoft.com/windows-server/identity/ad-ds/schema/how-to-extend-the-schema)
