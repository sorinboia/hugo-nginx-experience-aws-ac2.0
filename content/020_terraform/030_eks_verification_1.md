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
NAME                                          STATUS   ROLES    AGE   VERSION  
ip-10-0-2-32.eu-central-1.compute.internal    Ready     none    84s   v1.15.10-eks-bac369  
ip-10-0-3-217.eu-central-1.compute.internal   Ready     none    88s   v1.15.10-eks-bac369  
{{< /output >}}

And the **kube-system** pods (this is the namespace for objects created by the Kubernetes system):  
<pre>
Command:
kubectl get pods -n kube-system

Output:
NAME                       READY   STATUS    RESTARTS   AGE  
aws-node-9hrrm             1/1     Running   0          14m  
aws-node-gmfkm             1/1     Running   0          14m  
coredns-5b6dbb4b59-5r9kb   1/1     Running   0          17m  
coredns-5b6dbb4b59-k5z6k   1/1     Running   0          17m  
kube-proxy-7lv9h           1/1     Running   0          14m  
kube-proxy-wmmxw           1/1     Running   0          14m  
</pre>

At the moment we have our setup deployed as it can be seen in the bellow diagram.

![](/images/3env.JPG)

3. Change the directory back to the original repo folder:
```
cd ..
```