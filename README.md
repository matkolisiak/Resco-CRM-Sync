# Resco CRM Sync

This document is a brief summary of actual status of Resco CRM Sync.

# Content:
1. Getting started with Resco CRM Sync
2. Creating a connection
3. Updating metadata
4. Sync Data
5. Sync of deletions
6. Offline transactions
7. Sync job

# 1. Getting started with Resco CRM Sync
Resco CRM Sync is a tool that allows connecting two organizations. By creating a connection, you can synchronize data and metadata between them.<br/><br/>

The position of the organization can be as:<br/>
a) `Server`: The organization from which you create a client. Updates of metadata (on demand) and data (periodically) are sent to the client.<br/>
b) `Client`: Updates of data are instantly or periodically sent to the server. Updates of metadata can be pulled from server on demand.<br/>

![Screenshot](serverpositions.png)

# 2. Creating a connection
![Screenshot](connectpossibilities.png)
## a) From Woodford
1. Navigate to Woodford
2. Resco CRM Sync section
3. Connectior
4. Fill in info about `client` organization
5. Select `backend type` of server organization
6. Fill in info about `server` organization
7. Click on Connect button

![Screenshot](woodfordconnect.png)
## b) Externally - Webservice
Please, for more information about connecting through webservice, contact peter@resco.net or martin.liscinsky@resco.net 
```bash
https://connect.rescocrm.com/rest/v1/sync/myclientorganization/ConnectAsync
```
```xml
<ConnectRequest>     
    <SyncSetup>         
        <ProviderID>60650fb3-5d8b-45dd-b82e-459f3b019e9a</ProviderID>        
        <Url>urlofserverorganization</Url>   
        <UserName>martin.liscinsky@resco.net</UserName>       
        <Password>xxx</Password>         
        <RegistrationInfo>             
            <Email>martin.liscinsky@resco.net</Email>             
            <FirstName>Martin</FirstName>             
            <LastName>Liscinsky</LastName>             
            <Company>Resco</Company>         
        </RegistrationInfo>    
    </SyncSetup> 
</ConnectRequest>
```
## c) Connect Page - !!! Need to allow on our production servers !!!
https://connect.rescocrm.com/Register.aspx?otype=connect <br/>
https://build.rescocrm.com/Register.aspx?otype=connect <br/>
1. Navigate to one of the url above.
2. Fill in info about new `client` organization.
3. Select `backend type` of server organization.
4. Fill in info about `server` organization.
5. Click on Connect button.
![Screenshot](connectpage.png)

# 3. Updating metadata - !!!Rename to Update Client, not Update Server???!!!
Synchronization of metadata can be executed on demand:<br/> 
a) by clicking `Update Server` button, which is part of the Resco CRM Sync section in Woodford. <br/>
b) through webservice - ConnectRequest<br/>
*`Important note`: server metadata changes are migrated to the client´s, not vice versa.*

# 4. Sync Data

