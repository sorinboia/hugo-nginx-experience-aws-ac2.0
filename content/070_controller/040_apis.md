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
curl -k -sc cookie.txt -X POST --url "https://$controller_ip/api/v1/platform/login" --header 'Content-Type: application/json' --data '{"credentials": {"type": "BASIC","username": "admin@nginx.com","password": "Admin2021"}}'  

curl -k -sb cookie.txt -c cookie.txt --location --request PUT "https://$controller_ip/api/v1/security/identity-providers/apikey" --header 'Content-Type: application/json' --header 'Content-Type: text/plain' --data "@files/7controller/arcadia_idp.json"
curl -k -sb cookie.txt -c cookie.txt --location --request PUT "https://$controller_ip/api/v1/security/identity-providers/apikey/clients" --header 'Content-Type: application/json' --header 'Content-Type: text/plain' --data "@files/7controller/arcadia_idp_key.json"

curl -k -sb cookie.txt -c cookie.txt --location --request PUT "https://$controller_ip/api/v1/services/api-definitions/arcadia-api/versions/1.0.0" --header 'Content-Type: application/json' --header 'Content-Type: text/plain' --data "@files/7controller/arcadia_api_spec.json"
curl -k -sb cookie.txt -c cookie.txt --location --request PUT "https://$controller_ip/api/v1/services/environments/prod/apps/arcadia-api/published-apis/arcadia-api-pub" --header 'Content-Type: application/json' --header 'Content-Type: text/plain' --data "@files/7controller/arcadia_api_pub.json"
curl -k -sb cookie.txt -c cookie.txt --location --request PUT "https://$controller_ip/api/v1/services/environments/prod/apps/arcadia-api/components/arcadia-stocks" --header 'Content-Type: application/json' --header 'Content-Type: text/plain' --data "@files/7controller/arcadia_stocks_component.json"

```

We have just uploaded the OpenApi spec to the Nginx Controller.  
