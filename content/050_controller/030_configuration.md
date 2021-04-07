+++
title = "Build the configuration"
date = 2020-07-08T14:37:59+03:00
weight = 30
+++

1. Create an environment
##### "N" -> "Services" -> "Environments" -> "Create Environment"  
In all the fields, enter the following value: ```prod```.

Click on "View API Request".  
All the configuration on the Nginx Controller can be easlly automated with external orchestration systems, this view can help you in understanding how to generate the configuration API calls.  

The output will look like this:
{{< output >}}
{
  "metadata": {
    "name": "prod",
    "displayName": "prod",
    "description": "prod",
    "tags": [
      "prod"
    ]
  },
  "desiredState": {}
}
{{< /output >}}

Click "Submit".

2. Create the Certificate:  

##### "N" -> "Services" -> "Certs" -> "Create Cert"
> Name: ```server-cert```   
> Environment: ```prod ```  
> Chose "Copy and paste PEM text"  
> Private Key: Browse to https://raw.githubusercontent.com/sorinboia/nginx-experience-aws/master/certs_for_mtls/ca.key copy and paste.  
> Public Cert: Browse to https://raw.githubusercontent.com/sorinboia/nginx-experience-aws/master/certs_for_mtls/ca.pem copy and paste.  
> Submit

3. Create the Gateway:  

##### "N" -> "Services" -> "Gateways" -> "Create Gateway"
> Name: ```api.arcadia.aws.cloud```  
> Environment: ```prod```  
> Instance Refs: Select All  
> Hostname: ```https://<EXTERNAL-IP OF THE "microgateway" SERVICE>```  
> Cert Reference: ```server-cert```  
> Submit


4. Create the App: 

##### "N" -> "Services" -> "Apps" -> "Create App"
> Name: ```arcadia-api```   
> Environment: ```prod```  
> Submit

  
  
So far we have created an environment, uploaded the certificate/key that we will use for our HTTPS connection, created a gateway which represents our entry point into the API gateway and last defined a new application object.  