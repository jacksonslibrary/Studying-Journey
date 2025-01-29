## Logical Active Directory Components
- Partitions
- Scheme
- Domains
- Domain Trees
- Forests
- Sites
- Organizational units (OUs)

### AD DS Schema
- Like a rule book or a blueprint
- Defines every type of object that can be stored in the directory
- Enforces rules regarding object creation and configuration

Class Object:
  - Function: What objects can be created in the directory
  - What attributes an object can have
  - Its relationship to other objects (inheritance)
  - Its behavior in AD
  - Examples: user, computer, group, organizationalUnit, domainDNS, contact, printQueue, volume

Attribute Object:
  - Function: Information that can be attached to an object
  - The data type (string, integer, boolean, etc.)
  - Whether the attribute is mandatory or optional
  - How it is used within objects
  - Examples: displayName, sAMAccountName, userPrincipalName (UPN), objectGUID, distinguishedName (DN)

### Domain
- Used to group and manage objects in an organization
- An administrative boundary for applying policies to groups of objects
- A replication boundary for replicating data between domain controllers
- An authentication and authorization boundary that provides a way to limit the scope of access to resources
- Examples: contoso.com, contoso.local, contoso.org

### Trees
- A hierarchy of domains in AD DS
- Example:

  ```
  contoso.com
  ├── emea.contoso.com
  ├── na.contoso.com
    ├── sales.na.contoso.com
  ```

All domains in the tree:
- Share a contiguous namespace with the parent domain
- Can have additional child domains
- By default create a two-way transitive trust with other domains

### Forests
- A collection of one or more domain trees
- Share a common schema
- Share a common configuration partition
- Share a common global catalog to enable searching
- Enable trusts between all domains in the forest
- Share the Enterprise Admins and Scheme Admins groups

### Organizational Units (OUs)
- AD containers of users, groups, computers, and other OUs
- Represent your organization hierarchy and logically
- Manage a collection of objects in a consistent way
- Delegate permissions to administrator groups of objects
- Apply policies

### Trust
- Trusts provide a mechanism for users to gain access to resources in another domain

Directional Trust:
  - Trust flows from trusting domain to the trusted domain
  
    ![image](https://github.com/user-attachments/assets/9fef3039-b9ee-45d2-bc03-d7a13b280f30)

Transitive Trust:
  - Trust relationship is extended beyond a two-domain trust to include other trusted domains
  
    ![image](https://github.com/user-attachments/assets/abbe7092-07d2-4bb6-bbbc-7ca65268d6f0)

- All domains in a forest trust all other domains in the forest
- Trusts can extend outside the forest

### Objects
- User: Enables network resource access for a user
- InetOrgPerson: Like a user account but used for compatibility with other directory services
- Contacts: Used primarily to assign e-mail addresses to external users and does not enable network access
- Groups: Used to simplify the administration of access control
- Computers: Enables authentication and auditing of computer access to resources
- Printers: Used to simplify the process of locating and connecting to printers
- Shared folders: Enables users to search for shared folders based on properties
