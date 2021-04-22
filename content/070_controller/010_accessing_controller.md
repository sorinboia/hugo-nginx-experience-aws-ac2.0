+++
title = "Accessing the Nginx Controller"
menuTitle = "Nginx Controller Access"
date = 2020-07-08T14:37:59+03:00
weight = 10
+++

1. The Nginx Controller has already been deployed with the terraform declaration, we need to find the public IP address.

```
cd terraform
export controller_ip=$(terraform state show "aws_instance.controller" | grep "public_ip" | grep -v "associate_public_ip_address" | cut -d'"' -f2)
curl -k -c cookie.txt -X POST --url "https://$controller_ip/api/v1/platform/login" --header 'Content-Type: application/json' --data '{"credentials": {"type": "BASIC","username": "nginx@f5.com","password": "Admin2020"}}'
export controller_apikey=$(curl -k -sb cookie.txt -c cookie.txt https://$controller_ip/api/v1/platform/global | jq .currentStatus.agentSettings.apiKey | tr -d '"')

terraform state show "aws_instance.controller" | grep "public_ip" | grep -v "associate_public_ip_address" | cut -d'"' -f2
cd ..
```

2. Browse (using `HTTPS`) to the IP address of the Controller and verify you have access:

{{% notice %}}
Username (email): nginx@f5.com  
Password: Admin2020
{{% /notice %}}
