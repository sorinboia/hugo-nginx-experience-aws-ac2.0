+++
title = "Terraform Apply"
date = 2020-07-08T14:37:59+03:00
weight = 10
+++

1. Go to the "terraform" directory where we can find the terraform plan.

```bash
cd terraform
```

2. Run the following commands, terraform plan will show us what it is going to be deployed in AWS by Terraform:
```bash
terraform init
terraform plan
```


3. Now let's deploy the environment
```bash
terraform apply --auto-approve
```

{{% notice info %}}
It will take around 10 minutes for Terraform and AWS to finish the initial deployment.  
Please continue to the next page to learn about Kubernetes and Amazon EKS basics.
{{% /notice %}}
