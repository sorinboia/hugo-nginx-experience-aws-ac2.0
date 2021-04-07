+++
title = "Add authentication to the API"
date = 2020-07-08T14:37:59+03:00
weight = 60
+++

Our APIs are published and now we will want to add authentication based on API Keys  


1. Create an Identity Provider:
##### "N" -> "Services" -> "Identity Provider" -> "Create an Identity Provider"
> Name: ```api-protect```  
> Environment: **prod**  
> Type: **API Key**  


Under "API Clients"
> Name: ```test-client```  
> Key: ```1234567890```  
> Click Submit  

2. Go the the edit mode of APIs we have previously published
##### "N" -> "Services" -> "APIs" -> Click "arcadia-api" -> Click the edit icon

![](/images/050_060_1.png )

3. Attach the new Identity Provider to the APIs
> Click "Routing"  
> Click the edit icon of "arcadia-app2-component" component security setting  

![](/images/050_060_2.png )

4. Click "Add Authentication" and fill in the fields
> Identity Provider: ```api-protect```  
> Credential Location: **Header**  
> Credentials Value: ```apikey```  
> Click Done  
> Click Submit  
> Click Submit again on the "Edit Published API" page

5. Check that the authentication is failing when not providing an API key

```
curl -k --location --request POST https://$microhost/api/rest/execute_money_transfer.php --header 'Content-Type: application/json' --data-raw '{"amount":"77","account":"2075894","currency":"EUR","friend":"Alfredo"}'
```

{{< output >}}
<html>
<head><title>401 Authorization Required</title></head>
<body>
<center><h1>401 Authorization Required</h1></center>
<hr><center>nginx</center>
</body>
</html>
{{< /output >}}  

6. Try again but this time including the API key and the test-client value

```
curl -k --location --request POST https://$microhost/api/rest/execute_money_transfer.php --header "apikey: 1234567890" --header 'Content-Type: application/json' --data-raw '{"amount":"77","account":"2075894","currency":"EUR","friend":"Alfredo"}'
```

{{< output >}}
{"name":"Alfredo", "status":"success","amount":"77", "currency":"EUR", "transid":"772067369", "msg":"The money transfer has been successfully completed "}
{{< /output >}}  