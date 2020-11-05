# Resco CRM Sync

This document is a brief summary of actual status of Resco CRM Sync.

# Content:
1. Getting started with Resco CRM Sync
2. Creating a connection
3. Updating metadata
4. Sync Data
5. Sync of deletions
6. Logs of Synchronization

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
*`Important note`: Server metadata changes are migrated to the client, not vice versa.*

# 4. Sync Data
## Entities selection
Selecting of entities, which should be synchronized can be done:<br/>
a) by enabling them in the list of entities in Resco CRM Sync section in Woodford <br/>
![Screenshot](enableentities.png)<br/>
b) through webservice in ConnectRequest as Entities parameter <br/>
## Sync Filter
There´s possibility to apply `Sync Filter` per entity:<br/>
a) by clicking Sync Filter button, (Resco CRM Sync section). And setting conditions in the editor. <br/>
![Screenshot](syncfilter.png)
b) through webservice in ConnectRequest as SyncFiltersXml parameter <br/>

## Ways of Sync data execution
a) `Sync All` - On demand - by clicking Sync All (Resco CRM Sync section, in Woodford)<br/>
b) `Sync Job` - Periodically - by setting frequency of synchronization on the client.<br/> You can set it up in Admin Console->Processes. If the job (Sync Job) does not exist, you can create it. 
1. Click New
2. Select Job category
3. Enter descriptive name
4. Configure frequency (e.g Periodically, every Hour)
5. Insert a new step (click Step and select Function) and enter  the following Server SyncService
6. Click Save
7. Publish
8. Activate
![Screenshot](syncjob.png)

c) `Webservice`<br/>

```bash
https://build.rescocrm.com/rest/v1/sync/mtestingdyn2connect/SyncAsync
```
Please, for more information contact peter@resco.net or martin.liscinsky@resco.net<br/>
## Sync Data Execute from Server to Client
Once, is Sync executed by one of the way of execution descibed above, data are sent from Server to Client.
## Sync Data Execute from Client to Server
It depends on CrmWritePlugin.config setting `<add key="WriteOffline" value="true"/>`.<br/>
a) value = `"false"` <br/>
- Updates of data are sent to Server immediately.<br/>

b) value = `"true"` <br/>
- Info about data changes are stored to `[migrate_transaction]` entity. 
Once, is Sync executed by one of the way of execution descibed above, data are sent from Client to Server.<br/>
- In addition, Resco CRM Sync offers `Offline transaction tab`, for managing data transactions (records of `[migrate_transaction]` entity) from Client to Server.

## Offline Transactions tab
It shows the individual transactions along with their Status:<br/>
`All` - all transactions<br/>
`Pending` - transactions waiting for upload<br/>
`Processed` - transactions successfully delivered to Dynamics<br/>
`Error` - transactions unsuccessfully delivered<br/>
`Archived` - transactions manually marked as Archived<br/><br/>
Use the toolbar buttons to change the status of transactions:<br/>
`Set to Pending`: Schedule selected transactions for a new delivery.<br/>
`Set to Archived`: Mark selected transactions as Archived. These changes will not be delivered to Dynamics.<br/>
`Set all to Pending`: Schedule all failed transactions (with the status Error) for a new delivery.<br/>
