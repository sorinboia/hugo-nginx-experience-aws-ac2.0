+++
title = "Remove configuration"
date = 2020-07-08T14:37:59+03:00
weight = 10
+++

1. In order to delete the resources created during this workshop, run the commands below:   

```
kubectl delete --all svc --namespace=nginx-ingress
kubectl delete --all svc --namespace=default
cd terraform
terraform destroy --auto-approve
```
&nbsp;&nbsp;
  
2. Finally, delete the previously created Cloud9 stack in the [CloudFormation console](https://eu-central-1.console.aws.amazon.com/cloudformation/home?region=eu-central-1#/).

{{% notice warning %}}
Please note: This will also delete the Cloud9 IDE instance.
{{% /notice %}}
&nbsp;&nbsp;
