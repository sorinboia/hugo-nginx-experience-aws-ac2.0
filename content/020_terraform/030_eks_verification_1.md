+++
title = "EKS Verification"
date = 2020-07-08T14:37:59+03:00
weight = 30
+++

Wait for Terraform to finish and verify the deployment is working as expected and we are able to control the Kubernetes environment.

1. We need to save the remote access config for the Kubernetes cluster locally:  
```
mkdir ~/.kube/ 
terraform output > ~/.kube/config
```

2. Check and see that our cluster is up an running.  
Below we should see our two K8s worker nodes:
```
kubectl get nodes
```
{{< output >}}     
NAME                                          STATUS   ROLES    AGE    VERSION
ip-10-0-3-204.eu-central-1.compute.internal   Ready    <none>   100s   v1.19.6-eks-49a6c0  
{{< /output >}}

3. Change the directory back to the original repo folder:
```
cd ..
```