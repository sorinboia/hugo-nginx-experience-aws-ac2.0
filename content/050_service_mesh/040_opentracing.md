+++
title = "Service Mesh Visibility"
date = 2020-07-08T14:37:59+03:00
weight = 40
+++


In order to be able see and monitor all of our application calls we will need to connect to the Grafana and Zipkin services 

1. Get the svc external ip
```
kubectl get svc zipkin -n zipkin
```

{{< output >}}
NAME      TYPE           CLUSTER-IP     EXTERNAL-IP                                                                 PORT(S)          AGE
zipkin    LoadBalancer   172.20.88.44   a7c02ef8a39b74da4ad41085261859f8-765188493.eu-central-1.elb.amazonaws.com   9411:31661/TCP   4d23h
{{< /output >}}

2. Go and buy or sell crypto coin. We will use Zipkin to trace this operation through the different services.
![](/images/050_040_2.JPG)



3. Browse to the Zipkin dashboard `http://<ZIPKIN-EXTERNAL-IP>:9411`
Create a filter as in the bellow image.
Click on the result that looks like the first entry.
![](/images/050_040_1.JPG)

4. When buy or selling crypto the HTTP from the browser is sent to the application.  
 **1** - We see the transaction coming from the user and processed by the stock transaction service  
 **2** - The stock transaction service contacts the user service in order to translate the user email address to the account id  
 **3 and 4** - Next will again contact the users service to get the account information ( balance and crypto currencies), the stocks service to get the current buy and sell prices.  
 Once the operation has been validated and approved it will be recorded in the database and a success response returned to the client.   
 **5** - The stock transaction service updates the user service in order to update the users balance

![](/images/nsm_trace.png)