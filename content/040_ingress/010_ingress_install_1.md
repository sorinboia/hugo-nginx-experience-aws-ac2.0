+++
title = "Nginx Kubernetes Ingress Installation"
menuTitle = "Nginx K8s Ingress Installation"
date = 2020-07-08T14:37:59+03:00
weight = 10
+++

We are going to use the Nginx installation manifests based on the [Nginx Ingress Controller installation guide](https://docs.nginx.com/nginx-ingress-controller/installation/installation-with-manifests/).
For simplicity - we have already prepared an installation script.  
1. Run the command bellow:  

```
./files/4ingress/ingress_install.sh
```
{{< output >}}
Starting Nginx Ingress Install
Cloning into 'kubernetes-ingress'...
remote: Enumerating objects: 1270, done.
remote: Counting objects: 100% (1270/1270), done.
remote: Compressing objects: 100% (799/799), done.
remote: Total 34666 (delta 552), reused 1008 (delta 422), pack-reused 33396
Receiving objects: 100% (34666/34666), 46.90 MiB | 9.88 MiB/s, done.
Resolving deltas: 100% (18747/18747), done.
Updating files: 100% (782/782), done.
Updating files: 100% (3425/3425), done.
Note: switching to 'v1.10.1'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

Turn off this advice by setting config variable advice.detachedHead to false

HEAD is now at ba03a73d Release 1.10.1 (#1461)
namespace/nginx-ingress created
serviceaccount/nginx-ingress created
clusterrole.rbac.authorization.k8s.io/nginx-ingress created
clusterrolebinding.rbac.authorization.k8s.io/nginx-ingress created
clusterrole.rbac.authorization.k8s.io/nginx-ingress-app-protect created
clusterrolebinding.rbac.authorization.k8s.io/nginx-ingress-app-protect created
secret/default-server-secret created
configmap/nginx-config created
ingressclass.networking.k8s.io/nginx created
customresourcedefinition.apiextensions.k8s.io/virtualservers.k8s.nginx.org created
customresourcedefinition.apiextensions.k8s.io/virtualserverroutes.k8s.nginx.org created
customresourcedefinition.apiextensions.k8s.io/transportservers.k8s.nginx.org created
customresourcedefinition.apiextensions.k8s.io/policies.k8s.nginx.org created
customresourcedefinition.apiextensions.k8s.io/globalconfigurations.k8s.nginx.org created
globalconfiguration.k8s.nginx.org/nginx-configuration created
customresourcedefinition.apiextensions.k8s.io/aplogconfs.appprotect.f5.com created
customresourcedefinition.apiextensions.k8s.io/appolicies.appprotect.f5.com created
customresourcedefinition.apiextensions.k8s.io/apusersigs.appprotect.f5.com created
service/nginx-ingress created
deployment.apps/nginx-ingress created
configmap/nginx-config configured
Install finished
{{< /output >}}
  
2. Expose the Nginx Ingress Dashboard.
```
cat << EOF | kubectl apply -f -
apiVersion: v1
kind: Service
metadata:
  name: dashboard-nginx-ingress
  namespace: nginx-ingress
  annotations:
    service.beta.kubernetes.io/aws-load-balancer-backend-protocol: "tcp"
    service.beta.kubernetes.io/aws-load-balancer-type: nlb
spec:
  type: LoadBalancer
  ports:
  - port: 80
    targetPort: 8080
    protocol: TCP
    name: http
  selector:
    app: nginx-ingress
EOF
```

3. Check what we did so far is actually working:

```
kubectl get svc --namespace=nginx-ingress
```
{{< output >}}
NAME                      TYPE           CLUSTER-IP      EXTERNAL-IP                                                                 PORT(S)                      AGE
dashboard-nginx-ingress   LoadBalancer   172.20.36.60    aeb592ad4011544219c0bc49581baa13-421891138.eu-central-1.elb.amazonaws.com   80:32044/TCP                 11m
nginx-ingress             LoadBalancer   172.20.14.206   ab21b88fec1f445d98c79398abc2cd5d-961716132.eu-central-1.elb.amazonaws.com   80:30284/TCP,443:31110/TCP   5h35m
{{< /output >}}



Note the EXTERNAL-IP of the "dashboard-nginx-ingress". This is the hostname that we are going to use in order to view the Nginx Dashboard.  
Browse to the following location and verify you can see the dashboard: `http://<DASHBOARD-EXTERNAL-IP>/dashboard.html`

Note the EXTERNAL-IP of the "nginx-ingress". This is the hostname that we are going to use in order to publish the Arcadia web application.  
Browse to the following location and verify that you receive a 404 status code: `http://<INGRESS-EXTERNAL-IP>/`  

{{% notice warning %}}
Please note that it might take some time for the DNS names to become available.
{{% /notice %}}

4. Save the EXTERNAL-IPs as env variables for later use
```
export dashboard_nginx_ingress=$(kubectl get svc dashboard-nginx-ingress --namespace=nginx-ingress | tr -s " " | cut -d' ' -f4 | grep -v "EXTERNAL-IP")
export nginx_ingress=$(kubectl get svc nginx-ingress --namespace=nginx-ingress | tr -s " " | cut -d' ' -f4 | grep -v "EXTERNAL-IP")
```