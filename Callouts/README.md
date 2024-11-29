# Apex Calls to External Web Services (Callouts)

 * [Invoking Callouts using Apex](https://developer.salesforce.com/docs/atlas.en-us.206.0.apexcode.meta/apexcode/apex_callouts.htm)
 
 

To make a from a trigger, call a class method that executes `asynchronously`.  
This method is called a future method and is annotated with `@future(callout=true)`.

## Named Credentials
* A named credential specifies the URL of a callout endpoint and its required authentication parameters in one definition. Salesforce manages all authentication for Apex callouts that specify a named credential as the callout endpoint so that your code doesn’t have to. You can also skip remote site settings, which are otherwise required for callouts to external sites, for the site defined in the named credential.

#### In the following Apex code, a named credential and an appended path specify the callout’s endpoint.
```Apex
HttpRequest req = new HttpRequest();
req.setEndpoint('callout:***My_Named_Credential***/some_path');
req.setMethod('GET');
Http http = new Http();
HTTPResponse res = http.send(req);
System.debug(res.getBody());
```

#### Apex code looks like without a named credential. 
```Apex
HttpRequest req = new HttpRequest();
req.setEndpoint('***https://my_endpoint.example.com/some_path***');
req.setMethod('GET');

// Because we didn't set the endpoint as a named credential, 
// our code has to specify:
// - The required username and password to access the endpoint
// - The header and header information
 
String username = 'myname';
String password = 'mypwd';
  
Blob headerValue = Blob.valueOf(username + ':' + password);
String authorizationHeader = 'BASIC ' +
EncodingUtil.base64Encode(headerValue);
req.setHeader('Authorization', authorizationHeader);
   
// Create a new http object to send the request object
// A response object is generated as a result of the request  
  
Http http = new Http();
HTTPResponse res = http.send(req);
System.debug(res.getBody());

```

