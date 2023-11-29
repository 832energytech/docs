 [![N|Solid](https://github.com/832energytech/images/blob/main/logo.svg)](https://everythingblockchain.io)
# BuildDB&reg; API Client
## _The Official API Client for Blockchain-based Cloud Storage Services!_

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)
___
## Features
If your application requires cloud storage of unstructured JSON or key/value paired data, the EB Build client is an affordable and extremely fast alternative to any of the following solutions:

- ✨ Supabase
- ✨ AWS DynamoDB
- ✨ Azure CosmoDB
- ✨ Google Cloud Datastore 
- ✨ Oracle NoSQL Database
- ✨ PostgreSQL
- ✨ MongoDB
- ✨ RavenDB
- ✨ Redis
- ✨ Couchbase

##
___
## Benefits
- ✨ xtremely affordable flexible fee pricing based on database size!
- ✨ xtremely secure blockchain encryption!
- ✨ xtremely fast event messaging!
- ✨ xtremely powerful distributed denial of service (DDoS) features!
- ✨ xtremely scalable with built-in cluster-wide auto-scaling!
- ✨ xtremely powerful AI-based fuzzy matching for powerful filtering!
- ✨ xtremely powerful lazy loading of parent child relationship without indexes!
- ✨ xtremely simple integration with dynamic environments!
- ✨ xtremely powerful integration that combines NoSQL, relational and vector databases!

##
___
## Obtaining a Sandbox API Key
EBBuild cloud services can be accessed via our sandbox for a limited time.
The EBBuild Client requires an API key before using the sandbox.  Obtaining a sandbox API key is extremely easy! 
Follow the next steps to obtain an API key.
- ** Send an email with your company name to:  keyrequest@everythingblockchain.io**

___
## Pre-Installation Steps:  
## #1. [Click Here to Create a Private Cluster](https://aws.amazon.com/marketplace/pp/prodview-pbrque7tn5lv6?sr=0-1&ref_=beagle&applicationId=AWS-Marketplace-Console)
## #2. [Click Here to Download the EBBuild Client via nuget](https://www.nuget.org/packages/EBBuildClient/)
## #3. Add the following dependencies to your project:
       3.1 Microsoft.AspNetCore.SignalR.Client version 7.0.2 (or higher)
       3.2 Microsoft.AspNetCore.SignalR.Protocols.MessagePack version 7.0.2 (or higher)
##
##
___
## Post-Installation Steps:  
```sh
#1 After creating a private cluster you will receive an automated email with the following:
    #1.1 A link to your private cluster in the cloud.
    #1.2 A security token required to store and retrieve data.
#2 Install the BuildDB Client nuget package into your Microsoft Visual Studio project.
#2 Add an appsettings.json file to project, if you don't already have an appsettings.json file added.
#3 Add the following values to the root of your appsettings.json file added to your project:
  {
     "BuildDBApiBaseUri": [enter the url provided by EBI, INc],
     "BuildDBApiToken": [enter the API key obtained supplied to you.],
     "BuildDBApiRoles": [enter user groups here.  
				For example you could enter: "Testers"],
     "BuildDBRegistrationHost": [enter the name of company or entity: 
					"Company ABC, Inc"]
   }

```
##
___
## How To's
```sh
#1 After installing the BuildDBClient nuget package, add the following to your code:
      using BuildDBClient.Core;
      
#2 BuildDB allows you to store json data without any class/object or a concrete class/object.  For example, if you have the following defined data type and if you wanted to store its data onto the BuildDB cloud storage services, you would following for following example.  ALL classes passed in MUST implement the "CommonInterfaces" interface.  The "CommonInterfaces" interface ensure that all classes can support parent child relationships and aggregate function data.

    public class SampleDataClass: CommonInterfaces
    {
        public string ID {get; set;}
        public string Name {get; set;}
        public AddressClass Address1 {get; set;}
    }
    public class AddressClass: CommonInterfaces
    {
        public string City {get; set;}
        public string State {get; set;}
        public string Zip {get; set;}
    }       
    
#3 The BuildDB cloud services support both REST and Websocket endpoints.  Addtionally, the BuildDB client allows you to connect with using REST or a Websocket.

#4 Initialize the core BuildDB services in the following manner:
    #4.1 BuildDBApiFactory : This class is the core object used to manage connection objects.
         BuildDBApiFactory _BuildDBDBApiServiceFactory = 
	  new BuildDBApiFactory(
		_maxConnections, 
		Program._configuation, 
		_BuildDBDBUserEmail, 
		_BuildDBDBTenantId, 
		_BuildDBDBLedgerPreface, 
		_BuildDBDBLedgerName, 
		_BuildDBDBApiBaseUri, 
		_maxRecordsReturned, 
		_BuildDBDBUseWebSockets);
    
    #4.2 IBuildDBAPIService: This interface is used for all connection 
         object created by the BuildDBApiFactory.
         IBuildDBAPIService _client1 = 
		_BuildDBDBApiServiceFactory.GetApiClient();
         
    #4.3 BuildDBAPIService: This is a static class used to calling all CRUD 
	methods.
         List<string> recordList = BuildDBAPIService.GetLedgerRecords<string>(_filters, _client1).Result;
         List<string> recordList = BuildDBAPIService.GetLedgerRecords<string>(_filters, _BuildDBDBApiServiceFactory.GetApiClient()).Result;
         List<SampleDataClass> recordList = BuildDBAPIService.GetLedgerRecords<SampleDataClass>(_filters, _BuildDBDBApiServiceFactory.GetApiClient()).Result;

```

## Filters ##
```
#5 The EBBuild cloud services support query filters used to filter data objects.
#6 To simplify creating filter conditions, the EBBuild client provides a filter builder.
    #6.1 List<string> _filters = EBIBuildAPIHelper.BuildFilter(_filters, "email", FilterOperation.EQ, "dummyuser15@gmail.com");
    #6.2 List<string> _filters = EBIBuildAPIHelper.BuildFilter(_filters, "email", FilterOperation.EQ, "dummyuser15@gmail.com", BooleanOperation.AND);
```
   
## Aggregate Filter Functions 
```
#7 The EBBuild cloud services support aggregate functions used to obtain group counts and sorting on data objects.
#8 To simiplify creating aggregate functions, the EBBuild client provides a function builder.
    #8.1 List<string> _filters = EBIBuildAPIHelper.BuildFilterFunction(_filterFunctions, new List<string>() { "Gender", "Age" }, FilterFunctionOperation.GROUPBY);
    #8.2 List<string> _filters = EBIBuildAPIHelper.BuildFilterFunction(_filterFunctions, new List<string>() { "LastName" }, FilterFunctionOperation.ORDERBY_DESC);
    
NOTE: In-order to see the group count value, you must include into your DTO a field named "GroupCount".
      The results (when groupBy is used) will only include values for the fields in the groupby clause and the GroupCount field.
      
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

##
___
## Basic CRUD Functions
| Function | Description |
| ------ | ------ |
|Database Initialization |       
|Program._context = new ConfigurationRegistry.StartupContext();  
|Program._configuation = ConfigurationRegistry.StartupContext.Configuration;
|String _ebbuildDBApiBaseUri = StartupContext.Configuration?.GetValue<string>("EbbuildApiBaseUri");
|String _ebbuildDBApiToken = StartupContext.Configuration?.GetValue<string>("EbbuildApiToken");
|String _ebbuildDBApiRoles = StartupContext.Configuration?.GetValue<string>("EbbuildApiRoles");
|String _ebbuildDBUserEmail = StartupContext.Configuration?.GetValue<string>("EbbuildUserEmail");
|string _ebbuildDBTenantId = StartupContext.Configuration?.GetValue<string>("EbbuildTenantId");
|String _ebbuildDBLedgerEncryption = StartupContext.Configuration?.GetValue<string>("EbbuildLedgerEncryption"); 
|String _ebbuildDBLedgerPreface = StartupContext.Configuration?.GetValue<string>("EbbuildLedgerPreface");           
|String _ebbuildDBLedgerName = string.Format("{0}",  StartupContext.Configuration?.GetValue<string>("EbbuildLedgerName"));
|Bool _ebbuildDBUseWebSockets =  StartupContext.Configuration.GetValue<bool>("EbbuildUseWebSockets");
|Int32 _ebbuildDBTimeOut = StartupContext.Configuration.GetValue<Int32>("EbbuildTimeOut");
|Int32 _maxConnections = StartupContext.Configuration.GetValue<Int32>("EbbuildMaxConncetions");
|Int32 _maxRecordsReturned = StartupContext.Configuration.GetValue<Int32>("EbbuildMaxRecordsReturned");
|string  _hostRegistration = StartupContext.Configuration.GetValue<string>("EbbuildRegistrationHost");
|Int32 _asyncScale = StartupContext.Configuration.GetValue<Int32>("EbbuildAsyncScale"); 


## 
| Function | Description |
| ------ | ------ |
|Database Factory Creation |  
|EBBuildApiFactory _ebBuildDBApiServiceFactory = new EBBuildApiFactory(
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; _maxConnections, 
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;Program._configuation, 
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;_ebbuildDBApiToken, 
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;_ebbuildDBApiRoles, 
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;_ebbuildDBUserEmail, 
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;_ebbuildDBTenantId, 
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;_ebbuildDBLedgerPreface, 
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;_ebbuildDBLedgerName, 
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;_ebbuildDBApiBaseUri, 
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;_maxRecordsReturned, 
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;_ebbuildDBUseWebSockets,asyncScale: _asyncScale);

## 
| Function | Description |
| ------ | ------ |
|Authentication |  
|IEBBuildAPIService _client1 = _ebBuildDBApiServiceFactory.GetApiClient();
|AuthStatus authStatus = await _client1.IsCredentialsValid();
|if (authStatus != AuthStatus.Success)
|{
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  if (authStatus == AuthStatus.TokenInvalid)
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;   {
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;      Debug.WriteLine(string.Format("Your API Token is invalid!"));
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;      return;
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  }
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;   else
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;   {
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;   if (authStatus == AuthStatus.TokenInvalidForEnvironment)
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;   {
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;   Debug.WriteLine(string.Format("Your cluster is invaid!"));
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;   return;
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;   }
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;   else
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;   {
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;   if (authStatus == AuthStatus.Failed)
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;   {
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;   Debug.WriteLine(string.Format("Authentication failed!"));
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;   return;
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;   }
|&nbsp; &nbsp; &nbsp; &nbsp;    }
|&nbsp; &nbsp; }
|&nbsp; &nbsp;  
|}
            


## 
| Function | Description |
| ------ | ------ |
|Filter Initialization
|List<string> _filters = default(List<string>);
|List<string> _childFilters = default(List<string>);
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;_filters = EBIBuildAPIHelper.BuildFilter(
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;_filters, 
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;"Recent_Searches_FShop_Key", 
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;FilterOperation.REGEX, 
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;"CDG");
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;_childFilters = EBIBuildAPIHelper.BuildFilter(
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;_childFilters, 
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;"Origin", 
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;FilterOperation.REGEX, 
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;"IAH", 
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;BooleanOperation.AND);




## 
| Function | Description |
| ------ | ------ |
|Database CRUD | 
|GetAllLedgersAsync| Get list of all ledgers (tables) in EBBuild DB.
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;var allLedgers = _client1.GetAllLedgersAsync().Result;
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; if (!string.IsNullOrEmpty(allLedgers.errorMessage))
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;{
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;Debug.WriteLine(allLedgers.errorMessage);
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;return;
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;}
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;var ledgers = _client1.GetAllLedgersAsync(_client1.GetLedgerName()).Result;
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; if (!string.IsNullOrEmpty(ledgers.errorMessage))
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;{
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;Debug.WriteLine(allLedgers.errorMessage);
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;return;
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;}
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;if (ledgers.Item2?.Count() == 0) 
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;{
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;   var newLedger = _client1.CreateLedgerAsync(_client2.GetLedgerName()).Result;
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;  if(!string.IsNullOrEmpty(newLedger.errorMessage))
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;  {
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;  Debug.WriteLine(string.Format(newLedger.errorMessage));
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;  return;
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;  }
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;}

## 
| Function | Description |
| ------ | ------ |
|GetLedgerRecordsAsync|	Get all filtered blocks (records) in EBBuild DB for a specific ledger (table).
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; /*/
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; /*/ Load only parents 
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; /*/
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;(List<Recent_Searches_Flight_Shop> shopList, paginationDetails) = 
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;EBBuildAPIService.GetLedgerRecordsAsync<Recent_Searches_Flight_Shop>(
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;asyncWrapper: _ebBuildDBApiServiceFactory.GetAsyncWrapper(),
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;parentToLazyLoadChildren: null,
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;filterConditions: _filters,  
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;filterFunctions: _filterFunctions,
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;relationship: _schemaDefinition,
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;servicContext: _client1,
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;refreshCacheResults: false).Result;
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;/*/
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; * Lazyloading child of parent
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;/*/
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;(List<Recent_Searches_Flight_Shop> shopListChildren, paginationDetails) = 
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;EBBuildAPIService.GetLedgerRecordsAsync<Recent_Searches_Flight_Shop>(
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;asyncWrapper: _ebBuildDBApiServiceFactory.GetAsyncWrapper(),
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;parentToLazyLoadChildren: shopList[1],
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;filterConditions: _filters,
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;filterFunctions: _filterFunctions,
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;relationship: _schemaDefinition,
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;servicContext: _client1,
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;refreshCacheResults: false).Result;

## 
| Function | Description |
| ------ | ------ |
|SaveDataToLedgerWithNoResponse	| Save a block (record) to a ledger (Table) based on a schema (class).
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;EBBuildAPIService.SaveDataToLedgerWithNoResponse<Recent_Searches_Flight_Shop>(
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;shop, 
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;_ebBuildDBApiServiceFactory.GetAsyncWrapper(), 
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;_queryPatterns, 
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;_queryPatternsFunctions, 
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;_client1, 
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;blockName, 
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;blockHashKey);

## 
| Function | Description |
| ------ | ------ |
|DeleteDataFromLedgerWithNoResponse| Delete (archive) a block (record) in a ledger (table) based on a schema (class).
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;EBBuildAPIService.DeleteDataFromLedgerWithNoResponse<Recent_Searches_Flight_Shop>(
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;_filters, 
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;_client1, 
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;_ebBuildDBApiServiceFactory.GetAsyncWrapper());

## 
| Function | Description |
| ------ | ------ |
|UpdateDataToLedgerWithNoResponse|Create a vector object and add a search vector.
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; /*/
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; * Create vector search string
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;/*/
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;Vector<Int32> vector = new Vector<Int32>();
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;vector.AddVector(new int[] { 8, 2, 3, 4, 5 });

## 
| Vector Database Classes | Description |
| ------ | ------ |
|Vector|Create a vector object and add a search vector.
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; /*/
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; * Create vector search string
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;/*/
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;Vector<Int32> vector = new Vector<Int32>();
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;vector.AddVector(new int[] { 8, 2, 3, 4, 5 });
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;string vectorString = EBIBuildAPIHelper.ConvertVectorToString<Int32>(vector);
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;_filters = EBIBuildAPIHelper.BuildFilter(_filters, "Recent_Searches_FShop_LogData_Value", FilterOperation.VECTORMATCH, vectorString);
## 
| Vector Database Classes | Description |
| ------ | ------ |
|Embeddings|Create an embedding object to hold a matrix to be searched.
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; /*/
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; * Create embeddings matrix string
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;/*/
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;Embeddings<Vector<Int32>, Int32> embedding = new Embeddings<Vector<Int32>, Int32>();
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;embedding.AddEmbedding(new[,]
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;{
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;{ new Vector<Int32>(new Int32[] { 1, 2, 3, 4, 5 }) },
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;{ new Vector<Int32>(new Int32[] { 5, 6, 3, 8, 7 }) },
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;{ new Vector<Int32>(new Int32[] { 3, 7, 3, 9, 1 }) }
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;});
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;string embeddingString = embedding.GetEmbeddingString();


## 
| Function | Description |
| ------ | ------ |
| GetDDoSStatisticsAsync	|Conduct statistical analysis on records for a past time interval.
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;(List<Recent_Searches_Flight_Shop> shopListDoS, paginationDetails) = 
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;EBBuildAPIService.GetDDoSStatisticsAsync<Recent_Searches_Flight_Shop>(
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;asyncWrapper: _ebBuildDBApiServiceFactory.GetAsyncWrapper(),
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;filterKey: "Recent_Searches_FShop_Device_Id",
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;filterKeyValue: "cac5bf97-2acc-461c-b4dd-c962bd45f996",
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;DateTimeKey: "Recent_Searches_FShop_Search_Time_Stamp", 
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;timeInterval: DoSTimeInterval.ELAPSEDMINUTES,
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;DDoSTimeIntervalValue: "53100000",
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;servicContext: _client1,
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;EnforceCacheResults: false,
|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;StringNameOfType: typeof(Recent_Searches_Flight_Shop).Name).Result;


##
___
## Adding Filter Conditions
##
| Operation | Description | Example | Notes |
| ------ | ------ | ------ | ------ |
|Regexp	|Regular expressions	| {LastName:REGEX:\\b[L]\\w+} | ie: Match the lastname that begins with the letter "L".  See the "Regular Expression - Documentation (below)"
|FuzzyMatch	| AI pattern match |	{LastName:FuzzyMatch:Biden}	|ie: Match the lastname using artificial intelligence to find nuances in spelling.”
|Or|	Logical OR	| {FirstName:EQ:Joe:OR},{LastName:EQ:Biden}| Filter conditions combine multiple filter conditions by adding an optional "AND" "OR" boolean operator to the end of each filter.
|Gt, Gte|	Greater than or equal.|	{Age:Gte:25}
|Le, Lte|	Less than or less than or equal. |	{Age:Lte:25}
|Between|	Between two values.	| {Age:BETWEEN:[10;41]}
|Inq, Nin| In or not inv an array of values.|	{City:Inq:[New York; Phoenix; London; Miami; Berlin]}
|Eq| Equal.|	{LastName:Eq:”Miller”}
|Neq| Not equal.|	{LastName:Neq:”Miller”}
|Like, NLike|	Like or not like a value.|	{LastName:Like:”Miller”}
|ILike, NiLike|	Case insensitive like and not like.| {LastName:ILike:”Miller”}

___

## Adding Parent Child Relationships
| Defining Foreign Key Relationships |
| ------------------------ |
|BuildDB (now) supports foreign key relationships just like SQL relational databases, but without the limitation of foreign key constraints.  This means (unlike) tranditional foreign key contraints you can define foreign keys within your definition. Using the following class, SchemaRelationshipDef , it is possible to define foreign key constraints on the fly!  One of the parameters to the method "GetLedgerRecordsAsync()" is the SchemaRelationshipDef class.  As the retrieval of all child data is conducted as a lazy load operation, when passing in the SchemaRelationshipDef class, you must (also) pass in a single object that you wish to populate with children.|

SchemaRelationshipDef _schemaDefinition = new SchemaRelationshipDef()
{
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; RelationshipName = "RecentSearches2Children",
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; Relationships = new List<SchemaRelationshipDetails>()
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; { 
          &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; new SchemaRelationshipDetails()
          &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; {                           
                &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;TargetSchemaName = typeof(Search_Child).AssemblyQualifiedName.Substring(0,typeof(Search_Child).AssemblyQualifiedName.IndexOf(",")),
                 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;TargetSchemaType = typeof(Recent_Searche_Child),
                &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;Relationship = new SchemaRelationship()
                &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;{
                        &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;&nbsp;&nbsp;SourceSchemaKey = "PK", 
                        &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;&nbsp;TargetSchemaKey = "ParentPK",
                        &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;&nbsp;&nbsp; Filters= _childFilters,
                        &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;&nbsp;&nbsp; FilterFunctions = _childFilterFunctions                                    
                &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;},
                &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ChildRelationship = null
            &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp;}
     &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;}
&nbsp;&nbsp;};


___

## Distributed Denial of Service (DDoS) Features - Documentation
___
Once you have data stored securely within BuildDB, you need to ensure users can access it.
Combating DDoS requires capabilities that firewalls don't possess.
- ✨ Firewalls have no intelligence on what business transaction is being performed. Thus you cannot implement brute force check against transactions. It is either URL or IP.
- ✨ If a firewall has to implement brute force attack detection, it has to read the whole payload and then inspect for patterns. This requires high CPU Á Memory usage on Firewall. In case of https, it requires you to terminate https at firewall level so that it can read the received data.
- ✨ Most firewalls have basic scripting language to configure rules. Some do support JavaScript like language, but check the CPU cost of that and the price tag. With HackerSpray, you get .NET code, so the sky is the limit.
- ✨ Firewalls have limited storage for logs and shipping logs from firewall to analysis engines puts stress on the firewall, especially when you are under attack. Many a times, we experience Firewall CPU exhaustion when it is blocking DOS, while it is writing all those attacks in a log and also shipping the logs to our analysis servers.

The answer is more business level intelligent to analyze data before it is added to storage.
The solution is BuildDB!
Before storing data into BuildDB, simply invoke the following command "GetDDoSStatisticsAync" to instandly gather metrics on the frequency of records written using the same filterKey to allow or block adding additional records initiated by bad actors.
It's that easy to implement DDoS!
```
 (List<ClassTypeX> DoSRecordList, paginationDetails) = EBBuildAPIService.GetDDoSStatisticsAsync<ClassTypeX>(
              asyncWrapper: _ebBuildDBApiServiceFactory.GetAsyncWrapper(),
              filterKey: "Id_Field", 
              filterKeyValue: "cac5bf97-2acc-461c-b4dd-c962bd45f996",
              DateTimeKey: "Time_Stamp_Field",
              timeInterval: DoSTimeInterval.ELAPSEDMINUTES,
              DDoSTimeIntervalValue: "1",
              servicContext: _client1,
              EnforceCacheResults: false,
              StringNameOfType: typeof(ClassTypeX).Name).Result;
```

___

## Vector Database - Documentation
___
BuildDB operates as fully functioning vector database with support for both vectors and embedded objects on top of our highly performant and scalable architecture.
- ✨ Fi
- ✨ If a firewall has to implement brute force attack detection, it has to read the whole payload and then inspect for patterns. This requires high CPU Á Memory usage on Firewall. In case of https, it requires you to terminate https at firewall level so that it can read the received data.
- ✨ Most firewalls have basic scripting language to configure rules. Some do support JavaScript like language, but check the CPU cost of that and the price tag. With HackerSpray, you get .NET code, so the sky is the limit.
- ✨ Firewalls have limited storage for logs and shipping logs from firewall to analysis engines puts stress on the firewall, especially when you are under attack. Many a times, we experience Firewall CPU exhaustion when it is blocking DOS, while it is writing all those attacks in a log and also shipping the logs to our analysis servers.

The answer is more business level intelligent to analyze data before it is added to storage.
The solution is BuildDB!
Before storing data into BuildDB, simply invoke the following command "GetDDoSStatisticsAync" to instandly gather metrics on the frequency of records written using the same filterKey to allow or block adding additional records initiated by bad actors.
It's that easy to implement DDoS!
```
 (List<ClassTypeX> DoSRecordList, paginationDetails) = EBBuildAPIService.GetDDoSStatisticsAsync<ClassTypeX>(
              asyncWrapper: _ebBuildDBApiServiceFactory.GetAsyncWrapper(),
              filterKey: "Id_Field", 
              filterKeyValue: "cac5bf97-2acc-461c-b4dd-c962bd45f996",
              DateTimeKey: "Time_Stamp_Field",
              timeInterval: DoSTimeInterval.ELAPSEDMINUTES,
              DDoSTimeIntervalValue: "1",
              servicContext: _client1,
              EnforceCacheResults: false,
              StringNameOfType: typeof(ClassTypeX).Name).Result;
```


___
## Regular Expression - Documentation
| Character | What does it do? | 
| ------ | ------ |
|	\    |  Used to indicate that the next character should NOT be interpreted literally. For example, the character 'w' by itself will be interpreted as 'match the character w', but using '\w' signifies 'match an alpha-numeric character including underscore'.Used to indicate that a metacharacter is to be interpreted literally. For example, the '.' metacharacter means 'match any single character but a new line', but if we would rather match a dot character instead, we would use '\.'.
| ^	     | Matches the beginning of the input. If in multiline mode, it also matches after a line break character, hence every new line. When used in a set pattern ([^abc]), it negates the set; match anything not enclosed in the brackets
| $	     | Matches the end of the input. If in multiline mode, it also matches before a line break character, hence every end of line.    
| * | Matches the preceding character 0 or more times.  
| + | Matches the preceding character 1 or more times.
|?	| Matches the preceding character 0 or 1 time. When used after the quantifiers *, +, ? or {}, makes the quantifier non-greedy; it will match the minimum number of times as opposed to matching the maximum number of times.
| .	| Matches any single character except the newline character.
| (x)|Matches 'x' and remembers the match. Also known as capturing parenthesis.
| (?:x)	| Matches 'x' but does NOT remember the match. Also known as NON-capturing parenthesis.
| x(?=y)|	Matches 'x' only if 'x' is followed by 'y'. Also known as a lookahead.
| x(?!y)|	Matches 'x' only if 'x' is NOT followed by 'y'. Also known as a negative lookahead.
| x \| y| Matches 'x' OR 'y'.
| {n} |	Matches the preceding character exactly n times.
| {n,m}	| Matches the preceding character at least n times and at most m times. n and m can be omitted if zero..
| [abc]	| Matches any of the enclosed characters. Also known as a character set. You can create range of characters using the hyphen character such as A-Z (A to Z). Note that in character sets, special characters (., *, +) do not have any special meaning.
| [^abc]| Matches anything NOT enclosed by the brackets. Also known as a negative character set.
| [\b]	| Matches a backspace.
| \b	| Matches a word boundary. Boundaries are determined when a word character is NOT followed or NOT preceded with another word character.
| \B	| Matches a NON-word boundary. Boundaries are determined when two adjacent characters are word characters OR non-word characters.
| \cX	| Matches a control character. X must be between A to Z inclusive.
| \d	| Matches a digit character. Same as [0-9] or [0123456789].
| \D	| Matches a NON-digit character. Same as [^0-9] or [^0123456789].
| \f	| Matches a form feed.
| \n	| Matches a line feed.
| \r	| Matches a carriage return.
| \s	| Matches a single white space character. This includes space, tab, form feed and line feed.
| \S	| Matches anything OTHER than a single white space character. Anything other than space, tab, form feed and line feed.
| \t	| Matches a tab.
| \v	| Matches a vertical tab.
| \w	| Matches any alphanumeric character including underscore. Equivalent to [A-Za-z0-9_].
| \W	| Matches anything OTHER than an alphanumeric character including underscore. Equivalent to [^A-Za-z0-9_].
| \x	| A back reference to the substring matched by the x parenthetical expression. x is a positive integer.
| \0	| Matches a NULL character.
| \xhh	| Matches a character with the 2-digits hexadecimal code.
| \uhhhh|Matches a character with the 4-digits hexadecimal code.
___
## Company Contact Information
| Contact | Email |
| ------ | ------ |
| Technical Support | support@everythingblockchain.io |
| Sales | sales@everythingblockchain.io |
| Partners | partners@everythingblockchain.io |
| Marketing | marketing@everythingblockchain.io |
| Investors | invest@everythingblockchain.io |
| Press | press@everythingblockchain.io |
##
___
## License
MIT- The API is and will always remain free!  The data storage is based on a fixed fee.  Contact EverythingBlockchain, Inc for details.


