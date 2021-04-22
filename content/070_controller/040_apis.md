+++
title = "Import the OpenApi definition"
date = 2020-07-08T14:37:59+03:00
weight = 40
+++

Next we are going to publish the application APIs to the world.   
There are two ways of creating this configuration, the first one is manual similar to the way we performed the configuration until this point and the second one is described bellow.  

As part of their development cycle, the developers of the Arcadia application are generating an [OpenApi](https://swagger.io/docs/specification/about/) specification to describe their APIs.  
We are going to use this API specification in order to publish the services to the world.

1. Run the following curl commands.  

```
curl -k -sc cookie.txt -X POST --url "https://$controller_ip/api/v1/platform/login" --header 'Content-Type: application/json' --data '{"credentials": {"type": "BASIC","username": "nginx@f5.com","password": "Admin2020"}}'  
curl -k -sb cookie.txt -c cookie.txt --location --request PUT "https://$controller_ip/api/v1/services/api-definitions/arcadia-api/versions/v1" --header 'Content-Type: application/json' --header 'Content-Type: text/plain' --data "@files/6controller/arcadia_api_spec.json"
```

We have just uploaded the OpenApi spec to the Nginx Controller.  

2. Go to "N" -> "Services" -> "APIs".  
You can see the "Arcadia API" definition listed.    
 
3. Check the DNS name of the backend servers we need to point our APIs to:
 ```
kubectl get svc
 ```
 {{< output >}}
 NAME              TYPE           CLUSTER-IP       EXTERNAL-IP                                                                 PORT(S)                      AGE
 arcadia-app2      ClusterIP      172.20.103.189   none                                                                        80/TCP                       171m
 arcadia-app3      ClusterIP      172.20.238.13    none                                                                        80/TCP                       171m
 arcadia-backend   ClusterIP      172.20.228.83    none                                                                        80/TCP                       109m
 arcadia-main      ClusterIP      172.20.166.2     none                                                                        80/TCP                       7s
 backend           ClusterIP      172.20.44.133    none                                                                        80/TCP                       171m
 kubernetes        ClusterIP      172.20.0.1       none                                                                        443/TCP                      8h
 microgateway      LoadBalancer   172.20.81.110    a2fa7314165114fb9b16ebd92a890078-367878391.eu-central-1.elb.amazonaws.com   80:32293/TCP,443:32428/TCP   12m
 {{< /output >}}

We are interested in "main" and "app2" and their DNS names are `arcadia-main` and `arcadia-app2`.
