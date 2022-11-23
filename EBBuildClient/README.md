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


___
## Obtaining an API Key
___
The EB Build Client requires an API key before using.  Obtaining an API key is extremely easy! 
Follow the next steps to obtain an API key.
- **Send an email with your company name to:  keyrequest@everythingblockchain.io**

___
## Installation Steps
___
```sh
#1 Install this EBBuild Client nuget package to your Microsoft Visual Studio project.
#2 Add an appsettings.json file to project, if you don't already have an appsettings.json file added.
#3 Add the following values to the root of your appsettings.json file added to your project:
- "EbbuildApiBaseUri": "https://api.ebiblockchain.io/",
- "QueryChainToken": [enter the API key obtained supplied to you.],
- "QueryChainRoles": [enter user groups here.  For example you could enter: "Testers"],
```

___
## How To's
___
```sh
#1 After installing the EBBuildClient nuget package, add the following to your code:
      using EBBuildClient.Core;
      
#2 Defiine a data type that you want to store and or retrieve from the EBBuild cloud storage services.
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
    
    NOTE: If you require raw data, then use our internal RawDataType class type.
    NOTE: The RawDataType class has a single property called "RawData" which will contain your unstructured json payload.
    
    RawDataType rawData = await EBBuildAPIService.GetLedgerRecord<RawDataType>(EBBuildAPIServices);
    
#3 Set the following required parameters.
     Microsoft.Extensions.Configuration.IConfiguration configuration;
     string email = "a valid email address";
     string tenantID = "any unique string to identify your organization";
     string ledgerPreface = "any string to identify your ledger.  i.e.: prod, qa, dev, crypto, etc.";
     string ledgerName = "any name of your ledger where your data class will be stored.  i.e. payments";
    
#4 Create an instance of the EBBuildClient instance.
     var EBBuildAPIServices = new EBBuildAPIService(
     configuration, 
     email, 
     tenantID, 
     ledgerPreface,  
     ledgerName);
   
#5 After creating an instance of the EBBuild Client in step #4 you MUST call either of the two methods:
     NOTE: Blockchain ledgers can contain multiples data types.  
     NOTE: By defailt all blocks for all datat types can be returned.  
     NOTE: To filter by a specific data type you can specify the name of the blocks data type you want returned.
     
     string blockTypeName = "SampleDataClass";
     
     NOTE: This method is static.
   - SampleDataClass record = await EBBuildAPIService.GetLedgerRecord<SampleDataClass>(
      EBBuildAPIServices, 
      blockTypeName); 
     
     NOTE: After calling this method, the internal context need to save updates is set and can be retrieved by calling:
   - EBBuildAPIService.GetLedgerListResponseRecord();
    
     NOTE: If you want to retrieve multiple ledger block records, you must (first) define filter conditions.
     NOTE: This method is static.
   - List<string> ledgerFilterConditions = new List<string>() 
     { {"FirstName:EQ:Tom:AND"},
       {"Address1.State:IN:[New York;Phoenix;London;Miami;Berlin "} 
     };
   
    NOTE: Blockchain ledgers can contain multiples data types.  
    NOTE: By defailt all blocks for all datat types can be returned.  
    NOTE: To filter by a specific data type you can specify the name of the blocks data type you want returned.
     
    string blockTypeName = "SampleDataClass";
   
   - List<SampleDataClass> records = await EBBuildAPIService.GetLedgerRecords<SampleDataClass>(
     ledgerFilterConditions, 
     EBBuildAPIServices, 
     blockTypeName); 
     
     NOTE: After calling this method, the internal context need to save updates is set and can be retrieved by calling:
   - EBBuildAPIService.GetLedgerListResponseRecords();
    
#6. Once the internal context has been set, you can save updated blocks to the ledger by calling the following method:
     SampleDataClass dataContext = new SampleDataClass();
     NOTE: To enforce multi-factor authentication (MFA) you can set the enableMFA parameter.
     NOTE: When suspicious access to the ledger is detected on the write an email will be issued is MFA is set to true.
     
     bool enableMFA = true;   
     string blockTypeName = "SampleDataClass";
     
     NOTE: This method is static.
     
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
|Like, Nlike|	Like or not like a value.|	{LastName:Like:”Miller”}
|ILke, Nilike|	Case insensitive like and not like.| {LastName:ILike:”Miller”}





	


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


