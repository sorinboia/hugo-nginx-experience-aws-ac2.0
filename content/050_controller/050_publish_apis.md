+++
title = "Publish the external APIs"
date = 2020-07-08T14:37:59+03:00
weight = 50
+++

We are now going to publish the previously imported OpenApi definition.

1. Publish the APIs:
##### "N" -> "Services" -> "APIs" -> Click "arcadia-api" -> Click "Add Published API"

![](/images/050_050_1.png )

2. Fill in the data as described bellow

> Name: ```arcadia-published-api```  
> Click Next  

> Environment: prod  
> App: ```arcadia-api```  
> Gateways: **api.arcadia.aws.cloud**  
> Click Next  

3. We will now add the components that represent the workload for the main app that traffic will be sent to.   
Click "Add New" on the "Components" column and fill the data as described bellow

> Name: ```arcadia-main-component```  
> Click Next

> Workload Group Name: ```arcadia-main-wl```  
> URI: ```http://arcadia-main```  
> Click Submit  

4. Add the second component that represents the workload for app2   
Click "Add New" on the "Components" column and fill the data as described bellow

> Name: ```arcadia-app2-component```  
> Click Next

> Workload Group Name: ```arcadia-app2-wl```  
> URI: ```http://arcadia-app2```    
> Click Submit  

5. Move the routes that start with "/api" in the "Unrouted" to the "Components" column under the "arcadia-app2-component".
Move the remaining routes under "arcadia-main-component" and click Submit

![](/images/050_050_2.png )

6. We have finished publishing the API all is left is to test it. Run the bellow curl command, you should receive a success message and if you go to the main Arcadia application and refresh the page you will be able to see the transaction we just did in the "Transfer History" section.

```
curl -k --location --request POST https://$microhost/api/rest/execute_money_transfer.php --header 'Content-Type: application/json' --data-raw '{"amount":"77","account":"2075894","currency":"EUR","friend":"Alfredo"}'
```

{{< output >}}
{"name":"Alfredo", "status":"success","amount":"77", "currency":"EUR", "transid":"944962065", "msg":"The money transfer has been successfully completed "}
{{< /output >}}