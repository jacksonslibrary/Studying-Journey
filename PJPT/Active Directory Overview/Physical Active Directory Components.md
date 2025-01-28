## Physical Active Directory Components
- Data store
- Domain controllers
- Global catalog server
- Read-Only Domain Controller (RODC)
  
### Domain Controller
- Head honcho, controls everything
- A server with the AD DS server role installed that has specifically been promoted to domain controller
- Hosts a copy of the AD DS directory store
- Provides authentication and authorization services
- Replicates updates to other domain controllers in the domain and forest
- Allows administrative access to manage user accounts and network resources

### AD DS Data Store
- Contains the database files and processes that store and manages directory information for users, services, and apps
- Consists of the Ntds.dit file \*important\*
- File allows you to pull a lot of information, like password hashes
- Stored by default in the %SystemRoot%\NTDS folder on all domain controllers
- Accessible only through the domain controller processes and protocols
  
