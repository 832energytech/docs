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
##
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
         EBBuildApiFactory _ebBuildDBApiServiceFactory = new EBBuildApiFactory(
             _maxConnections, 
             Program._configuation, 
             _ebbuildDBUserEmail, 
             _ebbuildDBTenantId, 
             _ebbuildDBLedgerPreface, 
             _ebbuildDBLedgerName, 
             _ebbuildDBApiBaseUri, 
             _maxRecordsReturned, 
             _ebbuildDBUseWebSockets);
    
    #4.2 IEBBuildAPIService : This interface is used for all connection object created by the EBBuildApiFactory.
         IEBBuildAPIService _client1 = _ebBuildDBApiServiceFactory.GetApiClient();
         
    #4.3 EBBuildAPIService  : This is a static class used to calling all CRUD methods.
         List<string> recordList = EBBuildAPIService.GetLedgerRecords<string>(
             _filters, 
             _client1).Result;
         List<string> recordList = EBBuildAPIService.GetLedgerRecords<string>(
             _filters, 
             _ebBuildDBApiServiceFactory.GetApiClient()).Result;
         List<SampleDataClass> recordList = EBBuildAPIService.GetLedgerRecords<SampleDataClass>(
             _filters, 
             _ebBuildDBApiServiceFactory.GetApiClient()).Result;
         
#5 The EBBuild cloud servics support query filters used to filter data objects.
#6 To simplify creating filter conditions, the EBBuild client provides a filter builder.
    #6.1 List<string> _filters = EBIBuildAPIHelper.BuildFilter(
        _filters, 
        "email", 
        FilterOperation.EQ, 
        "dummyuser15@gmail.com");
    #6.2 List<string> _filters = EBIBuildAPIHelper.BuildFilter(
        _filters, 
        "email", 
        FilterOperation.EQ, 
        "dummyuser15@gmail.com", 
        BooleanOperation.AND);
    
 NOTE: If you require raw data, then use our internal RawDataType class type.
    NOTE: The RawDataType class has a single property called "RawData" or simply use the "String" data 
    type which will contain your unstructured json payload.
    
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
     NOTE: When saving data to the ledger, if you set the MFA parameter to "true" and if suspicious read access to the 
     ledger is detected that read access will be block temporarily until an email challenge is sent to you 
     (the owner of the ledger to allow you an opportunity to block or allow access).
     
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
## Adding Filter Conditions
___

| Operation | Description | Example | Notes |
| ------ | ------ | ------ | ------ |
|Regexp	|Regular expressions	| {LastName:REGEX:\\b[L]\\w+} | ie: Match the lastname that begins with the letter "L".  See the "Regular Expression - Documentation (below)"
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
## Regular Expression - Documentation
___
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
___

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
___

MIT- The API is and will always remain free!  The data storage is based on a fixed fee.  Contact EverythingBlockchain, Inc for details.


