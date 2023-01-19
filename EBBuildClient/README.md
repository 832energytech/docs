 [![N|Solid](https://github.com/832energytech/images/blob/main/logo.svg)](https://everythingblockchain.io)
# EB Build API Client
## _The Official API Client for Blockchain-based Cloud Storage Services!_

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)
___
## Features
___
If your application requires cloud storage of unstructured JSON or key/value paired data, the EB Build client is an affordable and extremely fast alternative to any of the following solutions:
1. Couchbase
2. Redis
3. DynamoDB
4. MongoDB
___
## Benefits
___
- ✨Extremely affordable fixed fee pricing!
- ✨Extremely secure blockchain encryption!
- ✨Extremely fast event messaging!
- ✨Extremely scalable with built-in cluster-wide auto-scaling!
- ✨Extremely simple integration with dynamic environments!
##
___
## Obtaining a Sandbox API Key
___
##
EBBuild cloud services can be accessed via our sandbox for a limited time.
The EBBuild Client requires an API key before using the sandbox.  Obtaining a sandbox API key is extremely easy! 
Follow the next steps to obtain an API key.
- ** Send an email with your company name to:  keyrequest@everythingblockchain.io**

___
## Pre-Installation Steps:  
## #1. [Click Here to Create a Private Cluster](https://aws.amazon.com/marketplace/pp/prodview-7ulzw27yljmlu?sr=0-261&ref_=beagle&applicationId=AWSMPContessa)
## #2. [Click Here to Download the EBBuild Client via nuget](https://www.nuget.org/packages/EBBuildClient/)
___
## Post-Installation Steps:  
___
```sh
#1 After creating a private cluster you will receive an automated email with the following:
    #1.1 A link to your private cluster in the cloud.
    #1.2 A security token required to store and retrieve data.
#2 Install the EBBuild Client nuget package into your Microsoft Visual Studio project.
#2 Add an appsettings.json file to project, if you don't already have an appsettings.json file added.
#3 Add the following values to the root of your appsettings.json file added to your project:
-  {
      "EbbuildApiBaseUri": "https://api.ebiblockchain.io/",
-     "QueryChainToken": [enter the API key obtained supplied to you.],
-     "QueryChainRoles": [enter user groups here.  For example you could enter: "Testers"],
   }
```
___
## How To's
___
```sh
#1 After installing the EBBuildClient nuget package, add the following to your code:
      using EBBuildClient.Core;
      
#2 EBBuild allows you to store json data without any class/object or a concrete class/object.  For example, if you have the following defined data type and if you wanted to store its data onto the EBBuild cloud storage services, you would following for following example.
    public class SampleDataClass
    {
        public string ID {get;set;}
        public string Name {get; set;}
        public AddressClass Address1 {get; set;}
    }
    public class AddressClass
    {
        public string City {get; set;}
        public string State {get; set;}
        public string Zip {get; set;}
    }
    
    
#3 The EBBuild cloud services support both REST and Websocket endpoints.  Addtionally, the EBBuild client allows you to connect eith using REST or a Websocket.

#4 Initialize the core EBBuild services in the following manner:
    #4.1 EBBuildApiFactory  : This class is the core object used to manage connection objects.
         EBBuildApiFactory _ebBuildDBApiServiceFactory = new EBBuildApiFactory(_maxConnections, Program._configuation, _ebbuildDBUserEmail, _ebbuildDBTenantId, _ebbuildDBLedgerPreface, _ebbuildDBLedgerName, _ebbuildDBApiBaseUri, _maxRecordsReturned, _ebbuildDBUseWebSockets);
    
    #4.2 IEBBuildAPIService : This interface is used for all connection object created by the EBBuildApiFactory.
         IEBBuildAPIService _client1 = _ebBuildDBApiServiceFactory.GetApiClient();
         
    #4.3 EBBuildAPIService  : This is a static class used to calling all CRUD methods.
         List<string> recordList = EBBuildAPIService.GetLedgerRecords<string>(_filters, _client1).Result;
         List<string> recordList = EBBuildAPIService.GetLedgerRecords<string>(_filters, _ebBuildDBApiServiceFactory.GetApiClient()).Result;
         List<SampleDataClass> recordList = EBBuildAPIService.GetLedgerRecords<SampleDataClass>(_filters, _ebBuildDBApiServiceFactory.GetApiClient()).Result;
         
#5 The EBBuild cloud servics support query filters used to filter data objects.
#6 To simplify creating filter conditions, the EBBuild client provides a filter builder.
    #6.1 List<string> _filters = EBIBuildAPIHelper.BuildFilter(_filters, "email", FilterOperation.EQ, "dummyuser15@gmail.com");
    #6.2 List<string> _filters = EBIBuildAPIHelper.BuildFilter(_filters, "email", FilterOperation.EQ, "dummyuser15@gmail.com", BooleanOperation.AND);
    
 NOTE: If you require raw data, then use our internal RawDataType class type.
    NOTE: The RawDataType class has a single property called "RawData" or simply use the "String" data type which will contain your unstructured json payload.
    
    RawDataType rawData = await EBBuildAPIService.GetLedgerRecord<RawDataType>(EBBuildAPIServices);
    string rawData = await EBBuildAPIService.GetLedgerRecord<string>(EBBuildAPIServices);
   
   
Set the following required parameters.
     Microsoft.Extensions.Configuration.IConfiguration configuration;
     string email = "a valid email address";
     string tenantID = "any unique string to identify your organization";
     string ledgerPreface = "any string to identify your ledger.  i.e.: prod, qa, dev, crypto, etc.";
     string ledgerName = "any name of your ledger where your data class will be stored.  i.e. payments";

NOTE: If you do not specify a ledgername then the email address will automatically be used as the ledger name.
      If you specify a ledgerPreface then the ledgerPreface will be prepended to the ledger name.
      

#7. To enforce multi-factor authentication (MFA) you can set the enableMFA parameter.
     NOTE: When saving data to the ledger, if you set the MFA parameter to "true" and if suspicious read access to the ledger is detected that read access will be block temporarily until an email challenge is sent to you (the owner of the ledger to allow you an opportunity to block or allow access).
     
     bool enableMFA = true;   
     string blockTypeName = "SampleDataClass";
   
   - EBBuildAPIService.SaveDataToLedger(
      dataContext,
      EBBuildAPIServices,
      enableMFS,
      blockTypeName);
```
___
## Adding Filter Conditions
___
| Operation | Description | Example | Notes |
| ------ | ------ | ------ | ------ |
|Regexp	|Regular expressions	| {LastName:REGEX:\\b[L]\\w+}
|Or|	Logical OR	| {FirstName:EQ:Joe:OR},{LastName:EQ:Biden}| Filter conditions combine multiple filter conditions by adding an optional "AND" "OR" boolean operator to the end of each filter.
|Gt, Gte|	Greater than or equal.|	{Age:Gte:25}
|Le, Lte|	Less than or less than or equal. |	{Age:Lte:25}
|Between|	Between two values.	| {Age:BETWEEN:[10;41]}
|Inq, Nin| In or not inv an array of values.|	{City:Inq:[New York; Phoenix; London; Miami; Berlin]}
|Neq| Not equal.|	{LastName:Neq:”Miller”}
|Like, NLike|	Like or not like a value.|	{LastName:Like:”Miller”}
|ILike, NiLike|	Case insensitive like and not like.| {LastName:ILike:”Miller”}


___
## Company Contact Information
___
| Contact | Email |
| ------ | ------ |
| Technical Support | support@everythingblockchain.io |
| Sales | sales@everythingblockchain.io |
| Partners | partners@everythingblockchain.io |
| Marketing | marketing@everythingblockchain.io |
| Investors | invest@everythingblockchain.io |
| Press | press@everythingblockchain.io |
___
## License
___
MIT- The API is and will always remain free!  The data storage is based on a fixed fee.  Contact EverythingBlockchain, Inc for details.


