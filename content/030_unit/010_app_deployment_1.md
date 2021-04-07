+++
title = "App deployment"
date = 2020-07-08T14:37:59+03:00
weight = 10
+++

1. Deploy the app
```
kubectl apply -f files/5ingress/1arcadia.yaml
```

{{< output >}}
deployment.apps/arcadia-main created  
deployment.apps/arcadia-backend created  
deployment.apps/arcadia-app2 created  
deployment.apps/arcadia-app3 created  
service/arcadia-main created  
service/arcadia-backend created  
service/arcadia-app2 created  
service/arcadia-app3 created  
{{< /output >}}

2. Check that all is deployed and working as expected:  
```
kubectl get pods
```
{{< output >}}
NAME                              READY   STATUS    RESTARTS   AGE  
arcadia-app2-64ccdcdc97-2vn6w     1/1     Running   0          38s  
arcadia-app3-5d76bf776b-sj446     1/1     Running   0          38s  
arcadia-backend-bc96d5754-grwfn   1/1     Running   0          38s  
arcadia-main-5d9bc94d55-cc597     1/1     Running   0          39s  
{{< /output >}}

```
kubectl get svc -owide
```
{{< output >}}
NAME           TYPE           CLUSTER-IP       EXTERNAL-IP                                                                 PORT(S)        AGE    SELECTOR  
arcadia-app2   ClusterIP      172.20.215.142    none                                                                       80/TCP         23m    app=arcadia-app2  
arcadia-app3   ClusterIP      172.20.97.115     none                                                                       80/TCP         23m    app=arcadia-app3  
arcadia-main   ClusterIP      172.20.102.115    none                                                                       80:32065/TCP   23m    app=arcadia-main  
backend        ClusterIP      172.20.84.9       none                                                                       80/TCP         5s     app=arcadia-backend  
kubernetes     ClusterIP      172.20.0.1        none                                                                       443/TCP        108m    none   
{{< /output >}}

{{% notice info %}}
The application is not accessible yet. We will deploy the NGINX Ingress in the following sections.
{{% /notice %}}