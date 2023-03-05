+++
title = "Nginx Kubernetes Ingress Installation"
menuTitle = "Nginx K8s Ingress Installation"
date = 2020-07-08T14:37:59+03:00
weight = 10
+++

We are going to use the Nginx Helm installation.

1. Run the command bellow:  

```
kubectl create ns nginx-ingress
helm install nginx-ingress nginx-stable/nginx-ingress -f /home/ubuntu/lab/files/4ingress/helm_ingress_values.yaml
```
{{< output >}}
Will put the output
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
dingress=$(kubectl get svc dashboard-nginx-ingress --namespace=nginx-ingress | tr -s " " | cut -d' ' -f4 | grep -v "EXTERNAL-IP")
echo "export dashboard_nginx_ingress=$dingress" >> ~/.bashrc 
ningress=$(kubectl get svc nginx-ingress-nginx-ingress --namespace=nginx-ingress | tr -s " " | cut -d' ' -f4 | grep -v "EXTERNAL-IP")
echo "export nginx_ingress=$ningress" >> ~/.bashrc 
source ~/.bashrc
```